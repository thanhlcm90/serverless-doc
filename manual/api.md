# Lambda function and API Gateway

In this tutorial, I will introduce how to create the Lambda function and API gateway to execute the business logic.

## Overview

Now we are going to add DynamoDB and S3 to the mix. We’ll also be adding a few other Lambda functions.

So our new app backend architecture will look something like this.

![Overview](https://d33wubrfki0l68.cloudfront.net/f16e86fee16a3bfdbfc4fb5fc023482f5054f988/57164/assets/diagrams/serverless-public-api-architecture.png)

## First Lambda function: List S3 Object

First, we go to Lambda service page

![Alt text](images/lambda1.png?raw=true)

Next, we go to `Function`

![Alt text](images/lambda2.png?raw=true)

Click `Create function` button

![Alt text](images/lambda3.png?raw=true)

Choose `Author from scratch` and filling the information about the function

`Function name = ListFileS3`
`Runtime = Node.js 14.x`

So, we will so NodeJS to write the code for this function

![Alt text](images/lambda4.png?raw=true)

Scroll down and choose 

`Execution role = Create a new role from AWS policy templates`

`Policy templates = Amazon S3 object read-only permission`

Filling the role name

`Role name = ListFileS3Role`

![Alt text](images/lambda5.png?raw=true)

Then scroll all the way down and click `Create function`

Finally, we will see the function detail screen with the coding editor.

![Alt text](images/lambda6.png?raw=true)

Enter the sample code bellow:

```javascript
const AWS = require("aws-sdk");
const s3 = new AWS.S3();

exports.handler = async (event) => {
    const params = {
        Bucket: 'DatacomPOCUploads',
    };
    const data = await s3.listObjectsV2(params).promise();
    return {
        statusCode: 200,
        headers,
        body: JSON.stringify(data),
    };
}
```

Click `Deploy` to save and deploy the code

### Add Trigger: API Gateway

After we have the function, next step we will create the API Gateway, so that we can trigger the function by API Gateway

In the function detail screen, we click button `Add trigger`

![Alt text](images/lambda7.png?raw=true)

Choose the `API Gateway` in the list, filling the information as bellow

`API = Create an API`

`API Type = HTTP API`

`Security = Open`

![Alt text](images/lambda8.png?raw=true)

For Security, we will do later with Cognito intergration

Finally, click `Add` button

The resuls for API Gateway after creating

![Alt text](images/lambda9.png?raw=true)

We can execute the API via the API endpoint `https://2gih5rdu29.execute-api.ap-southeast-1.amazonaws.com/default/ListFileS3`

Example:

```shell
curl https://2gih5rdu29.execute-api.ap-southeast-1.amazonaws.com/default/ListFileS3
```