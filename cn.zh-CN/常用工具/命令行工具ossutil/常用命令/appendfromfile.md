# appendfromfile {#concept_303823 .concept}

appendfromfile命令用于将本地文件内容以追加上传的方式上传到OSS中的appendable Object中。

**说明：** 追加上传详情请参见[追加上传](../../../../cn.zh-CN/开发指南/上传文件（Object）/追加上传.md#)。

## 命令格式 {#section_0jn_cya_wr4 .section}

``` {#codeblock_vld_o31_nhy}
 ./ossutil appendfromfile local_file_name oss://bucket/object [--meta=meta-value]
```

如果Object不存在，可通过附加`--meta`选项设置Object的meta信息，例如`--meta "X-Oss-Meta-Author:test"`。如果Object已经存在，不可以在追加上传时附加`--meta`选项。

**说明：** 

-   通过追加上传方式上传的Object，必须手动指定Object的名称。
-   只有通过追加上传创建的Object才可以后续继续被追加上传。
-   您可以通过[set-meta](cn.zh-CN/常用工具/命令行工具ossutil/常用命令/set-meta.md#)命令修改Object的meta信息。

## 使用示例 {#section_oea_76k_nb1 .section}

通过追加上传上传文件：

``` {#codeblock_lyh_dwx_zd3}
./ossutil appendfromfile /file/test.txt oss://bucket1/test.txt 
total append 64(100.00%) byte,speed is 0.00(KB/s)
local file size is 64,the object new size is 64,average speed is 0.34(KB/s)
```

## 常用选项 {#section_o36_riw_p7l .section}

您可以在使用appendfromfile命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--meta`|设置Object的meta信息。如果Object已经存在，不可附加此项。|
|`--maxupspeed`|设置最大上传速度，单位：KB/s，缺省值为0（不限速）。|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

