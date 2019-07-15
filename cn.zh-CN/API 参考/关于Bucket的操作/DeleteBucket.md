# DeleteBucket {#reference_o1j_rrw_tdb .reference}

DeleteBucket用于删除某个存储空间（Bucket）。

**说明：** 

-   只有Bucket的拥有者才有权限删除该Bucket。
-   为了防止误删除的发生，OSS不允许删除一个非空的Bucket。

## 请求语法 {#section_ign_23w_bz .section}

``` {#codeblock_mfc_xsx_b88}
DELETE / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_md1_g3w_bz .section}

-   正常删除的请求示例

    ``` {#codeblock_nos_oif_cg5}
    DELETE / HTTP/1.1
    Host: test.oss-cn-hangzhou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    date: Tue, 15 Jan 2019 08:19:04 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3****
    Content-Length: 0
    ```

    返回示例

    ``` {#codeblock_c0f_0cp_ygo}
    HTTP/1.1 204 No Content
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 08:19:04 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5C3D9778CC1C2AEDF85B****
    x-oss-server-time: 190
    ```

-   删除的Bucket不存在的请求示例

    ``` {#codeblock_nuf_noc_vq9}
    DELETE / HTTP/1.1
    Host: test.oss-cn-hangzhou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    date: Tue, 15 Jan 2019 07:53:24 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3****
    Content-Length: 0
    ```

    返回示例

    ``` {#codeblock_2de_4na_ef7}
    HTTP/1.1 404 Not Found
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 07:53:25 GMT
    Content-Type: application/xml
    Content-Length: 288
    Connection: keep-alive
    x-oss-request-id: 5C3D9175B6FC201293AD****
    
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>NoSuchBucket</Code>
      <Message>The specified bucket does not exist.</Message>
      <RequestId>5C3D9175B6FC201293AD****</RequestId>
      <HostId>test.oss-cn-hangzhou.aliyuncs.com</HostId>
      <BucketName>test</BucketName>
    </Error>
    ```

-   删除的Bucket非空的请求示例

    ``` {#codeblock_f5s_pdz_2p6}
    DELETE / HTTP/1.1
    Host: test.oss-cn-hangzhou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: */*
    Connection: keep-alive
    date: Tue, 15 Jan 2019 07:35:06 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:ceOEyZavKY4QcjoUWYSpYbJ3****
    Content-Length: 0
    ```

    返回示例

    ``` {#codeblock_gd9_fk6_hsv}
    HTTP/1.1 409 Conflict
    Server: AliyunOSS
    Date: Tue, 15 Jan 2019 07:35:06 GMT
    Content-Type: application/xml
    Content-Length: 296
    Connection: keep-alive
    x-oss-request-id: 5C3D8D2A0ACA54D87B43****
    x-oss-server-time: 16
    
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>BucketNotEmpty</Code>
      <Message>The bucket you tried to delete is not empty.</Message>
      <RequestId>5C3D8D2A0ACA54D87B43****</RequestId>
      <HostId>test.oss-cn-hangzhou.aliyuncs.com</HostId>
      <BucketName>test</BucketName>
    </Error>
    ```


## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../cn.zh-CN/SDK 示例/Java/存储空间.md)
-   [Python](../../../../cn.zh-CN/SDK 示例/Python/存储空间.md)
-   [PHP](../../../../cn.zh-CN/SDK 示例/PHP/存储空间.md)
-   [Go](../../../../cn.zh-CN/SDK 示例/Go/存储空间.md)
-   [C](../../../../cn.zh-CN/SDK 示例/C/存储空间.md)
-   [.NET](../../../../cn.zh-CN/SDK 示例/.NET/存储空间.md)
-   [Android](../../../../cn.zh-CN/SDK 示例/Android/存储空间.md)
-   [iOS](../../../../cn.zh-CN/SDK 示例/iOS/存储空间.md)
-   [Node.js](../../../../cn.zh-CN/SDK 示例/Node.js/存储空间.md)
-   [Ruby](../../../../cn.zh-CN/SDK 示例/Ruby/存储空间.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|AccessDenied|403 Forbidden|没有删除该Bucket的权限。只有Bucket的拥有者才能删除该Bucket。|

