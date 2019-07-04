# bucket-encryption {#concept_627780 .concept}

bucket-encryption命令用于添加、修改、查询、删除Bucket的加密配置。

**说明：** Bucket加密介绍请参见[服务端加密](https://help.aliyun.com/document_detail/119320.html)。

## 命令格式 {#section_e65_hqy_hpm .section}

-   添加/修改Bucket加密配置

    ``` {#codeblock_1yn_lfz_g1u}
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm algorithmName [--kms-masterkey-id  keyid]
    ```

    -   `--sse-algorithm`：选择Bucket的加密方式，可选值为KMS和AES256。
    -   `--kms-masterkey-id`：设置指定的CMK ID进行加密。当`--sse-algorithm`值为AES256时，不可附加此项；值为KMS时，根据您的需求选择是否添加。

        **说明：** 使用指定CMK ID加密目前处于公测中，请联系[售后技术支持](https://selfservice.console.aliyun.com/ticket/createIndex)开通试用。

    若Bucket未设置加密，此命令将为Bucket添加指定的加密方式；若Bucket已配置加密，此命令将更改Bucket加密配置。

-   获取Bucket加密配置

    ``` {#codeblock_oms_yix_l2h}
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   删除Bucket加密配置

    ``` {#codeblock_f0j_qnt_x22}
    ./ossuitl bucket-encryption --method delete oss://bucket
    ```


## 使用示例 {#section_olq_pl3_3cm .section}

-   添加/修改Bucket加密配置

    ``` {#codeblock_nh9_qn9_a3c}
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm KMS --kms-masterkey-id 9468da86-3509-4f8d-a61e-6eab1eac****
    ```

-   获取Bucket加密配置

    ``` {#codeblock_mz7_shl_bpd}
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   删除Bucket加密配置

    ``` {#codeblock_5l8_ux8_mtn}
    ./ossuitl bucket-encryption --method delete oss://bucket
    ```


## 常用选项 {#section_glm_p7r_0j5 .section}

您可以在使用bucket-encryption命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--sse-algorithm`|表示服务端加密算法，取值为KMS或AES256。|
|`--kms-masterkey-id`|设置指定的CMK ID进行加密。当`--sse-algorithm`值为AES256时，不可附加此项；值为KMS时，根据您的需求选择是否添加。|
|`--method`|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。

 |
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/ossutil多版本文档/查看选项.md#)。

