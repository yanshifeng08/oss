# object-tagging {#concept_1614441 .concept}

The object-tagging command is used to add, modify, query, or delete tagging configurations for objects.

**Note:** For more information about object tagging, see [Object tagging](../../../../reseller.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

## Command syntax {#section_b54_bhy_ay7 .section}

-   Add or modify object tagging configurations

    ``` {#codeblock_0kq_6jl_lxo}
    ./ossutil object-tagging --method put oss://bucket[/prefix] key#value [--encoding-type url] [-r] [--payer requester] [--version-id versionId] [-c file]
    ```

    If the object has no tags, you can run this command to add a specified tag to the object. If the object has tags, you can run this command to overwrite existing tags.

    **Note:** 

    -   A maximum of 10 tags can be set for each object. Each object tag must have a unique tag key.
    -   A tag key can be maximum of 128 characters in length. A tag value can be a maximum of 256 characters in length.
    -   Keys and values are case-sensitive.
    -   The key and value of the tag can contain letters, digits, spaces, and special characters such as

        + = . \_ : /

    -   Only the bucket owner and authorized users have the read and write permissions on object tags. These permissions are independent of object ACLs.
    -   During cross-region replication, object tags are also replicated to the destination bucket.
-   Query object tagging configurations

    ``` {#codeblock_0co_t0m_83h}
    ./ossutil object-tagging --method get oss://bucket[/prefix] [--encoding-type url] [-r]  [--payer requester] [--version-id versionId] [-c file]
    ```

-   Delete object tagging configurations

    ``` {#codeblock_02n_fuu_flv}
    ./ossuitl object-tagging --method delete oss://bucket[/prefix] [--encoding-type url] [-r] [--payer requester] [--version-id versionId] [-c file]
    ```


## Examples {#section_tz5_rck_g69 .section}

-   Add object tagging configurations

    ``` {#codeblock_uiy_001_e77}
    ./ossutil object-tagging --method put oss://bucket1/test.jpg a#1 b#2 c#3
    0.168034(s) elapsed
    ```

-   Query object tagging configurations

    ``` {#codeblock_vss_kgc_csv}
    ./ossutil object-tagging --method get oss://bucket1/test.jpg
    object index   tag index      tag key   tag value       object
    ---------------------------------------------------------------------------
    1              0              "a"       "1"     123/test.jpg
    1              1              "b"       "2"     123/test.jpg
    1              2              "c"       "3"     123/test.jpg
    
    0.228023(s) elapsed
    ```

-   Delete object tagging configurations

    ``` {#codeblock_8uz_loe_ozo}
    ./ossutil object-tagging --method delete oss://bucket1/test.jpg
    0.200020(s) elapsed
    ```


## Common options {#section_0ex_g2z_jjz .section}

The following table describes the options you can add to the object-tagging command.

|Option|Description|
|------|-----------|
|--method|Specifies the HTTP request method. Valid values: -   put: adds or modifies object tagging configurations.
-   get: obtains object tagging configurations.
-   delete: deletes object tagging configurations.

 |
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|
|--payer|Specifies the payer of the request. To enable the pay-by-requester mode, set this option to requester.|
|-j, --jobs|Specifies the number of concurrent tasks when multiple objects are operated. Valid values: 1 to 10000. Default value: 3.|
|-r,--recursive|Recursively performs operations on objects in a bucket. If this option is specified, commands that support this option will perform operations on all objects in a bucket that meet the specified conditions. If this option is not specified, the commands will only perform operations on a single specified object.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

