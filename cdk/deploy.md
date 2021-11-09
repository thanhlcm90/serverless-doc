# Deploy Your Serverless Infrastructure

Now that we have all our resources configured, let’s go ahead and deploy our entire infrastructure.

## Deploy the CDK App

Run the following from your root directory:

```shell
$ cdk deploy
```

You should see something like this in your output.

```shell
StackDev: deploying...
[0%] start: Publishing 2a7da9c0078815cc29a576511f6dac1953c243cc0a415edd8c86ed56943000c1:current
[50%] success: Published 2a7da9c0078815cc29a576511f6dac1953c243cc0a415edd8c86ed56943000c1:current
[50%] start: Publishing 577a52b9113e34cf42bf89d042a0e9b36d3417c592e6953fd14de3f860f50079:current
[100%] success: Published 577a52b9113e34cf42bf89d042a0e9b36d3417c592e6953fd14de3f860f50079:current
StackDev: creating CloudFormation changeset...
[█████████████████████████████·····························] (15/30)



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