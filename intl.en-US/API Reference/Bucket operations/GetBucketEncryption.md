# GetBucketEncryption {#concept_262215 .concept}

Obtains the encryption rule for a bucket.

## Request syntax {#section_l1n_o8i_vkx .section}

``` {#codeblock_hbf_i2q_313}
Get /?encryption HTTP/1.1
Date: GMT Date
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## Response elements {#section_1zq_10m_68a .section}

|Element|Type|Required?|Description|
|-------|----|---------|-----------|
|ServerSideEncryptionRule|Container|Yes| Indicates the container used to store the server-side encryption rule.

 Sub-node: ApplyServerSideEncryptionByDefault

 |
|ApplyServerSideEncryptionByDefault|Container|Yes| Indicates the container used to store the default server-side encryption method.

 Sub-node: SSEAlgorithm, KMSMasterKeyID

 |
|SSEAlgorithm|String|Yes| Indicates the default server-side encryption method.

 Valid value: KMS, AES256

 |
|KMSMasterKeyID|String|No|Indicates the ID of CMK that is currently used. This element is only returned when the value of SSEAlgorithm is KMS and the CMK ID is specified. In other cases, the value of this element is null.|

## Detail analysis {#section_3tj_pgm_pr2 .section}

-   Only the bucket owner and authorized RAM users can view the encryption rule for a bucket. Otherwise, the 403 Forbidden error is returned with the error code: AccessDenied.
-   If the target bucket does not exist, the 404 error is returned with the error code: NoSuchBucket.
-   If no encryption rule is configured for the bucket, the 404 error is returned with the error code: NoSuchServerSideEncryptionRule.

## Examples {#section_85z_xj0_el4 .section}

-   Request example:

    ``` {#codeblock_t8t_xaw_u0d}
    Get /?encryption HTTP/1.1
    Date: Tue, 20 Dec 2018 11:20:10 GMT
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    ```

-   Response example:

    ``` {#codeblock_sj6_7zp_0at}
    HTTP/1.1 204 NoContent
    x-oss-request-id: 5C1B138A109F4E405B2D8AEF
    Date: Tue, 20 Dec 2018 11:22:05 GMT
    <?xml version="1.0" encoding="UTF-8"?>
    <ServerSideEncryptionRule>
      <ApplyServerSideEncryptionByDefault>
        <SSEAlgorithm>KMS</SSEAlgorithm>
        <KMSMasterKeyID>9468da86-3509-4f8d-a61e-6eab1eac****</KMSMasterKeyID>
      </ApplyServerSideEncryptionByDefault>
    </ServerSideEncryptionRule>
    ```


