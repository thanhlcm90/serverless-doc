# Infrastructure as Code

Currently, you go through a bunch of manual steps with a lot of clicking around to configure the backend. This makes it pretty tricky to recreate this stack for a new project. Or to configure a new environment for the same project. CDK Framework is really good for converting this entire stack into code. This means that it can automatically recreate the entire project from scratch without ever touching the AWS Console. This general pattern is called `Infrastructure as code` and it has some massive benefits

## AWS CloudFormation

To do this we are going to be using AWS CloudFormation. CloudFormation is an AWS service that takes a template (written in JSON or YAML), and provisions your resources based on that

![CloudFormation](https://d33wubrfki0l68.cloudfront.net/8d8130ee0ddaa46f5d0b19dffe1de91fff238aeb/23c47/assets/diagrams/how-cloudformation-works.png)

It creates a CloudFormation stack from the submitted template, and that stack is directly tied to the resources that have been created. So if you remove the stack, the services that it created will be removed as well

## Problems with CloudFormation

CloudFormation is great for defining your AWS resources. However it has a few major drawbacks.

1. **Template is big**: In a CloudFormation template you need to define all the resources that your app needs. This includes quite a large number of minor resources that you won’t be directly interacting with. So your templates can easily be a few hundred lines long.

2. **Hard to maintain**: YAML and JSON are easy to get started with. But it can be really hard to maintain large CloudFormation templates. And since these are just simple definition files, it makes it hard to reuse and compose them.

3. **Difficult to learn**: Finally, the learning curve for CloudFormation templates can be really steep. You’ll find yourself constantly looking at the documentation to figure out how to define your resources.

To fix these issues, AWS launched the [AWS CDK project back in August 2018](https://aws.amazon.com/blogs/developer/aws-cdk-developer-preview/). It allows you to use modern programming languages like JavaScript or Python, instead of YAML or JSON.

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
2. [Configure DynamoDB in CDK](dynamo.md)
3. [Configure S3 in CDK](s3.md)
4. [Configure Cognito User Pool in CDK](cognito.md)
5. [Configure Lambda and API Gateway](api.md)
6. [CORS with CDK](cors.md)
7. [Deploy Your Serverless Infrastructure](deploy.md)

## Best practice

For POC project, we create each lambda function for each Http request. 

But in the big/complex project, we should build API Gateway with `RestAPI` instead of `LambdaRestApi`. 

In that case, we will use `proxy` technology with `aws-serverless-express`. Thinking about use NodeJS `ExpressJS` to build lambda function and proxy to the API Gateway.
