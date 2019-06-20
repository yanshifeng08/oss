# GetBucketEncryption {#concept_262215 .concept}

GetBucketEncryption接口用于获取Bucket加密规则。

## 请求语法 {#section_l1n_o8i_vkx .section}

``` {#codeblock_hbf_i2q_313}
Get /?encryption HTTP/1.1
Date: GMT Date
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
```

## 响应元素 {#section_1zq_10m_68a .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|ServerSideEncryptionRule|容器|是| 服务端加密规则的容器。

 子元素：ApplyServerSideEncryptionByDefault

 |
|ApplyServerSideEncryptionByDefault|容器|是| 服务端默认加密方式的容器。

 子元素：SSEAlgorithm，KMSMasterKeyID

 |
|SSEAlgorithm|字符串|是| 显示服务端默认加密方式。

 取值：KMS、AES256

 |
|KMSMasterKeyID|字符串|否|显示当前使用的KMS密钥ID。仅当SSEAlgorithm为KMS且指定了密钥ID时返回，其他情况下，取值为空。|

## 细节分析 {#section_3tj_pgm_pr2 .section}

-   只有Bucket的拥有者及授权子账户才能查看Bucket的加密规则，否则返回403 Forbidden错误，错误码：AccessDenied。
-   如果Bucket不存在，返回404错误。错误码：NoSuchBucket。
-   如果Bucket未设置加密规则，返回404错误。错误码：NoSuchServerSideEncryptionRule。

## 示例 {#section_85z_xj0_el4 .section}

-   请求示例

    ``` {#codeblock_t8t_xaw_u0d}
    Get /?encryption HTTP/1.1
    Date: Tue, 20 Dec 2018 11:20:10 GMT
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    ```

-   返回示例

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


