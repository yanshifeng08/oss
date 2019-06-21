# lifecycle {#concept_744987 .concept}

lifecycle命令用于添加、修改、查询、删除生命周期规则配置。

**说明：** 生命周期规则介绍请参见[管理文件生命周期](../../../../cn.zh-CN/开发指南/文件生命周期/管理文件生命周期.md#)。

## 命令格式 {#section_2iw_zhv_dwe .section}

-   添加/修改生命周期规则配置

    ``` {#codeblock_g9q_6fi_lv6}
    ./ossutil lifecycle --method put oss://bucket local_xml_file [options]
    ```

    ossutil从配置文件local\_xml\_file中读取生命周期规则配置，并在Bucket中添加对应规则。若添加的生命周期规则ID已经存在，则会覆盖已有ID的生命周期规则配置。

    **说明：** 配置文件是一个xml格式的文件，可以配置文件过期后转为低频访问、归档存储，或直接删除文件。您可以选择部分规则进行配置，完整配置举例如下：

    ``` {#codeblock_zg9_mu0_50r}
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>RuleID</ID>
        <Prefix>Prefix</Prefix>
        <Status>Status</Status>
        <Expiration>
          <Days>Days</Days>
        </Expiration>
        <Transition>
          <Days>Days</Days>
          <StorageClass>StorageClass</StorageClass>
        </Transition>
        <AbortMultipartUpload>
          <Days>Days</Days>
        </AbortMultipartUpload>
      </Rule>
    </LifecycleConfiguration>
    ```

    配置文件内容介绍请参见[生命周期配置示例](../../../../cn.zh-CN/开发指南/文件生命周期/生命周期配置示例.md#) 。

-   获取生命周期规则配置

    ``` {#codeblock_gy0_orp_8t2}
    ossutil lifecycle --method get oss://bucket [local_file]
    ```

    local\_file为文件路径参数。若填写，则将生命周期规则配置保存为本地文件；若置空，则将生命周期规则配置输出到屏幕上。

-   删除生命周期规则配置

    ``` {#codeblock_gxn_k9h_1qb}
    ./ossuitl lifecycle --method delete oss://bucket
    ```


## 使用示例 {#section_xo0_sqp_7f0 .section}

-   添加生命周期规则，将已上传10天，且前缀为test/的文件转为低频访问类型

    ``` {#codeblock_7am_73d_kc4}
    ./ossutil lifecycle --method put oss://bucket1  /file/lifecycle.xml
    ```

    website.xml文件内容为：

    ``` {#codeblock_y97_m4t_oy1}
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>1</ID>
        <Prefix>Prefix</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>10</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
     </Rule>
    </LifecycleConfiguration>
    ```

-   添加生命周期规则，将文件最后修改日期2019年1月1日前，且前缀为test/的文件删除

    ``` {#codeblock_84x_m7p_1yf}
    ./ossutil lifecycle --method put oss://bucket1  /file/lifecycle.xml
    ```

    website.xml文件内容为：

    ``` {#codeblock_x3u_77l_gpk}
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>RuleID</ID>
        <Prefix>test/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <CreatedBeforeDate>2019-01-01T00:00:00.000Z</CreatedBeforeDate>
        </Expiration>
     </Rule>
    </LifecycleConfiguration>
    ```

-   获取生命周期规则配置

    ``` {#codeblock_mau_wb3_tsc}
    ./ossutil lifecycle --method get oss://bucket1  /file/lifecycle.xml
    ```

-   删除生命周期规则配置

    ``` {#codeblock_bc7_d4w_ngs}
    ./ossutil lifecycle --method delete oss://bucket1  
    ```


## 常用选项 {#section_632_cpr_dve .section}

您可以在使用lifecycle命令时附加如下选项：

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

