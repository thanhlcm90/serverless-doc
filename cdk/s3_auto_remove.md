# Automatically delete trash files

Let’s improve the S3, add feature: Automatically delete trash files

## Amazon S3 Lifecycle

To manage your objects so that they are stored cost effectively throughout their lifecycle, configure their Amazon S3 Lifecycle. An S3 Lifecycle configuration is a set of rules that define actions that Amazon S3 applies to a group of objects. In that case, we will use the **Expiration actions** - Define when objects expire. Amazon S3 deletes expired objects on your behalf.

[Official document from AWS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)

## Setting lifecycle with CDK

Modify the S3 resource file `s3-resource.ts` with following

```typescript
this.uploads = new s3.Bucket(this, "Uploads", {
  lifecycleRules: [
    {
      id: "auto_remove",
      expiration: cdk.Duration.days(1),
      tagFilters: ["remove"],
    },
  ],
});
```

## Update the lambda function with tagging

In the lambda function, when move file to `trash` folder, remember to add `remove` tag to object.