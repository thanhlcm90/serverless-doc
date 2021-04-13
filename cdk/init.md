# Initial the CDK project

## Install CDK SDK

You must configure your workstation with your credentials and an AWS Region, if you have not already done so. If you have the AWS CLI installed, the easiest way to satisfy this requirement is issue the following command

```shell
$ aws configure
```

Install the AWS CDK Toolkit globally using the following Node Package Manager command.

```shell
$ npm -g install cdk
```

Run the following command to verify correct installation and print the version number of the AWS CDK.

```shell
$ cdk --version
```

## Initial the project

Many AWS CDK stacks that you write will include assets: external files that are deployed with the stack, such as AWS Lambda functions Docker images. The AWS CDK uploads these to an Amazon S3 bucket or other container so they are available to AWS CloudFormation during deployment. Deployment requires that these containers already exist in the account and region you are deploying into. Creating them is called bootstrapping:

```shell
$ cdk bootstrap
```

## Application Stack

Create the `app.ts` file in folder `bin`

```typescript
import * as cdk from "@aws-cdk/core";
import POCStack from "../lib/poc-stack";

const app = new cdk.App();
const envConfig: cdk.Environment = {
  region: "ap-southeast-1",
  account: "964867551044",
};

new POCStack(app, "DataComPocStackDev", {
  env: envConfig,
});
```

Next, we will create application stack to implement all aws service, we call it `POCStack`. Create the file `poc-stack.ts` in folder `lib`

```typescript
import * as cdk from "@aws-cdk/core";
import {S3Stack} from "./s3-stack";

export default class POCStack extends cdk.Stack {
  private readonly s3: S3Stack;

  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // example initial S3 service
    this.s3 = new S3Stack(this, "S3");
  }
}
```

## Setup CDK config file

Open the file `cdk.json` and replace the code bellow

```json
{
  "app": "npx ts-node bin/app.ts",
  "context": {
    "@aws-cdk/core:enableStackNameDuplicates": "true",
    "aws-cdk:enableDiffNoFail": "true",
    "@aws-cdk/core:stackRelativeExports": "true"
  }
}
```

So, in that project, we will using `TypeScript` language to code infrastucture
