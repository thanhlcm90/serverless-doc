# Configure Cognito User Pool in CDK

So far we’ve configured [our DynamoDB table](dynamo.md) and [S3 bucket](s3.md) in CDK. We are now ready to setup our Cognito User Pool. The User Pool stores our user credentials and allows our users to login to our app.

## Create a Cognito Resource

Add the following to `lib/cognito-stack.ts`

```typescript
import * as cdk from "@aws-cdk/core";
import * as iam from "@aws-cdk/aws-iam";
import * as cognito from "@aws-cdk/aws-cognito";
import {CfnOutput} from "@aws-cdk/core";
import {UserPool, UserPoolClient, CfnIdentityPool} from "@aws-cdk/aws-cognito";
import {IBucket} from "@aws-cdk/aws-s3";

export interface CognitoStackProps {
  bucket: IBucket;
}

export default class CognitoStack extends cdk.Construct {
  public readonly userPool: UserPool;
  public readonly userPoolClient: UserPoolClient;
  public readonly identityPool: CfnIdentityPool;

  constructor(scope: cdk.Construct, id: string, props: CognitoStackProps) {
    super(scope, id);

    const {bucket} = props;

    this.userPool = new cognito.UserPool(this, "UserPool", {
      selfSignUpEnabled: false, // Allow users to sign up
      passwordPolicy: {
        minLength: 6,
        requireDigits: true,
        requireLowercase: true,
        requireUppercase: true,
        requireSymbols: false,
      },
      autoVerify: {
        email: false,
      }, // Verify email addresses by sending a verification code
      signInAliases: {
        email: true,
      }, // Set email as an alias
    });

    this.userPoolClient = new cognito.UserPoolClient(this, "UserPoolClient", {
      userPool: this.userPool,
      authFlows: {
        userPassword: true,
        userSrp: true,
      },
      generateSecret: false, // Don't need to generate secret for web app running on browsers
    });

    this.identityPool = new cognito.CfnIdentityPool(this, "IdentityPool", {
      allowUnauthenticatedIdentities: false, // Don't allow unathenticated users
      cognitoIdentityProviders: [
        {
          clientId: this.userPoolClient.userPoolClientId,
          providerName: this.userPool.userPoolProviderName,
        },
      ],
    });

    // default admin user
    new cognito.CfnUserPoolUser(this, "UserPoolUser", {
      userPoolId: this.userPool.userPoolId,
      desiredDeliveryMediums: ["EMAIL"],
      username: "admin@local.com",
    });

    // Export values
    new CfnOutput(this, "UserPoolId", {
      value: this.userPool.userPoolId,
    });
    new CfnOutput(this, "UserPoolClientId", {
      value: this.userPoolClient.userPoolClientId,
    });
    new CfnOutput(this, "IdentityPoolId", {
      value: this.identityPool.ref,
    });
  }
}
```

Let’s add the Cognito CDK package. Run the following script

```shell
$ npm i @aws-cdk/aws-cognito --save
```

## Add resource to the Stack

Now let’s add this stack to our app.

Replace your `lib/poc-stack.ts` with this:

```typescript
import * as cdk from "@aws-cdk/core";
import * as s3 from "@aws-cdk/aws-s3";
import {S3Stack} from "./s3-stack";
import CognitoStack from "./cognito-stack";
import {DynamoStack} from "./dynamo-stack";

export default class POCStack extends cdk.Stack {
  private readonly s3: S3Stack;
  private readonly cognito: CognitoStack;
  private readonly dynamo: DynamoStack;

  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    this.s3 = new S3Stack(this, "S3");
    this.cognito = new CognitoStack(this, "Cognito");
    this.dynamo = new DynamoStack(this, "Dynamo");
  }
}
```

## Deploy the Stack

To deploy your app, run the following

```shell
$ cdk deploy
```

You should see something like this at the end of the deploy process.

```shell
 ✅  DataComPocStackDev

Outputs:
DataComPocStackDev.CognitoIdentityPoolId42D6FEAB = ap-southeast-1:52b58632-deba-4f82-b648-21ed0f9bec1a
DataComPocStackDev.CognitoUserPoolClientId2F6CFE90 = 45d1h4bciqlk6d3uu0vilhhfjq
DataComPocStackDev.CognitoUserPoolId622CD4B2 = ap-southeast-1_RivuYdwTo
DataComPocStackDev.DynamoTableArn478698E3 = arn:aws:dynamodb:ap-southeast-1:964867551044:table/DataComPOCTable
DataComPocStackDev.DynamoTableName4C29EEE6 = DataComPOCTable
DataComPocStackDev.S3AttachmentsBucketNameC3FD403A = datacompocstackdev-s3uploads570493ed-7g8l8a76eviy

Stack ARN:
arn:aws:cloudformation:ap-southeast-1:964867551044:stack/DataComPocStackDev/f7fb4a40-983d-11eb-8d7e-06850023b7d0
```

You’ll notice the output has the name of our newly created Cognito User Poll and Cognito Identity Pool.