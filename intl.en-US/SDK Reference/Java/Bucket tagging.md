# Bucket tagging {#concept_266388 .concept}

By using the bucket tagging function, you can classify your buckets to manage them. For example, you can specify that only buckets with specified tags are returned in ListBucket operations.

-   Only the bucket owner and authorized RAM users can add tags to a bucket. Otherwise, the 403 Forbidden error is returned with the error code: AccessDenied.
-   You can add a maximum of 20 tags \(key-value pairs\) to a bucket.
-   The maximum size of a key is 64 bytes. The key of a tag cannot be null and cannot be prefixed with `http://`, `https://`, or `Aliyun`.
-   The maximum size of a tag value is 128 bytes. The value of a tag can be null.
-   The key and value of a tag must be UTF-8 encoded.
-   If you use PutBucketTagging to add tags to a bucket, the original tags added to the bucket are completely overwritten.

## Add tags to a bucket {#section_1jq_cmh_wdl .section}

You can run the following code to add tags to a bucket:

``` {#codeblock_lfa_w5l_ask}
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Adds tags to the bucket.
SetBucketTaggingRequest request = new SetBucketTaggingRequest("<yourBucketName>");
request.setTag("<yourTagkey1>", "<yourTagValue1>");
request.setTag("<yourTagkey2>", "<yourTagValue2>");
ossClient.setBucketTagging(request);

// Closes the OSSClient.
ossClient.shutdown();
```

## Obtain the tags added to a bucket {#section_q4o_gc1_131 .section}

You can run the following code to obtain the tags to a bucket:

``` {#codeblock_8io_1a4_psw}
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Obtains the tags added to the bucket.
TagSet tagSet = ossClient.getBucketTagging(new GenericRequest("<yourBucketName>"));
Map<String, String> tags = tagSet.getAllTags();

// Closes the OSSClient instance.
ossClient.shutdown();
```

## List buckets with a specified tag {#section_nt0_gl3_aey .section}

You can run the following code to list buckets with a specified tag:

``` {#codeblock_owg_ora_n24}
// This example uses the China East 1 (Hangzhou) endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Lists buckets with the specified tag. 
ListBucketsRequest listBucketsRequest = new ListBucketsRequest();
listBucketsRequest.setTag("<yourTagKey>", "<yourTagValue>");
BucketList bucketList = ossClient.listBuckets(listBucketsRequest);
for (Bucket o : bucketList.getBucketList()) {
  System.out.println("list result bucket: " + o.getName());
}

// Closes the OSSClient instance.
ossClient.shutdown();
				
```

## Delete the tags added to a bucket {#section_cw4_uca_rms .section}

You can run the following code to delete the tags added to a bucket:

``` {#codeblock_zn9_7x5_9yd}
// This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Deletes the tags added to the bucket.
ossClient.deleteBucketTagging(new GenericRequest("<yourBucketName>"));

// Closes the OSSClient instance.
ossClient.shutdown();
```

