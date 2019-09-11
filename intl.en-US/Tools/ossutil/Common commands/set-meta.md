# set-meta {#concept_303809 .concept}

The set-meta command is used to set object metadata for objects that have been uploaded.

## Command syntax {#section_bju_d7q_85v .section}

``` {#codeblock_94n_tx2_m5g}
./ossutil set-meta oss://bucket[/prefix] [header:value#header:value...] [--update] [--delete] [-r] [-f] [-c file]
```

## Examples {#section_cmg_g1k_upe .section}

-   Configure the metadata of a specified object

    ``` {#codeblock_oth_e60_oec}
    ./ossutil set-meta oss://bucket1/path/object x-oss-object-acl:private 
    ```

    If the --update and --delete options are not specified, this command replaces the existing metadata with the specified metadata. If the `[header:value#header:value...]` option is not specified, the values of headers that cannot be deleted, such as headers without the X-Oss-meta prefix, will remain unchanged. Other metadata will be deleted.

    **Note:** Headers are case-insensitive, whereas values are case-sensitive. The list of headers you can set for objects is as follows:

    ``` {#codeblock_zjm_eln_3ti}
    Headers:
          Expires(time.RFC3339:2006-01-02T15:04:05Z07:00)
          X-Oss-Object-Acl
          Origin
          X-Oss-Storage-Class
          Content-Encoding
          Cache-Control
          Content-Disposition
          Accept-Encoding
          X-Oss-Server-Side-Encryption
          Content-Type
          and headers with the X-Oss-Meta- prefix.
    ```

    For more information, see [Manage Object Meta](../../../../reseller.en-US/Developer Guide/Objects/Manage files/Manage Object Meta.md#).

-   Configure the metadata of all objects that have a specified prefix

    ``` {#codeblock_rup_31r_dh6}
    ./ossutil set-meta oss://bucket1/path/ Cache-Control:no-cache#x-oss-object-acl:private -r                             
    ```

    If the -r option is specified, the metadata of multiple objects with the specified prefix will be configured. When an error occurs during an operation on an object, ossutil records the error information of the object in a report object and then continues to perform the operation on other objects. The information of objects that were processed will not be recorded in the report object. Separate multiple metadata items with number signs \(\#\).

-   Update the metadata of a specified object

    ``` {#codeblock_quu_i12_zp6}
    ./ossutil set-meta oss://bucket1/path/object x-oss-object-acl:private --update
    ```

    The command only updates the specified header of the specified object as the input value. The header value can be null. The other metadata of the specified object remains unchanged. The --update \(abbreviated as-u\) and --delete options cannot be specified together.

-   Delete the metadata of a specified object

    ``` {#codeblock_txu_5mk_r7s}
    ./ossutil set-meta oss://bucket1/obj1 X-Oss-Meta-delete --delete
    ```

    If the --delete option is specified, ossutil deletes the specified object header and the value becomes null. The other metadata of the object remains unchanged. This option is not valid for headers that cannot be deleted, such as headers without the X-Oss-Meta prefix. The --update and --delete options cannot be specified together.

-   Configure the metadata of multiple objects with specified conditions

    When configuring metadata, you can use the --include/--exclude command to select objects that meet the specified conditions. For more information, see [cp](reseller.en-US/Tools/ossutil/Common commands/cp.md#li_1ff_wwf_29y).

    -   Set all objects with a jpg extension to the Infrequent Access \(IA\) storage class

        ``` {#codeblock_ang_n1m_trb}
        ./ossutil64 set-meta oss://my-bucket/path X-Oss-Storage-Class:IA --include "*.jpg" -u -r                                        
        ```

    -   Set all objects that contain abc in their names and are not in the jpg or txt format to the Standard storage class

        ``` {#codeblock_ugv_nwy_f5l}
        ./ossutil set-meta oss://my-bucket/path X-Oss-Storage-Class:Standard --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -u -r
        ```


## Common options {#section_6gq_0rd_0c5 .section}

The following table describes the options that you can add to the set-meta command.

|Option|Description|
|------|-----------|
|-u, --update|Updates the metadata of a specified object.|
|--delete|Deletes the metadata of a specified object.|
|-r, --recursive|Recursively performs operations on objects in a bucket. If this option is specified, commands that support this option will perform operations on all objects in a bucket that meet the specified conditions. If this option is not specified, the commands will only perform operations on a single specified object.|
|-f, --force|Forces an operation without prompting the user for confirmation.|
|--include|Includes objects that match a specified string, such as \*.jpg.|
|--exclude|Excludes objects that match a specified string, such as \*.txt.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|-j, --jobs|Specifies the number of concurrent tasks when multiple objects are operated. Valid values: 1 to 10000. Default value: 3.|
|-L, --language|Specifies the language ossutil uses. Valid values: CH and EN. Default value: CH. To set this option to CH, ensure that your system supports UTF-8 encoding.|
|--output-dir|Specifies the directory in which output objects are located. Output objects include report objects generated due to errors that occur when you use the cp command to copy multiple objects. For more information about the report objects, see the help information of the cp command. The default value is the ossutil\_output directory in the current directory.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

