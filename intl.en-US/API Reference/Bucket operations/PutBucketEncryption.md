# PutBucketEncryption {#concept_262214 .concept}

Configures the encryption rule for a bucket.

## Request syntax {#section_45c_rqd_cp3 .section}

``` {#codeblock_ak9_pgk_vei}
PUT /?encryption HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<ServerSideEncryptionRule>
  <ApplyServerSideEncryptionByDefault>
    <SSEAlgorithm>AES256</SSEAlgorithm>
    <KMSMasterKeyID></KMSMasterKeyID>
  </ApplyServerSideEncryptionByDefault>
</ServerSideEncryptionRule>
```

## Request elements {#section_505_ygm_0ph .section}

|Element|Type|Required?|Description|
|-------|----|---------|-----------|
|ServerSideEncryptionRule|Container|Yes| Specifies the container used to store the server-side encryption rule.

 Sub-node: ApplyServerSideEncryptionByDefault

 |
|ApplyServerSideEncryptionByDefault|Container|Yes| Specifies the container used to store the default server-side encryption method.

 Sub-element: SSEAlgorithm, KMSMasterKeyID

 |
|SSEAlgorithm|String|Yes| Specifies the default server-side encryption method.

 Valid value: KMS, AES256

 |
|KMSMasterKeyID|String|No|Specifies the CMK ID when the value of SSEAlgorithm is KMS and a specified CMK is used for encryption. If the value of SSEAlgorithm is not KMS, this element must be null.|

## Detail analysis {#section_st4_wua_4ty .section}

-   Only the bucket owner and authorized RAM users can configure encryption rules for a bucket. Otherwise, the 403 Forbidden error is returned.
-   If the value of SSEAlgorithm is not KMS or AES256, the 400 InvalidEncryptionAlgorithm error is returned with the following error message: The Encryption request you specified is not valid. Supported value: AES256/KMS.
-   If the value of SSEAlgorithm is AES256 and KMSMasterKeyID is not null, the 400 InvalidArgument error is returned with the following error message: KMSMasterKeyID is not applicable if the default sse algorithm is not KMS.

## Examples {#section_cji_p6q_bgl .section}

-   Request example:

    ``` {#codeblock_0qh_t1t_isn}
    PUT /?encryption HTTP/1.1
    Date: Tue, 20 Dec 2018 11:09:13 GMT
    Content-Length：ContentLength
    Content-Type: application/xml
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    <?xml version="1.0" encoding="UTF-8"?>
    <ServerSideEncryptionRule>
      <ApplyServerSideEncryptionByDefault>
        <SSEAlgorithm>KMS</SSEAlgorithm>
        <KMSMasterKeyID>9468da86-3509-4f8d-a61e-6eab1eac****</KMSMasterKeyID>
      </ApplyServerSideEncryptionByDefault>
    </ServerSideEncryptionRule>
    ```

-   Response example:

    ``` {#codeblock_lui_yna_sge}
    HTTP/1.1 200 OK
    x-oss-request-id: 5C1B138A109F4E405B2D8AEF
    Date: Thu, 20 Dec 2018 11:11:06 GMT
    ```


