# 上传Object后如何获取访问URL {#concept_39607_zh .concept}

本文举例说明如何在上传文件（Object）后获取文件的访问地址。

假如您在地域`华东1`名为`aliyun-abc`的Bucket中上传了文件`123.jpg`。

-   如果Object是公共读/公共读写权限，那么访问URL的格式为：`Bucket名称.Endpoint/Object名称` 

    -   外网访问URL：`aliyun-abc.oss-cn-hangzhou.aliyuncs.com/123.jpg`
    -   阿里云内网ECS/VPC访问URL：`aliyun-abc.oss-cn-hangzhou-internal.aliyuncs.com/123.jpg`
    **说明：** 有关各个地域（Region）所对应的访问域名（Endpoint）信息，请参见[访问域名和数据中心](cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

-   如果Object是私有权限，则必须进行签名操作。访问URL的格式为：`Bucket名称.Endpoint/Object名称？签名参数`。您可以通过以下任一方法获取Object的访问URL：
    -   通过控制台获取Object的URL，请参见[下载文件](../../../../cn.zh-CN/控制台用户指南/上传、下载和管理文件/下载文件.md#)
    -   调用API的签名算法，请参见[在URL中包含签名](../../../../cn.zh-CN/API 参考/访问控制/在URL中包含签名.md#)
    -   使用SDK生成签名：
        -   Java：[授权访问](../../../../cn.zh-CN/SDK 示例/Java/授权访问.md#)
        -   Python：[授权访问](../../../../cn.zh-CN/SDK 示例/Python/授权访问.md#)
        -   Go：[授权访问](../../../../cn.zh-CN/SDK 示例/Go/授权访问.md#)
        -   PHP：[授权访问](../../../../cn.zh-CN/SDK 示例/PHP/授权访问.md#)
        -   C：[授权访问](../../../../cn.zh-CN/SDK 示例/C/授权访问.md#)
        -   .Net：[授权访问](../../../../cn.zh-CN/SDK 示例/.NET/授权访问.md#)
        -   Android：[授权访问](../../../../cn.zh-CN/SDK 示例/Android/授权访问.md#)
        -   iOS：[授权访问](../../../../cn.zh-CN/SDK 示例/iOS/授权访问.md#)
        -   Node.js：[授权访问](../../../../cn.zh-CN/SDK 示例/Node.js/授权访问.md#)
        -   Browser.js：[授权访问](../../../../cn.zh-CN/SDK 示例/Browser.js/授权访问.md#)

更多信息请参见[OSS访问域名使用规则](cn.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md#)。

