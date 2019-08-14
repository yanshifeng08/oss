# create-symlink {#concept_303812 .concept}

create-symlink命令用于创建符号链接（软链接）。

**说明：** 关于软链接的介绍请参见[设置软链接](../../../../cn.zh-CN/控制台用户指南/上传、下载和管理文件/设置软链接.md#)。

## 命令格式 {#section_34m_6n0_uoj .section}

``` {#codeblock_jvc_tzy_fjf}
./ossutil create-symlink cloud_url target_object [--encoding-type url] [--payer requester] [-c file]
```

## 使用示例 {#section_szr_ttw_av9 .section}

``` {#codeblock_fwu_450_3ia}
./ossutil create-symlink  oss://bucket1/b oss://bucket1/path/a.txt
```

此命令为bucket1内path目录下的a.txt创建一个名为b，存放在bucket1根目录下的软链接文件。

**说明：** 创建符号链接时：

-   不检查目标文件是否存在。
-   不检查目标文件类型是否合法。
-   不检查目标文件是否有权限访问。
-   以上检查在GetObject等需要访问目标文件的API接口时进行。
-   如果试图添加的文件已经存在，并且有访问权限，新添加的文件将覆盖原来的文件。

可通过[stat](cn.zh-CN/常用工具/命令行工具ossutil/常用命令/stat.md#)或[read-symlink](cn.zh-CN/常用工具/命令行工具ossutil/常用命令/read-symlink.md#)命令可以查看软链接的目标文件。

## 常用选项 {#section_71t_ic2_lio .section}

您可以在使用create-symlink命令时附加如下选项：

|选项名称|描述|
|----|--|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|--retry-times|当错误发生时的重试次数，默认值：10，取值范围：1-500。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|--payer|请求的支付方式，如果为请求者付费模式，需将该值设置为requester。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如https://120.79.128.211:3128、 socks5://120.79.128.211:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

