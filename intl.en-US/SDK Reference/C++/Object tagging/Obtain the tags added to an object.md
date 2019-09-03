# Obtain the tags added to an object {#concept_727373 .concept}

This topic describes how to obtain the tags added to an object.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

Run the following code to obtain the tags added to an object:

``` {#codeblock_rov_0ef_4rl}
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

    /* Obtains the tags added to the object. */
    getTaggingOutcome = client.GetObjectTagging(GetObjectTaggingRequest(BucketName, key));

    if (!getTaggingOutcome.isSuccess()) {
        /* Handles exceptions. */
        std::cout << "getTaggingOutcome fail" <<
        ",code:" << getTaggingOutcome.error().Code() <<
        ",message:" << getTaggingOutcome.error().Message() <<
        ",requestId:" << getTaggingOutcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    else {
        auto taglist = getTaggingOutcome.result().Tagging();
        for (const auto& tag : taglist)
        {
          std::cout <<"GetObjectTagging success, Key:" 
          << tag.Key() << "; Value:" << tag.Value() << std::endl;
        }
    }

    /* Releases resources such as networks. */
    ShutdownSdk();
    return 0;
}
```

For more information about how to obtain the tags added to an object, see [GetObjectTagging](../../../../intl.en-US/API Reference/Object operations/GetObjectTagging.md#).

