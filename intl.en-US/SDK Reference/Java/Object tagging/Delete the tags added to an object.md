# Delete the tags added to an object {#concept_727776 .concept}

This topic describes how to delete the tags added to an object.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

Run the following code to delete the tags added to an object:

``` {#codeblock_p41_zhr_4tf}
// This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Deletes the tags added to the object.
ossClient.deleteObjectTagging(bucketName, objectName);
```

For more information about how to delete the tags added to an object, see [DeleteObjectTagging](../../../../intl.en-US/API Reference/Object operations/DeleteObjectTagging.md#).

