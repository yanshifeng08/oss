# set-acl {#concept_303807 .concept}

set-acl is used to configure the access control list \(ACL\) for a bucket or object.

## Command syntax {#section_1b2_ahh_2hq .section}

``` {#codeblock_4xm_u4h_uwr}
./ossutil set-acl oss://bucket[/prefix] [acl] [-r] [-b] [-f] [-c file]
```

## Examples {#section_tq9_rm7_7da .section}

-   Configure the ACL for a bucket

    ``` {#codeblock_evp_z8l_yxq}
    ./ossutil set-acl oss://bucket1 private -b       
    ```

    **Note:** A bucket can have any of the following ACL policies:

    -   private
    -   public-read
    -   public-read-write
    For more information about ACLs, see [Access control based on ACLs](../../../../reseller.en-US/Developer Guide/Access and control/Access control based on ACLs.md#).

-   Configure the ACL for a specified object

    ``` {#codeblock_0mk_dmf_ypm}
    ./ossutil set-acl oss://bucket1/path/object private                  
    ```

    **Note:** An object can have any of the following ACL policies:

    -   default: The object inherits the ACL for the bucket it belongs to.
    -   private
    -   public-read
    -   public-read-write
-   Configure the ACL for all objects that have a specified prefix

    ``` {#codeblock_fd6_hd3_0h1}
    ./ossutil set-acl oss://bucket1/path/ private -r
    ```

-   Configure the ACL for objects that meet specified conditions

    When configuring ACLs, you can use the --include/--exclude command to select objects that meet specified conditions. For more information, see [cp](reseller.en-US/Tools/ossutil/Common commands/cp.md#li_1ff_wwf_29y).

    -   Set the ACL to private for all objects except jpg objects

        ``` {#codeblock_7q7_1oe_gem}
        ./ossutil set-acl oss://my-bucket1/path private --exclude "*.jpg" -r
        ```

    -   Set the ACL to private for all objects that contain abc in their names and are not in the jpg or txt format

        ``` {#codeblock_xh3_awk_wr4}
        ./ossutil set-acl oss://my-bucket1/path private --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
        ```


## Common options {#section_ty9_we7_nk4 .section}

The following table describes the options you can add to the set-acl command to set different ACLs for different objects.

|Option|Description|
|------|-----------|
|-r, --recursive|Recursively performs operations on objects in a bucket. If this option is specified, commands that support this option will perform operations on all objects in a bucket that meet the specified conditions. If this option is not specified, the commands will only perform operations on a single specified object.|
|-b, --bucket|Specifies the bucket on which to perform an operation.|
|-f, --force|Forces an operation without prompting the user for confirmation.|
|--include|Includes objects that match a specified string, such as \*.jpg.|
|--exclude|Excludes objects that match a specified string, such as \*.txt.|
|-j, --jobs|Specifies the number of concurrent tasks when multiple objects are operated. Valid values: 1 to 10000. Default value: 3.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--output-dir|Specifies the directory in which output objects are located. Output objects include report objects generated due to errors that occur when you use the cp command to copy multiple objects. For more information about the report objects, see the help information of the cp command. The default value is the ossutil\_output directory in the current directory.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

