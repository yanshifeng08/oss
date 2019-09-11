# mb {#concept_303803 .concept}

The mb command is used to create a bucket.

## Command syntax {#section_wd4_nh0_986 .section}

``` {#codeblock_9vo_qgr_eeq}
./ossutil mb oss://bucketname [--acl=ACL][--storage-class sc][-c file]
```

## Examples {#section_xmo_lxl_54k .section}

-   Create a bucket

    ``` {#codeblock_kr3_78a_i5s}
    ./ossutil mb oss://bucket1
    ```

    **Note:** The bucket name must be unique. If a bucket of the specified name already exists, an error message will be displayed.

-   Specify the access control list \(ACL\) when creating a bucket

    ``` {#codeblock_0rv_19w_27d}
    ./ossutil mb oss://bucket1 --acl=public-read-write
    ```

-   Specify the storage class when creating a bucket

    ``` {#codeblock_q4j_1e3_abu}
    ./ossutil mb oss://bucket1 --storage-class IA
    ```

-   Create a bucket in a specified region

    ``` {#codeblock_2vz_9wc_39i}
    ./ossutil mb oss://bucket1 -e oss-cn-beijing.aliyuncs.com
    ```

-   Create a bucket from a specified configuration file

    ``` {#codeblock_z7m_7vl_hk9}
    ./ossutil mb oss://bucket1 -c your_config_file_path
    ```


## Common options {#section_agf_huu_93z .section}

The following table describes the options you can add to the mb command to specify bucket attributes.

|Option|Description|
|------|-----------|
|--acl|Specifies the ACL for the bucket. Default value: private. Valid values: -   private
-   public-read
-   public-read-write

 For more information about ACLs, see [Access control based on ACLs](../../../../reseller.en-US/Developer Guide/Access and control/Access control based on ACLs.md#).|
|--storage-class|Specifies the default storage class of the bucket. Default value: Standard. Valid values: -   Standard
-   IA
-   Archive

 For more information about storage classes, see [Introduction to storage class](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage class.md#).

 |
|-c, --config-file|Specifies the configuration file used to create the bucket. If this option is not specified, the default configuration file is used to create the bucket. For more information about the configuration file, see [config](reseller.en-US/Tools/ossutil/Common commands/config.md#).|
|-e, --endpoint|Specifies the endpoint corresponding to the region where the bucket resides. If this option is not specified, the endpoint in the default configuration file is used to specify the region of the bucket.|
|-L, --language|Specifies the language ossutil uses. Valid values: CH and EN. Default value: CH. If you plan to set this option to CH, ensure that your system supports UTF-8 encoding.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

