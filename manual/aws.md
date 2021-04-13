# Set up your AWS account

## Create an AWS Account

Visit the [AWS homepage](https://aws.amazon.com/) and hit the `Create a Free Account` and follow the steps to create your account.

![Create an AWS Account](https://d33wubrfki0l68.cloudfront.net/95863dd1718f385fa8e563fcfbf969008c53129b/42dc3/assets/create-an-aws-account.png)

## Create an IAM User <a name="IAM"></a>

Onec we have the AWS Account, in next step, we will create an IAM user to programmatically interact with it

Amazon IAM (Identity and Access Management) enables you to manage users and user permissions in AWS. You can create one or more IAM users in your AWS account. You might create an IAM user for someone who needs access to your AWS console, or when you have a new application that needs to make API calls to AWS. This is to add an extra layer of security to your AWS account.

[Please read the Offical document](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)

## Configure the AWS CLI

### Install the AWS CLI

AWS CLI needs Python 2 version 2.6.5+ or Python 3 version 3.3+ and Pip.

Using Pip you can install the AWS CLI (on Linux, macOS, or Unix) by running:

```shell
$ sudo pip install awscli
```

Or using Homebrew on macOS:

```shell
$ brew install awscli
```

### Add Your Access Key to AWS CLI

Now, we need to tell the AWS CLI to use your Access Keys from the above step: [Create an IAM User](#IAM)

It should look something like this:

- Access key ID: `ATIAOOSTODNN7EXAMPLE`
- Secret access key: `pJalrXUznFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

Next, we just run the following with your Secret Key ID and your Access Key

```shell
$ aws configure
```