# rm {#concept_303805 .concept}

The `rm` command is used to delete a bucket, object, or part.

## Command syntax {#section_1tw_5i4_1rv .section}

``` {#codeblock_wro_vje_8m2}
./ossutil rm oss://bucket[/prefix] [-r] [-b] [-f][-c file] [--include include-pattern] [--payer requester] [--exclude exclude-pattern]
```

## Examples {#section_5ha_5jq_rv3 .section}

-   Delete an empty bucket

    ``` {#codeblock_ci7_i9e_ukz}
    ./ossutil rm oss://bucket1 -b
    ```

    **Note:** 

    -   To delete a bucket, you must set the -b option.
    -   Deleted buckets can be re-created by other users, at which point you will no longer be able to use the buckets.
-   Clear bucket data and delete the bucket

    If the bucket contains data such as objects or parts, you must delete all data stored in the bucket before the bucket can be deleted. The command is as follows:

    ``` {#codeblock_078_4w3_9pb}
    ./ossutil rm oss://bucket1 -bar
    ```

    **Warning:** This command will clear all data in the bucket. Exercise caution when using this command.

-   Delete a single object

    ``` {#codeblock_d3q_rw2_3ty}
    ./ossutil rm oss://bucket1/path/object
    ```

-   Delete all objects that have a specified prefix

    You can add the -r option to the rm command to recursively delete all objects that have a specified prefix.

    ``` {#codeblock_hmc_tb8_i8g}
    ./ossutil rm oss://bucket1/path/ -r
    ```

-   Delete multiple objects that meet specified conditions

    You can set --include/--exclude to list objects that meet specified conditions. For more information about `--include/--exclude`, see [cp](reseller.en-US/Tools/ossutil/Common commands/cp.md#li_m1z_9gy_lbp).

    -   Delete all objects except jpg objects

        ``` {#codeblock_f58_1tf_2fy}
        ./ossutil rm oss://my-bucket/path  --exclude "*.jpg" -r
        ```

    -   Delete all objects that contain abc in their names and are not in the jpg or txt format

        ``` {#codeblock_v7a_pds_l3i}
        ./ossutil rm oss://my-bucket1/path  --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
        ```

-   Delete the upload IDs of incomplete multipart upload tasks initiated to a specified object

    You can add the -m option to the rm command to delete the upload IDs of incomplete multipart upload tasks initiated to a specified object.

    ``` {#codeblock_ono_v1u_k2l}
    ./ossutil rm -m oss://bucket1/obj1/test.txt
    Succeed: Total 1 uploadIds. Removed 1 uploadIds. 
    0.900715(s) elapsed
    ```

-   Delete the upload IDs of all incomplete multipart upload tasks initiated to objects that have a specified prefix

    You can add the -m and -r options to the rm command to delete the upload IDs of all incomplete multipart upload tasks initiated to objects that have a specified prefix.

    ``` {#codeblock_bm7_ns5_jwq}
    ./ossutil rm -m oss://bucket1/ob -r 
    Do you really mean to remove recursively multipart uploadIds of oss:bucket1/ob(y or N)? y 
    Succeed: Total 4 uploadIds. Removed 4 uploadIds. 
    1.922915(s) elapsed
    ```

-   Delete the upload IDs of all incomplete multipart upload tasks initiated to objects with a specified prefix and the uploaded objects with the prefix

    You can add the -a and -r options to the rm command to delete the upload IDs of all incomplete multipart upload tasks initiated to objects with a specified prefix and the uploaded objects with the prefix.

    ``` {#codeblock_ojr_3v8_d5i}
    ./ossutil rm  oss://hello-hangzws-1/obj -a -r
    Do you really mean to remove recursively objects and multipart uploadIds of oss://obj(y or N)? y
    Succeed: Total 1 objects, 3 uploadIds. Removed 1 objects, 3 uploadIds.
    ```


## Common options {#section_zwo_axv_q3i .section}

The following table describes the options you can add to the rm command to delete different content.

|Option|Description|
|------|-----------|
|-r, --recursive|Recursively performs operations on objects in a bucket. If this option is specified, commands that support this option will perform operations on all objects in a bucket that meet the specified conditions. If this option is not specified, the commands will only perform operations on a single specified object.|
|-b, --bucket|Specifies the bucket on which to perform an operation.|
|-m, --multipart|Lists only incomplete multipart upload tasks in the bucket. Objects are not displayed.|
|-a, --all-type|Specifies operations are to be performed on both the objects and incomplete multipart upload tasks in a bucket.|
|-f, --force|Forces an operation without prompting the user for confirmation.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--retry-times=|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--include|Includes objects that match a specified string, such as \*.jpg.|
|--exclude|Excludes objects that match a specified string, such as \*.txt.|
|--payer|Specifies the payer of the request. To enable the pay-by-requester mode, set this option to requester.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

