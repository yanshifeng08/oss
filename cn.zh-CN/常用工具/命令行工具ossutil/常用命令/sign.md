# sign {#concept_303817 .concept}

sign命令用于生成经过签名的url供第三方用户访问存储空间（Bucket）内的对象（Object）。

**说明：** 关于文件URL的介绍请参见[上传Object后如何获取访问URL](https://help.aliyun.com/knowledge_detail/39607.html)。

## 命令格式 {#section_67z_ip4_e5t .section}

``` {#codeblock_a7g_7j1_yc1}
./ossutil sign oss://bucket/object [--timeout time]
```

 `--timeout`：可以设置文件URL的超时时间，单位为秒，默认值为60，取值范围为0-9223372036854775807。

## 使用示例 {#section_m41_ssr_ipw .section}

-   生成默认超时时间的文件URL，默认超时时间为60秒

    ``` {#codeblock_m0c_vjl_ef3}
    ./ossutil sign oss://bucket/path/object                       
    ```

-   生成指定超时时间的文件URL

    ``` {#codeblock_itd_hmn_x76}
    ./ossutil sign oss://bucket/path/object --timeout 3600
    ```


## 常用选项 {#section_x2p_nzu_vex .section}

您可以在使用sign命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--timeout`|设置文件URL的超时时间，单位为秒，默认值为60，取值范围为0-9223372036854775807。|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

