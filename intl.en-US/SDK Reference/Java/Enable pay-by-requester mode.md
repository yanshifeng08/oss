# Enable pay-by-requester mode {#concept_943914 .concept}

After pay-by-requester mode is enabled for a bucket in Alibaba Cloud OSS, the requester instead of the bucket owner pays the cost of requests and traffic. The bucket owner always pays the cost of storing data. You can enable this feature to share data without having to pay for all requests and traffic by yourself.

## Request methods {#section_13d_o22_s0z .section}

-   Access from anonymous users is not allowed

    If you enable pay-by-requester mode for a bucket, anonymous users are not allowed to access the bucket. Requesters must provide authentication information so that OSS can identify and charge them, but not the bucket owner, for requests and traffic.

    If requesters assume the RAM role of an Alibaba Cloud account to request data, OSS charges this Alibaba Cloud account for such requests and traffic.

-   Requesters must include the x-oss-request-payer request header

    If you enable pay-by-requester mode for a bucket, requesters must include the x-oss-request-payer:requester request header in a PUT, POST, GET, or HEAD request. This field indicates that requesters understand that their requests and data downloads incur fees. Otherwise, requests cannot pass authentication.

    Bucket owners do not need to include the x-oss-request-payer request header when accessing their own buckets. In this case, bucket owners are requesters who pay for such requests and traffic.


For more information about pay-by-requester mode, see [Enable the pay-by-requester mode](../../../../reseller.en-US/Developer Guide/Buckets/Enable the pay-by-requester mode.md#) in the OSS Developer Guide.

## Enable pay-by-requester mode {#section_moe_ihr_qp8 .section}

The following code provides an example of how to enable pay-by-requester mode:

``` {#codeblock_2tm_1v2_dc0 .language-java}
// This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Enable requester payment mode.
Payer payer = Payer.Requester;
ossClient.setBucketRequestPayment(bucketName, payer);

// Close your OSSClient.
ossClient.shutdown();
```

## Obtain pay-by-requester configurations {#section_flp_uaa_dte .section}

The following code provides an example of how to obtain pay-by-requester configurations:

``` {#codeblock_4q2_w9e_exw .language-java}
// This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Obtain pay-by-requester configurations.
GetBucketRequestPaymentResult result = ossClient.getBucketRequestPayment(bucketName);
System.out.println("Payer:" + result.getPayer());

// Close your OSSClient.
ossClient.shutdown();
```

## Specify a third party to pay for access to objects {#section_tqa_fxw_6gb .section}

The following code provides an example of how to specify a third party to pay for access to objects when you use PutObject, GetObject, and DeleteObject:

``` {#codeblock_obh_nlp_pie .language-java}
// This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
Payer payer = Payer.Requester;

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Specify the payer when you call the PutObject operation.
String content = "hello";
PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, objectName, new ByteArrayInputStream(content.getBytes()));
putObjectRequest.setRequestPayer(payer);
ossClient.putObject(putObjectRequest);

// Specify the payer when you call the GetObject operation.
GetObjectRequest getObjectRequest = new GetObjectRequest(bucketName, objectName);
getObjectRequest.setRequestPayer(payer);
OSSObject ossObject = ossClient.getObject(getObjectRequest);
ossObject.close();

// Specify the payer when you call the DeleteObject operation.
GenericRequest genericRequest = new GenericRequest(bucketName, objectName);
genericRequest.setRequestPayer(payer);
ossClient.deleteObject(genericRequest);

// Close your OSSClient.
ossClient.shutdown();
```

