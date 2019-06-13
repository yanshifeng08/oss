# config {#concept_627783 .concept}

config命令用于创建配置文件来存储OSS访问信息。您可以在使用其他命令时添加`-c`选项，在访问OSS时提供访问信息。

## 命令格式 {#section_dm4_tyc_q0l .section}

``` {#codeblock_wx2_okq_0oc}
./ossutil config [-e endpoint] [-i id] [-k key] [-t token] [-L language] [--output-dir outdir] [-c file]
```

**说明：** 该命令有两种用法，交互式和非交互式。推荐用法为交互式，因为交互式用法拥有更好的安全性。

## 使用示例 {#section_v7p_yo3_c1b .section}

-   交互式配置

    ``` {#codeblock_lro_daw_alt}
    ./ossutil config
    该命令将创建一个配置文件，在其中存储配置信息。请输入配置文件路径（默认为：/home/user/.ossutilconfig，回车将使用默认路径。如果用户设置为其它路径，在使用命令时需要将--config-file选项设置为该路径）： 
    未输入配置文件路径，将使用默认配置文件：/home/user/.ossutilconfig。 
    对于下述配置，回车将跳过相关配置项的设置，配置项的具体含义，请使用"help config"命令查看。 
    请输入endpoint：http://oss-cn-shenzhen.aliyuncs.com 
    请输入accessKeyID：yourAccessKeyID 
    请输入accessKeySecret：yourAccessKeySecret
    请输入stsToken： 
    ```

    -   **endpoint**：填写Bucket所在地域的域名信息，可参考[访问域名和数据中心](../../../../cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。
    -   **accessKeyID**：查看方式请参考[创建AccessKey](../../../../cn.zh-CN/通用参考/创建AccessKey.md#)。
    -   **accessKeySecret**：查看方式请参考[创建AccessKey](../../../../cn.zh-CN/通用参考/创建AccessKey.md#)。
    -   **stsToken**：非必配项，若采用STS临时授权方式访问OSS需要配置该项，否则置空即可。stsToken生成方式参考[临时访问凭证](../../../../cn.zh-CN/开发指南/上传文件（Object）/授权给第三方上传.md#section_dvv_hkb_5db)。
-   非交互式配置

    ``` {#codeblock_15f_6m2_yo5}
    ./ossutil config -e oss-cn-beijing.aliyuncs.com -i LTAIbZcdVCmQ**** -k D26oqKBudxDRBg8Wuh2EWDBrM0****  -L CH -c /myconfig
    ```

    如果您使用命令时输入了除`--language`和`--config-file`之外的任意选项，则进入非交互式模式，所有的配置项需使用选项指定。


## 配置文件格式 {#section_6ws_r5m_bes .section}

对于已经生成的配置文件，您也可以通过直接编辑配置文件来修改OSS访问信息。ossutil工具的配置文件格式如下：

``` {#codeblock_uac_5oj_gnt}
[Credentials]
        language = CH
        endpoint = oss.aliyuncs.com
        accessKeyID = your_key_id
        accessKeySecret = your_key_secret
        stsToken = your_sts_token
        outputDir = your_output_dir
[Bucket-Endpoint]
        bucket1 = endpoint1
        bucket2 = endpoint2
        ...
[Bucket-Cname]
        bucket1 = cname1
        bucket2 = cname2
        ...
```

-   Bucket-Endpoint：对每个指定的Bucket单独配置Endpoint，当对某Bucket进行操作时，ossutil会在该选项中寻找该Bucket对应的Endpoint，如果找到，该Endpoint会覆盖Credentials选项中的endpoint。
-   Bucket-Cname：Bucket-Cname为每个指定的Bucket单独配置CNAME域名（CDN加速域名），此配置会优先于配置文件中Bucket-Endpoint及Credentials选项中endpoint的配置。关于CNAME域名的更多信息请参见[配置CNAME](../../../../cn.zh-CN/快速入门/快速入门.md#substeps_9my_37d_bc3)。

**说明：** 

-   在新版本中，ossutil取消了交互式配置中关于Bucket-Endpoint和Bucket-Cname项的配置，但配置文件中该项配置仍然起效，您可以在配置文件中对每个Bucket单独指定Endpoint或CNAME。
-   ossutil工具可以在多个地方指定Endpoint，生效优先级为：`--endpoint`\(命令选项\) \> Bucket-Cname \> Bucket-Endpoint \> endpoint\(Credentials项\) 。您在输入操作命令时，如果指定了`--endpoint`（可简写为`-e`）选项，以`--endpoint`的值为准。若未指定，则会在配置文件中依次查找Bucket-Cname、Bucket-Endpoint 、Credentials内的配置，以高优先级的Endpoint配置为准。

## 常用选项 {#section_ta0_zab_6ka .section}

您可以在使用config命令时，指定如下选项来生成指定的配置项：

|选项名称|描述|
|----|--|
|`-e，--endpoint`|设置配置文件中Credentials选项的endpoint项。|
|`-i，--access-key-id`|设置配置文件中Credentials选项的accessKeyID项。|
|`-k，--access-key-secret`|设置配置文件中Credentials选项的accessKeySecret项。|
|`-t，--sts-token`|设置配置文件中Credentials选项的stsToken项，非必填项。|
|`--output-dir`|指定输出文件所在的目录。输出文件目前包含：cp命令批量拷贝文件出错时所产生的report文件。默认值为：当前目录下的ossutil\_output目录。|
|`-L，--language`|设置ossutil工具的语言。默认值：CH，取值范围：CH、EN。若设置成CH，请确保您的系统编码为UTF-8。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/ossutil多版本文档/查看选项.md#)。

