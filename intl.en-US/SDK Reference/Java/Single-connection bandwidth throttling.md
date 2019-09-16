# Single-connection bandwidth throttling {#concept_1614716 .concept}

This topic describes how to ensure other applications have sufficient bandwidth when you perform upload and download operations on OSS. You can include the x-oss-traffic-limit parameter in your requests and set bandwidth limits.

**Note:** For more information about the scenarios and precautions of the single-connection bandwidth throttling feature, see [Single-connection bandwidth throttling](../../../../reseller.en-US/Developer Guide/Single-connection bandwidth throttling.md#) in the OSS Developer Guide.

## Configure bandwidth throttling for common uploads and downloads {#section_vos_eb3_kvt .section}

Use the following code to configure bandwidth throttling when you upload and download objects to and from OSS:

``` {#codeblock_4hm_7v1_axe}
// This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String bucketName = "<yourBucketName>"
String objectName = "<yourObjectName>"
String localFileName = "<yourLocalFileName>"
String downLoadFileName = "<yourDownLoadFileName>"

// Set the bandwidth limit to 100 KB/s (819200 bit/s).
int limitSpeed = 100 * 1024 * 8;

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Configure bandwidth throttling for the object to be uploaded.
InputStream inputStream = new FileInputStream(localFileName);
PutObjectRequest PutObjectRequest = new PutObjectRequest(bucketName, key, inputStream);
PutObjectRequest.setTrafficLimit(limitSpeed);
ossClient.putObject(PutObjectRequest);

// Configure bandwidth throttling for the object to be downloaded.
GetObjectRequest getObjectRequest = new GetObjectRequest(bucketName, key);
getObjectRequest.setTrafficLimit(limitSpeed);
File localFile = new File(downLoadFileName);
ossClient.getObject(getObjectRequest, localFile);

 ossClient.shutdown();
```

## Configure bandwidth throttling for uploads and downloads that use signed URLs {#section_r4f_mvy_rea .section}

Use the following code to configure bandwidth throttling when you upload and download objects from and to OSS by using signed URLs.

``` {#codeblock_ukn_qy3_342}
// This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
String localFileName = "<yourLocalFileName>";
String downLoadFileName = "<yourDownLoadFileName>";

// Set the bandwidth limit to 100 KB/s (819200 bit/s).
int limitSpeed = 100 * 1024 * 8;

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Create a signed URL for uploading an object and set the valid period of the URL to 60s.
Date date = new Date(); 
date.setTime(date.getTime() + 60 * 1000);
GeneratePresignedUrlRequest request = new GeneratePresignedUrlRequest(bucketName, objectName, HttpMethod.PUT);
request.setExpiration(date);
request.setTrafficLimit(limitSpeed);
URL signedUrl = ossClient.generatePresignedUrl(request);
System.out.println("put object url" = signedUrl);

// Configure bandwidth throttling for the object to be uploaded.
InputStream inputStream = new FileInputStream(localFileName);
ossClient.putObject(signedUrl, instream, -1, null, true);

// Create a signed URL for downloading an object and set the valid period of the URL to 60s.
date = new Date(); 
date.setTime(date.getTime() + 60 * 1000);
request = new GeneratePresignedUrlRequest(bucketName, key, HttpMethod.GET);
request.setExpiration(date);
request.setTrafficLimit(limitSpeed);
signedUrl = ossClient.generatePresignedUrl(request);
System.out.println("get object url" = signedUrl);

// Configure bandwidth throttling for the object to be downloaded.
GetObjectRequest getObjectRequest =  new GetObjectRequest(signedUrl, null);
ossClient.getObject(getObjectRequest, downLoadFileName);

ossClient.shutdown();
```

