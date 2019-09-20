# GetBucketRequestPayment {#reference_1340313 .reference}

You can call this operation to obtain pay-by-requester configurations for a bucket.

## Request syntax {#section_0f5_5u9_fsc .section}

``` {#codeblock_zhs_ev6_hby}
GET /? requestPayment HTTP/1.1
Date: GMT Date
Host: BucketName.oss.aliyuncs.com
Authorization: authorization string
```

## Response elements {#section_tuo_5an_dl4 .section}

|Element|Type|Description|
|-------|----|-----------|
|RequestPaymentConfiguration|Container|Indicates the container for the payer. Child: Payer

 |
|Payer|String|Indicates who pays the download and request fees. Valid values: BucketOwner and Requester

 Parent: RequestPaymentConfiguration

 |

## Examples {#section_lgm_n0h_f6s .section}

Request sample

``` {#codeblock_v93_25m_bcr}
GET /? requestPayment
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Tue, 23 Jul 2019 01:23:20 GMT
Authorization: OSS LTAICQmy1********:Rk7tT30yhDO3D************
```

Response sample

``` {#codeblock_uju_2gd_8i7}
200 (OK)
content-length: 129
server: AliyunOSS
x-oss-request-id: 5D366188B007B79097EC****
date: Tue, 23 Jul 2019 01:23:20 GMT
content-type: application/xml
<? xml version="1.0" encoding="UTF-8"? >
<RequestPaymentConfiguration>
  <Payer>BucketOwner</Payer>
</RequestPaymentConfiguration>
```

## SDKs {#section_1jw_ip2_04f .section}

SDKs that support this operation use the following languages:

-   [Java](../../../../reseller.en-US/SDK Reference/Java/Enable pay-by-requester mode.md#)
-   [Python](../../../../reseller.en-US/SDK Reference/Python/Enable pay-by-requester mode.md#)
-   [Go](../../../../reseller.en-US/SDK Reference/Go/Pay-by-requester mode.md#)
-   [C++](../../../../reseller.en-US/SDK Reference/C++/Pay-by-requester mode.md#)

## Error codes {#section_huw_to1_21x .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the specified bucket does not exist.|

