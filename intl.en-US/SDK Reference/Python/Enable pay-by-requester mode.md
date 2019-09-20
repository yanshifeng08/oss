# Enable pay-by-requester mode {#concept_944152 .concept}

After pay-by-requester mode is enabled for a bucket in Alibaba Cloud OSS, the requester instead of the bucket owner pays the cost of requests and traffic. The bucket owner always pays the cost of storing data. You can enable this feature to share data without paying for all requests and traffic by yourself.

## Request methods {#section_trk_ree_pwu .section}

-   Access from anonymous users is not allowed

    If you enable pay-by-requester mode for a bucket, anonymous users are not allowed to access the bucket. Requesters must provide authentication information so that OSS can identify and charge them, but not the bucket owner, for requests and traffic.

    If requesters assume the RAM role of an Alibaba Cloud account to request data, OSS charges this Alibaba Cloud account for such requests and traffic.

-   Requesters must include the x-oss-request-payer request header

    If you enable pay-by-requester mode for a bucket, requesters must include the x-oss-request-payer:requester request header in a PUT, POST, GET, or HEAD request. This field indicates that requesters understand that their requests and data downloads incur fees. Otherwise, requests cannot pass authentication.

    Bucket owners do not need to include the x-oss-request-payer request header when accessing their own buckets. In this case, bucket owners are requesters who pay for such requests and traffic.


For more information about pay-by-requester mode, see [Enable the pay-by-requester mode](../../../../reseller.en-US/Developer Guide/Buckets/Enable the pay-by-requester mode.md#) in the OSS Developer Guide.

## Enable pay-by-requester mode {#section_zzk_6kw_o4k .section}

The following code provides an example of how to enable pay-by-requester mode:

``` {#codeblock_ln2_n9z_0q1}
# -*- coding: utf-8 -*-

import oss2
from oss2.models import PAYER_BUCKETOWNER, PAYER_REQUESTER

# Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Enable the pay-by-requester mode. The default request payment mode is PAYER_BUCKETOWNER.
result = bucket.put_bucket_request_payment(PAYER_REQUESTER)

printt("http respon status: ", result.status)
```

## Obtain pay-by-requester configurations {#section_dy0_iot_dqo .section}

The following code provides an example of how to obtain pay-by-requester configurations:

``` {#codeblock_sbo_c1m_ifu}
# -*- coding: utf-8 -*-

import oss2

# Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Obtain pay-by-requester configurations.
result = bucket.get_bucket_request_payment()
print('payer:', result.payer)
```

## Specify a third party to pay for access to objects {#section_tcf_rdq_ei0 .section}

The following code provides an example of how to specify a third party to pay for access to objects when you use PutObject and DeleteObject:

``` {#codeblock_cj3_5m6_vyj}
# -*- coding: utf-8 -*-

import oss2
from oss2.headers import OSS_REQUEST_PAYER

# Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourRequestBucketName>')

object_name = '<yourRequestobjectName>'
headers = dict()
headers[OSS_REQUEST_PAYER] = "requester"

# Specify headers when you upload an object.
result = bucket.put_object(object_name, 'test-content', headers=headers)

# Specify headers when you delete an object.
result = bucket.delete_object(object_name, headers=headers);
```

