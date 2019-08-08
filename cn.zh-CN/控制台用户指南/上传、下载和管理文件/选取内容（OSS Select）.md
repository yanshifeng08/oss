# 选取内容（OSS Select） {#task_c2l_sc1_ffb .task}

利用 OSS Select，您可以使用简单的SQL语句从OSS的单个文件中选取内容，仅获取所需要的数据，从而减少从OSS传输的数据量，提升您获取数据的效率。

-   目前支持RFC 4180标准的CSV（包括TSV等类CSV文件，文件的行列分隔符以及Quote字符都可自定义）和JSON文件，且文件编码为UTF-8。
-   通过控制台可以对128MB以下的文件提取40MB以下的数据记录。如果您需要处理更大的文件或返回更多的记录，请使用 API：[SelectObject](../../../../cn.zh-CN/API 参考/关于Object操作/SelectObject.md#)。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。
2.  在左侧存储空间列表中，单击目标存储空间名称，打开该存储空间概览页面。
3.  单击**文件管理**页签。
4.  选择目标文件对应的**更多** \> **选取内容**。
5.  在选取内容页面设置相关参数。 
    -   文件类型：按文件实际情况选择文件的类型，可选项为：CSV和JSON。
    -   分隔符（针对CSV文件）：选择逗号（,）或自定义分隔符。
    -   标题行（针对CSV文件）：选择文件第一行是否包含列标题。
    -   JSON格式符（针对JSON文件）：选择您的JSON文件对应的格式。
    -   压缩格式：选择您当前的文件是否为压缩文件。目前压缩文件仅支持GZIP文件。
6.  单击**显示文件预览**可预览文件。 

    **说明：** 预览文件会产生Select扫描费用。

    -   标准存储类型：Select扫描费用
    -   低频访问和归档存储类型：Select扫描费用和数据取回费用。
7.  单击**下一步**，输入SQL语句并执行。 

    **说明：** 关于SQL语句的使用说明，请参见SelectObject API文档中的[常见SQL用例](../../../../cn.zh-CN/API 参考/关于Object操作/SelectObject.md#table_kh4_4z2_2fb)。

8.  查看执行结果。单击**下载**，下载所选取的内容到本地。

假如名为People的CSV文件，有3列数据，分别是姓名、公司和年龄。

-   如果想查找年龄大于50岁，并且名字以Lora开头的人（其中\_1,\_2,\_3是列索引，代表第一列、第二列、第三列），可以执行如下SQL语句：

    ``` {#codeblock_4ja_w9s_ovv}
    select * from ossobject where _1 like 'Lora*' and _3 > 50
    ```

-   如果想统计这个文件有多少行，最大年龄与最小年龄是多少，可以执行如下SQL语句：

    ``` {#codeblock_otu_psk_y7o}
    select count(*), max(cast(_3 as int)), min(cast(_3 as int)) from ossobject
    ```


