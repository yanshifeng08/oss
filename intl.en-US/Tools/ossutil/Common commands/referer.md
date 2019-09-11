# referer {#concept_303819 .concept}

The referer command is used to add, modify, query, or delete hotlink protection configurations for a bucket.

**Note:** For more information about hotlink protection, see [Configure hotlinking protection](../../../../reseller.en-US/Developer Guide/Buckets/Configure hotlinking protection.md#).

## Command syntax {#section_06n_hl2_i26 .section}

-   Add or modify hotlink protection configurations for a specified bucket

    ``` {#codeblock_hoh_yk4_uxv}
    ./ossutil referer --method put oss://bucket referer-value [--disable-empty-referer]
    ```

    If the specified bucket does not have hotlink protection configured, you can run this command to add hotlink protection configurations. If the specified bucket has hotlink protection configured, you can run this command to overwrite the existing hotlink protection configurations.

    -   referer-value: specifies a Referer whitelist. Only the specified domain names are allowed to access OSS resources. Asterisks \(\*\) and question marks \(?\) are supported as wildcards. Multiple domain names must be separated with spaces.
    -   --disable-empty-referer: specifies whether the Referer field can be left empty in an access request. If this option is specified, the Referer field cannot be left empty. If the Referer field is required, you must include the Referer field in the HTTP or HTTPS request header to access OSS resources.
-   Obtain the hotlink protection configurations of a bucket

    ``` {#codeblock_40a_bic_qqo}
    ./ossutil referer --method get oss://bucket  [local_xml_file]
    ```

    The local\_xml\_file parameter specifies the name of the configuration file. If this parameter is specified, the hotlink protection configurations are saved as a local file. If this parameter is not specified, ossutil displays the hotlink protection configurations.

-   Delete the hotlink protection configurations of a bucket

    ``` {#codeblock_1rf_ipd_n3q}
    ./ossutil referer --method delete oss://bucket
    ```


## Examples {#section_teu_6p6_zzv .section}

-   Configure hotlink protection and specify the Referer field

    ``` {#codeblock_f5r_ofn_0yf}
    ./ossutil referer --method put oss://bucket1 www.test1.com www.test2.com --disable-empty-referer
    ```

-   Obtain the hotlink protection configurations of a bucket

    ``` {#codeblock_fzd_54n_zi2}
    ./ossutil referer --method get oss://bucket1  /file/referer.xml
    ```

-   Delete the hotlink protection configurations of a bucket

    ``` {#codeblock_8ag_ave_x72}
    ./ossutil referer --method delete oss://bucket1
    ```


## Common options {#section_5k9_786_3ck .section}

The following table describes the options you can add to the referer command.

|Option|Description|
|------|-----------|
|--method|Specifies the HTTP request method. Valid values: -   put: adds or modifies hotlink protection configurations.
-   get: obtains hotlink protection configurations.
-   delete: deletes hotlink protection configurations.

 |
|--disable-empty-referer|Specifies whether the Referer field can be left empty. If this option is specified, the Referer field cannot be left empty. If the Referer field is required, you must include the Referer field in the HTTP or HTTPS request header to access OSS resources.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

