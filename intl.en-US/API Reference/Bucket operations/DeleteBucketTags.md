# DeleteBucketTags {#concept_261821 .concept}

Deletes the tags added for a bucket.

## Request syntax {#section_fzu_pcu_9bs .section}

``` {#codeblock_os0_2ud_jdo}
DELETE /?tagging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_m1y_jbh_t3q .section}

-   If the target bucket does not exist, the 404 No Content error is returned with the error code: NoSuchBucket.
-   Only the bucket owner can delete the tags added for a bucket. If you try to delete the tags for a bucket owned by another user, the 403 Forbidden error is returned with the error code: AccessDenied.
-   If no tags are added for the bucket or the key of the specified tag does not exist, the HTTP status code 204 is returned.

## Examples {#section_5hq_030_5ue .section}

-   Request example 1 \(Delete all tags for a bucket\):

    ``` {#codeblock_h3c_dsg_ftu}
    DELETE /?tagging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:6ZVHOehYzxoC1yxRydPQs/Cn****
    ```

    Response example:

    ``` {#codeblock_ipm_m7x_hgt}
    HTTP/1.1 204 No Content
    x-oss-request-id: 5C22E0EFD127F6810B1A92A8
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Connection: keep-alive
    Content-Length: 0
    Server: AliyunOSS
    ```

-   Request example 2 \(Delete specified tags for a bucket, for example, tags of which the keys are k1 and k2\):

    ``` {#codeblock_dol_40e_zdr}
    DELETE /?tagging=k1,k2 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:6ZVHOehYzxoC1yxRydPQs/Cn****
    ```

    Response example:

    ``` {#codeblock_2yi_420_eg4}
    HTTP/1.1 204 No Content
    x-oss-request-id: 5C22E0EFD127F6810B1A92A8
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Connection: keep-alive
    Content-Length: 0
    Server: AliyunOSS
    ```


