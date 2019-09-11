# stat {#concept_303806 .concept}

The stat command is used to obtain the description of buckets or objects. For example, you can run the stat command to view the object metadata that is set by running the set-meta command.

## Command syntax {#section_liy_tm3_sni .section}

``` {#codeblock_rfh_wfz_50f}
./ossutil stat oss://bucket[/object] [--encoding-type url] [--payer requester] [-c file]
```

## Examples {#section_vnz_n0s_8bv .section}

-   Obtain information of a bucket

    ``` {#codeblock_4dg_535_k9b}
     ./ossutil stat oss://bucket1
    ```

-   Obtain information of a specified object

    ``` {#codeblock_a8b_zec_e1a}
    ./ossutil stat oss://bucket1/object
    ```

-   Obtain information of an object whose names contain special characters

    ossutil only supports URL encoding for object names. If an object name contains special characters, you can encode these special characters before you use the object name in the command. For example, you can run the following command to obtain information of the 示例.txt object:

    ``` {#codeblock_pya_uir_67f}
    ./ossutil stat oss://bucket1/%E7%A4%BA%E4%BE%8B.txt --encoding-type url
    ```

    **Note:** Bucket names cannot be URL-encoded.


## Common options {#section_bft_79i_yvp .section}

The following table describes the options you can add to the stat command.

|Option|Description|
|------|-----------|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|--payer|Specifies the payer of the request. To enable the pay-by-requester mode, set this option to requester.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

