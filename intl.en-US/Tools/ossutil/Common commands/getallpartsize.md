# getallpartsize {#concept_303821 .concept}

The getallpartsize command is used to obtain the size of each part generated in incomplete multipart upload tasks initiated to objects in a bucket, and the total size of all parts.

## Command syntax {#section_wj2_5o6_mes .section}

``` {#codeblock_5ol_i2y_zpx}
./ossutil getallpartsize oss://bucket 
```

## Examples {#section_35k_zhh_svf .section}

List the size of each part generated in incomplete multipart upload tasks initiated to objects in a bucket and the total size of all parts.

``` {#codeblock_jhb_9de_6ep}
./ossutil getallpartsize oss://bucket1
PartNumber      UploadId                                Size(Byte)      Path
1               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
2               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
3               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
4               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
5               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
6               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt

total part count:6     total part size(MB):300.00
```

## Common options {#section_zcp_wm8_b3t .section}

The following table describes the options you can add to the getallpartsize command.

|Option|Description|
|------|-----------|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

