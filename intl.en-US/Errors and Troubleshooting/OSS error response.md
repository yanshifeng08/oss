# OSS error response {#concept_dt2_hq3_wdb .concept}

If an error occurs when you access OSS, OSS returns the error code and error message so that you can locate the problem and handle it properly.

## Response message body {#section_zgc_wq3_wdb .section}

If an error occurs when you access OSS, OSS returns an HTTP status code \(3xx, 4xx, or 5xx\) and a message body in application or XML format.

An example of the message body of a error response is as follows:

``` {#codeblock_29z_w98_evy}
<? xml version="1.0" ? >
<Error xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Code>
        AccessDenied
    </Code>
    <Message>
        Query-string authentication requires the Signature, Expires and OSSAccessKeyId parameters
    </Message>
    <RequestId>
        1D842BC5425544BB
    </RequestId>
    <HostId>
        oss-cn-hangzhou.aliyuncs.com
    </HostId>
</Error>
```

The message body of an error response includes the following elements:

-   Code: The error code that OSS returns to the user
-   Message: The detailed error message returned by OSS
-   RequestId: The UUID that uniquely identifies a request. When you cannot solve the error, you can provide this RequestId to Alibaba Cloud OSS technical support to get help.
-   HostId: Used to identify the accessed OSS cluster, which is the same as the Host ID carried in the user request.

For special error information elements, see specific request descriptions.

## OSS error codes {#section_zzm_z5v_zi9 .section}

The following table lists the OSS error codes:

