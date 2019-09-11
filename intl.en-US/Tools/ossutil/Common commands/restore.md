# restore {#concept_303811 .concept}

The restore command is used to restore an object from the frozen state to the readable state.

## Command syntax {#section_7b1_mva_jpi .section}

``` {#codeblock_3bl_llt_ivk}
./ossutil restore cloud_url [--encoding-type url] [--payer requester] [-r] [-f] [--output-dir=odir] [-c file]
```

## Examples {#section_0yr_8af_sba .section}

-   Restore an object from the frozen state to the readable state

    ``` {#codeblock_ybc_l6h_g53}
    ./ossutil restore oss://bucket/object
    ```

-   Restore all objects that have a specified prefix to the readable state

    ``` {#codeblock_3o9_0jp_aol}
    ./ossutil restore oss://bucket/path/ -r                         
    ```

    **Note:** 

    -   The restore operation takes about one minute. You cannot read an object that is being restored.
    -   By default, the object will remain in the readable state for one day. Running the restore command for an object in the readable state will prolong the period for another day. You can prolong this period to a maximum of seven days. After this period ends, the object returns to the frozen state.

## Common options {#section_bn9_td1_gfo .section}

The following table describes the options you can add to the restore command.

|Option|Description|
|------|-----------|
|-r, --recursive|Recursively performs operations on objects in a bucket. If this option is specified, commands that support this option will perform operations on all objects in a bucket that meet the specified conditions. If this option is not specified, the commands will only perform operations on a single specified object.|
|-f, --force|Forces an operation without prompting the user for confirmation.|
|-j, --jobs|Specifies the number of concurrent tasks when multiple objects are operated. Valid values: 1 to 10000. Default value: 3.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--output-dir=|Specifies the directory in which output objects are located. Output objects include report objects generated due to errors that occur when you use the cp command to copy multiple objects. For more information about the report objects, see the help information of the cp command. The default value is the ossutil\_output directory in the current directory.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|--payer|Specifies the payer of the request. To enable the pay-by-requester mode, set this option to requester.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

