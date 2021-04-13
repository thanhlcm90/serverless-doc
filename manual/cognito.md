# Users and authentication

In [Lambda and API Gateway](api.md) section, we created a Serverless REST API and deployed it. But there are a couple of things missing.

- It’s not secure
- And, it’s not linked to a specific user

These two problems are connected. We need a way to allow users to sign in for our app and then only allow authenticated users to access it.
## Authenticated API Architecture

To allow users to sign in for our app and to secure our infrastructure, we’ll be moving to an architecture that looks something like this

![text](https://d33wubrfki0l68.cloudfront.net/f1be0d1d57a97bcbbee496376bd6a774d3ae8250/05e9d/assets/diagrams/serverless-auth-api-architecture.png)

## Cognito User Pool

To manage sign up and login functionality for our users, we’ll be using an AWS service called, [Amazon Cognito User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html). It’ll store our user’s login info. It’ll also be managing user sessions in our React app.

Please read the [official document](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-create-user-pool.html) about Create User Pool from AWS

## Cognito Identity Pool

To manage access control to our AWS infrastructure we use another service called [Amazon Cognito Identity Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html). This service decides if our previously authenticated user has access to the resources he/she is trying to connect to. Identity Pools can have different authentication providers (like Cognito User Pools, Facebook, Google etc.). In our case, our Identity Pool will be connected to our User Pool.

Please read the [official document](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-create-identity-pool.html) about Create User Pool from AWS

# Authentication Flow

## Sign up

That will done manual by ADMIN user. This that POC project, we don't implement sign up feature.

## Login

A signed up user can now login using their email and password. Our React app will send this info to the User Pool. If these are valid, then a session is created in React.

## Authenticated API Requests

To connect to our API.

1. The React client makes a request to API Gateway secured using IAM Auth.
2. API Gateway will check with our Identity Pool if the user has authenticated with our User Pool.
3. It’ll use the Auth Role to figure out if this user can access this API.
4. If everything looks good, then our Lambda function is invoked and it’ll pass in an Identity Pool user id.

## S3 File Uploads

In that project, the S3 Bucket will be private and only access via the API (build from Lambda function).

`Each user will have 1 folder with name is user id. The API will get user's id from authentication info and get correct files in user's folder. So, the user can not access others user data.`

![Alt text](images/auth_s3.png?raw=true)

We also have `zip` folder for temporary zip files when user want download multi files.