# cat {#concept_303824 .concept}

cat命令可以将文件内容输出到ossutil。

## 命令格式 {#section_knw_2en_vat .section}

``` {#codeblock_jtw_o3k_lkv}
./ossutil cat oss://bucket/object [--payer requester]
```

**说明：** 仅建议使用此命令查看文本文件内容。

## 使用示例 {#section_zs8_iqa_zos .section}

查看指定文件内容 ：

``` {#codeblock_7vn_40t_zy5}
./ossutil cat oss://bucket1/test.txt
language=ch
endpoint=oss-cn-hangzhou.aliyuncs.com

0.273027(s) elapsed
```

## 常用选项 {#section_1b1_ai5_0g6 .section}

您可以在使用cat命令时附加如下选项：

|选项名称|描述|
|----|--|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|--payer|请求的支付方式，如果为请求者付费模式，需将该值设置为requester。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如https://120.79.128.211:3128、 socks5://120.79.128.211:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

