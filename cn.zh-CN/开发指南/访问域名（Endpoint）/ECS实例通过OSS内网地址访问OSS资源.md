# ECS实例通过OSS内网地址访问OSS资源 {#concept_39584_zh .concept}

当您通过OSS内网地址访问OSS资源时，不收取流量费用。本文介绍ECS实例如何通过OSS内网地址访问OSS资源。

通过OSS内网地址访问OSS资源有以下两种方式：

-   与OSS同地域ECS实例可以直接通过内网访问有权限的OSS资源。
-   与OSS不同地域的ECS实例或公网用户可通过配置ECS反向代理，间接实现通过OSS内网地址访问OSS资源。

## 获取OSS内网地址 {#section_oa9_7tk_ths .section}

-   通过OSS控制台获取

    登录[OSS管理控制台](https://oss.console.aliyun.com/overview)，打开指定Bucket的概览页面，在**访问域名**区域查看Bucket的Endpoint和Bucket域名，如下图所示。![访问域名区域](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/pic/39584/cn_zh/1562123983121/Image%201.png)

-   通过固定格式获取

    OSS的访问地址为固定格式：`BucketName.Endpoint`。其中，`BucketName`为您的存储空间名称，`Endpoint`为存储空间所在的地域对应的访问域名。详情请参见[OSS访问域名使用规则](cn.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md#)。


## 同地域ECS实例访问OSS资源 {#section_4ed_ssu_5rv .section}

与OSS同地域的ECS实例可以通过以下方式使用内网访问OSS资源：

-   通过URL直接访问OSS资源

    您可以直接使用OSS资源的内网地址访问有权限的OSS资源。例如，杭州地域某Bucket名为test，根目录下有个Object名为1.jpg，处于公共读状态。此时，杭州地域的ECS实例均可以使用`http://test.oss-cn-hangzhou-internal.aliyuncs.com/1.jpg`访问此Object。因此，您可以将OSS资源的访问URL嵌入到您的网站中，提供给同地域的ECS用户或已通过专线接入到与OSS同地域内网的用户访问。

    **警告：** 为了您的数据安全，不建议您将OSS资源设置为公共读或公共读写，您可以通过[Bucket Policy](../../../../cn.zh-CN/控制台用户指南/上传、下载和管理文件/使用Bucket Policy授权其他用户访问OSS资源.md#)授权给指定用户访问您的资源。

-   通过ossbrowser访问OSS资源

    您可以在配置ossbrowser访问参数的时候，将Endpoint设置为自定义，并填写OSS的内网Endpoint地址。详情请参见[ossbrowser](../../../../cn.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md#)。

-   通过ossutil访问OSS资源

    您可以在配置ossutil访问参数的时候，将Endpoint设置为OSS的内网Endpoint地址。详情请参见[ossutil](../../../../cn.zh-CN/常用工具/命令行工具ossutil/概述.md#)。

-   通过SDK访问OSS资源

    SDK初始化client的时候，Endpoint配置OSS内网对应的Endpoint即可。

    -   Java SDK

        ``` {#codeblock_5oa_hnl_en6 .language-java}
        String endpoint = "http://oss-cn-hangzhou-internal.aliyuncs.com";//以华东 1为例
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        OSSClient client = new OSSClient(endpoint, accessKeyId, accessKeySecret);          
        ```

        更多详情请参见[Java SDK初始化](../../../../cn.zh-CN/SDK 示例/Java/初始化.md#)。

    -   PHP SDK

        ``` {#codeblock_zzp_81h_5g7 .language-php}
        $accessKeyId = "<您从OSS获得的AccessKeyId>";
        $accessKeySecret = "<您从OSS获得的AccessKeySecret>";
        $endpoint = "<您选定的OSS数据中心访问域名，例如http://oss-cn-hangzhou-internal.aliyuncs.com>";           
        ```

        更多详情请参见[PHP SDK初始化](../../../../cn.zh-CN/SDK 示例/PHP/初始化.md#)。

    -   Python SDK

        ``` {#codeblock_7jm_ksr_p2q .language-python}
        auth = oss2.Auth('您的AccessKeyId', '您的AccessKeySecret')
        endpoint = 'http://oss-cn-hangzhou-internal.aliyuncs.com' # 假设Bucket处于杭州地域
        bucket = oss2.Bucket(auth, endpoint, '您的Bucket名称')         
        ```

        更多详情请参见[Python SDK初始化](../../../../cn.zh-CN/SDK 示例/Python/初始化.md#)。

    -   .NET SDK

        ``` {#codeblock_aqv_nwx_km4 .lanuage-csharp}
        const string accessKeyId = "<your AccessKeyId>";
        const string accessKeySecret = "<your AccessKeySecret>";
        const string endpoint = "http://oss-cn-hangzhou-internal.aliyuncs.com";
        var ossClient = new OssClient(endpoint, accessKeyId, accessKeySecret);   
        ```

        更多详情请参见[.NET SDK初始化](../../../../cn.zh-CN/SDK 示例/.NET/初始化.md#)。

    -   C SDK

        ``` {#codeblock_40z_esp_ono .language-c}
        ptions->config = oss_config_create(options->pool);
        aos_str_set(&options->config->endpoint, "http://oss-cn-hangzhou-internal.aliyuncs.com");
        aos_str_set(&options->config->access_key_id, "<您的AccessKeyId>");
        aos_str_set(&options->config->access_key_secret, "<您的AccessKeySecret>");
        options->config->is_cname = 0;
        options->ctl = aos_http_controller_create(options->pool, 0);         
        ```

        更多详情请参见[C SDK初始化](../../../../cn.zh-CN/SDK 示例/C/初始化.md#)。


## 通过ECS反向代理访问OSS资源 {#section_rdh_lue_i5g .section}

不同地域的ECS实例或外网用户是无法直接通过OSS内网地址访问OSS资源的，但是您可以通过配置ECS反向代理来间接实现：

1.  在OSS同地域创建一个有公网地址的ECS实例。详情请参见[创建ECS实例](../../../../cn.zh-CN/个人版快速入门/创建ECS实例.md#)。
2.  在ECS实例上配置反向代理。详情请参见[基于CentOS的ECS实例实现OSS反向代理](../../../../cn.zh-CN/最佳实践/使用ECS实例反向代理OSS/基于CentOS的ECS实例实现OSS反向代理.md#)和[基于Ubuntu的ECS实例实现OSS反向代理](../../../../cn.zh-CN/最佳实践/使用ECS实例反向代理OSS/基于Ubuntu的ECS实例实现OSS反向代理.md#)。
3.  OSS配置Bucket Policy，允许该ECS实例的内网地址访问OSS资源。详情请参见[使用Bucket Policy授权其他用户访问OSS资源](../../../../cn.zh-CN/控制台用户指南/上传、下载和管理文件/使用Bucket Policy授权其他用户访问OSS资源.md#)。

以上步骤配置完成后，您的用户将通过您的ECS公网地址访问您的OSS资源。当用户访问时，ECS实例通过内网向OSS请求资源，之后再返回给用户。

