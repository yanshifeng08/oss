# probe {#concept_303814 .concept}

The probe command is used to monitor access to OSS, and troubleshoot problems caused during the upload and download process by network faults or incorrect parameter configurations.

## Command syntax {#section_wgg_g3z_1lt .section}

-   Download an object from a bucket to your local device using the object URL and generate a troubleshooting report

    ``` {#codeblock_r7c_bfk_jod}
    ./ossutil probe --download --url http_url [--addr=domain_name] [file_name]
    ```

    After downloading an object from a bucket to your local device using the object URL, you can test the network transmission quality and generate a troubleshooting report.

    -   --url: specifies the URL of an object in a specified bucket.
        -   If the ACL for the object is public-read, the URL does not carry a signature. Example: `https://bucketname.oss-cn-beijing.aliyuncs.com/myphoto.jpg`.
        -   If the ACL for the object is private, the URL carries a signature and starts and ends with a double quotation mark \("\). Example: `“https://bucketname.oss-cn-beijing.aliyuncs.com/myphoto.jpg?Expires=1552015472&OSSAccessKeyId=TMP.xxxxxxxx5r9f1FV12y8_Qis6LUVmvoSCUSs7aboCCHtydQ0axN32Sn-UvyY3AAAwLAIUarYNLcO87AKMEcE5O3AxxxxxxoCFAQuRdZYyVFyqOW8QkGAN-bamUiQ&Signature=bIa4llbMbldrl7rwckr%2FXXvTtxw%3D”`.
    -   --addr=domain\_name: optional. This parameter specifies the domain name or IP address to ping while the object is being downloaded. If this parameter is not specified, no ping operations are performed.
        -   If the --addr= option is set to the default value, ossutil pings `www.aliyun.com`.
        -   If the --addr= option is set to a domain name or IP address, ossutil pings the specified domain name or IP address.
    -   file\_name: optional. This parameter specifies the file path in which to store the downloaded object. If file\_name is not specified, ossutil saves the downloaded object to the current directory and determines the object name. If you use file\_name to specify an object or directory name, ossutil uses the specified file\_name value to name the downloaded object or saves the downloaded object to the directory specified by file\_name.
-   Download an object from a bucket and generate a troubleshooting report

    ``` {#codeblock_slv_mrg_c3m}
    ./ossutil probe --download --bucketname bucket-name [--object=object_name] [--addr=domain_name] [file_name]
    ```

    -   --bucketname: specifies the name of the bucket that contains the object to be downloaded.
    -   --object=: optional. This parameter specifies the file path where the downloaded object is stored. Example: path/myphoto.jpg. If --object= is not specified, ossutil generates a temporary object, uploads the object to the bucket specified by the bucket-name parameter, and then downloads the object. After this object is downloaded, ossutil deletes this object from your local device and bucket.
-   Upload an object and generate a troubleshooting report

    ``` {#codeblock_gop_cwr_l00}
    ./ossutil probe --upload [file_name] --bucketname bucket-name [--object=obj
    ect_name] [--addr=domain_name] [--upmode]
    ```

    -   file\_name: optional. This parameter specifies the name of the object that you want to upload to the bucket specified by the bucket-name parameter. If file\_name is not specified, ossutil generates a temporary object and uploads it to the specified bucket. After completing the probe, ossutil deletes this temporary object.
    -   --object=: optional. This parameter specifies the name of an object or directory. Example: path/myphoto.jpg. If --object= is not specified, ossutil generates a name for the uploaded object. After completing the probe, ossutil deletes this object.
    -   --upmode: optional. This parameter specifies the upload method. Default value: normal. Valid values:
        -   normal
        -   append
        -   multipart

## Examples {#section_gfs_bub_1fk .section}

-   Download an object from a bucket to your local device using the object URL and generate a troubleshooting report

    ``` {#codeblock_j7s_dj2_yic}
    ./ossutil probe --download --url https://bucket1.oss-cn-beijing.aliyuncs.com/myphoto.jpg --addr=www.aliyun.com /file/myphoto.jpg
    					
    ```

-   Download an object from a bucket and generate a troubleshooting report

    ``` {#codeblock_0yc_2p9_2k2}
    ./ossutil probe --download --bucketname bucket1 --object=myphoto.jpg --addr=www.aliyun.com &nbsp;/file/myphoto.jpg
    ```

-   Check the upload result and generate a troubleshooting report

    ``` {#codeblock_0bf_g52_ne2}
    ./ossutil probe --upload /file/myphoto.jpg --bucketname bucket1 --object=myphoto.jpg &nbsp;--upmode normal
    ```


## View a troubleshooting report {#section_ohh_b42_eeb .section}

After running the probe command, you can view each task execution procedure and overall upload or download results.

-   If an × is displayed following a procedure, the operation failed.
-   If no × is displayed following a procedure, the operation succeeded. If the upload or download was successful, ossutil displays the object size and the time the object was uploaded or downloaded. If the upload or download failed, ossutil displays the failure cause or troubleshooting advice.

    **Note:** ossutil may not generate troubleshooting advice for a few errors. In this case, you can troubleshoot the problems based on the error codes by following the instructions provided in [OSS error codes](../../../../reseller.en-US/SDK Reference/Java/Exception handling.md#section_gn4_55c_bhb).


After running the probe command, ossutil generates an object whose name begins with logOssProbe in your current directory. This object contains details about the commands that you have run to troubleshoot problems.

## Common options {#section_0eb_p3n_qk7 .section}

The following table describes the options you can add to the probe command.

|Option|Description|
|------|-----------|
|--url|Specifies the network address of the object. ossutil will use this address to download the object.|
|--bucketname|Specifies the name of the bucket in OSS.|
|--object|Specifies the name of the object in OSS.|
|--addr|Specifies the domain name to ping. Default value: `www.aliyun.com`.|
|--upmode|Specifies the upload method. Default value: normal. Valid values: -   normal
-   append
-   multipart

 |
|--upload|Uploads the object to OSS.|
|--download|Downloads the object from OSS.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

