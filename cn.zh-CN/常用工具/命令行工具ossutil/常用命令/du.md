# du {#concept_1614437 .concept}

du命令用于获取指定存储空间（Bucket）或者指定文件（Object）/文件目录所占的存储空间大小。

## 命令格式 {#section_nnm_414_r24 .section}

``` {#codeblock_edw_04l_weh}
./ossutil du oss://bucket[/prefix] [--payer requester]
```

## 使用示例 {#section_l42_dar_a74 .section}

-   查询指定Bucket所占存储空间大小

    ``` {#codeblock_uzf_i2s_h2p}
    ./ossutil du oss://bucket1
    object count:9  object sum size:471075
    part count:0    part sum size:0
    total du size(byte):471075
    ```

-   查看指定文件目的所占存储空间大小

    ``` {#codeblock_tgi_opf_dc1}
    ./ossutil du oss://bucket1/test/
    object count:2  object sum size:64482
    part count:0    part sum size:0
    total du size(byte):64482
    
    0.176018(s) elapsed
    ```

-   查看指定Object所占存储空间大小

    ``` {#codeblock_wrd_vzv_79w}
    ./ossutil du oss://bucket1/test.txt
    object count:1  object sum size:27856
    part count:0    part sum size:0
    total du size(byte):27856
    
    0.241024(s) elapsed
    ```

-   查看指定前缀的所有Object所占存储空间大小

    ``` {#codeblock_4qi_fiu_oxz}
    ./ossutil du oss://bucket1/test
    object count:3  object sum size:92338
    part count:0    part sum size:0
    total du size(byte):92338
    
    0.184018(s) elapsed
    ```


## 常用选项 {#section_uu4_nwb_29o .section}

您可以在使用du命令时，附加如下选项：

|选项名称|描述|
|----|--|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|
|--payer|请求的支付方式，如果为请求者付费模式，需将该值设置为requester。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

