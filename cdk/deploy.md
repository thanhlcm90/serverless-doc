# Deploy Your Serverless Infrastructure

Now that we have all our resources configured, let’s go ahead and deploy our entire infrastructure.

## Deploy the CDK App

Run the following from your root directory:

```shell
$ cdk deploy
```

You should see something like this in your output.

```shell
DataComPocStackDev: deploying...
[0%] start: Publishing 2a7da9c0078815cc29a576511f6dac1953c243cc0a415edd8c86ed56943000c1:current
[50%] success: Published 2a7da9c0078815cc29a576511f6dac1953c243cc0a415edd8c86ed56943000c1:current
[50%] start: Publishing 577a52b9113e34cf42bf89d042a0e9b36d3417c592e6953fd14de3f860f50079:current
[100%] success: Published 577a52b9113e34cf42bf89d042a0e9b36d3417c592e6953fd14de3f860f50079:current
DataComPocStackDev: creating CloudFormation changeset...
[█████████████████████████████·····························] (15/30)



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