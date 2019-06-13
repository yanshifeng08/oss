# listpart {#concept_303820 .concept}

listpart命令用于列出没有完成分片上传的Object的分片信息。

## 命令格式 {#section_n6e_cb6_me6 .section}

``` {#codeblock_rsk_kxt_58l}
./ossutil listpart oss://bucket/object uploadid [options]
```

您需要先通过[ls -m](cn.zh-CN/常用工具/命令行工具ossutil/常用命令/ls.md#li_euf_ow6_koe)命令查看Bucket的Object和UploadID信息，再用本命令查看分片的详细信息。

## 使用示例 {#section_u9s_fjm_ect .section}

列举单个未完成分片上传的Object的分片信息：

``` {#codeblock_fys_l31_tf6}
./ossutil listpart oss://bucket/object 15754AF7980C4DFB8193F190837520BB
```

## 常用选项 {#section_84p_gtd_ebr .section}

您可以在使用listpart命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

