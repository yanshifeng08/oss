# UploadPart {#reference_pnq_2px_wdb .reference}

初始化一个MultipartUpload之后，可以根据指定的Object名和Upload ID来分块（Part）上传数据。

**说明：** 

-   调用该接口上传Part数据前，必须先调用InitiateMultipartUpload接口来获取一个OSS服务器颁发的Upload ID。对于同一个Upload ID，该号码不但唯一标识这一块数据，也标识了这块数据在整个文件内的相对位置。
-   每一个上传的Part都有一个标识它的号码（part number，范围是1-10000），单个Part大小范围100KB-5GB。MultipartUpload要求除最后一个Part以外，其他的Part大小都要大于100KB。因不确定是否为最后一个Part，UploadPart接口并不会立即校验上传Part的大小，只有当CompleteMultipartUpload的时候才会校验。
-   如果你用同一个part number上传了新的数据，那么OSS上已有的这个号码的Part数据将被覆盖。
-   OSS会将服务器端收到Part数据的MD5值放在ETag头内返回给用户。
-   若调用InitiateMultipartUpload接口时，指定了x-oss-server-side-encryption请求头，则会对上传的Part进行加密编码，并在UploadPart响应头中返回x-oss-server-side-encryption头，其值表明该Part的服务器端加密算法，详情请参考[InitiateMultipartUpload](cn.zh-CN/API 参考/关于MultipartUpload的操作/InitiateMultipartUpload.md)。

## 请求语法 {#section_efc_3px_wdb .section}

``` {#codeblock_9su_6as_87z}
PUT /ObjectName?partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: SignatureValue
```

## 示例 {#section_xbd_4px_wdb .section}

请求示例

``` {#codeblock_zh7_763_5uk}
PUT /multipart.data?partNumber=1&uploadId=0004B9895DBBB6EC98E36  HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length：6291456
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:J/lICfXEvPmmSW86bBAfMmUm****
[6291456 bytes data]
```

返回示例

``` {#codeblock_m9p_p97_gr8}
HTTP/1.1 200 OK
Server: AliyunOSS
Connection: keep-alive
ETag: 7265F4D211B56873A381D321F586****
x-oss-request-id: 3e6aba62-1eae-d246-6118-8ff42cd0****
Date: Wed, 22 Feb 2012 08:32:21 GMT
```

## SDK {#section_tc3_36p_q5w .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../cn.zh-CN/SDK 示例/Java/上传文件/分片上传.md#)
-   [Python](../../../../cn.zh-CN/SDK 示例/Python/上传文件/分片上传.md#)
-   [Go](../../../../cn.zh-CN/SDK 示例/Go/上传文件/分片上传.md#)
-   [C++](../../../../cn.zh-CN/SDK 示例/C++/上传文件/分片上传.md#)
-   [PHP](../../../../cn.zh-CN/SDK 示例/PHP/上传文件/分片上传.md#)
-   [C](../../../../cn.zh-CN/SDK 示例/C/上传文件/分片上传.md#)
-   [.NET](../../../../cn.zh-CN/SDK 示例/.NET/上传文件/分片上传.md#)
-   [Node.js](../../../../cn.zh-CN/SDK 示例/Node.js/上传文件/分片上传.md#)

## 错误码 {#section_h89_e61_iyf .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidArgument|400|超出Part number范围（1-10000）|
|InvalidDigest|400|为了保证数据在网络传输过程中不出现错误，用户发送请求时可以携带Content-MD5，OSS计算上传数据的MD5与用户上传的MD5值不一致|

