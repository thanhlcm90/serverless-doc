# Welcome to Datacom POC project documents!

This repository contains project documents for the Datacom POC project

## Table of Contents
1. [About this Repo](#About)
2. [Project Overview](#Overview)
2. [Manual Setup](#Manual)
3. [Infrastructure as Code](#Code)
4. [Setting ReactJS App](#React)
4. [License](#License)

## About this Repo <a name="About"></a>
This repo is my official document how to build serverless application for Datacom POC project. 

## Project Overview <a name="Overview"></a>

The Datacom POC project is builded on Serverless Infrastucture Stack using AWS service

### What is Serverless?

Serverless computing (or serverless for short), is an execution model where the cloud provider (AWS, Azure, or Google Cloud) is responsible for executing a piece of code by dynamically allocating the resources. And only charging for the amount of resources used to run the code. The code is typically run inside stateless containers that can be triggered by a variety of events including http requests, database events, queuing services, monitoring alerts, file uploads, scheduled events (cron jobs), etc. The code that is sent to the cloud provider for execution is usually in the form of a function. Hence serverless is sometimes referred to as “Functions as a Service” or “FaaS”. Following are the FaaS offerings of the major cloud providers:

- AWS: AWS Lambda
- Microsoft Azure: Azure Functions
- Google Cloud: Cloud Functions

### AWS Infrastucture

- Authentication with AWS Cognito
- Logic function with AWS Lambda and AWS API Gateway
- Database with AWS Dynamo
- Storage with AWS S3
- Logging with AWS CloudWatch
- Cloud-powered mobile and web apps with AWS Amplify
- Infrastucture as Code with AWS CDK

### Backend Technical Stack
- Lambda NodeJS
- AWS CDK

### Frontend Technical Stack
- ReactJS
- Amplify
- UmiJs
- Ant Design

## Manual Setup <a name="Manual"></a>

In that block, I will help you to setup the project's AWS infrastructure as manual way. The document will include those steps:

- [Set up your AWS account](manual/aws.md)
- [Setup Dynamo Database](manual/dynamo.md)
- [Setup AWS S3](manual/s3.md)
- [Users and authentication](manual/cognito.md)
- [CORS in Serverless](manual/cors.md)
- [Logging](manual/log.md)

## Infrastructure as Code <a name="Code"></a>

Currently, you go through a bunch of manual steps with a lot of clicking around to configure the backend. This makes it pretty tricky to recreate this stack for a new project. Or to configure a new environment for the same project. CDK Framework is really good for converting this entire stack into code. This means that it can automatically recreate the entire project from scratch without ever touching the AWS Console. This general pattern is called `Infrastructure as code` and it has some massive benefits

[Read more detail](cdk/README.md)

## Setting ReactJS App <a name="React"></a>

My frontend web application is builded on ReactJS with [UmiJS](https://v2.umijs.org) and [Ant Design](https://3x.ant.design). To work with AWS Cognito and API Gateway, we using the framework [AWS Amplify](https://aws.amazon.com/amplify/framework). The setup the frontend repo, please follow those steps:

- [Initialize the frontend repo](frontend/init.md)
- [Configure AWS Amplify](frontend/amplify.md)
- [Build and Deploy a React app](frontend/build.md)

## License <a name="License"></a>
This document is licensed under the Apache 2.0 License and written by Thanh Le (thanhlcm90@gmail.com)
