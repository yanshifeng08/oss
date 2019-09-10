# DeleteBucketEncryption {#concept_262216 .concept}

Deletes the encryption rule for a bucket.

## Request syntax {#section_08q_257_1wu .section}

``` {#codeblock_jgq_f0o_sq0}
DELETE /?encryption HTTP/1.1
Date: GMT Date
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## Detail analysis {#section_6dn_gza_5oh .section}

-   Only the bucket owner and authorized RAM users can delete the encryption rule for a bucket. Otherwise, the 403 Forbidden error is returned with the error code: AccessDenied.
-   If the target bucket does not exist, the 404 error is returned with the error code: NoSuchBucket.

## Examples {#section_4n5_4jz_vho .section}

-   Request example:

    ``` {#codeblock_8kr_zqk_dku}
    DELETE /?encryption HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 20 Dec 2018 11:35:24 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:6ZVHOehYzxoC1yxRydPQs/Cn****
    ```

-   Response example:

    ``` {#codeblock_hit_x1b_2d4}
    HTTP/1.1 204 OK
    x-oss-request-id: 5C22E0EFD127F6810B1A92A8
    Date: Tue, 20 Dec 2018 11:37:05 GMT
    Connection: keep-alive
    Content-Length: 0
    ```


