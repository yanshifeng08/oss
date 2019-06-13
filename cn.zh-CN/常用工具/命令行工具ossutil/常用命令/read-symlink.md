# read-symlink {#concept_303813 .concept}

read-symlink命令用于读取符号链接（软链接）文件的描述信息。

**说明：** 

-   关于软链接的介绍请参见[设置软链接](../../../../cn.zh-CN/控制台用户指南/上传、下载和管理文件/设置软链接.md#)。
-   创建软链接的命令请参见[create-symlink](cn.zh-CN/常用工具/命令行工具ossutil/常用命令/create-symlink.md#)。

## 命令格式 {#section_zpu_s31_zhs .section}

``` {#codeblock_s8t_ttp_kii}
./ossutil read-symlink oss://bucket/object [--encoding-type url] [-c file]
```

此操作要求用户对该符号链接文件有读权限。返回的项中`X-Oss-Symlink-Target`表示符号链接的目标文件。如果操作的Object并非符号链接文件，该操作返回错误：NotSymlink。

## 使用示例 {#section_o40_z3k_ced .section}

查看object1的软链接信息：

``` {#codeblock_pbo_n6r_al5}
./ossutil read-symlink oss://bucket1/object1
Etag                    : 455E20DBFFF1D588B67D092C46B16DB6
Last-Modified           : 2017-04-17 14:49:42 +0800 CST
X-Oss-Symlink-Target    : a
```

## 常用选项 {#section_3t8_b58_fyc .section}

您可以在使用create-symlink命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|`--retry-times`|当错误发生时的重试次数，默认值：10，取值范围：1-500。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

