# getallpartsize {#concept_303821 .concept}

getallpartsize命令用于获取存储空间（Bucket）内所有未完成上传的Multipart任务的每个分片大小以及分片总大小。

## 命令格式 {#section_wj2_5o6_mes .section}

``` {#codeblock_5ol_i2y_zpx}
./ossutil getallpartsize oss://bucket 
```

## 使用示例 {#section_35k_zhh_svf .section}

列举所有未完成上传的Mutipart Object的分片信息以及总大小：

``` {#codeblock_jhb_9de_6ep}
./ossutil getallpartsize oss://bucket1
PartNumber      UploadId                                Size(Byte)      Path
1               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
2               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
3               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
4               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
5               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt
6               F18A92392DFD4B3FA897C267829FE417        52428800        oss://bucket1/test/test1.txt

total part count:6     total part size(MB):300.00
```

## 常用选项 {#section_zcp_wm8_b3t .section}

您可以在使用getallpartsize命令时附加如下选项：

|选项名称|描述|
|----|--|
|`--encoding-type`|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

