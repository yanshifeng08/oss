# probe {#concept_303814 .concept}

probe命令是针对OSS访问的检测命令，可用于排查上传、下载过程中因网络故障或基本参数设置错误导致的问题。

## 命令格式 {#section_wgg_g3z_1lt .section}

-   下载http\_url地址到本地，并输出探测报告

    ``` {#codeblock_r7c_bfk_jod}
    ./ossutil probe --download --url http_url [--addr=domain_name] [file_name]
    ```

    通过文件URL将存储空间内的一个文件下载到本地来测试网络传输质量，并输出探测报告。

    -   `--url`：填写指定Bucket内文件的URL地址。

        -   公共读文件：直接输入文件URL，例如：`https://bucketname.oss-cn-beijing.aliyuncs.com/myphoto.jpg`。
        -   私有文件：输入带签名的文件URL，并且在URL前后需加双引号（“”）。例如：`“https://bucketname.oss-cn-beijing.aliyuncs.com/myphoto.jpg?Expires=1552015472&OSSAccessKeyId=TMP.xxxxxxxx5r9f1FV12y8_Qis6LUVmvoSCUSs7aboCCHtydQ0axN32Sn-UvyY3AAAwLAIUarYNLcO87AKMEcE5O3AxxxxxxoCFAQuRdZYyVFyqOW8QkGAN-bamUiQ&Signature=bIa4llbMbldrl7rwckr%2FXXvTtxw%3D”`。
        **说明：** 如何文件URL请参考[获取URL](https://help.aliyun.com/knowledge_detail/39607.html)。

    -   `--addr=domain_name`（可选）：可在下载文件时同时向一个指定的域名或IP地址发起ping操作。不添加该选项，ossutil不进行额外的探测操作。
        -   增加`--addr=`选项，但是缺省值的话，默认向`www.aliyun.com`发起ping操作。
        -   增加`--addr=`选项，并指定一个域名或IP，则向指定的地址发起ping操作。
    -   `file_name`（可选）：为下载的文件指定存储路径。如果不输入`file_name`，则下载文件保存在当前目录下，文件名由工具自动判断；如果输入`file_name`，则`file_name`为文件名或者目录名，下载的文件名为`file_name`或者保存在`file_name`目录下。
-   下载指定Bucket中的Object，并输出探测报告

    ``` {#codeblock_slv_mrg_c3m}
    ./ossutil probe --download --bucketname bucket-name [--object=object_name] [--addr=domain_name] [file_name]
    ```

    -   `--bucketname`：填写Bucket名。
    -   `--object=`（可选）：填写指定的Object路径可下载指定的Object。例如：path/myphoto.jpg。不指定`--object`，则工具会生成一个临时文件上传到`bucket-name`后再将其下载，下载结束后会将该临时文件从本地和Bucket中删除。
-   上传Object并输出探测报告

    ``` {#codeblock_gop_cwr_l00}
    ./ossutil probe --upload [file_name] --bucketname bucket-name [--object=obj
    ect_name] [--addr=domain_name] [--upmode]
    ```

    -   `file_name`（可选）： 指定`file_name`可将指定的文件上传到`bucket-name`中；不指定`file_name`，则工具会生成一个临时文件上传到`bucket-name`中，探测结束后会将临时文件删除。
    -   `--object=`（可选）：输入文件名或文件路径，可指定Object上传后的名称，例如：path/myphoto.jpg。若不配置`--object`，则由工具自动生成上传后的Object名称，探测结束后会将该临时Object删除。
    -   `--upmode`（可选）：指定上传的方式，缺省该项则使用正常上传。可选值为：
        -   normal：正常上传
        -   append：追加上传
        -   multipart：分片上传

## 使用示例 {#section_gfs_bub_1fk .section}

-   下载http\_url地址到本地，并输出探测报告

    ``` {#codeblock_j7s_dj2_yic}
    ./ossutil probe --download --url https://bucket1.oss-cn-beijing.aliyuncs.com/myphoto.jpg --addr=www.aliyun.com /file/myphoto.jpg
    					
    ```

-   下载指定Bucket中的Object，并输出探测报告

    ``` {#codeblock_0yc_2p9_2k2}
    ./ossutil probe --download --bucketname bucket1 --object=myphoto.jpg --addr=www.aliyun.com  /file/myphoto.jpg
    ```

-   上传探测并输出探测报告

    ``` {#codeblock_0bf_g52_ne2}
    ./ossutil probe --upload /file/myphoto.jpg --bucketname bucket1 --object=myphoto.jpg  --upmode normal
    ```


## 查看探测报告 {#section_ohh_b42_eeb .section}

probe命令运行后，您可以看到任务执行的步骤及结果。

-   执行步骤后出现×表示没有通过，否则表示通过。
-   结果显示整个上传下载成功还是失败。当成功时，会给出文件的大小和上传下载时间；失败时，会给出导致错误的原因，或直接给出修改建议。

    **说明：** 并不是每次错误的检测都能提示出修改建议，对于没有提示修改建议的检测，请根据错误码提示，并结合[oss错误码ErrorCode](../../../../cn.zh-CN/SDK 示例/Java/异常处理.md#section_gn4_55c_bhb)进行问题排查。


probe命令执行完毕后，ossutil会在当前目录下生成一个以logOssProbe开头的文件，里面包含此次探测命令执行的详细信息。

## 常用选项 {#section_0eb_p3n_qk7 .section}

您可以在使用probe命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--url`|表示一个网络地址，ossutil会下载该地址。|
|`--bucketname`|OSS中Bucket的名称。|
|`--object`|OSS中Object的名称。|
|`--addr`|需要网络探测的域名，工具会对该域名进行ping等操作，默认值为`www.aliyun.com`。|
|`--upmode`|指定上传的方式，缺省该项则使用正常上传。可选值为： -   normal：正常上传
-   append：追加上传
-   multipart：分片上传

 |
|`--upload`|表示上传到OSS。|
|`--download`|表示从OSS下载。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

