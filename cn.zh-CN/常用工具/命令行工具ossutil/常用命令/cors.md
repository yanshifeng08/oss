# cors {#concept_303816 .concept}

cors命令用于添加、修改、查询、删除Bucket的跨域资源共享（CORS）配置。

**说明：** CORS介绍请参见[设置跨域资源共享](../../../../cn.zh-CN/开发指南/存储空间（Bucket）/设置跨域资源共享.md#)。

## 命令格式 {#section_atj_6bc_o6k .section}

-   添加/修改CORS配置

    ``` {#codeblock_wxf_gql_wyx}
    ./ossutil cors --method put oss://bucket  local_xml_file
    ```

    若Bucket未配置CORS，ossutil从配置文件local\_xml\_file中读取CORS配置，并在Bucket中添加对应规则；若Bucket已配置CORS，ossutil将Bucket的CORS配置修改为配置文件内的配置。

    **说明：** local\_xml\_file是一个xml格式的文件，举例如下：

    ``` {#codeblock_p1s_q1x_p3u}
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

    ``` {#codeblock_vut_t5s_mxo}
    ./ossutil cors --method get oss://bucket  [local_xml_file]
    ```

    local\_xml\_file为文件路径参数。若填写，则将CORS的配置保存为本地文件；若置空，则将CORS的配置输出到屏幕上。

-   删除CORS配置

    ``` {#codeblock_mm6_4la_ejt}
    ./ossutil cors --method delete oss://bucket
    ```


## 使用示例 {#section_k5l_tvs_x1h .section}

-   添加CORS配置

    ``` {#codeblock_s64_4p1_asf}
    ./ossutil cors --method put oss://bucket1  /file/cors.xml
    ```

-   获取CORS配置

    ``` {#codeblock_1de_rbo_rk1}
    ./ossutil cors --method get oss://bucket1  /file/cors.xml
    ```

-   删除CORS配置

    ``` {#codeblock_1jd_5wm_ywg}
    ./ossutil cors --method delete oss://bucket1  
    ```


## 常用选项 {#section_7xa_nl1_qmv .section}

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

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

