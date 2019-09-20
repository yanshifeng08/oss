# PutBucketRequestPayment {#reference_1340164 .reference}

You can call this operation to configure pay-by-requester mode for a bucket.

**Note:** You can set the payer to BucketOwner or Requester.

-   If the bucket has pay-by-requester mode enabled, access from anonymous users is denied.
-   If the bucket has pay-by-requester mode enabled and the requester is not the bucket owner, the requester must include the x-oss-request-payer: requester request header. This way, the requester understands that their requests and data downloads incur fees. x-oss-request-charged: requester is included in the server response. If the oss-request-payer: requester request header is not included, the access is denied.

## Request syntax {#section_mxt_j6z_hwk .section}

``` {#codeblock_uqr_3me_rwm}
PUT /? requestPayment HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue 
<? xml version="1.0" encoding="UTF-8"? >
<RequestPaymentConfiguration>
  <Payer>Requester</Payer>
</RequestPaymentConfiguration>
```

## Request elements {#section_wr6_4x0_9th .section}

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|RequestPaymentConfiguration|Container|Yes|Specifies the container for the payer. Child: Payer

 |
|Payer|String|Yes|Specifies who pays the download and request fees. Valid values: BucketOwner and Requester

 Parent: RequestPaymentConfiguration

 |

## Examples {#section_uko_rct_vlp .section}

Request sample

``` {#codeblock_oua_n40_uzd}
PUT /? requestPayment
Content-Length: 83
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Tue, 23 Jul 2019 01:33:47 GMT
Authorization: OSS LTAIC***********:FsDgQiO+RMwLq***********
<RequestPaymentConfiguration><Payer>Requester</Payer></RequestPaymentConfiguration>
```

Response sample

``` {#codeblock_kne_n9k_q12}
200 (OK)
content-length: 0
x-oss-request-id: 5D3663FBB007B79097FC****
date: Tue, 23 Jul 2019 01:33:47 GMT
```

## SDKs {#section_f2e_j6b_ihz .section}

SDKs that support this operation use the following languages:

-   [Java](../../../../reseller.en-US/SDK Reference/Java/Enable pay-by-requester mode.md#)
-   [Python](../../../../reseller.en-US/SDK Reference/Python/Enable pay-by-requester mode.md#)
-   [Go](../../../../reseller.en-US/SDK Reference/Go/Pay-by-requester mode.md#)
-   [C++](../../../../reseller.en-US/SDK Reference/C++/Pay-by-requester mode.md#)

## Error codes {#section_26y_dvm_ibb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the specified bucket does not exist.|

