# Setup Dynamo Database

Why do we need the Dynamo?

In this Datacom POC project, we need store the data about user's download history when user click to download file. Meaning that we’ll be performing these operations on our database.

### About DynamoDB

Amazon DynamoDB is a fully managed NoSQL database that provides fast and predictable performance with seamless scalability. DynamoDB is also a Serverless database, which means (as you guessed) it’ll scale automatically and you only pay for what you use.

Similar to other databases, DynamoDB stores data in tables. Each table contains multiple items, and each item is composed of one or more attributes. We are going to cover some basics in the following chapters. But to get a better feel for it, here is a [great guide on DynamoDB](https://www.dynamodbguide.com/).

### Create Table

Go to DynamoDB service:

![Create Table 1](https://d33wubrfki0l68.cloudfront.net/d671288f5fbc3cf9d1956ead460e5ad3136c0d6e/5f794/assets/dynamodb/select-dynamodb-service.png)

Click `Create table`

![Create Table 2](https://d33wubrfki0l68.cloudfront.net/76ca015e6ee527d0cf49becd3ca188a51baf554c/44239/assets/dynamodb/dynamodb-dashboard-create-table.png)

Enter the Table name and Primary key info:

`Table name = DatacomPOC`

`Primary key = fileKey`

![Create Table 3](https://d33wubrfki0l68.cloudfront.net/394de2aa974bf0520889d282b01723d3def1ed9d/013a7/assets/dynamodb/set-table-primary-key.png)

Each DynamoDB table has a primary key, which cannot be changed once set. The primary key uniquely identifies each item in the table, so that no two items can have the same key.

We choose the primary key is `fileKey` to store the S3 Object Key that marked downloaded status.

Next scroll down and deselect Use default settings.

![Create Table 4](https://d33wubrfki0l68.cloudfront.net/cc4633103fb8e596d1e0c1738e36b5b5ffd7e1e9/229d3/assets/dynamodb/deselect-use-default-settings.png)

Scroll down further and select On-demand instead of Provisioned.

![Create Table 5](https://d33wubrfki0l68.cloudfront.net/d89f2fed9add68495cbc07e69d8a47ac8c8fcaac/248d6/assets/dynamodb/select-on-demand-capacity.png)

On-Demand Capacity is DynamoDB’s pay per request mode. For workloads that are not predictable or if you are just starting out, this ends up being a lot cheaper than the Provisioned Capacity mode.

Finally, scroll down and hit Create.

![Create Table 6](https://d33wubrfki0l68.cloudfront.net/ef37c515bcf6def2f4b56aad61b32ee40c67663a/5a94a/assets/dynamodb/create-dynamodb-table.png)