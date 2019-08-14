# bucket-encryption {#concept_354612 .concept}

bucket-encryption命令用于添加、修改、查询、删除Bucket的加密配置。

**说明：** Bucket加密介绍请参见[服务器端加密编码](../../../../cn.zh-CN/开发指南/数据加密/服务器端加密.md#)。

## 命令格式 {#section_qz7_p38_o8w .section}

-   添加/修改Bucket加密配置

    ``` {#codeblock_3im_uy8_y74}
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm algorithmName [--kms-masterkey-id  keyid]
    ```

    -   --sse-algorithm：选择Bucket的加密方式，可选值为KMS和AES256。
    -   --kms-masterkey-id：设置指定的CMK ID进行加密。当--sse-algorithm值为AES256时，不可附加此项；值为KMS时，根据您的需求选择是否添加。

        **说明：** 使用指定CMK ID加密目前处于公测中，请联系[售后技术支持](https://selfservice.console.aliyun.com/ticket/createIndex)开通试用。

    若Bucket未设置加密，此命令将为Bucket添加指定的加密方式；若Bucket已配置加密，此命令将更改Bucket加密配置。

-   获取Bucket加密配置

    ``` {#codeblock_icr_n19_4ff}
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   删除Bucket加密配置

    ``` {#codeblock_ixq_txk_rpz}
    ./ossuitl bucket-encryption --method delete oss://bucket
    ```


## 使用示例 {#section_23p_qal_6on .section}

-   添加/修改Bucket加密配置

    ``` {#codeblock_aud_92w_uuz}
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm KMS --kms-masterkey-id 9468da86-3509-4f8d-a61e-6eab1eac****
    ```

-   获取Bucket加密配置

    ``` {#codeblock_eut_5dv_ks8}
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   删除Bucket加密配置

    ``` {#codeblock_w70_afy_cwg}
    ./ossuitl bucket-encryption --method delete oss://bucket
    ```


## 常用选项 {#section_v4k_n4k_j0a .section}

您可以在使用bucket-encryption命令时附加如下选项：

|选项名称|描述|
|----|--|
|--sse-algorithm|表示服务端加密算法，取值为KMS或AES256。|
|--kms-masterkey-id|设置指定的CMK ID进行加密。当`--sse-algorithm`值为AES256时，不可附加此项；值为KMS时，根据您的需求选择是否添加。|
|--method|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。

 |
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如https://120.79.128.211:3128、 socks5://120.79.128.211:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

