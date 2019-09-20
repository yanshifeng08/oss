# Pay-by-requester mode {#concept_944152 .concept}

After pay-by-requester mode is enabled for a bucket in Alibaba Cloud OSS, the requester instead of the bucket owner pays the cost of requests and traffic. The bucket owner always pays the cost of storing data. You can enable this feature to share data without having to pay for all requests and traffic by yourself.

## Request methods {#section_yq1_1g5_rdr .section}

-   Access from anonymous users is not allowed

    If you enable pay-by-requester mode for a bucket, anonymous users are not allowed to access the bucket. Requesters must provide authentication information so that OSS can identify and charge them, but not the bucket owner, for requests and traffic.

    If requesters assume the RAM role of an Alibaba Cloud account to request data, OSS charges this Alibaba Cloud account for such requests and traffic.

-   Requesters must include the x-oss-request-payer request header

    If you enable pay-by-requester mode for a bucket, requesters must include the x-oss-request-payer:requester request header in a PUT, POST, GET, or HEAD request. This field indicates that requesters understand that their requests and data downloads incur fees. Otherwise, requests cannot pass authentication.

    Bucket owners do not need to include the x-oss-request-payer request header when accessing their own buckets. In this case, bucket owners are requesters who pay for such requests and traffic.


For more information about pay-by-requester mode, see [Enable the pay-by-requester mode](../../../../reseller.en-US/Developer Guide/Buckets/Enable the pay-by-requester mode.md#) in the OSS Developer Guide.

## Enable pay-by-requester mode {#section_zzk_6kw_o4k .section}

The following code provides an example of how to enable pay-by-requester mode:

``` {#codeblock_4i3_6ax_ni0}
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Enable pay-by-requester mode. */
    SetBucketRequestPaymentRequest request(BucketName);
    request.setRequestPayer(RequestPayer::Requester);

    auto outcome = client.SetBucketRequestPayment(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "SetBucketRequestPayment fail" <<
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

## Obtain pay-by-requester configurations {#section_dy0_iot_dqo .section}

The following code provides an example of how to obtain pay-by-requester configurations:

``` {#codeblock_is6_g6l_q9g}
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Obtain pay-by-requester configurations. */
    GetBucketRequestPaymentRequest request(BucketName)
    auto outcome = client.GetBucketRequestPayment(request);

    if (outcome.isSuccess())
    {
        std::cout << "GetBucketRequestPayment success Payer:" << outcome.result().Payer() << std::endl;
    }
    else {
        /* Handle exceptions. */
        std::cout << "GetBucketPayment fail" <<
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

## Specify a third party to pay for access to objects {#section_tcf_rdq_ei0 .section}

The following code provides an example of how to specify a third party to pay for access to objects when you use PutObject, GetObject, and DeleteObject:

``` {#codeblock_eqw_pih_dww}
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string PayerAccessKeyId = "yourPayerAccessKeyId";
    std::string PayerAccessKeySecret = "yourPayerAccessKeySecret";
    std::string PayerEndpoint = "yourPayerEndpoint";
    std::string BucketName = "yourBucketName";
    std::string PayerUID = "PayerUID";
    std::string ObjectName = "yourObjectName";


    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    OssClient payerClient(PayerEndpoint, PayerAccessKeyId, PayerAccessKeySecret, conf);

    /* Create the bucket. */
    CreateBucketOutcome outCome = client.CreateBucket(CreateBucketRequest(BucketName));

    /* Enable pay-by-requester mode. */
    SetBucketRequestPaymentRequest request(BucketName);
    request.setRequestPayer(RequestPayer::Requester);
    auto payoutcome = client.SetBucketRequestPayment(request);

    /* Enable pay-by-requester mode when you upload the object. */
    *content << "test cpp sdk";
    PutObjectRequest putrequest(BucketName, ObjectName, content);
    putrequest.setRequestPayer(RequestPayer::Requester);
    auto putoutcome = payerClient.PutObject(putrequest);

    /* Enable pay-by-requester mode when you download the object to the local memory. */
    GetObjectRequest getrequest(BucketName, ObjectName);
    getrequest.setRequestPayer(RequestPayer::Requester);
    auto getoutcome = payerClient.GetObject(getrequest);

    /* Enable pay-by-requester mode when you delete the object. */
    DeleteObjectRequest delrequest(BucketName, ObjectName);
    delrequest.setRequestPayer(RequestPayer::Requester);
    auto deloutcome = payerClient.DeleteObject(delrequest);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

