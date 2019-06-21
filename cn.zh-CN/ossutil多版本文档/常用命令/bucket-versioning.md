# bucket-versioning {#concept_627782 .concept}

bucket-versioning命令用于开启、暂停存储空间（Bucket）版本控制，以及查询Bucket版本控制状态。

**说明：** Bucket版本控制介绍请参见[版本控制介绍](https://help.aliyun.com/document_detail/119296.html)。

## 命令格式 {#section_4xw_xir_xlg .section}

-   开启/暂停版本控制

    ``` {#codeblock_zmd_uzq_mfo}
    ./ossutil bucket-versioning --method put oss://bucket versioning_parameter
    ```

     `--method`选项为put时，versioning状态参数只能为enabled或suspended。

    -   当versioning状态参数为enabled时，表示开启版本控制。
    -   当versioning状态参数为suspended时，表示暂停版本控制。
-   查询版本控制状态

    ``` {#codeblock_zpb_m35_gh4}
    ./ossutil bucket-versioning --method get oss://bucket
    ```


## 使用示例 {#section_i49_3et_hi6 .section}

-   开启Bucket版本控制

    ``` {#codeblock_z18_z3b_84f}
    ./ossutil bucket-versioning --method put oss://bucket1 enabled
    ```

-   暂停Bucket版本控制

    ``` {#codeblock_6q8_3oa_klq}
    ./ossutil bucket-versioning --method put oss://bucket1 suspended
    ```

-   查询Bucket版本控制状态

    ``` {#codeblock_pvz_yxz_opp}
    ./ossutil bucket-versioning --method get oss://bucket1
    bucket versioning status:Suspended
    ```

    **说明：** Bucket版本控制有如下三个状态：

    -   Enabled：表示该Bucket的版本控制功能处于开启状态。此状态下，删除或覆写Object都会生成对应的Object版本。
    -   Suspended：表示该Bucket的版本控制功能处于暂停状态。此状态下，之前生成的Object版本都会作为历史版本保存下来。再次对Object进行覆写或删除操作时，会生成一个versionid为null的版本，此versionid唯一，新生成的null版本会覆盖旧的null版本。
    -   Null：表示没有开启过版本控制功能。若Bucket曾经开启过版本控制功能，则Bucket的版本控制状态仅可以为Enabled或Suspended状态，无法再变为Null状态。

## 常用选项 {#section_ubv_l0b_dft .section}

您可以在使用bucket-versioning命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--method`|表示http的请求类型。取值： -   put：开启或暂停版本控制功能。
-   get：查看版本控制状态。

 |
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/ossutil多版本文档/查看选项.md#)。

