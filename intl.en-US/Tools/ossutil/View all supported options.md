# View all supported options {#concept_j45_dg4_vdb .concept}

You can use the -h option to view all options supported by ossutil.

## Command syntax {#section_sux_e1l_z44 .section}

``` {#codeblock_eqi_wur_sd0}
./ossutil -h
```

To view the options supported by a command, run the ossutil help \[command\] command, such as ossutil help cp.

## Common options {#section_yhn_ko6_gqj .section}

The following section describes some common options that can be used in most ossutil commands:

-   -c, --config-file 

    Specifies the configuration file path of ossutil. ossutil reads the configuration file during startup, and writes configurations to the file by using the config command. When managing buckets that belong to different accounts, you can generate multiple configuration files, and select one to serve as the default configuration file. When managing buckets that belong to other accounts, you can use the -c option to specify the corresponding configuration files.

-   -e, --endpoint 

    Specifies the endpoint of a bucket. When you perform across-region bucket management, you can use the -e option to specify the regions.

-   -i, --access-key-id 

    Specifies the AccessKey ID used to access OSS. When managing buckets that belong to different accounts, you can use the -i option to specify the corresponding AccessKey ID.

-   -k, --access-key-secret 

    Specifies the AccessKey Secret used to access OSS. When managing buckets that belong to different accounts, you can use the -k option to specify the corresponding AccessKey Secret.

-   --loglevel 

    Generates ossutil log files ossutil.log in the current working directory. The default value is null, indicating that no log files are generated.

    -   Valid values:
    -   info: generates operations logs.

        ``` {#codeblock_sza_g3t_y3f}
        ./ossutil [command] --loglevel=info
        ```

    -   debug: generates logs that contain HTTP requests and responses and original signature strings to locate problems.

        ``` {#codeblock_cy4_5dz_knn}
        ./ossutil [command] --loglevel=debug
        ```

-   --proxy-host, --proxy-user, --proxy-pwd 

    If your environment requires a proxy server to access websites, you need to use these three options to specify the proxy server information. ossutil uses the specified information to access OSS through the proxy server. Example:

    ``` {#codeblock_pdn_lqk_1e7}
    ./ossutil ls oss://bucket1 --proxy-host http://47.88. **.**:3128 --proxy-user test --proxy-pwd test
    ```


## Options {#section_efm_mjz_ngv .section}

The following table describes all options supported by ossutil.

