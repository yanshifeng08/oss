# Configure object tagging {#concept_727741 .concept}

This topic describes how to configure object tagging.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

**Note:** 

-   You can add tags to an object when uploading it or add tags for an uploaded object. If add tags to an object that already has tags, the original tags are overwritten. For more information about adding tags, see [PutObjectTagging](../../../../intl.en-US/API Reference/Object operations/PutObjectTagging.md#).
-   To add tags to an object, you must have the permission to call PutObjectTagging.
-   The Last-Modified value of an object is not updated when its tags are changed.
-   A tag can contain letters, numbers, spaces, and the following symbols: + ‑ = . \_ : /

## Add tags to an object when uploading it {#section_dl7_ktr_ujf .section}

-   Add tags to an object when uploading it by calling putObject.

    Run the following code to add tags to an object when uploading it by calling putObject:

    ``` {#codeblock_u5e_dt7_c5j}
    // This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Creates an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    Map<String, String> tags = new HashMap<String, String>();
    tags.put("key1", "value1");
    tags.put("key2", "value2");
    
    // Configures the tags in the HTTP headers.
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setObjectTagging(tags);
    
    // Uploads the object and add tags to it.
    String content = "<yourtContent>";
    ossClient.putObject(bucketName, objectName, new ByteArrayInputStream(content.getBytes()), metadata);
    
    // Closes the OSSClient.
    ossClient.shutdown();
    ```

-   Add tags to an object when uploading it in the multipart upload method.

    Run the following code to add tags to an object when uploading it in the multipart upload method.

    ``` {#codeblock_6p7_6zq_hh6}
    // This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    String localFile = "<yourLocalFile>";
    
    // Creates an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    /* Step 1: Initializes a multipart upload task.
    */
    // Configures the tags in the HTTP headers.
    Map<String, String> tags = new HashMap<String, String>();
    tags.put("key1", "value1");
    tags.put("key2", "value2");
    
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setObjectTagging(tags);
    
    // Sends an InitiateMultipartUploadRequest request and adds tags to the object.
    InitiateMultipartUploadRequest request = new InitiateMultipartUploadRequest(bucketName, objectName);
    InitiateMultipartUploadResult result = ossClient.initiateMultipartUpload(request);
    // Obtains the uploadId, which uniquely identifies the multipart upload task. You can use it to cancel or query the multipart upload task.
    String uploadId = result.getUploadId();
    
    /* Step 2: Uploads the parts.
    */
    // The partEtags parameter is a set of PartTags. A PartETag is composed of the ETag and number of the part.
    List<PartETag> partETags =  new ArrayList<PartETag>();
    // Calculates the number of parts that the object can be divided into.
    final long partSize = 1 * 1024 * 1024L;   // 1MB
    final File sampleFile = new File("<localFile>");
    long fileLength = sampleFile.length();
    int partCount = (int) (fileLength / partSize);
    if (fileLength % partSize != 0) {
        partCount++;
     }
    // Uploads the parts one by one.
    for (int i = 0; i < partCount; i++) {
        long startPos = i * partSize;
        long curPartSize = (i + 1 == partCount) ? (fileLength - startPos) : partSize;
        InputStream instream = new FileInputStream(sampleFile);
        // Skips the uploaded parts.
        instream.skip(startPos);
        UploadPartRequest uploadPartRequest = new UploadPartRequest();
        uploadPartRequest.setBucketName(bucketName);
        uploadPartRequest.setKey(objectName);
        uploadPartRequest.setUploadId(uploadId);
        uploadPartRequest.setInputStream(instream);
        // Sets the part size. The size of a part must be equal to or larger than 100 KB. However, the size of the last part is not limited.
        uploadPartRequest.setPartSize(curPartSize);
        // Sets the serial number of each part. Each part has a serial number ranging from 1 to 10,000. If the serial number of a part exceeds the range, the InvalidArgument error code is returned.
        uploadPartRequest.setPartNumber( i + 1);
      // The parts can be uploaded in different orders and even from different clients. OSS combines the parts following the serial numbers into the complete object.
        UploadPartResult uploadPartResult = ossClient.uploadPart(uploadPartRequest);
      // A PartTag is returned each time when a part is uploaded and is stored in the partETags parameter.
        partETags.add(uploadPartResult.getPartETag());
    }
    
    /* Step 3: Completes the multipart upload task.
    */
    // Sorts the partETags in an ascending order.
    Collections.sort(partETags, new Comparator<PartETag>() {
        public int compare(PartETag p1, PartETag p2) {
            return p1.getPartNumber() - p2.getPartNumber();
        }
    });
    // All valid partETags must be provided to perform this step. OSS verifies the availability of each part according to the partETags and combines the parts into the complete object.
    CompleteMultipartUploadRequest completeMultipartUploadRequest =
            new CompleteMultipartUploadRequest(bucketName, objectName, uploadId, partETags);
    ossClient.completeMultipartUpload(completeMultipartUploadRequest);
    
    // Views the tags added to the object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, objectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("object tagging: "+ getTags.toString());
    
    // Closes the OSSClient.
    ossClient.shutdown();
    ```

    ``` {#codeblock_513_d9a_pye}

    ```

-   Add tags to an object when uploading it in the append upload method.

    Run the following code to add tags to an object when uploading it in the append upload method:

    ``` {#codeblock_nhk_6tq_rx9}
    // This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Creates an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    String content1 = "Hello OSS A \n";
    String content2 = "Hello OSS B \n";
    String content3 = "Hello OSS C \n";
    
    Map<String, String> tags = new HashMap<String, String>();
    tags.put("key1", "value1");
    tags.put("key2", "value2");
    
    ObjectMetadata meta = new ObjectMetadata();
    // Configures the tags to be added to the object that needs to be uploaded.
     meta.setObjectTagging(tags);
    // Specifies the content type of the object to be uploaded.
    meta.setContentType("text/plain");
    
    // Sets parameters by calling AppendObjectRequest.
    AppendObjectRequest appendObjectRequest = new AppendObjectRequest(bucketName, objectName, new ByteArrayInputStream(content1.getBytes()), meta);
    
    // Sets each parameter by calling AppendObjectRequest.
    // Sets the name of the bucket that the object is uploaded to.
    //appendObjectRequest.setBucketName("<yourBucketName>");
    // Sets the object name.
    //appendObjectRequest.setKey("<yourObjectName>");
    // Sets the content to be appended to the object, which has two types: InputStream and File. This line sets the type to InputStream.
    //appendObjectRequest.setInputStream(new ByteArrayInputStream(content1.getBytes()));
    // Sets the content to be appended to the object, which has two types: InputStream and File. This line sets the type to File.
    //appendObjectRequest.setFile(new File("<yourLocalFile>"));
    // Sets the metadata of the object. The metadata settings take effect only when the object is appended for the first time.
    //appendObjectRequest.setMetadata(meta);
    
    // Appends the object for the first time. Only the tags set when the object is appended for the first time are added to the object.
    // Sets the position from where the object is appended.
    appendObjectRequest.setPosition(0L);
    AppendObjectResult appendObjectResult = ossClient.appendObject(appendObjectRequest);
    // Outputs the CRC64 value of the object, which is calculated according to the ECMA-182 standard.
    System.out.println(appendObjectResult.getObjectCRC());
    
    // Appends the object for the second time.
    // The nextPosition parameter indicates the position (object size) required for the next upload request.nextPosition.
    appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
    appendObjectRequest.setInputStream(new ByteArrayInputStream(content2.getBytes()));
    appendObjectResult = ossClient.appendObject(appendObjectRequest);
    
    // Appends the object for the third time.
    appendObjectRequest.setPosition(appendObjectResult.getNextPosition());
    appendObjectRequest.setInputStream(new ByteArrayInputStream(content3.getBytes()));
    appendObjectResult = ossClient.appendObject(appendObjectRequest);
    
    // Views the tags added to the uploaded object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, objectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("object tagging: "+ getTags.toString());
    
    // Closes the OSSClient.
    ossClient.shutdown();
    ```

-   Add tags to an object when uploading it in the resumable upload method.

    Run the following code to add tags to an object when uploading it in the resumable upload method:

    ``` {#codeblock_r2t_cqv_f8i}
    // This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Creates an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Configures the tags to be added to the object.
    Map<String, String> tags = new HashMap<String, String>();
    tags.put("key1", "value1");
    tags.put("key2", "value2");
    
    ObjectMetadata meta = new ObjectMetadata();
    // Specifies the content type of the object to be uploaded.
    meta.setContentType("text/plain");
    // Sets the tags to be added to the object.
    meta.setObjectTagging(tags);
    
    // Sets parameters by calling UploadFileRequest.
    UploadFileRequest uploadFileRequest = new UploadFileRequest("<yourBucketName>","<yourObjectName>");
    
    // Sets each parameter by calling UploadFileRequest.
    // Sets the name of the bucket that the object is uploaded to.
    //uploadFileRequest.setBucketName("<yourBucketName>");
    // Sets the object name.
    //uploadFileRequest.setKey("<yourObjectName>");
    // Specifies the local file to be uploaded.
    uploadFileRequest.setUploadFile("<yourLocalFile>");
    // Sets the number of upload tasks that can be simultaneously performed. The default value is 1. 
    uploadFileRequest.setTaskNum(5);
    // Specifies the size of the parts to be uploaded. The part size ranges from 100 KB to 5 GB. The default value is: object size/10000.
    uploadFileRequest.setPartSize(1 * 1024 * 1024);
    // Enables the resumable upload, which is disabled by default.
    uploadFileRequest.setEnableCheckpoint(true);
    // Sets the local file used to store the results of resumable upload tasks. This parameter must be set if the resumable upload function is enabled. The upload progress is stored in the file. If a part fails to be uploaded, the next upload task continues from the position recorded in the file. This file is deleted after the object is uploaded and is stored in the same directory as the local file that needs to be uploaded, that is, uploadFile.ucp.
    uploadFileRequest.setCheckpointFile("<yourCheckpointFile>");
    // Sets the metadata of the object.
    uploadFileRequest.setObjectMetadata(meta);
    // Sets the upload callback function. The parameter type is Callback.
    uploadFileRequest.setCallback("<yourCallbackEvent>");
    
    // Starts the resumable upload task and adds the tags to the object.
    ossClient.uploadFile(uploadFileRequest);
    
    // Views the tags added to the uploaded object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, objectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("object tagging: "+ getTags.toString());
    
    // Closes the OSSClient.
    ossClient.shutdown();
    ```


## Add tags to an uploaded object or modify the tags added to an object {#section_90h_uoo_3yw .section}

Run the following code to add tags to an uploaded object or modify the tags added to an object:

``` {#codeblock_17u_26a_kwj}
// This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

Map<String, String> tags = new HashMap<String, String>();
tags.put("key1", "value1");
tags.put("key2", "value2");

// Adds tags to the object. 
ossClient.setObjectTagging(bucketName, objectName, tags);

// Closes the OSSClient.
ossClient.shutdown();
```

## Add tags to an object when copying it {#section_mu0_e06_311 .section}

You can specify one of the following tagging rules when copying an object:

-   Copy \(default\): Copy the tags of the source object to the destination object.
-   Replace: Add the tags specified in the request to the destination object.

Examples of adding tags to objects of different sizes when copying them are described as follows:

-   Run the following code to add tags to an object smaller than 1 GB when directyly copying it:

``` {#codeblock_f7l_7sx_zrc}
// This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String sourceBucketName = "<yourSourceBucketName>";
String sourceObjectName = "<yourSourceObjectName>";
String destinationBucketName = "<yourDestinationBucketName>";
String destinationObjectName = "<yourDestinationObjectName>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Creates a CopyObjectRequest object.
CopyObjectRequest copyObjectRequest = new CopyObjectRequest(sourceBucketName, sourceObjectName, destinationBucketName, destinationObjectName);

// Configures the tags to be added to the object. If the headers are not configured, the tags of the source object are added to the destination object by default.
Map<String, String> headers = new HashMap<String, String>();
headers.put(OSSHeaders.COPY_OBJECT_TAGGING_DIRECTIVE, "REPLACE");
headers.put(OSSHeaders.OSS_TAGGING, "key1=value1&key2=value2");
copyObjectRequest.setHeaders(headers);

// Copies the source object to the destination object.
CopyObjectResult result = ossClient.copyObject(copyObjectRequest);
System.out.println("ETag: " + result.getETag() + " LastModified: " + result.getLastModified());

// Views the tags added to the destination object.
TagSet tagSet = ossClient.getObjectTagging(bucketName, destinationObjectName);
Map<String, String> getTags = tagSet.getAllTags();
System.out.println("dest object tagging: "+ getTags.toString());

// Closes the OSSClient.
ossClient.shutdown();
```

-   Run the following code to add tags to an object larger than 1 GB when copying it in the multipart copy method:

    ``` {#codeblock_oga_v7p_c23}
    // This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    String sourceBucketName = "<yourSourceBucketName>";
    String sourceObjectName = "<yourSourceObjectName>";
    String destinationBucketName = "<yourDestinationBucketName>";
    String destinationObjectName = "<yourDestinationObjectName>";
    
    ObjectMetadata objectMetadata = ossClient.getObjectMetadata(sourceBucketName, sourceObjectName);
    // Obtains the size of the source object.
    long contentLength = objectMetadata.getContentLength();
    // Sets the part size to 10 MB.
    long partSize = 1024 * 1024 * 10;
    // Calculates the number of parts.
    int partCount = (int) (contentLength / partSize);
    if (contentLength % partSize != 0) {
        partCount++;
    }
    
    System.out.println("total part count:" + partCount);
    
    // Configures the tags in the HTTP headers.
    Map<String, String> tags2 = new HashMap<String, String>();
    tags2.put("key3", "value3");
    tags2.put("key4", "value4");
    
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setObjectTagging(tags2);
    
    // Initializes a copy task and sets the tags to be added to the destination object.
    InitiateMultipartUploadRequest initiateMultipartUploadRequest = new InitiateMultipartUploadRequest(destinationBucketName, destinationObjectName, metadata);
    InitiateMultipartUploadResult initiateMultipartUploadResult = ossClient.initiateMultipartUpload(initiateMultipartUploadRequest);
    String uploadId = initiateMultipartUploadResult.getUploadId();
    
    // Copy the object part by part.
    List<PartETag> partETags = new ArrayList<PartETag>();
    for (int i = 0; i < partCount; i++) {
         // Calculates the part size.
        long skipBytes = partSize * i;
        long size = partSize < contentLength - skipBytes ? partSize : contentLength - skipBytes;
    
        // Creates an UploadPartCopyReuqest. You can specify conditions by setting UploadPartCopyReuqest.
        UploadPartCopyRequest uploadPartCopyRequest = 
            new UploadPartCopyRequest(sourceBucketName, sourceObjectName, destinationBucketName, destinationObjectName);
        uploadPartCopyRequest.setUploadId(uploadId);
        uploadPartCopyRequest.setPartSize(size);
        uploadPartCopyRequest.setBeginIndex(skipBytes);
        uploadPartCopyRequest.setPartNumber(i + 1);
    
        UploadPartCopyResult uploadPartCopyResult = ossClient.uploadPartCopy(uploadPartCopyRequest);
        // Stores the returned ETags of parts in partETags.
        partETags.add(uploadPartCopyResult.getPartETag());
    }
    
    // Completes the multipart copy task.
    CompleteMultipartUploadRequest completeMultipartUploadRequest = new CompleteMultipartUploadRequest(
                        destinationBucketName, destinationObjectName, uploadId, partETags);
    CompleteMultipartUploadResult completeMultipartUploadResult = ossClient.completeMultipartUpload(completeMultipartUploadRequest);
    
    // Views the tags added to the source object.
    TagSet tagSet = ossClient.getObjectTagging(bucketName, sourceObjectName);
    Map<String, String> getTags = tagSet.getAllTags();
    System.out.println("src object tagging: "+ getTags.toString());
    
    // Views the tags added to the destination object.
    tagSet = ossClient.getObjectTagging(bucketName, destinationObjectName);
    getTags = tagSet.getAllTags();
    System.out.println("dest object tagging: "+ getTags.toString());
    
    // Closes the OSSClient.
    ossClient.shutdown();
    ```


## Add tags to a symbolic link object {#section_p7j_aj1_thc .section}

Run the following code to add tags to a symbolic link object:

``` {#codeblock_5sx_mbe_ivg}
// This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String symLink = "<yourSymLink>";
String destinationObjectName = "<yourDestinationObjectName>";

// Creates an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Configures the tags to be added to the symbolic link object.
Map<String, String> tags = new HashMap<String, String>();
tags.put("key1", "value1");
tags.put("key2", "value2");

// Generates the metadata of the object.
ObjectMetadata metadata = new ObjectMetadata();
metadata.setObjectTagging(tags);

// Creates a CreateSymlinkRequest.
CreateSymlinkRequest createSymlinkRequest = new CreateSymlinkRequest(bucketName, symLink, destinationObjectName);

// Sets the metadata of the symbolic link object.
createSymlinkRequest.setMetadata(metadata);

// Creates a symbolic link object. 
ossClient.createSymlink(createSymlinkRequest);

// Views the tags added to the symbolic link object.
TagSet tagSet = ossClient.getObjectTagging(bucketName, symLink);
Map<String, String> getTags = tagSet.getAllTags();
System.out.println("symLink tagging: "+ getTags.toString());

// Closes the OSSClient.
ossClient.shutdown();
```

