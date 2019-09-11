# cors-options {#concept_744986 .concept}

The cors-options command is used to test whether a bucket has cross-origin resource sharing \(CORS\) configured.

**Note:** 

-   For more information about CORS, see [Set CORS rules](../../../../reseller.en-US/Developer Guide/Buckets/Set CORS rules.md#).
-   For more information about commands used to configure CORS, see [cors](reseller.en-US/Tools/ossutil/Common commands/cors.md#).

## Command syntax {#section_crb_16k_05k .section}

``` {#codeblock_sj6_9ud_spr}
./ossutil cors-options --acr-method <value> --origin <value> --acr-headers <value> oss://bucket/[object]
```

The cors-options command is used to send an HTTP OPTIONS request to OSS to test whether a cross-origin request is allowed.

-   --acr-method: specifies the value of the Access-Control-Request-Method field in the HTTP header. Valid values: GET, PUT, POST, DELETE, and HEAD.
-   --origin: specifies the value of the Origin field in the HTTP header. This parameter specifies the origin where a cross-origin request is from and is used to identify the origin that you will allow to access your bucket.
-   --acr-headers: specifies the value of the Access-Control-Request-Headers field in the HTTP header. This parameter specifies actual headers except commonly used headers. To specify multiple headers, separate different headers with commas \(,\) and enclose the headers with double quotation marks \("\). Example: --acr-headers "header1,header2,header3."

## Examples {#section_lg3_v6d_whp .section}

``` {#codeblock_5qx_etg_nr4}
./ossutil cors-options --acr-method  put --origin "www.aliyun.com" oss://bucket1
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Origin: *
Access-Control-Max-Age: 0
```

## Common options {#section_sc2_luv_tx8 .section}

The following table describes the options you can add to the cors-options command.

|Option|Description|
|------|-----------|
|--origin|Specifies the value of the Origin field in the HTTP header. This parameter specifies the origin where a cross-origin request is from and is used to identify the origin that you will allow to access your bucket.|
|--acr-method|Specifies the value of the Access-Control-Request-Method field in the HTTP header. Valid values: GET, PUT, POST, DELETE, and HEAD.|
|--acr-headers|Specifies the value of the Access-Control-Request-Headers field in the HTTP header. This parameter specifies actual headers except commonly used headers. To specify multiple headers, separate different headers with commas \(,\) and enclose the headers with double quotation marks \("\). Example: --acr-headers "header1,header2,header3."|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

