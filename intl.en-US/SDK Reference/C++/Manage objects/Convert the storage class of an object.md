# Convert the storage class of an object {#concept_188109 .concept}

OSS provides the following storage classes to cover various data storage scenarios from hot data to cold data: Standard, Infrequent Access \(IA\), and Archive. This topic describes how to convert the storage class of an object.

For more information about these storage classes, see [Introduction to storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#) and [Convert between storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Convert between storage classes.md#) in the OSS Developer Guide.

The following code provides an example of how to convert the storage class of an object.

-   The following code provides an example of how to convert the storage class of an object from Standard or IA to Archive:

    ``` {#codeblock_hrg_9i5_jc7}
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initialize the OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string SourceBucketName = "yourSourceBucketName";
        std::string SourceObjectName = "yourSourceObjectName";
    
    
        /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    
        /* Set the storage class to Archive. */
        ObjectMetaData objectMeta;
        objectMeta.addHeader("x-oss-storage-class", "Archive");
        CopyObjectRequest request(SourceBucketName, SourceBucketName,objectMeta);
        request.setCopySource(SourceBucketName, SourceObjectName);
    
        /* Modify the storage class. */
        auto outcome = client.CopyObject(request);
    
        if (! outcome.isSuccess()) {
            /* Handle exceptions. */
            std::cout << "CopyObject fail" <<
            ",code:" << outcome.error().Code() <<
            ",message:" << outcome.error().Message() <<
            ",requestId:" << outcome.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
    
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```

-   The following code provides an example of how to convert the storage class of an object from Archive to IA:

    ``` {#codeblock_u1x_35g_ohz}
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
        /* Initialize the OSS account information. */
        std::string AccessKeyId = "yourAccessKeyId";
        std::string AccessKeySecret = "yourAccessKeySecret";
        std::string Endpoint = "yourEndpoint";
        std::string SourceBucketName = "yourSourceBucketName";
        std::string SourceObjectName = "yourSourceObjectName";
    
        /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    
        /* Check whether the storage class of the source object is Archive. If the storage class of the source object is Archive, you must restore the object before you can modify the storage class. */
        auto restoreOutcome = client.RestoreObject(SourceBucketName, SourceObjectName);
        if (! restoreOutcome.isSuccess()) {
            /* Handle exceptions. */
            std::cout << "RestoreObject fail" <<
            ",code:" << restoreOutcome.error().Code() <<
            ",message:" << restoreOutcome.error().Message() <<
            ",requestId:" << restoreOutcome.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
    
        std::string onGoingRestore("ongoing-request=\"false\"");
    
        int maxWaitTimeInSeconds = 600;
        while (maxWaitTimeInSeconds > 0)
        {
            auto meta = client.HeadObject(SourceBucketName, SourceObjectName);
    
            std::string restoreStatus = meta.result().HttpMetaData()["x-oss-restore"];
            std::transform(restoreStatus.begin(), restoreStatus.end(), restoreStatus.begin(), ::tolower);
            if (! restoreStatus.empty() && 
            restoreStatus.compare(0, onGoingRestore.size(), onGoingRestore)==0) {
                std::cout << " success, restore status:" << restoreStatus << std::endl;
                /* The Archive object is restored.*/
                break;
            }
    
            std::cout << " info, WaitTime:" << maxWaitTimeInSeconds
            << "; restore status:" << restoreStatus << std::endl;
    
            std::this_thread::sleep_for(std::chrono::seconds(10));
            maxWaitTimeInSeconds--;     
        }
    
        /* Set the storage class to IA. */
        ObjectMetaData objectMeta;
        objectMeta.addHeader("x-oss-storage-class", "IA");
        CopyObjectRequest request(SourceBucketName, SourceBucketName,objectMeta);
        request.setCopySource(SourceBucketName, SourceObjectName);
    
        /* Modify the storage class. */
        auto outcome = client.CopyObject(request);
    
        if (! outcome.isSuccess()) {
            /* Handle exceptions. */
            std::cout << "CopyObject fail" <<
            ",code:" << outcome.error().Code() <<
            ",message:" << outcome.error().Message() <<
            ",requestId:" << outcome.error().RequestId() << std::endl;
            ShutdownSdk();
            return -1;
        }
    
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```


For more information about the storage classes and storage fees, see the [Storage fees](../../../../reseller.en-US/Pricing/Billing items.md#section_jmi_2z4_hka) section in Billing items.

