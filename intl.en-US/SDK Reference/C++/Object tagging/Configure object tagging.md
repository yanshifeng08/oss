# Configure object tagging {#concept_727372 .concept}

This topic describes how to configure object tagging.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

**Note:** 

-   You can add tags to an object when uploading it or add tags for an uploaded object. If add tags to an object that already has tags, the original tags are overwritten. For more information about adding tags, see [PutObjectTagging](../../../../intl.en-US/API Reference/Object operations/PutObjectTagging.md#).
-   To add tags to an object, you must have the permission to call PutObjectTagging.
-   The Last-Modified value of an object is not updated when its tags are changed.
-   A tag can contain letters, numbers, spaces, and the following symbols: + ‑ = . \_ : /

## Add tags to an object when uploading it {#section_bia_xdn_yuv .section}

-   Add tags to an object when uploading it directly.

    Run the following code to add tags to an object when uploading it directly:

    ``` {#codeblock_bum_79s_zwv}
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initializes the Alibaba Cloud OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
        std::string ObjectName = "yourObjectName";
    
        /* Initializes resources such as networks. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
        std::shared_ptr<std::iostream> content = std::make_shared<std::stringstream>();
        *content << "test cpp sdk";
        PutObjectRequest request(BucketName, ObjectName, content);
    
        /* Sets x-oss-tagging to configure the tags to be added.*/
        Tagging tagging;
        tagging.addTag(Tag("key1", "value1"));
        tagging.addTag(Tag("key2", "value2"));
        request.setTagging(tagging.toQueryParameters());
    
        /* Uploads the object. */
        auto outcome = client.PutObject(request);
    
        if (!outcome.isSuccess()) {
            /* Handles exceptions. */
            std::cout << "PutObject fail" <<
            ",code:" << outcome.error().Code() <<
            ",message:" << outcome.error().Message() <<
            ",requestId:" << outcome.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
    
        /* Releases resources such as networks. */
        ShutdownSdk();
        return 0;
    }
    ```

-   Add tags to an object when uploading it in the multipart upload method.

    Run the following code to add tags to an object when uploading it in the multipart upload method:

    ``` {#codeblock_hau_721_9n4}
    #include <alibabacloud/oss/OssClient.h>
    
    int64_t getFileSize(const std::string& file)
    {
        std::fstream f(file, std::ios::in | std::ios::binary);
        f.seekg(0, f.end);
        int64_t size = f.tellg();
        f.close();
        return size;
    }
    
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initializes the Alibaba Cloud OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
        std::string ObjectName = "yourObjectName";
    
        /* Initializes resources such as networks. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
        InitiateMultipartUploadRequest initUploadRequest(BucketName, ObjectName);
    
        /* Sets x-oss-tagging to configure the tags to be added. */
        Tagging tagging;
        tagging.addTag(Tag("key1", "value1"));
        tagging.addTag(Tag("key2", "value2"));
        initUploadRequest.setTagging(tagging.toQueryParameters());
    
        /* Initializes a multipart upload task. */
        auto uploadIdResult = client.InitiateMultipartUpload(initUploadRequest);
        auto uploadId = uploadIdResult.result().UploadId();
        std::string fileToUpload = "yourLocalFilename";
        int64_t partSize = 100 * 1024;
        PartList partETagList;
        auto fileSize = getFileSize(fileToUpload);
        int partCount = static_cast<int>(fileSize / partSize);
        /* Calculates the number of parts. */
        if (fileSize % partSize != 0) {
            partCount++;
        }
    
        /* Uploads each part. */
        for (int i = 1; i <= partCount; i++) {
            auto skipBytes = partSize * (i - 1);
            auto size = (partSize < fileSize - skipBytes) ? partSize : (fileSize - skipBytes);
            std::shared_ptr<std::iostream> content = std::make_shared<std::fstream>(fileToUpload, std::ios::in|std::ios::binary);
            content->seekg(skipBytes, std::ios::beg);
    
            UploadPartRequest uploadPartRequest(BucketName, ObjectName, content);
            uploadPartRequest.setContentLength(size);
            uploadPartRequest.setUploadId(uploadId);
            uploadPartRequest.setPartNumber(i);
            auto uploadPartOutcome = client.UploadPart(uploadPartRequest);
            if (uploadPartOutcome.isSuccess()) {
                Part part(i, uploadPartOutcome.result().ETag());
                partETagList.push_back(part);
            }
            else {
                std::cout << "uploadPart fail" <<
                ",code:" << uploadPartOutcome.error().Code() <<
                ",message:" << uploadPartOutcome.error().Message() <<
                ",requestId:" << uploadPartOutcome.error().RequestId() << std::endl;
            }
    
        }
    
        /* Completes the multipart upload task. */
        CompleteMultipartUploadRequest request(BucketName, ObjectName);
        request.setUploadId(uploadId);
        request.setPartList(partETagList);
        auto outcome = client.CompleteMultipartUpload(request);
    
        if (!outcome.isSuccess()) {
            /* Handles exceptions. */
            std::cout << "CompleteMultipartUpload fail" <<
            ",code:" << outcome.error().Code() <<
            ",message:" << outcome.error().Message() <<
            ",requestId:" << outcome.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
    
        /* Releases resources such as networks. */
        ShutdownSdk();
        return 0;
    }
    ```

-   Add tags to an object when uploading it by calling append\_object.

    Run the following code to add tags to an object when uploading it in the append object method:

    ``` {#codeblock_e7f_liw_7v1}
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initializes the Alibaba Cloud OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
        std::string ObjectName = "yourObjectName";
    
        /* Initializes resources such as networks. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    
        auto meta = ObjectMetaData();
        meta.setContentType("text/plain");
    
        /* Appends the file to the object. The default position from where the file is appended is 0. */
        std::shared_ptr<std::iostream> content1 = std::make_shared<std::stringstream>();
        *content1 <<"Thank you for using Aliyun Object Storage Service!";
        AppendObjectRequest request(BucketName, ObjectName, content1, meta);
    
        /* Configures the tags to be added. */
        Tagging tagging;
        tagging.addTag(Tag("key1", "value1"));
        tagging.addTag(Tag("key2", "value2"));
        request.setTagging(tagging.toQueryParameters());
    
        /* Appends the file to the object. */
        auto result = client.AppendObject(request);
    
        if (!result.isSuccess()) {
            /* Handles exceptions. */
            std::cout << "AppendObject fail" <<
            ",code:" << result.error().Code() <<
            ",message:" << result.error().Message() <<
            ",requestId:" << result.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
    
        /* Releases resources such as networks. */
        ShutdownSdk();
        return 0;
    }
    ```

-   Add tags to an object when uploading it in the resumable upload method.

    Run the following code to add tags to an object when uploading it in the resumable upload method:

    ``` {#codeblock_t8j_xab_n7q}
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initializes the Alibaba Cloud OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string BucketName = "yourBucketName";
        std::string ObjectName = "yourObjectName";
        std::string UploadFilePath = "yourUploadfilePath";
    
        /* Initializes resources such as networks. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    
        /* Uploads the object in the resumable upload method. */
        UploadObjectRequest request(BucketName, ObjectName, YourUploadFileName);
    
        /* Configures the tags to be added. */
        Tagging tagging;
        tagging.addTag(Tag("key1", "value1"));
        tagging.addTag(Tag("key2", "value2"));
        request.setTagging(tagging.toQueryParameters());
    
        auto outcome = client.ResumableUploadObject(request);
    
        if (!outcome.isSuccess()) {
            /* Handles exceptions. */
            std::cout << "ResumableUploadObject fail" <<
            ",code:" << outcome.error().Code() <<
            ",message:" << outcome.error().Message() <<
            ",requestId:" << outcome.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
    
        /* Releases resources such as networks. */
        ShutdownSdk();
        return 0;
    }
    ```


## Add tags to an uploaded object or modify the tags added to an object {#section_xbu_02l_wtu .section}

Run the following code to add tags to an uploaded object or modify the tags added to an object:

``` {#codeblock_ybk_zuf_vdb}
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initializes the Alibaba Cloud OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";

    /* Initializes resources such as networks. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    std::shared_ptr<std::iostream> content = std::make_shared<std::stringstream>();
    *content << "test cpp sdk";
    PutObjectRequest request(BucketName, ObjectName, content);

    /* Uploads the object. */
    auto outcome = client.PutObject(request);

    /* Configures the tags to be added. */
    Tagging tagging;
    tagging.addTag(Tag("key1", "value1"));
    tagging.addTag(Tag("key2", "value2"));
    auto putTaggingOutcome = client.SetObjectTagging(SetObjectTaggingRequest(BucketName, ObjectName, tagging));

    if (!putTaggingOutcome.isSuccess()) {
        /* Handles exceptions. */
        std::cout << "SetObjectTagging fail" <<
        ",code:" << putTaggingOutcome.error().Code() <<
        ",message:" << putTaggingOutcome.error().Message() <<
        ",requestId:" << putTaggingOutcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Releases resources such as networks. */
    ShutdownSdk();
    return 0;
}
```

## Add tags to an object when copying it {#section_als_92c_780 .section}

You can specify one of the following tagging rules when copying an object:

-   Copy \(default\): Copy the tags of the source object to the destination object.
-   Replace: Add the tags specified in the request to the destination object.

Examples of adding tags to objects of different sizes when copying them are described as follows:

-   Run the following code to add tags to an object smaller than 1 GB when directyly copying it:

``` {#codeblock_bgn_cdv_4tn}
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initializes the Alibaba Cloud OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
    std::string CopyObjectName = "yourCopyObjectName";

    /* Initializes resources such as networks. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    std::shared_ptr<std::iostream> content = std::make_shared<std::stringstream>();
    *content << "test cpp sdk";
    PutObjectRequest request(BucketName, ObjectName, content);

    /* Sets x-oss-tagging to configure the tags to be added. */
    Tagging tagging;
    tagging.addTag(Tag("key1", "value1"));
    tagging.addTag(Tag("key2", "value2"));
    request.setTagging(tagging.toQueryParameters());

    /* Uploads the object. */
    auto outcome = client.PutObject(request);

    /* Copies the object and sets the tags to be added. */
    CopyObjectRequest cpRequest(yourBucketName, CopyObjectName);
    cpRequest.setCopySource(yourBucketName, yourObjectName);
    Tagging tagging2;
    tagging2.addTag(Tag("key1-2", "value1-2"));
    tagging2.addTag(Tag("key2-2", "value2-2"));
    tagging2.addTag(Tag("key3-2", "value3-2"));
    cpRequest.setTagging(tagging2.toQueryParameters());

    /* Ignores the tags of the source object. Adds the tags specified in the request to the object. */
    cpRequest.setTaggingDirective(CopyActionList::Replace);

    /* Copies the object. */
    auto copyoutcome = client.CopyObject(cpRequest);
    if (!copyoutcome.isSuccess()) {
        /* Handles exceptions. */
        std::cout << "CopyObject fail" <<
        ",code:" << copyoutcome.error().Code() <<
        ",message:" << copyoutcome.error().Message() <<
        ",requestId:" << copyoutcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Releases resources such as networks. */
    ShutdownSdk();
    return 0;
}
```

-   Run the following code to add tags to an object larger than 1 GB when copying it the multipart upload method:

    ``` {#codeblock_gwo_koi_siu}
    #include <alibabacloud/oss/OssClient.h>
    #include <sstream>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initializes the Alibaba Cloud OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string SourceBucketName = "yourSourceBucketName";
        std::string CopyBucketName = "yourCopyBucketName";
        std::string SourceObjectName = "yourSourceObjectName";
        std::string CopyObjectName = "yourCopyObjectName";
    
        /* Initializes resources such as networks. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    
        auto getObjectMetaReq = GetObjectMetaRequest(SourceBucketName, SourceObjectName);
        auto getObjectMetaResult = GetObjectMeta(getObjectMetaReq);
        if (!getObjectMetaResult.isSuccess()) {
            std::cout << "GetObjectMeta fail" <<
            ",code:" << outcome.error().Code() <<
            ",message:" << outcome.error().Message() <<
            ",requestId:" << outcome.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
        /* Obtains the size of the source object. */
        auto objectSize = getObjectMetaResult.result().ContentLength();
    
        /* Copies the large object. */
        ObjectMetaData metaData;
        std::stringstream ssDesc;
        ssDesc << "/" << SourceBucketName << "/" << SourceObjectName;
        std::string value = ssDesc.str();
        metaData.addHeader("x-oss-copy-source", value);
        InitiateMultipartUploadRequest initUploadRequest(BucketName, ObjectName, metaData);
    
        /* Sets x-oss-tagging to configure the tags to be added. */
        Tagging tagging;
        tagging.addTag(Tag("key1", "value1"));
        tagging.addTag(Tag("key2", "value2"));
        initUploadRequest.setTagging(tagging.toQueryParameters());
    
        /* Initializes a multipart copy task. */
        auto uploadIdResult = client.InitiateMultipartUpload(initUploadRequest);
        auto uploadId = uploadIdResult.result().UploadId();
        int64_t partSize = 100 * 1024;
        PartList partETagList;
        int partCount = static_cast<int>(objectSize / partSize);
        /* Calculates the number of parts.*/
        if (objectSize % partSize != 0) {
            partCount++;
        }
    
        /* Copies each part. */
        for (int i = 1; i <= partCount; i++) {
            auto skipBytes = partSize * (i - 1);
            auto size = (partSize < objectSize - skipBytes) ? partSize : (objectSize - skipBytes);
            auto uploadPartCopyReq = UploadPartCopyRequest(CopyBucketName, CopyObjectName, SourceBucketName, SourceObjectName,uploadId, i + 1);
            uploadPartCopyReq.setCopySourceRange(skipBytes, skipBytes + size -1);
            auto uploadPartOutcome = client.UploadPartCopy(uploadPartCopyReq);
            if (uploadPartOutcome.isSuccess()) {
                Part part(i, uploadPartOutcome.result().ETag());
                partETagList.push_back(part);
            }
            else {
                std::cout << "UploadPartCopy fail" <<
                ",code:" << outcome.error().Code() <<
                ",message:" << outcome.error().Message() <<
                ",requestId:" << outcome.error().RequestId() << std::endl;
            }
    
        }
    
        /* Completes the multipart copy task. */
        CompleteMultipartUploadRequest request(CopyBucketName, CopyObjectName, partETagList, uploadId);
        auto outcome = client.CompleteMultipartUpload(request);
    
        if (!outcome.isSuccess()) {
            /* Handles exceptions. */
            std::cout << "CompleteMultipartUpload fail" <<
            ",code:" << outcome.error().Code() <<
            ",message:" << outcome.error().Message() <<
            ",requestId:" << outcome.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
    
        /* Releases resources such as networks. */
        ShutdownSdk();
        return 0;
    }
    ```


## Add tags to a symbolic link object {#section_779_8sl_f3g .section}

Run the following code to add tags to a symbolic link object:

``` {#codeblock_kxd_ml8_ppj}
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initializes the Alibaba Cloud OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
    std::string LinkName = "yourLinkName";

    /* Initializes resources such as networks. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    std::shared_ptr<std::iostream> content = std::make_shared<std::stringstream>();
    *content << "test cpp sdk";
    PutObjectRequest request(BucketName, ObjectName, content);

    /* Uploads the object. */
    auto outcome = client.PutObject(request);

    /* Creates a symbolic link. */
    CreateSymlinkRequest csRequest(BucketName, ObjectName);
    csRequest.SetSymlinkTarget(LinkName);

    /* Sets x-oss-tagging to configure the tags to be added. */
    Tagging tagging;
    tagging.addTag(Tag("key1", "value1"));
    tagging.addTag(Tag("key2", "value2"));
    csRequest.setTagging(tagging.toQueryParameters());

    auto csoutcome = client.CreateSymlink(csRequest);

    if (!csoutcome.isSuccess()) {
        /* Handles exceptions. */
        std::cout << "CreateSymlink fail" <<
        ",code:" << csoutcome.error().Code() <<
        ",message:" << csoutcome.error().Message() <<
        ",requestId:" << csoutcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Releases resources such as networks. */
    ShutdownSdk();
    return 0;
}
```

