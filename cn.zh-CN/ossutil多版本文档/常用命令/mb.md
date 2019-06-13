# mb {#concept_627795 .concept}

mb命令用于创建存储空间（Bucket）。

## 命令格式 {#section_2o3_hxt_fnc .section}

``` {#codeblock_4nu_pje_q4q}
./ossutil mb oss://bucketname [--acl=ACL][--storage-class sc][-c file]
```

## 使用示例 {#section_fnu_6gt_8yo .section}

-   创建Bucket

    ``` {#codeblock_wc9_0zm_uef}
    ./ossutil mb oss://bucket1
    ```

    **说明：** Bucket名称唯一，若创建的Bucket名称已存在，会出现Bucket已存在的报错信息。

-   创建Bucket时指定访问权限

    ``` {#codeblock_lrx_lko_835}
    ./ossutil mb oss://bucket1 --acl=public-read-write
    ```

-   创建Bucket时指定存储类型

    ``` {#codeblock_qsq_wlk_e1o}
    ./ossutil mb oss://bucket1 --storage-class IA
    ```

-   在指定地域创建Bucket

    ``` {#codeblock_nmt_vrx_vvy}
    ./ossutil mb oss://bucket1 -e oss-cn-beijing.aliyuncs.com
    ```

-   使用指定的配置文件创建Bucket

    ``` {#codeblock_7zd_cqh_kow}
    ./ossutil mb oss://bucket1 -c your_config_file_path
    ```


## 常用选项 {#section_h4x_3mz_yjl .section}

您可以在使用mb命令时，指定如下选项来指定Bucket属性：

|选项名称|描述|
|----|--|
|`--acl`|用来指定存储空间访问权限，默认为私有（private）。取值范围： -   private：私有
-   public-read：公共读
-   public-read-write：公共读写

 ACL详情请参见[基于读写权限ACL的权限控制](../../../../cn.zh-CN/开发指南/权限控制/基于读写权限ACL的权限控制.md#)。|
|`--storage-class`|用来指定Bucket的默认存储类型。默认存储类型为标准存储类型（Standard）。 取值范围： -   Standard：标准存储
-   IA：低频访问
-   Archive：归档存储

 存储类型详情请参见[存储类型介绍](../../../../cn.zh-CN/开发指南/存储类型/存储类型介绍.md#)。

 |
|`-c，--config-file`|使用指定的config文件来创建Bucket。未附加该参数则表示使用默认配置文件创建Bucket。config配置请参见[config](cn.zh-CN/ossutil多版本文档/常用命令/config.md#)。|
|`-e，--endpoint`|用来指定Bucket的地域（Region）。未附加该参数则表示使用默认配置文件中的Endpoint来指定Bucket的地域。|
|-L，--language|设置ossutil工具的语言，默认值：CH，取值范围：CH/EN，若设置成“CH”，请确保您的系统编码为UTF-8。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|`--retry-times`|当错误发生时的重试次数，默认值：10，取值范围：1-500。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/ossutil多版本文档/查看选项.md#)。

