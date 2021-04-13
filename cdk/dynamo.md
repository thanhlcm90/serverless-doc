# Configure DynamoDB in CDK

We are now going to start creating our resources using CDK. Starting with DynamoDB.

## Create a Dynamo Resource

Add the following to `lib/dynamo-stack.ts`

```typescript
import * as cdk from "@aws-cdk/core";
import * as dynamodb from "@aws-cdk/aws-dynamodb";

export class DynamoStack extends cdk.Construct {
  public readonly table: dynamodb.Table;

  constructor(scope: cdk.Construct, id: string) {
    super(scope, id);

    this.table = new dynamodb.Table(this, "DataComPOCTable", {
      tableName: "DataComPOCTable",
      billingMode: dynamodb.BillingMode.PAY_PER_REQUEST, // Use on-demand billing mode
      partitionKey: {name: "fileKey", type: dynamodb.AttributeType.STRING},

      // The default removal policy is RETAIN, which means that cdk destroy will not attempt to delete
      // the new table, and it will remain in your account until manually deleted. By setting the policy to
      // DESTROY, cdk destroy will delete the table (even if it has data in it)
      removalPolicy: cdk.RemovalPolicy.DESTROY, // NOT recommended for production code
    });

    // Output values
    new cdk.CfnOutput(this, "TableName", {
      value: this.table.tableName,
    });
    new cdk.CfnOutput(this, "TableArn", {
      value: this.table.tableArn,
    });
  }
}
```

Let’s add the DynamoDB CDK package. Run the following script

```shell
$ npm i @aws-cdk/aws-dynamodb --save
```

## Add resource to the Stack

Now let’s add this stack to our app.

Replace your `lib/poc-stack.ts` with this:

```typescript
import * as cdk from "@aws-cdk/core";
import {DynamoStack} from "./dynamo-stack";

export default class POCStack extends cdk.Stack {
  private readonly dynamo: DynamoStack;

  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

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
DataComPocStackDev.ApidataComPOCLambdaRestApiEndpoint3D8B1823 = https://fjburztudh.execute-api.ap-southeast-1.amazonaws.com/prod/
DataComPocStackDev.CognitoAuthenticatedRoleName0DCC2D8C = DataComPocStackDev-CognitoCognitoAuthRoleCognitoDe-9IG7Y0L57KOA
DataComPocStackDev.CognitoIdentityPoolId42D6FEAB = ap-southeast-1:52b58632-deba-4f82-b648-21ed0f9bec1a
DataComPocStackDev.CognitoUserPoolClientId2F6CFE90 = 45d1h4bciqlk6d3uu0vilhhfjq
DataComPocStackDev.CognitoUserPoolId622CD4B2 = ap-southeast-1_RivuYdwTo
DataComPocStackDev.DynamoTableArn478698E3 = arn:aws:dynamodb:ap-southeast-1:964867551044:table/DataComPOCTable
DataComPocStackDev.DynamoTableName4C29EEE6 = DataComPOCTable
DataComPocStackDev.S3AttachmentsBucketNameC3FD403A = datacompocstackdev-s3uploads570493ed-7g8l8a76eviy

Stack ARN:
arn:aws:cloudformation:ap-southeast-1:964867551044:stack/DataComPocStackDev/f7fb4a40-983d-11eb-8d7e-06850023b7d0
```

You’ll notice the table name and ARN in the output and exported values.

Note that, we created a completely new DynamoDB table here. If you want to remove the old table we created manually through the console, you can do so now. We are going to leave it as is, in case you want to refer back to it at some point.