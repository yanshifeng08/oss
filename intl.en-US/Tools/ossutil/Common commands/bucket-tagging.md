# bucket-tagging {#concept_354610 .concept}

The bucket-tagging command is used to add, modify, query, or delete tagging configurations for a bucket.

**Note:** For more information, see .

## Command syntax {#section_ajb_zss_915 .section}

-   Add or modify bucket tagging configurations

    ``` {#codeblock_p1t_azn_vvg}
    ./ossutil bucket-tagging --method put oss://bucket  tagkey1#tagvalue1 tagkey2#tagvalue2
    ```

    If the bucket has no tags, you can run this command to add tags to the bucket. If the bucket has tags, you can run this command to overwrite the existing tags.

    **Note:** 

    -   Only the bucket owner and authorized users can configure bucket tagging. Otherwise, OSS returns 403 Forbidden with error code AccessDenied.
    -   Separate the key from the value in each tag with number signs \(\#\).
    -   You can configure a maximum of 20 tags for a bucket. Separate multiple tags with spaces.
    -   The key and value of the tag can contain letters, digits, spaces, and special characters such as

        + ‑ = . \_ : /

    -   The tag key is required. The tag key can be a maximum of 64 Bytes in length and cannot start with `http://`, `https://`, or `Aliyun`.
    -   The tag value is optional. The tag value can be a maximum of 128 Bytes in length.
    -   The key and value of the tag must be encoded in UTF-8.
-   Query bucket tagging configurations

    ``` {#codeblock_fdb_4o3_gmz}
    ./ossutil bucket-tagging --method get oss://bucket
    ```

-   Delete bucket tagging configurations

    ``` {#codeblock_r0q_75t_vel}
    ./ossutil bucket-tagging --method delete oss://bucket
    ```


## Examples {#section_tad_17w_2bg .section}

-   Add bucket tagging configurations

    ``` {#codeblock_b1b_pg9_a9x}
    ./ossutil bucket-tagging --method put oss://bucket1  tag1#test1 tag2#test2
    ```

-   Query bucket tagging configurations

    ``` {#codeblock_qry_ltw_o0n}
    ./ossutil bucket-tagging --method get oss://bucket1
    ```

-   Delete bucket tagging configurations

    ``` {#codeblock_m87_807_u09}
    ./ossutil bucket-tagging --method delete oss://bucket1
    ```


## Common options {#section_rc0_dzr_n3g .section}

The following table describes the options you can add to the bucket-tagging command.

|Option|Description|
|------|-----------|
|--method|Specifies the HTTP request method. Valid values: -   put: adds or modifies bucket tagging configurations.
-   get: obtains bucket tagging configurations.
-   delete: deletes bucket tagging configurations.

 |
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

