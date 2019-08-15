# CompleteMultipartUpload {#reference_lq1_dtx_wdb .reference}

在将所有数据Part都上传完成后，必须调用CompleteMultipartUpload接口来完成整个文件的MultipartUpload。

在执行该操作时，用户必须提供所有有效的数据的Part列表（包括Part号码和ETag）。OSS收到用户提交的Part列表后，会逐一验证每个数据Part的有效性。当所有的数据Part验证通过后，OSS将把这些数据part组合成一个完整的Object。

**说明：** 

-   CompleteMultipartUpload时会确认除最后一块以外所有块的大小是否都大于100KB，并检查用户提交的Part列表中的每一个Part号码和Etag。所以在上传Part时，客户端除了需要记录Part号码外，还需要记录每次上传Part成功后服务器返回的ETag值。
-   由于OSS处理CompleteMultipartUpload请求时会持续一定的时间。在这段时间内，如果客户端与OSS之间连接中断，OSS仍会继续将该请求。
-   用户提交的Part列表中，Part号码可以不连续。例如第一块的Part号码是1，第二块的Part号码是5。
-   OSS处理CompleteMultipartUpload请求成功后，该Upload ID就会变成无效。
-   同一个Object可以同时拥有不同的Upload ID，当Complete一个Upload ID后，该Object的其他Upload ID不受影响。
-   若调用InitiateMultipartUpload接口时，指定了x-oss-server-side-encryption请求头，则在CompleteMultipartUpload的响应头中返回x-oss-server-side-encryption，其值表明该Object的服务器端加密算法。

## 请求语法 {#section_xfs_ftx_wdb .section}

``` {#codeblock_fu6_zni_2b2}
POST /ObjectName?uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: Signature
<CompleteMultipartUpload>
<Part>
<PartNumber>PartNumber</PartNumber>
<ETag>ETag</ETag>
</Part>
...
</CompleteMultipartUpload>
```

## 请求参数\(Request Parameters\) {#section_x4m_3tx_wdb .section}

Complete Multipart Upload时，可以通过Encoding-type对返回结果中的Key进行编码。

|名称|类型|描述|
|:-|:-|:-|
|Encoding-type|字符串|指定对返回的Key进行编码，目前支持url编码。Key使用UTF-8字符，但xml 1.0标准不支持解析一些控制字符，比如ascii值从0到10的字符。对于Key中包含xml 1.0标准不支持的控制字符，可以通过指定encoding-type对返回的Key进行编码。 默认值：无

 可选值：url

 |

## 请求元素 {#section_evg_qtx_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|CompleteMultipartUpload|容器|保存Complete Multipart Upload请求内容的容器。 子节点：一个或多个Part元素

 父节点：无

 |
|ETag|字符串|Part成功上传后，OSS返回的ETag值。 父节点：Part

 |
|Part|容器|保存已经上传Part信息的容器。 子节点：ETag, PartNumber

 父节点：InitiateMultipartUploadResult

 |
|PartNumber|整数|Part数目。 父节点：Part

 |

## 响应元素 {#section_dps_15x_wdb .section}

|名称|类型|描述|
|:-|:-|:-|
|Bucket|字符串|Bucket名称。 父节点：CompleteMultipartUploadResult

 |
|CompleteMultipartUploadResult|容器|保存Complete Multipart Upload请求结果的容器。 子节点：Bucket, Key, ETag, Location

 父节点：None

 |
|ETag|字符串|ETag \(entity tag\) 在每个Object生成的时候被创建，用于标示一个Object的内容。Complete Multipart Upload请求创建的Object，ETag值是其内容的UUID。ETag值可以用于检查Object内容是否发生变化。 父节点：CompleteMultipartUploadResult

 |
|Location|字符串|新创建Object的URL。 父节点：CompleteMultipartUploadResult

 |
|Key|字符串|新创建Object的名字。 父节点：CompleteMultipartUploadResult

 |
|EncodingType|字符串|指明返回结果中编码使用的类型。如果请求的参数中指定了encoding-type，那返回的结果会对Key进行编码。 父节点：容器

 |

## 示例 {#section_ddv_t5x_wdb .section}

-   请求示例

    ``` {#codeblock_9ul_y3t_g6g}
    POST /multipart.data? uploadId=0004B9B2D2F7815C432C9057C03134D4  HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 1056
    Date: Fri, 24 Feb 2012 10:19:18 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:8VwFhFUWmVecK6jQlHlXMK/z****
    <CompleteMultipartUpload> 
        <Part> 
            <PartNumber>1</PartNumber>  
            <ETag>"3349DC700140D7F86A0784842780****"</ETag> 
        </Part>  
        <Part> 
            <PartNumber>5</PartNumber>  
            <ETag>"8EFDA8BE206636A695359836FE0A****"</ETag> 
        </Part>  
        <Part> 
            <PartNumber>8</PartNumber>  
            <ETag>"8C315065167132444177411FDA14****"</ETag> 
        </Part> 
    </CompleteMultipartUpload>
    ```

    返回示例

    ``` {#codeblock_e8p_3qq_jym}
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Content-Length: 329
    Content-Type: Application/xml
    Connection: keep-alive
    x-oss-request-id: 594f0751-3b1e-168f-4501-4ac71d21****
    Date: Fri, 24 Feb 2012 10:19:18 GMT
    <?xml version="1.0" encoding="UTF-8"?>
    <CompleteMultipartUploadResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Location>http://oss-example.oss-cn-hangzhou.aliyuncs.com /multipart.data</Location>
        <Bucket>oss-example</Bucket>
        <Key>multipart.data</Key>
        <ETag>B864DB6A936D376F9F8D3ED3BBE540****</ETag>
    </CompleteMultipartUploadResult>
    ```


## SDK {#section_b6o_tc4_egz .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../cn.zh-CN/SDK 示例/Java/上传文件/分片上传.md#)
-   [Python](../../../../cn.zh-CN/SDK 示例/Python/上传文件/分片上传.md#)
-   [Go](../../../../cn.zh-CN/SDK 示例/Go/上传文件/分片上传.md#)
-   [C++](../../../../cn.zh-CN/SDK 示例/C++/上传文件/分片上传.md#)
-   [PHP](../../../../cn.zh-CN/SDK 示例/PHP/上传文件/分片上传.md#)
-   [C](../../../../cn.zh-CN/SDK 示例/C/上传文件/分片上传.md#)
-   [.NET](../../../../cn.zh-CN/SDK 示例/.NET/上传文件/分片上传.md#)

## 错误码 {#section_mqt_veo_uni .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidDigest|400|为了保证数据在网络传输过程中不出现错误，用户发送请求时可以携带Content-MD5，OSS计算上传数据的MD5与用户上传的MD5值不一致|

