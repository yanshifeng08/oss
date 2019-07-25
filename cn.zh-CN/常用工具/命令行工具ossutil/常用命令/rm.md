# rm {#concept_303805 .concept}

`rm`命令用于删除存储空间（Bucket）、对象（Object）或碎片（Part）。

## 命令格式 {#section_1tw_5i4_1rv .section}

``` {#codeblock_wro_vje_8m2}
./ossutil rm oss://bucket[/prefix] [-r] [-b] [-f]undefined[-c file] [--include include-pattern] [--exclude exclude-pattern]
```

## 使用示例 {#section_5ha_5jq_rv3 .section}

-   删除空Bucket

    ``` {#codeblock_ci7_i9e_ukz}
    ./ossutil rm oss://bucket1 -b
    ```

    **说明：** 

    -   删除Bucket必须设置`-b`选项。
    -   被删除的Bucket可能被其他用户重新创建，您不再拥有该Bucket。
-   清除Bucket数据并删除Bucket

    如果Bucket中有Object或Multipart等数据，需要先删除所有数据再删除Bucket。命令如下：

    ``` {#codeblock_078_4w3_9pb}
    ./ossutil rm oss://bucket1 -bar
    ```

    **警告：** 该命令将清除Bucket中所有数据，属于危险操作，请谨慎使用。

-   删除单个Object

    ``` {#codeblock_d3q_rw2_3ty}
    ./ossutil rm oss://bucket1/path/object
    ```

-   删除指定前缀的所有Object

    配合`-r`选项，可以递归执行删除操作，删除指定前缀的所有Object。

    ``` {#codeblock_hmc_tb8_i8g}
    ./ossutil rm oss://bucket1/path/ -r
    ```

-   批量删除符合条件的文件

    您可以使用`--include/--exclude`参数，批量删除符合条件的Object。`--include/--exclude`参数详情请参见[过滤选项](#li_1ff_wwf_29y)。

    -   删除所有文件格式不为jpg的Object

        ``` {#codeblock_f58_1tf_2fy}
        ./ossutil rm oss://my-bucket/path  --exclude "*.jpg" -r
        ```

    -   删除所有文件名包含abc且不是jpg和txt格式的Object

        ``` {#codeblock_v7a_pds_l3i}
        ./ossutil rm oss://my-bucket1/path  --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
        ```

-   删除指定的Object下未完成的Multipart上传任务的ID

    使用加-m选项的rm命令来删除指定未完成的Multipart上传任务的ID。

    ``` {#codeblock_ono_v1u_k2l}
    ./ossutil rm -m oss://bucket1/obj1/test.txt
    Succeed: Total 1 uploadIds. Removed 1 uploadIds. 
    0.900715(s) elapsed
    ```

-   删除对应前缀的所有未完成的Multipart上传任务的ID

    使用加-m和-r选项的rm命令来删除对应前缀的所有未完成的Multipart上传任务的ID。

    ``` {#codeblock_bm7_ns5_jwq}
    ./ossutil rm -m oss://bucket1/ob -r 
    Do you really mean to remove recursively multipart uploadIds of oss:bucket1/ob(y or N)? y 
    Succeed: Total 4 uploadIds. Removed 4 uploadIds. 
    1.922915(s) elapsed
    ```

-   删除对应前缀的所有已上传完成的Object和未完成的Multipart上传任务的ID

    使用加-a和-r选项的rm命令来删除对应前缀的所有已上传完成的Object和未完成的Multipart上传任务的ID。

    ``` {#codeblock_ojr_3v8_d5i}
    ./ossutil rm  oss://hello-hangzws-1/obj -a -r
    Do you really mean to remove recursively objects and multipart uploadIds of oss://obj(y or N)? y
    Succeed: Total 1 objects, 3 uploadIds. Removed 1 objects, 3 uploadIds.
    ```


## 常用选项 {#section_zwo_axv_q3i .section}

使用rm命令加不同的选项可以指定删除不同的内容，常用选项如下：

|参数名称|描述|
|----|--|
|`-r，--recursive`|递归进行操作。当指定该选项时，命令会对Bucket下所有符合条件的Object进行操作，否则只对指定的单个Object进行操作。|
|`-b，--bucket`|对Bucket进行操作，该选项用于确认操作作用于Bucket。|
|`-m，--multipart`|指定操作的对象为Bucket中未完成的Multipart事件，而非默认情况下的Object。|
|`-a，--all-type`|指定操作的对象为Bucket中的Object和未完成的Multipart事件。|
|`-f，--force`|强制操作，不进行询问提示。|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|`--retry-times=`|当错误发生时的重试次数，默认值：10，取值范围：1-500。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志\(包括http请求和响应信息\)。

 |
|`--include`|包含对象匹配模式，如：\*.jpg。|
|`--exclude`|不包含对象匹配模式，如：\*.txt。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

