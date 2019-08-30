# bucket-tagging {#concept_354610 .concept}

bucket-tagging命令用于添加、修改、查询、删除Bucket的标签配置。

**说明：** Bucket标签介绍请参见[存储空间标签](../cn.zh-CN/开发指南/存储空间（Bucket）/存储空间标签.md#)。

## 命令格式 {#section_ajb_zss_915 .section}

-   添加/修改Bucket标签

    ``` {#codeblock_p1t_azn_vvg}
    ./ossutil bucket-tagging --method put oss://bucket  tagkey1#tagvalue1 tagkey2#tagvalue2
    ```

    若Bucket未设置标签，此命令将为Bucket添加指定的标签；若Bucket已配置标签，此命令将覆盖Bucket原有标签。

    -   只有Bucket的拥有者及授权子账户才能为Bucket设置用户标签，否则返回403 Forbidden错误，错误码：AccessDenied。
    -   最多可设置20对Bucket用户标签（Key-Value对）。
    -   Key最大长度为64字节，不能以`http ://`、`https://`、`Aliyun`为前缀，且不能为空。
    -   Value最大长度为128字节，可以为空。
    -   Key和Value必须为UTF-8编码。
-   查询Bucket标签

    ``` {#codeblock_fdb_4o3_gmz}
    ./ossutil bucket-tagging --method get oss://bucket
    ```

-   删除Bucket标签

    ``` {#codeblock_r0q_75t_vel}
    ./ossutil bucket-tagging --method delete oss://bucket
    ```


## 使用示例 {#section_tad_17w_2bg .section}

-   添加Bucket标签

    ``` {#codeblock_b1b_pg9_a9x}
    ./ossutil bucket-tagging --method put oss://bucket1  tag1#test1 tag2#test2
    ```

-   查询Bucket标签

    ``` {#codeblock_qry_ltw_o0n}
    ./ossutil bucket-tagging --method get oss://bucket1
    ```

-   删除Bucket标签

    ``` {#codeblock_m87_807_u09}
    ./ossutil bucket-tagging --method delete oss://bucket1
    ```


## 常用选项 {#section_rc0_dzr_n3g .section}

您可以在使用bucket-tagging命令时附加如下选项：

|选项名称|描述|
|----|--|
|--method|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。

 |
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

