# listpart {#concept_303820 .concept}

The listpart command is used to list information of parts that are generated when multiplart upload tasks fail.

## Command syntax {#section_n6e_cb6_me6 .section}

``` {#codeblock_rsk_kxt_58l}
./ossutil listpart oss://bucket/object uploadid [options]
```

Before you run this command to obtain information of parts, run the [ls -m](reseller.en-US/Tools/ossutil/Common commands/ls.md#li_euf_ow6_koe) command to obtain information of upload IDs corresponding to buckets and objects.

## Examples {#section_u9s_fjm_ect .section}

Lists parts in an object that are generated when multipart upload tasks have not been completed.

``` {#codeblock_fys_l31_tf6}
./ossutil listpart oss://bucket/object 15754AF7980C4DFB8193F190837520BB
```

## Common options {#section_84p_gtd_ebr .section}

The following table describes the options that you can add to the listpart command.

|Option|Description|
|------|-----------|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

