# Infrastructure as Code

Currently, you go through a bunch of manual steps with a lot of clicking around to configure the backend. This makes it pretty tricky to recreate this stack for a new project. Or to configure a new environment for the same project. CDK Framework is really good for converting this entire stack into code. This means that it can automatically recreate the entire project from scratch without ever touching the AWS Console. This general pattern is called `Infrastructure as code` and it has some massive benefits

## AWS CloudFormation

To do this we are going to be using AWS CloudFormation. CloudFormation is an AWS service that takes a template (written in JSON or YAML), and provisions your resources based on that

![CloudFormation](https://d33wubrfki0l68.cloudfront.net/8d8130ee0ddaa46f5d0b19dffe1de91fff238aeb/23c47/assets/diagrams/how-cloudformation-works.png)

It creates a CloudFormation stack from the submitted template, and that stack is directly tied to the resources that have been created. So if you remove the stack, the services that it created will be removed as well

## AWS CDK

AWS CDK (Cloud Development Kit), released in Developer Preview back in August 2018; allows you to use TypeScript, JavaScript, Java, .NET, and Python to create AWS infrastructure.

### How CDK works

CDK internally uses CloudFormation. It converts your code into a CloudFormation template. So in the above example, you write the code at the bottom and it generates the CloudFormation template at the top.

![How CDK works](https://d33wubrfki0l68.cloudfront.net/d3eafac875a44ca3b5e5353c5a7646338d0801ee/e0470/assets/diagrams/how-cdk-works.png)

When you run `cdk synth`, it converts these stacks into CloudFormation templates. And when you run `cdk deploy`, it’ll submit these to CloudFormation. CloudFormation creates these stacks and all the resources that are defined in them.

### Official Resources

- [Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/home.html)
- [API Reference](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-construct-library.html)
- [CDK Workshop](https://cdkworkshop.com/)
- [CDK Repository](https://github.com/aws/aws-cdk)

## Table of Contents
1. [Initial the CDK project](init.md)
1. [Configure DynamoDB in CDK](dynamo.md)
2. [Configure S3 in CDK](s3.md)
3. [Configure Cognito User Pool in CDK](cognito.md)
4. [Configure Lambda and API Gateway](api.md)
5. [Deploy Your Serverless Infrastructure](deploy.md)

