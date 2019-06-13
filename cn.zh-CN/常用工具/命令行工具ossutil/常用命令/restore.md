# restore {#concept_303811 .concept}

restore命令用于恢复冷冻状态的对象（Object）为可读状态。

## 命令格式 {#section_7b1_mva_jpi .section}

``` {#codeblock_3bl_llt_ivk}
./ossutil restore cloud_url [--encoding-type url] [-r] [-f] [--output-dir=odir] [-c file]
```

## 使用示例 {#section_0yr_8af_sba .section}

-   恢复单个冷冻状态的Object为可读状态

    ``` {#codeblock_ybc_l6h_g53}
    ./ossutil restore oss://bucket/object
    ```

-   恢复指定前缀的所有状态的Object为可读状态

    ``` {#codeblock_3o9_0jp_aol}
    ./ossutil restore oss://bucket/path/ -r                         
    ```

    **说明：** 

    -   restore操作解冻过程需要1分钟时间，解冻中无法读取文件。
    -   解冻状态默认持续 1 天，对解冻状态的文件调用restore命令，会将文件的解冻状态延长1天，最多可以延长到7天，之后文件又回到初始时的冷冻状态。

## 常用选项 {#section_bn9_td1_gfo .section}

您可以在使用restore命令时附加如下选项：

|选项名称|描述|
|----|--|
|`-r，--recursive`|递归进行操作。当指定该选项时，命令会对Bucket下所有符合条件的Object进行操作，否则只对指定的单个Object进行操作。|
|`-f，--force`|强制操作，不进行询问提示。|
|`-j，--jobs`|多文件操作时的并发任务数，默认值：3，取值范围：1-10000。|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|`--output-dir=`|指定输出文件所在的目录，输出文件目前包含：cp命令批量拷贝文件出错时所产生的report文件（关于report文件更多信息，请参考cp命令帮助）。默认值为：当前目录下的ossutil\_output目录。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|`--retry-times`|当错误发生时的重试次数，默认值：10，取值范围：1-500。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