|HTTP status code|Error code|Description|Cause and solution|
|----------------|----------|-----------|------------------|
|203|CallbackFailed|Upload callback fails.|The setting or format of the callback parameters is incorrect. For example, upload callback fails because the callback parameters within the ArgumentValue is not in the valid JSON format. To learn the cause and troubleshooting, see [Upload callback](intl.en-US/Errors and Troubleshooting/Upload callback.md#).

 |
|400|InvalidBucketName|The bucket name is invalid.|The bucket name does not conform to the naming conventions. For more information about the bucket naming conventions, see [Bucket](../../../../intl.en-US/Developer Guide/Basic concepts.md#section_yxy_jmt_tdb).

 |
|InvalidObjectName|The object name is invalid.|The object name does not conform to the naming conventions. For more information about the object naming conventions, see [Object](../../../../intl.en-US/Developer Guide/Basic concepts.md#section_ihw_kmt_tdb).

 |
|TooManyBuckets|The number of buckets exceeds the limit.|An Alibaba CLoud account can create a maximum of 30 buckets in a region. To adjust the limit, [open a bucket](https://workorder.console.aliyun.com/#/ticket/createIndex).

 |
|RequestIsNotMultiPartContent|The content-type of the Post request is invalid.|The content-type header in the Post request is not `multipart/form-data`. The content-type header in a Post request must be `multipart/form-data` and in the Content-Type为multipart/form-data;boundary=xxxxxx format, in which boundary is the boundary string. For more information about troubleshooting, see [PostObject](../../../../intl.en-US/API Reference/Object operations/PostObject.md#).

 |
|RequestTimeout|Request timeout occurs.|The request timeout occurs because of network environment or configurations. For more information about troubleshooting, see [Network connection timeout handling](intl.en-US/Errors and Troubleshooting/Network connection timeout handling.md#).

 |
|NotImplemented|The method cannot be implemented.|This error occurs because parameters are incorrectly passed when the API is encapsulated. For more information about troubleshooting, see the parameters described in [API overview](../../../../intl.en-US/API Reference/API overview.md#).

 |
|MaxPOSTPreDataLengthExceededError|The size of the body except for the uploaded file content of the Post request is too large.|The file uploaded by the Post request is larger than 5 GB. Only the `file` field can exceed 4 KB. For more information, see [PostObject](intl.en-US/Errors and Troubleshooting/PostObject.md#).

 |
|MalformedPOSTRequest|The format of the Post request body is invalid.|The format of the form field is invalid. For more information about troubleshooting, see [PostObject](intl.en-US/Errors and Troubleshooting/PostObject.md#).

 |
|MalformedXML|The XML format is invalid.|The XML format in the Post request is invalid. For more information about troubleshooting, see the following topics:

-   [DeleteObjects](../../../../intl.en-US/API Reference/Object operations/DeleteMultipleObjects.md#)
-   [CompleteMultipartUpload](../../../../intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#)
-   [PutBucketLogging](../../../../intl.en-US/API Reference/Bucket operations/PutBucketLogging.md#)
-   [PutBucketWebsite](../../../../intl.en-US/API Reference/Bucket operations/PutBucketWebsite.md#)
-   [PutBucketLifecycle](../../../../intl.en-US/API Reference/Bucket operations/PutBucketLifecycle.md#)
-   [PutBucketReferer](../../../../intl.en-US/API Reference/Bucket operations/PutBucketReferer.md#)
-   [PutBucketCORS](../../../../intl.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#)

 |
|InvalidTargetBucketForLogging|The target bucket specified in the logging operation is invalid.|The target bucket to store logs is invalid. Specify a valid target bucket.|
|InvalidPolicyDocument|The policy document is invalid.|The policy format in the Post request is incorrect. For more information about troubleshooting, see [PostObject](intl.en-US/Errors and Troubleshooting/PostObject.md#section_uxq_lfj_wdb).

 |
|InvalidPart|Invalid parts exist.|A part uploaded by CompleteMultipartUpload is invalid because its PartNumber or ETag is incorrect. For more information about troubleshooting, see [CompleteMultipartUpload](../../../../intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#).

 |
|InvalidPartOrder|The part order is invalid.|The parts submitted by CompleteMultipartUpload is invalid. Parts must be submitted in an ascending sort order of PartNumber. For more information about troubleshooting, see [CompleteMultipartUpload](../../../../intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#).

 |
|InvalidEncryptionAlgorithmError|The specified entropy encryption algorithm is incorrect.|The specified value of x-oss-server-side-encryption is invalid. Only AES256 and KMS are supported. For more information about troubleshooting, see [PutObject](../../../../intl.en-US/API Reference/Object operations/PutObject.md#).

 |
|InvalidDigest|The digest is invalid.|If the Content-MD5 header is included in the request, OSS calculates the Content-MD5 value of the request body. If the Content-MD5 values are inconsistent, this error code is returned. For more information about troubleshooting, see [PutObject](../../../../intl.en-US/API Reference/Object operations/PutObject.md#).

 |
|InvalidTargetType|The type of the object that the symbol link directs to is invalid.|The object is a symbol link and the object that the link directs to is also a symbol link.|
|InvalidArgument|The parameter format is incorrect.|The parameter format is incorrect. For more information about the parameter format, see [API overview](../../../../intl.en-US/API Reference/API overview.md#).

 |
|IncorrectNumberOfFilesInPOSTRequest|The number of files in the Post request is invalid.|Only one file field is allowed in the form fields of a Post request. For more information, see [PostObject](intl.en-US/Errors and Troubleshooting/PostObject.md#).

 |
|FilePartNotExist|The file part does not exist.|The part submitted by CompleteMultipartUpload is not uploaded. For more information about troubleshooting, see [CompleteMultipartUpload](../../../../intl.en-US/API Reference/Multipart upload operations/CompleteMultipartUpload.md#).

 |
|FieldItemTooLong|The form field in the Post request is too large.|Only the file field can exceed 4 KB. For more information, see [PostObject](intl.en-US/Errors and Troubleshooting/PostObject.md#).

 |
|EntityTooSmall|The entity is too small.|Set the Post policy to specify the valid values of form fields when using PostObject to upload files. For example, content-length-range can be used to set the maximum and minimum size of an uploaded object. The condition supports the matching method of contion-length-range, that is, the error is reported when the value is extremely large or small, For more information about troubleshooting, see [PostObject](../../../../intl.en-US/API Reference/Object operations/PostObject.md#).

 |
|EntityTooLarge|The entity is too large.|
|403|AccessDenied|The access is denied.|You do not have the permission to perform the operation. For more information, see [OSS permission](intl.en-US/Errors and Troubleshooting/OSS permission.md#).

 |
|InvalidAccessKeyId|The AccessKeyId is invalid.|The AccessKeyId is invalid or expired. For more information, see [OSS 403](intl.en-US/Errors and Troubleshooting/OSS 403.md#).

 |
|InvalidObjectState|The object state is invalid.|When you download an object of the Archive class, the state of the object is invalid in the following two conditions: -   The RestoreObject request is not submitted or the last RestoreObject request is timeout.
-   The RestoreObject request is submitted but the RestoreObject operation is not complete.

 For more information about troubleshooting, see [RestoreObject](../../../../intl.en-US/API Reference/Object operations/RestoreObject.md#).

 |
|RequestTimeTooSkewed|The time when OSS receives the request is more than 15 minutes later than the time when the request is sent.|Check the system time of the device from where the request is sent, and then adjust the time according to your time zone. For more information, see [OSS 403](intl.en-US/Errors and Troubleshooting/OSS 403.md#).

 |
|SignatureDoesNotMatch|The signature is incorrect.| The signature of the request is incorrect.

 |
|SignatureDoesNotMatch|The signature is incorrect.|The signature of the request is incorrect.|
|404|SymlinkTargetNotExist|The target object that the symbol link directs to does not exist.|The object is a symbol link, and the target object that the symbol link directs to does not exist.|
|NoSuchBucket|The bucket does not exist.|The requested bucket does not exist.|
|NoSuchKey|The object does not exist.|The requested object does not exist.|
|NoSuchUpload|The ID of the MultipartUpload task does not exist.|The MultipartUpload task is not initialized or the initialized MultipartUpload task is expired. For more information about troubleshooting, see [InitiateMultipartUpload](../../../../intl.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#).

 |
|405|MethodNotAllowed|The method is not supported.|The operation is not supported.|
|409|BucketAlreadyExists|The bucket already exists.|The specified bucket name already exists. Specify another name for the new bucket. For more information about bucket naming conventions, see [Bucket](../../../../intl.en-US/Developer Guide/Basic concepts.md#section_yxy_jmt_tdb).

 |
|BucketNotEmpty|The bucket is not empty.|The bucket to be deleted includes objects that are not deleted or multipart upload tasks that are not complete. For more information about troubleshooting, see [DeleteBucket](../../../../intl.en-US/API Reference/Bucket operations/DeleteBucket.md#).

 |
|ObjectNotAppendable|The object is not appendable.|OSS objects can be classified into three types: normal, appendable, and multipart. The AppendObject operation can be performed only on objects of the appendable type.|
|PositionNotEqualToLength|The position where the object is appended does not equal to the object size.| -   The value of position is inconsistent with the size of the current object.

**Note:** You can obtain the position for the next operation from the response header x-oss-next-append-position, and then send the next request. However, the same error may occur even if the value of position is set to x-oss-next-append-position because of the concurrency of the requests.

-   When the value of position is 0, if appendable objects with the same name specified in the request does not exist or the size of an appendable object with the same name is 0, the request is successful. Otherwise, the value of position and the size of the object does not match and this error code is returned.

 |
|411|MissingContentLength|The content length is not included in the request.|The request header is not encoded by using [chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1), and does not include the Content-Length parameter.|
|412|PreconditionFailed|The pre-processing fails.| -   The value of If-Unmodified-Since is specified, but the specified time is before the modification time of the object.
-   The value of If-Match is specified, but the ETag of the original object is different from the ETag value in the request.

 For more information about troubleshooting, see [GetObject](../../../../intl.en-US/API Reference/Object operations/GetObject.md#).

 |
|503|Downloadtrafficratelimitexceeded|The downloading traffic exceeds the limit.|The default limit of the downloading traffic through the Internet and intranet is 5 Gbit/s. To adjust the limit, [open a ticket](https://workorder.console.aliyun.com/#/ticket/createIndex).|
|UploadTrafficRateLimitExceeded|The uploading traffic exceeds the limit.|The default limit of the uploading traffic through the Internet and intranet is 5 Gbit/s. To adjust the limit, [open a ticket](https://workorder.console.aliyun.com/#/ticket/createIndex).|
|MetaOperationQpsLimitExceeded|The QPS exceeds the default limit.|OSS limits the QPS for the following APIs: -   Service-related operations, such as GetService \(ListBuckets\)
-   Bucket-related operations, such as PutBucket and GetBucketLifecycle
-   CORS-related operations, such as PutBucketCORS and GetBucketCORS
-   LiveChannel-related operations, such as PutLiveChannel and DeleteLiveChannel.

 If the QPS exceeds the limit, this error code is returned. We recommend that you perform the operation again after a few seconds.

 |

**Note:** For more information about OSS error code, see [OSS API error center](https://error-center.alibabacloud.com/status/product/Oss).

## Common errors and troubleshooting {#section_g4x_wq3_wdb .section}

For more information about OSS common errors and troubleshooting, see:

-   [Upload callback](intl.en-US/Errors and Troubleshooting/Upload callback.md#)
-   [OSS 403](intl.en-US/Errors and Troubleshooting/OSS 403.md#)
-   [PostObject](intl.en-US/Errors and Troubleshooting/PostObject.md#)
-   [OSS permission](intl.en-US/Errors and Troubleshooting/OSS permission.md#)
-   [OSS CORS](intl.en-US/Errors and Troubleshooting/OSS CORS.md#)
-   [Referer](intl.en-US/Errors and Troubleshooting/Referer.md#)
-   [STS](intl.en-US/Errors and Troubleshooting/STS common errors and troubleshooting.md#)

For more information about common errors and troubleshooting for SDK or tools, see:

-   Java SDK:[FAQ](../../../../intl.en-US/SDK Reference/Java/FAQ.md#)
-   Node.js SDK:[FAQ](../../../../intl.en-US/SDK Reference/Node. js/FAQ.md#)
-   [ossfs](../../../../intl.en-US/Tools/ossfs/FAQ.md#)
-   [ossftp](../../../../intl.en-US/Tools/ossftp/FAQ.md#)

## Unsupported operations {#section_ecd_wr3_wdb .section}

If you access OSS resources by performing an operation that is not supported by OSS, the 405 Method Not Allowed error is returned.

Example of an incorrect request:

``` {#codeblock_f2f_20k_qpg}
ABC /1.txt HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Date: Thu, 11 Aug 2016 03:53:40 GMT
Authorization: signatureValue
```

Response example:

``` {#codeblock_gcr_ldn_qkb}
HTTP/1.1 405 Method Not Allowed
Server: AliyunOSS
Date: Thu, 11 Aug 2016 03:53:44 GMT
Content-Type: application/xml
Content-Length: 338
Connection: keep-alive
x-oss-request-id: 57ABF6C8BC4D25D86CBA5ADE
Allow: GET DELETE HEAD PUT POST OPTIONS
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>MethodNotAllowed</Code>
  <Message>The specified method is not allowed against this resource. </Message>
  <RequestId>57ABF6C8BC4D25D86CBA5ADE</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Method>abc</Method>
  <ResourceType>Bucket</ResourceType>
</Error>
```

**Note:** If the resource to be accessed is /bucket/, the ResourceType is bucket. If the resource to be accessed is /bucket/object, the ResourceType is object.

## Unsupported parameters in supported operations {#section_hrc_fs3_wdb .section}

If unsupported parameters \(such as If-Modified-Since in PutObject operations\) is specified in supported OSS operations, the 400 Bad Request error is returned.

Example of an incorrect request:

``` {#codeblock_sx0_e8f_qh8}
PUT /abc.zip HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Accept: */*
Date: Thu, 11 Aug 2016 01:44:50 GMT
If-Modified-Since: Thu, 11 Aug 2016 01:43:51 GMT
Content-Length: 363
```

Response example:

``` {#codeblock_j4t_8kv_8yx}
HTTP/1.1 400 Bad Request
Server: AliyunOSS
Date: Thu, 11 Aug 2016 01:44:54 GMT
Content-Type: application/xml
Content-Length: 322
Connection: keep-alive
x-oss-request-id: 57ABD896CCB80C366955187E
x-oss-server-time: 0
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>NotImplemented</Code>
  <Message>A header you provided implies functionality that is not implemented. </Message>
  <RequestId>57ABD896CCB80C366955187E</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Header>If-Modified-Since</Header>
</Error>
```

