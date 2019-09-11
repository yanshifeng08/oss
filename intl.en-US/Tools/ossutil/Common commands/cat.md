# cat {#concept_303824 .concept}

The cat command is used to export the content of a specified object to ossutil.

## Command syntax {#section_knw_2en_vat .section}

``` {#codeblock_jtw_o3k_lkv}
./ossutil cat oss://bucket/object [--payer requester]
```

**Note:** We recommend that you only use this command to view the content of a TXT file.

## Examples {#section_zs8_iqa_zos .section}

View the content of a specified object.

``` {#codeblock_7vn_40t_zy5}
./ossutil cat oss://bucket1/test.txt
language=ch
endpoint=oss-cn-hangzhou.aliyuncs.com

0.273027(s) elapsed
```

## Common options {#section_1b1_ai5_0g6 .section}

The following table describes the options you can add to the cat command.

|Option|Description|
|------|-----------|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--payer|Specifies the payer of the request. To enable the pay-by-requester mode, set this option to requester.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

