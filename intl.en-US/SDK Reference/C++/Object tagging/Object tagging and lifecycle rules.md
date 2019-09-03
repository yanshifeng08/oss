# Object tagging and lifecycle rules {#concept_727376 .concept}

Lifecycle rules can take effect on objects with specified prefixes or tags. You can also specify prefixes and tags as the condition of a lifecycle rule at the same time.

**Note:** If you set a specified tag as the condition of a lifecycle rule, the rule applies to an object only when the key and value in the tag of the object both match the specified tag. If you specify a prefix and multiple tags as the condition of a lifecycle rule, the rule applies to an object only when the prefix and tags of the object match the prefix and all tags specified in the rule.

## Set tags as the matching condition of a lifecycle rule {#section_a4k_q31_zvh .section}

Run the following code to set tags as the matching condition of a lifecycle rule:

``` {#codeblock_q2l_s96_sih}
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initializes the Alibaba Cloud OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initializes resources such as networks. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    SetBucketLifecycleRequest request(BucketName);

    /* Configures the tags to be set as the matching condition. */
    Tagging tagging;
    tagging.addTag(Tag("key1", "value1"));
    tagging.addTag(Tag("key2", "value2"));

    /* Specifies the lifecycle rule for the object. */
    auto rule1 = LifecycleRule();
    rule1.setID("rule1");
    rule1.setPrefix("test1/");
    rule1.setStatus(RuleStatus::Enabled);
    rule1.setExpiration(3);
    rule1.setTags(tagging.Tags());

    /* Sets the tags as the matching condition of the lifecycle rule. */
    LifecycleRuleList list{rule1};
    request.setLifecycleRules(list);
    auto outcome = client.SetBucketLifecycle(request);

    if (!outcome.isSuccess()) {
        /* Handles exceptions. */
        std::cout << "SetBucketLifecycle fail" <<
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

## View the tags set as the matching condition of a lifecycle rule {#section_0l9_57q_b9x .section}

Run the following code to view the tags set as the matching condition of a lifecycle rule:

``` {#codeblock_ax4_c4q_0sp}
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initializes the Alibaba Cloud OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initializes resources such as networks. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Views the lifecycle rule that includes the tags set as the matching condition. */
    auto outcome = client.GetBucketLifecycle(BucketName);

    if (outcome.isSuccess()) {
        std::cout << "GetBucketLifecycle success," << std::endl;
        for (auto const rule : outcome.result().LifecycleRules()) {
            std::cout << "rule:" << rule.ID() << "," << rule.Prefix() << "," << rule.Status() << ","
            "hasExpiration:" << rule.hasExpiration() << "," <<
            "hasTransitionList:" << rule.hasTransitionList() << "," << std::endl;

          auto taglist = rule.Tags();
          for (const auto& tag : taglist)
          {
              std::cout <<"GetBucketLifecycle tag success, Key:" 
              << tag.Key() << "; Value:" << tag.Value() << std::endl;
          }
        }
    }
    else {
        /* Handles exceptions. */
        std::cout << "GetBucketLifecycle fail" <<
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

