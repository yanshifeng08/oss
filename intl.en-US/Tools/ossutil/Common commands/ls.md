# ls {#concept_303804 .concept}

The ls command is used to list buckets, objects, or parts.

## Command syntax {#section_22j_0g5_zxr .section}

``` {#codeblock_iqe_41b_uc4}
./ossutil ls [oss://bucket[/prefix]] [-s] [-d] [--limited-num num] [--marker marker] [--upload-id-marker umarker] [--payer requester] [-c file] [--include include-pattern] [--exclude exclude-pattern] [--payer requester]
```

## Examples {#section_qz8_3f3_3pp .section}

-   List buckets

    ``` {#codeblock_vy0_qp2_0xu}
    ./ossutil ls
    CreationTime                                 Region    StorageClass    BucketName
    2016-10-2116:18:37 +0800 CST       oss-cn-hangzhou         Archive    oss://bucket1
    2016-12-0115:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://bucket2
    2016-07-2010:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://bucket3
    Bucket Number is:3
    0.252174(s) elapsed                        
    ```

    **Note:** You can also use the ./ossutil ls oss:// command to list buckets.

-   List buckets by page

    ``` {#codeblock_6og_21x_9gk}
    ./ossutil ls oss:// --limited-num=${num} --marker=${bucketname}
    ```

    When the number of buckets is large, you can use the --limited-num and --marker options to list buckets by page.

    -   --limited-num is used to control the number of entries displayed on each page.
    -   --marker specifies the bucket that you want to start listing from. The results displayed by ossutil are returned starting from the first bucket in alphabetical order after the marker value is set. In most cases, this parameter value is set to the last bucket name displayed on the previous page.
    The following code provides an example of how to list the first two buckets by page:

    ``` {#codeblock_amm_54a_vj1}
    ./ossutil ls oss:// --limited-num=1 -s 
    oss://bucket1
    Bucket Number is:1
    0.303869(s) elapsed
    ./ossutil ls oss:// --limited-num=1 -s --marker=bucket1
    oss://bucket2
    Bucket Number is:1
    0.257636(s) elapsed
    ```

-   List all objects in a specified bucket

    ``` {#codeblock_tzz_thx_qmu}
    ./ossutil ls oss://bucket1
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:37 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://bucket1/a1
    2016-12-0115:06:42 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://bucket1/a2
    2016-12-0115:06:45 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://bucket1/a3
    Object Number is:3
    0.007379(s) elapsed                 
    ```

-   List all objects by page

    ``` {#codeblock_9fv_q3f_jep}
    ./ossutil ls oss://bucket --limited-num=${num} --marker=${obj}
    ```

    Similar to the operation of listing buckets by page, you can use the --limited-num and --marker options to list objects by page. The following code provides an example of how to list the first two objects in the bucket1 root directory:

    ``` {#codeblock_ez7_l99_q05}
    ./ossutil ls oss://bucket1 --limited-num=1
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:37 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://bucket1/a1
    Object Number is:1
    0.007379(s) elapsed
    $./ossutil ls oss://bucket1 --limited-num=1 --marker=a1
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:42 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://bucket1/a2
    Object Number is:1
    0.008392(s) elapsed
    						
    ```

-   List objects that meet specified conditions

    You can set --include/--exclude to list objects that meet specified conditions. For more information about --include/--exclude, see [cp](reseller.en-US/Tools/ossutil/Common commands/cp.md#li_m1z_9gy_lbp).

    -   List all objects whose format is not jpg 

        ``` {#codeblock_4fk_fhu_377}
        ./ossutil ls oss://my-bucket/path  --exclude "*.jpg" -r
        ```

    -   List all objects that contain abc in their names and are not in the jpg or txt format

        ``` {#codeblock_nzw_1tf_l4p}
        ./ossutil ls oss://my-bucket1/path  --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
        ```

-   List upload IDs

    Add the -m option to the ls command to list upload IDs of all incomplete multipart upload tasks initiated to objects with a specified prefix.

    ``` {#codeblock_yjz_k6k_w5w}
    ./ossutil ls oss://bucket1/obj1 -m
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    UploadId Number is: 2
    0.070070(s) elapsed
    ```

-   List all objects and incomplete multipart upload tasks

    You can add the -a option to the ls command to list the upload IDs of all incomplete multipart upload tasks initiated to objects with a specified prefix and the uploaded objects with the prefix.

    ``` {#codeblock_r9d_ktg_l9u}
    ./ossutil ls oss://bucket1 -a 
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2015-06-0514:06:29 +0000 CST        201933      Standard   7E2F4A7F1AC9D2F0996E8332D5EA5B41        oss://bucket1/dir1/obj11
    2015-06-0514:36:21 +0000 CST        201933      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1
    2016-04-0814:50:47 +0000 CST       6476984      Standard   4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/sample.txt
    Object Number is:3
    InitiatedTime                     UploadID                           ObjectName
    2017-01-1303:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-1303:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    2017-01-1303:45:25 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://bucket1/obj2
    2017-01-2011:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://bucket1/tobj
    UploadId Number is:4
    0.191289(s) elapsed                           
    ```

-   Display the listed results in short format

    ``` {#codeblock_9ds_vdu_jnu}
    ./ossutil ls oss://bucket1 -s
    oss://bucket1/a1
    oss://bucket1/a2
    oss://bucket1/a3
    Object Number is:3
    0.007379(s) elapsed  
    ```

-   List buckets in the form of directories

    Use the -d option to list objects and subdirectories in the current directory, rather than recursively displaying all objects in all subdirectories. Example:

    ``` {#codeblock_d30_6mh_t2i}
    ./ossutil ls oss://bucket1 -s -d
    oss://bucket1/obj1
    oss://bucket1/sample.txt
    oss://bucket1/dir1/
    Object and Directory Number is:3 
    0.119884(s) elapsed                   
    ```

-   List objects or subdirectories with a specified prefix in the current directory

    ``` {#codeblock_ysw_i4c_3pe}
    ./ossutil ls oss://bucket1/test -d
    oss://bucket1/test.jpg
    oss://bucket1/test/
    Object and Directory Number is: 2
    ```

-   List buckets in the pay-by-requester mode

    ``` {#codeblock_huc_ek2_ixl}
    ./ossutil ls oss://bucket --payer=requester
    ```


## Common options {#section_u0p_bn4_loi .section}

The following table describes the options you can add to the ls command to specify the items to be listed and how they can be listed.

|Option|Description|
|------|-----------|
|-s|Lists items in short format.|
|-d|Lists objects and subdirectories in the current directory, rather than recursively displaying all objects in all subdirectories.|
|-m|Lists only incomplete multipart upload tasks in the bucket. Objects are not displayed.|
|-a|Lists both objects and incomplete multipart upload tasks in the bucket.|
|--limited-num|Specifies the maximum number of returned results. This option can be used together with the `--marker` option to list objects by page.|
|--marker|Specifies the bucket name, object name, or multipart upload ID that you want to start listing from.|
|--upload-id-marker|Specifies the multipart upload ID that you want to start listing from.|
|--payer|Specifies the payer of the request. To enable the pay-by-requester mode, set this option to requester.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--include|Includes objects that match a specified string, such as \*.jpg.|
|--exclude|Excludes objects that match a specified string, such as \*.txt.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

