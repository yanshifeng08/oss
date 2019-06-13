# cat {#concept_627787 .concept}

cat命令可以将文件内容输出到ossutil。

## 命令格式 {#section_yzr_ebw_wjz .section}

``` {#codeblock_30l_i7m_kzg}
./ossutil cat oss://bucket/object [--version-id versionId]
```

**说明：** 仅建议使用此命令查看文本文件内容。

## 使用示例 {#section_nts_z20_biu .section}

-   查看指定文件内容

    ``` {#codeblock_nix_yyg_zzw}
    ./ossutil cat oss://bucket1/test.txt
    language=ch
    endpoint=oss-cn-hangzhou.aliyuncs.com
    
    0.273027(s) elapsed
    ```

-   在已开启版本控制的Bucket内查看指定版本文件的内容

    ``` {#codeblock_87v_sly_2oa}
    ./ossutil cat oss://bucket1/test.txt --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk
    language=ch
    endpoint=oss-cn-hangzhou.aliyuncs.com
    
    0.375125(s) elapsed
    ```

    在使用`--version-id`选项前，需使用[ls --all-versions](cn.zh-CN/ossutil多版本文档/常用命令/ls.md#li_694_77a_018)命令获取文件的versionid。

    **说明：** `--version-id`选项仅支持在已开启版本控制的Bucket内使用。开启Bucket版本控制命令请参见[bucket-versioning](cn.zh-CN/ossutil多版本文档/常用命令/bucket-versioning.md#)。


## 常用选项 {#section_xrm_hlu_nom .section}

您可以在使用cat命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|`--version-id`|操作指定版本的Object，仅支持在已开启版本控制的Bucket内使用。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/ossutil多版本文档/查看选项.md#)。

