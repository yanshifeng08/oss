# update {#concept_627809 .concept}

update命令用于更新ossutil版本。

## 命令格式 {#section_d4j_quz_6ez .section}

``` {#codeblock_ch3_9li_xcu}
./ossutil update [-f]
```

该命令检查当前ossutil的版本与最新版本，输出两者的版本号，如果有更新版本，询问是否进行升级。如果指定了`--force`（简写为`-f`）选项，当有可用更新时不再询问，直接升级。

## 使用示例 {#section_d5h_t24_1ia .section}

升级ossutil版本：

``` {#codeblock_ih4_n80_qa6}
./ossutil update 
当前版本为：v1.5.1，最新版本为：1.6.0
确定更新版本(y or N)? y
更新成功!
```

## 常用选项 {#section_1b1_ido_j5i .section}

您可以在使用update命令时附加如下选项：

|选项名称|描述|
|----|--|
|`-f，--force`|强制操作，不进行询问提示。|
|`--retry-times`|当错误发生时的重试次数，默认值：10，取值范围：1-500。|
|`--loglevel`|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/ossutil多版本文档/查看选项.md#)。

