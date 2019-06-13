# ls {#concept_303804 .concept}

ls命令用于列举存储空间（Bucket）、对象（Object）和碎片（Part）。

## 命令格式 {#section_22j_0g5_zxr .section}

``` {#codeblock_iqe_41b_uc4}
./ossutil ls [oss://bucket[/prefix]] [-s] [-d] [--limited-num=${num}] [--marker marker] [--upload-id-marker umarker] [--payer requester] [-c file]
```

## 使用示例 {#section_qz8_3f3_3pp .section}

-   列举Bucket

    ``` {#codeblock_vy0_qp2_0xu}
    ./ossutil ls
    CreationTime                                 Region    StorageClass    BucketName
    2016-10-2116:18:37 +0800 CST       oss-cn-hangzhou         Archive    oss://bucket1
    2016-12-0115:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://bucket2
    2016-07-2010:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://bucket3
    Bucket Number is:3
    0.252174(s) elapsed                        
    ```

    **说明：** 您也可以使用`./ossutil ls oss://`命令列举Bucket。

-   分页列举Bucket

    ``` {#codeblock_6og_21x_9gk}
    ./ossutil ls oss:// --limited-num=${num} --marker=${bucketname}
    ```

    当Bucket数目太多时，可以使用`--limited-num`与`--marker`选项来分页列举Bucket。

    -   `--limited-num`用于控制分页展示条数。
    -   `--marker`用于控制分页从哪个Bucket开始列举，ossutil显示的结果从marker设定值之后按字母排序的第一个Bucket开始返回。该值一般为上一页查询显示的最后一个BucketName。
    示例：分页列举前两个Bucket。

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

-   列举指定Bucket下所有的Object

    ``` {#codeblock_tzz_thx_qmu}
    ./ossutil ls oss://bucket1
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:37 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://bucket1/a1
    2016-12-0115:06:42 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://bucket1/a2
    2016-12-0115:06:45 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://bucket1/a3
    Object Number is:3
    0.007379(s) elapsed                 
    ```

-   分页列举所有的Object

    ``` {#codeblock_9fv_q3f_jep}
    ./ossutil ls oss://bucket --limited-num=${num} --marker=${obj}
    ```

    与分页列举Bucket类似，可以使用`--limited-num`与`--marker`选项来分页列举Object。示例，列举bucket1根目录下前两个Object：

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

-   列举UploadID

    使用加`-m`选项的ls命令列举对应前缀的Object下的所有未完成的Multipart上传任务的ID。

    ``` {#codeblock_yjz_k6k_w5w}
    ./ossutil ls oss://bucket1/obj1 -m
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    UploadId Number is: 2
    0.070070(s) elapsed
    ```

-   列举所有的Object和未完成的Multipart事件

    使用加`-a`选项的ls命令列举对应前缀的Object下所有已上传完成的文件和未完成的Multipart上传任务的ID。

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

-   以精简模式显示列举结果

    ``` {#codeblock_9ds_vdu_jnu}
    ./ossutil ls oss://bucket1 -s
    oss://bucket1/a1
    oss://bucket1/a2
    oss://bucket1/a3
    Object Number is:3
    0.007379(s) elapsed  
    ```

-   模拟目录方式列举

    使用`-d`选项可以显示当前目录下的文件和子目录，而非递归显示所有子目录下的所有Object。示例：

    ``` {#codeblock_d30_6mh_t2i}
    ./ossutil ls oss://bucket1 -s -d
    oss://bucket1/obj1
    oss://bucket1/sample.txt
    oss://bucket1/dir1/
    Object and Directory Number is:3 
    0.119884(s) elapsed                   
    ```

-   列举当前目录下指定前缀的Object或文件目录

    ``` {#codeblock_ysw_i4c_3pe}
    ./ossutil ls oss://bucket1/test -d
    oss://bucket1/test.jpg
    oss://bucket1/test/
    Object and Directory Number is: 2
    ```

-   请求者付费模式下列举

    ``` {#codeblock_huc_ek2_ixl}
    ./ossutil ls oss://bucket --payer=requester
    ```


## 常用选项 {#section_u0p_bn4_loi .section}

您可以在使用ls命令时附加如下选项指定列举模式及列举的内容：

|选项名称|描述|
|----|--|
|`-s`|以精简模式列举。|
|`-d`|返回当前目录下的文件和子目录，而非递归显示所有子目录下的所有Object。|
|`-m`|指定操作的对象为bucket中未完成的Multipart事件，而非默认情况下的object。|
|`-a`|指定操作的对象为bucket中的object和未完成的Multipart事件。|
|`--limited-num`|返回结果的最大个数。与`--marker`同用可以分页列举目标。|
|`--marker`|列举Buckets时的marker，或列举objects或Multipart Uploads时的key marker。|
|`--upload-id-marker`|列举Multipart Uploads时的uploadID marker。|
|`--payer`|请求的支付方式，如果为请求者付费模式，可以将该值设置成requester。|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

