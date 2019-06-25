# hash {#concept_627791 .concept}

 hash命令用于计算本地文件的CRC64或MD5。

## 命令格式 {#section_xzh_f9y_nd0 .section}

``` {#codeblock_6ul_8qp_cgu}
./ossutil hash file_url [--type=hashtype]
```

 `--type`选项用来控制计算的类型，可选值为crc64和md5，默认为crc64。

**说明：** 

-   OSS文件的CRC64和Content-MD5值可通过[stat](cn.zh-CN/ossutil多版本文档/常用命令/stat.md#)命令查看到，参考`X-Oss-Hash-Crc64ecma`字段和`Content-Md5`字段。
-   若文件在OSS支持CRC64功能之前上传，则无法使用stat命令查看CRC64值。
-   对于追加上传和分片上传类型的文件，无法使用stat命令查看Content-MD5值。
-   计算类型为md5时，会同时输出文件的MD5以及Content-MD5值。Content-MD5值其实是先计算5值获得128比特位数字，然后对该数字进行base64编码得到的值。
-   CRC64的计算标准参考[ECMA-182标准](http://www.ecma-international.org/publications/standards/Ecma-182.htm)。
-   关于Content-MD5的更多信息， 请参考[RFC1864](https://tools.ietf.org/html/rfc1864)。

## 使用示例 {#section_i8e_frm_um5 .section}

-   计算本地文件的CRC64

    ``` {#codeblock_yaf_iht_2gp}
    ./ossutil hash test.txt --type=crc64
    CRC64-ECMA                  : 295992936743767023
    ```

-   计算本地文件的md5

    ``` {#codeblock_d3i_4ap_jf9}
    ./ossutil hash test.txt --type=md5
     MD5                         : 01C3C45C03B2AF225EFAD9F911A33D73
     Content-MD5                 : AcPEXAOyryJe+tn5EaM9cw==
    ```


## 常用选项 {#section_usf_cqy_9ae .section}

您可以在使用hash命令时附加如下选项：

|选项名称|描述|
|----|--|
| `--type` |计算的类型，默认值：crc64，取值范围: crc64、md5。|
| `--loglevel` |设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/ossutil多版本文档/查看选项.md#)。

