# DeleteBucketEncryption {#concept_262216 .concept}

DeleteBucketEncryption接口用于删除Bucket加密规则。

## 请求语法 {#section_08q_257_1wu .section}

``` {#codeblock_jgq_f0o_sq0}
DELETE /?encryption HTTP/1.1
Date: GMT Date
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## 细节分析 {#section_6dn_gza_5oh .section}

-   只有Bucket的拥有者及授权子账户才能删除Bucket的加密规则，否则返回403 Forbidden错误，错误码：AccessDenied。
-   如果Bucket不存在，返回404错误。错误码：NoSuchBucket。

## 示例 {#section_4n5_4jz_vho .section}

-   请求示例

    ``` {#codeblock_8kr_zqk_dku}
    DELETE /?encryption HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 20 Dec 2018 11:35:24 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:6ZVHOehYzxoC1yxRydPQs/Cn****
    ```

-   响应示例

    ``` {#codeblock_hit_x1b_2d4}
    HTTP/1.1 204 OK
    x-oss-request-id: 5C22E0EFD127F6810B1A92A8
    Date: Tue, 20 Dec 2018 11:37:05 GMT
    Connection: keep-alive
    Content-Length: 0
    ```


