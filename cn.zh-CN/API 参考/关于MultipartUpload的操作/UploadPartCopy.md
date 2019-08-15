# UploadPartCopy {#reference_t4b_vpx_wdb .reference}

UploadPartCopy通过从一个已存在的Object中拷贝数据来上传一个Part。

通过在UploadPart请求的基础上增加一个Header:x-oss-copy-source来调用该接口。当拷贝一个大于1GB的文件时，必须使用UploadPartCopy的方式进行拷贝。如果想通过单个操作拷贝小于1GB的文件，可以参考[CopyObject](cn.zh-CN/API 参考/关于Object操作/CopyObject.md)。

**说明：** 

-   该操作不能拷贝以AppendObject方式上传的Object。
-   UploadPartCopy的源Bucket地址和目标Bucket地址必须是同一个Region。
-   调用该接口上传Part数据前，必须先调用InitiateMultipartUpload接口来获取一个OSS服务器颁发的Upload ID。
-   若调用InitiateMultipartUpload接口时，指定了x-oss-server-side-encryption请求头，则会对上传的Part进行加密编码，并在UploadPart响应头中返回x-oss-server-side-encryption头，其值表明该Part的服务器端加密算法，详情请参考[InitiateMultipartUpload](cn.zh-CN/API 参考/关于MultipartUpload的操作/InitiateMultipartUpload.md)。
-   MultipartUpload要求除最后一个Part以外，其他的Part大小都要大于100KB。因不确定是否为最后一个Part，UploadPart接口并不会立即校验上传Part的大小，只有当CompleteMultipartUpload的时候才会校验。

## 请求语法 {#section_gqh_crx_wdb .section}

``` {#codeblock_4e9_l81_r5r}
PUT /ObjectName? partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: SignatureValue
x-oss-copy-source: /SourceBucketName/SourceObjectName
x-oss-copy-source-range:bytes=first-last
```

## 请求头 {#section_d23_2rx_wdb .section}

除了通用的请求头，UploadPartCopy请求中通过下述请求头指定拷贝的源Object地址和拷贝的范围。

|名称|类型|描述|
|:-|:-|:-|
|x-oss-copy-source|字符串|拷贝源地址（必须有可读权限） 默认值：无

 |
|x-oss-copy-source-range|整型|源Object的拷贝范围。如，设定 bytes=0-9，表示传送第0到第9这10个字符。 当拷贝整个源Object时不需要该请求Header。 默认值：无

 **说明：** 不指定x-oss-copy-source-range请求头时，表示拷贝整个源Object。当指定该请求头时，则返回消息中会包含整个文件的长度和此次拷贝的范围，例如：Content-Range: bytes 0-9/44，表示整个文件长度为44，此次拷贝的范围为0-9。当指定的范围不符合范围规范时，则拷贝整个文件，并且不在结果中提及Content-Range。

 |

下述请求Header作用于x-oss-copy-source指定的源Object。

|名称|类型|描述|
|:-|:-|:-|
|x-oss-copy-source-if-match|字符串|如果源Object的ETAG值和用户提供的ETAG相等，则执行拷贝操作；否则返回412 HTTP错误码（预处理失败）。 默认值：无

 |
|x-oss-copy-source-if-none-match|字符串|如果传入的ETag值和Object的ETag不匹配，则正常传输文件，并返回200 OK；否则返回304 Not Modified 默认值：无

 |
|x-oss-copy-source-if-unmodified-since|字符串|如果传入参数中的时间等于或者晚于文件实际修改时间，则正常传输文件，并返回200 OK；否则返回412 precondition failed错误。 默认值：无

 |
|x-oss-copy-source-if-modified-since|字符串|如果指定的时间早于实际修改时间，则正常传送文件，并返回200 OK；否则返回304 not modified 默认值：无

 时间格式：GMT时间，例如Fri, 13 Nov 2015 14:47:53 GMT

 |

## 示例 {#section_sv4_hsx_wdb .section}

-   请求示例

    ``` {#codeblock_rg4_jfn_r8u}
    PUT /multipart.data?partNumber=1&uploadId=0004B9895DBBB6EC98E36  HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length：6291456
    Date: Wed, 22 Feb 2012 08:32:21 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:J/lICfXEvPmmSW86bBAfMmUm****
    x-oss-copy-source: /oss-example/ src-object
    x-oss-copy-source-range:bytes=100-6291756
    ```

    返回示例

    ``` {#codeblock_b1e_0o9_u0w}
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Connection: keep-alive
    x-oss-request-id: 3e6aba62-1eae-d246-6118-8ff42cd0****
    Date: Thu, 17 Jul 2014 06:27:54 GMT'
    <?xml version="1.0" encoding="UTF-8"?>
    <CopyPartResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <LastModified>2014-07-17T06:27:54.000Z </LastModified>
        <ETag>"5B3C1A2E053D763E1B002CC607C5****"</ETag>
    </CopyPartResult>
    ```


## SDK {#section_g7z_xfb_so2 .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../cn.zh-CN/SDK 示例/Java/上传文件/分片上传.md#)
-   [Python](../../../../cn.zh-CN/SDK 示例/Python/上传文件/分片上传.md#)
-   [Go](../../../../cn.zh-CN/SDK 示例/Go/上传文件/分片上传.md#)
-   [C++](../../../../cn.zh-CN/SDK 示例/C++/上传文件/分片上传.md#)
-   [PHP](../../../../cn.zh-CN/SDK 示例/PHP/上传文件/分片上传.md#)
-   [C](../../../../cn.zh-CN/SDK 示例/C/上传文件/分片上传.md#)
-   [.NET](../../../../cn.zh-CN/SDK 示例/.NET/上传文件/分片上传.md#)

## 错误码 {#section_0dk_hv5_2rf .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|OperationNotSupported|400|Bucket的类型为Archive时调用UploadPartCopy接口。|

