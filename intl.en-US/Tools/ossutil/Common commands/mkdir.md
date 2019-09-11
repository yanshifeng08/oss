# mkdir {#concept_303815 .concept}

The mkdir command is used to create a directory in a bucket.

## Command syntax {#section_580_tiw_43c .section}

``` {#codeblock_n4i_koq_19l}
./ossutil mkdir oss://bucket/dir/
```

## Examples {#section_fco_slv_996 .section}

-   Create a directory

    ``` {#codeblock_oj4_qo1_coh}
    ./ossutil mkdir oss://bucket1/dir1/
    ```

    **Note:** 

    -   A directory must end with a forward slash \(/\). If the specified directory name does not end with a forward slash, ossutil will automatically add one forward slash \(/\).
    -   If the specified directory already exists, an error is displayed.
-   Create a multi-level directory

    ``` {#codeblock_sgf_krs_92z}
    ./ossutil mkdir oss://bucket/dir1/dir2/
    ```

    When creating a multi-level directory, ossutil only creates the last level of directory. If dir2/ is deleted and there are no objects in dir1/, dir1/ will also be deleted after the command is run.


## Common options {#section_x10_4cl_z5i .section}

The following table describes the options you can add to the mkdir command.

|Option|Description|
|------|-----------|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

