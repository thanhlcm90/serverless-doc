# Configure Lambda and API Gateway in CDK

Finally, we will create the Lambda function and setup API Gateway via CDK.

## Create a API Resource

Add the following to `lib/api-stack.ts`

```typescript
import * as cdk from "@aws-cdk/core";
import * as lambda from "@aws-cdk/aws-lambda";
import * as apigw from "@aws-cdk/aws-apigateway";
import {UserPool} from "@aws-cdk/aws-cognito";
import {IBucket} from "@aws-cdk/aws-s3";

export interface ApiStackProps {
  userPool: UserPool;
  bucket: IBucket;
  table: Table;
}

export class ApiStack extends cdk.Construct {
  constructor(scope: cdk.Construct, id: string, props: ApiStackProps) {
    super(scope, id);

    const {userPool, bucket, table} = props;

    // defines an AWS Lambda resource
    const fileListFunc = new lambda.Function(this, "fileListFunction", {
      runtime: lambda.Runtime.NODEJS_14_X, // execution environment
      code: lambda.Code.fromAsset("lambda"), // code loaded from "lambda" directory
      handler: "file_list.handler", // file is "file_list", function is "handler"
      environment: {
        BUCKET_NAME: bucket.bucketName,
      },
    });

    // define role for lambda function to access the bucket
    bucket.grantRead(fileListFunc);

    // defines an API Gateway REST API resource backed by our "hello" function.
    const restApi = new apigw.LambdaRestApi(this, "LambdaRestApi", {
      restApiName: "API",
      handler: welcomeFunc,
      proxy: false,
      defaultCorsPreflightOptions: {
        allowOrigins: apigw.Cors.ALL_ORIGINS,
        allowMethods: apigw.Cors.ALL_METHODS,
        allowCredentials: true,
      },
    });

    // config CORS for 4xx error
    new apigw.CfnGatewayResponse(this, "Default4XXResponse", {
      responseType: "DEFAULT_4XX",
      restApiId: restApi.restApiId, // reference to RestApi created earlier
      responseParameters: {
        "gatewayresponse.header.Access-Control-Allow-Origin": "'*'",
      },
    });

    // config CORS for 5xx error
    new apigw.CfnGatewayResponse(this, "Default5XXResponse", {
      responseType: "DEFAULT_5XX",
      restApiId: restApi.restApiId, // reference to RestApi created earlier
      responseParameters: {
        "gatewayresponse.header.Access-Control-Allow-Origin": "'*'",
      },
    });

    // Authorizer for the Hello World API that uses the
    // Cognito User pool to Authorize users.
    const authorizer = new apigw.CfnAuthorizer(this, "cfnAuth", {
      restApiId: restApi.restApiId,
      name: "APIAuthorizer",
      type: apigw.AuthorizationType.COGNITO,
      identitySource: "method.request.header.Authorization",
      providerArns: [userPool.userPoolArn],
    });

    // API for GET /file
    const fileApi = restApi.root.addResource("file");
    fileApi.addMethod("GET", new apigw.LambdaIntegration(fileListFunc), {
      authorizationType: apigw.AuthorizationType.COGNITO,
      authorizer: {
        authorizerId: authorizer.ref,
      },
    });
  }
}
```

Let’s add the Lambda and Gateway CDK package. Run the following script

```shell
$ npm i @aws-cdk/aws-lambda --save
$ npm i @aws-cdk/aws-apigateway --save
```

## Write Lambda function

Next step, we will write sample lambda function in local project and deploy by CDK

Add the following to `lambda/file_list.ts`

```typescript
import * as AWS from "aws-sdk";
const s3 = new AWS.S3();
import {APIGatewayProxyEvent, APIGatewayProxyResult} from "aws-lambda";

if (!process.env.BUCKET_NAME) {
  throw new Error("Required environment variable BUCKET_NAME is missing");
}

const Bucket = process.env.BUCKET_NAME;

export const handler = async (
  event: APIGatewayProxyEvent
): Promise<APIGatewayProxyResult> => {
  console.log(event);

  // get user id via authorizer claims
  const sub = event.requestContext.authorizer?.claims.sub;
  
  // declare headers for CORS
  const headers = {
    "Access-Control-Allow-Credentials": true,
    "Access-Control-Allow-Origin": "*",
  };

  // declare StartAfter attribute to get only objects in user's folder
  const params: AWS.S3.Types.ListObjectsV2Request = {
    Bucket,
    StartAfter: `${sub}/private/`,
    Prefix: `${sub}/private/`,
  };

  const data = await s3.listObjectsV2(params).promise();
  
  return {
    statusCode: 200,
    headers,
    body: JSON.stringify(data),
  };
};
```

## Add resource to the Stack

Now let’s add this stack to our app.

Replace your `lib/poc-stack.ts` with this:

```typescript
import * as cdk from "@aws-cdk/core";
import * as s3 from "@aws-cdk/aws-s3";
import {S3Stack} from "./s3-stack";
import CognitoStack from "./cognito-stack";
import {ApiStack} from "./api-stack";

export default class POCStack extends cdk.Stack {
  private readonly s3: S3Stack;
  private readonly cognito: CognitoStack;
  private readonly api: ApiStack;

  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    this.s3 = new S3Stack(this, "S3");
    this.cognito = new CognitoStack(this, "Cognito");
    this.api = new ApiStack(this, "Api", {
      userPool: this.cognito.userPool,
      bucket: this.s3.uploads,
    });
  }
}

```

## Deploy the Stack

Before deploy the stack, we need build the Typescript code into Javascript code, because the Lambda service only support Javascript file.

```shell
$ tsc
```

To deploy your app, run the following

```shell
$ cdk deploy
```

You should see something like this at the end of the deploy process.

```shell
 ✅  StackDev

Outputs:
StackDev.ApiLambdaRestApiEndpoint3D8B1823 = https://fjburztudh.execute-api.ap-southeast-1.amazonaws.com/prod/
StackDev.CognitoAuthenticatedRoleName0DCC2D8C = StackDev-CognitoCognitoAuthRoleCognitoDe-9IG7Y0L57KOA
StackDev.CognitoIdentityPoolId42D6FEAB = ap-southeast-1:52b58632-deba-4f82-b648-21ed0f9bec1a
StackDev.CognitoUserPoolClientId2F6CFE90 = 45d1h4bciqlk6d3uu0vilhhfjq
StackDev.CognitoUserPoolId622CD4B2 = ap-southeast-1_RivuYdwTo
StackDev.S3AttachmentsBucketNameC3FD403A = stackdev-s3uploads570493ed-7g8l8a76eviy

Stack ARN:
arn:aws:cloudformation:ap-southeast-1:964867551044:stack/StackDev/f7fb4a40-983d-11eb-8d7e-06850023b7d0
```

You’ll notice the output has the name of our newly created Cognito User Poll and Cognito Identity Pool.