# cors {#concept_627786 .concept}

cors命令用于添加、修改、查询、删除Bucket的CORS配置。

**说明：** CORS介绍请参见[设置跨域资源共享](../../../../intl.zh-CN/开发指南/存储空间（Bucket）/设置跨域资源共享.md#)。

## 命令格式 {#section_wfn_ib1_1gy .section}

-   添加/修改CORS配置

    ``` {#codeblock_vgd_6an_mm9}
    ./ossutil cors --method put oss://bucket  local_xml_file
    ```

    若Bucket未配置CORS，ossutil从配置文件local\_xml\_file中读取CORS配置，并在Bucket中添加对应规则；若Bucket已配置CORS，ossutil将Bucket的CORS配置修改为配置文件内的配置。

    **说明：** local\_xml\_file是一个xml格式的文件，举例如下：

    ``` {#codeblock_4pc_xiz_cop}
    <?xml version="1.0" encoding="UTF-8"?>
       <CORSConfiguration>
         <CORSRule>
             <AllowedOrigin>www.aliyun.com</AllowedOrigin>
             <AllowedMethod>PUT</AllowedMethod>
             <MaxAgeSeconds>10000</MaxAgeSeconds>
         </CORSRule>
     </CORSConfiguration>
    ```

-   获取CORS配置

    ``` {#codeblock_3hf_4iq_2yt}
    ./ossutil cors --method get oss://bucket  [local_xml_file]
    ```

    local\_xml\_file为文件路径参数。若填写，则将CORS的配置保存为本地文件；若置空，则将CORS配置的输出到屏幕上。

-   删除CORS配置

    ``` {#codeblock_185_dk5_tzg}
    ./ossutil cors --method delete oss://bucket
    ```


## 使用示例 {#section_xls_k28_p0a .section}

-   添加CORS配置

    ``` {#codeblock_5jn_d2v_396}
    ./ossutil cors --method put oss://bucket1  /file/cors.xml
    ```

-   获取CORS配置

    ``` {#codeblock_2au_72i_gzu}
    ./ossutil cors --method get oss://bucket1  /file/cors.xml
    ```

-   删除CORS配置

    ``` {#codeblock_clx_g8h_u49}
    ./ossutil cors --method delete oss://bucket1  
    ```


## 常用选项 {#section_n9g_kca_ohl .section}

您可以在使用cors命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--method`|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。

 |
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](intl.zh-CN/ossutil多版本文档/查看选项.md#)。