|Option|Description|
|------|-----------|
|-s, --short-format|Lists items in short format. The long format is used if this option is not specified.|
|--bigfile-threshold|Specifies the threshold for enabling the resumable data transfer for large files. Unit: Byte. Valid values: non-negative integers. Default value: 100 MB.|
|--acl|Configures the access control list \(ACL\) for an object or bucket.|
|--range|Specifies the range of an object to be downloaded. The value must be in the 3-9, 3-, or -9 format.|
|--type|Specifies the calculation type. Valid values: crc64 and md5. Default value: crc64.|
|-v, --version|Displays the ossutil version and exits.|
|-u, --update|Updates the ossutil version.|
|--origin|Specifies the value of the Origin header in an HTTP request.|
|--upmode|Specifies the upload method. Valid values: normal, append, and multipart. Default value: normal. This option is used with the probe command.|
|--sse-algorithm|Specifies the server-side encryption algorithm. Valid values: KMS and AES256.|
|--include|Includes objects that match a specified string, such as \*.jpg.|
|--exclude|Excludes objects that match a specified string, such as \*.txt.|
|-r, --recursive|Recursively performs operations on objects in a bucket. If this option is specified, commands that support this option will perform operations on all objects in a bucket that meet the specified conditions. If this option is not specified, the commands will only perform operations on the specified object.|
|--addr|Specifies a network address, which is a domain name in most cases. This option is used with the probe command.|
|--kms-masterkey-id|Specifies the CMK ID used for encryption in Key Management Service \(KMS\).|
|-m, --multipart|Specifies that operations are only to be performed on uncompleted multipart upload tasks in a bucket instead of objects.|
|-d, --directory|Lists files and subdirectories in the current directory, instead of recursively displaying all objects in all subdirectories.|
|--payer|Specifies the payer of the request. You can set this option to requester to enable the pay-by-requester mode.|
|--maxupspeed|Specifies the maximum upload speed. Unit: KByte/s. Default value: 0 \(unlimited\).|
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|-c, --config-file|Specifies the configuration file path of ossutil. ossutil reads the configuration file during startup, and writes configurations to the file by using the config command.|
|--download|Downloads an object from OSS. This option is used with the probe command.|
|-j, --jobs|Specifies the number of concurrent operations performed on multiple objects. Valid values: 1 to 10000. Default value: 3.|
|-a, --all-type|Specifies that operations are to be performed on both the objects and uncompleted multipart upload tasks in a bucket.|
|--disable-empty-referer|Indicates that the referer field cannot be left empty. This option is used with the referer command.|
|--method|Specifies the HTTP request method, which can be PUT, GET, or DELETE.|
|--output-dir|Specifies the destination to write output files. Output files include report files generated due to errors that occur when you use the cp command to copy multiple files at a time. To obtain more information about the report files, use the help option with the cp command. The default value is the ossutil\_output directory in the current directory.|
|--meta|Configures the metadata of an object in the \[header:value\#header:value...\] format. Example: `Cache-Control: no-cache#Content-Encoding: gzip`.|
|--object|Specifies the name of an object in OSS. This option is used with the probe command.|
|-e, --endpoint|Specifies the basic endpoint of ossutil. This option value will overwrite the corresponding endpoint in the configuration file. Note that this value must be a second-level domain name.|
|--limited-num|Specifies the number of buckets displayed on each page.|
|-L, --language|Specifies the language of ossutil. Valid values: CH and EN. Default value: CH. If you plan to set this option to CH, ensure that your system supports UTF-8 encoding.|
|--delete|Specifies a delete operation.|
|-b, --bucket|Specifies the bucket on which to perform the operation.|
|--disable-crc64|Disables CRC64. By default, ossutil enables CRC64 during data transmission.|
|--upload|Uploads an object to OSS. This option is used with the probe command.|
|--part-size|Specifies the part size in Byte. By default, ossutil calculates the appropriate part size based on the object size. If you need to optimize performance or are operating under special constraints, you can set this option to any positive integer.|
|--timeout|Specifies the timeout period for a signed URL request. Unit: seconds. Valid values: non-negative integers. Default value: 60.|
|-k, --access-key-secret|Specifies the AccessKey Secret used to access OSS. This option value will overwrite the corresponding configurations in the configuration file.|
|--checkpoint-dir|Specifies the checkpoint directory path. Default value: .ossutil\_checkpoint. If a resumable data transfer fails, ossutil automatically creates this directory and records the checkpoint information in the directory. If a resumable data transfer succeeds, ossutil deletes this directory. If this option is specified, ensure that you have permissions to delete the specified directory.|
|--url|Specifies the URL of an object. This option is used with the probe command.|
|--marker|Specifies the bucket name, object name, or multipart upload ID from which you want to start listing.|
|-f, --force|Forces an operation without prompting the user for confirmation.|
|--snapshot-path|Specifies the directory used in certain scenarios to accelerate the incremental upload or download of multiple objects. If you use this option to upload or download objects, ossutil takes a snapshot of the upload or download and stores it in a specified directory. Then, the next time objects are uploaded or downloaded, ossutil will read the snapshot information from the specified directory to perform incremental upload or download. The snapshot directory you specified must be a writable directory in your local file system. If the directory does not exist, ossutil creates a directory to store the snapshot information. If the directory already exists, ossutil reads and updates the snapshot information in the directory, and incrementally uploads or downloads objects that previously failed to be uploaded or downloaded and have been modified. Note: If you use this option, the recorded local lastModifiedTime of previously uploaded or downloaded objects is compared with that of objects pending upload or download to determine whether to skip these objects. If you use this option to upload an object, make sure that no other users update the object in OSS between two upload operations. In other scenarios where objects are updated in OSS between two upload operations, use the --update option to incrementally upload or download the objects. ossutil does not automatically delete the snapshot information in the directory specified by snapshot-path. If you no longer need the snapshot information, delete the directory.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain HTTP requests and responses.

 |
|--storage-class|Specifies the storage method of an object. Valid values: Standard, IA, and Archive. Default value: Standard.|
|-i, --access-key-id|Specifies the AccessKey ID used to access OSS. This option value will overwrite the corresponding configurations in the configuration file.|
|-t, --sts-token|Optional. This option specifies the STS token used to access OSS. This option value will overwrite the corresponding configurations in the configuration file.|
|--parallel|Specifies the number of concurrent operations performed on a single object. Valid values: 1 to 10000. By default, ossutil automatically sets the value of this option based on the operation type and object size.|
|--partition-download|Specifies the partition to download. The value of this option is in the "partition number: total number of partitions" format. A value of 1:5 indicates that ossutil downloads partition 1 out of the total five partitions. Partitioning rules for objects are determined by ossutil. This option divides an object to be downloaded into multiple partitions that can be downloaded by multiple ossutil commands. Each ossutil command downloads its own partition. You can run multiple ossutil commands on different machines at the same time.|
|--bucketname|Specifies the name of a bucket. This option is used with the probe.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded. **Note:** The value of this option must be in the oss://bucket/url\_encode \(object\) format. For example, the cloud\_url oss://bucket/object must be entered as oss://bucket/url\_encode \(object\). The oss://bucket/ string does not need to be encoded.

 |
|--origin|Specifies the value of the Origin header in an HTTP request. This option value indicates the source domain of a cross-origin request.|
|--acr-method|Specifies the value of the Access-Control-Request-Method header in an HTTP request. Valid values: GET, PUT, POST, DELETE, and HEAD.|
|--acr-headers|Specifies the values of the Access-Control-Request-Headers header in an HTTP request. This option value indicates header fields, except simple ones, to be used in an actual request. To specify multiple headers, separate different headers with commas \(,\) and enclose the headers with double quotation marks \("\). Example: -- acr-headers "header1, header2, header3 ".|
|--upload-id-marker|Specifies the multipart upload ID from which you want to start listing.|
|-h, --help|Displays help information for a specified command.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 proxies are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|
|--trafic-limit|Specifies the bandwidth that is used to access the object over HTTP. Unit: bit/s. Valid values: 819200 to 838860800 \(100 KByte/s to 100 MByte/s\). Default value: 0 \(unlimited\). This option is used with the sign command.|

