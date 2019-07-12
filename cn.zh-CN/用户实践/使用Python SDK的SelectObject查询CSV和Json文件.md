# 使用Python SDK的SelectObject查询CSV和Json文件 {#concept_1113040 .concept}

本文介绍如何使用Python SDK的SelectObject查询CSV和Json文件。

**说明：** 

本文示例由阿里云用户fralychen提供。

## 上传CSV或Json格式文件 {#section_2tp_9mu_6iy .section}

您可以根据业务需求，在OSS管理控制台将CSV或Json格式文件上传到OSS bucket中。如何将文件上传至OSS bucket，请参考[上传文件](../../../../cn.zh-CN/控制台用户指南/上传、下载和管理文件/上传文件.md#)。

## 调用测试 {#section_pv2_vl4_eoh .section}

通过put\_object中的key、contant参数创建并上传了一个名为python\_select的文件。

**说明：** 以下Json与CSV示例需分开执行。

``` {#codeblock_a7x_se8_mup}
import os
import oss2
from itertools import islice

# 首先初始化AccessKeyId、AccessKeySecret、Endpoint等信息。
# 通过环境变量获取，或者把诸如“<yourAccessKeyId>”替换成真实的AccessKeyId等。

# 以杭州区域为例，Endpoint可以是：
# http://oss-cn-hangzhou.aliyuncs.com
# https://oss-cn-hangzhou.aliyuncs.com

access_key_id = os.getenv('OSS_TEST_ACCESS_KEY_ID', '<yourAccessKeyId>')
access_key_secret = os.getenv('OSS_TEST_ACCESS_KEY_SECRET', '<yourAccessKeySecret>')
bucket_name = os.getenv('OSS_TEST_BUCKET', '<yourBucket>')
endpoint = os.getenv('OSS_TEST_ENDPOINT', '<yourEndpoint>')

# 创建存储空间实例，所有文件相关的方法都需要通过存储空间实例来调用。
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)


### CSV示例

key =  'python_select.csv'
# 文件内容
content =  'fralychen,love,china'*10
filename =  'python_select.csv'
# 上传一个名为python_select.csv文件
bucket.put_object(key, content)

# 遍历文件上传后的文件目录。
for b in islice(oss2.ObjectIterator(bucket),10):
  print(b.key)


# 通过select_object使用sql语法查询文件。
# def select_object(self, key, sql,
#                   progress_callback=None,
#                   select_params=None,
#                   byte_range=None
#                   )

result = bucket.select_object(key, 'select * from ossobject')
print(result.read())


### JSON示例

key = 'python_select.json'
#content = "{contacts:
#        [
#            {
#            "key1":1,
#            "key2":"love china"
#            },
#            {
#            "key1":2,
#            "key2":"fralychen"
#            }
#        ]
#    }"
content = "{\"contacts\":[{\"key1\":1,\"key2\":\"love china\"},{\"key1\":2,\"key2\":\"fralychen\"}]}"

# 上传一个名为python_select.json文件。
bucket.put_object(key,content)

result = bucket.select_object(key,'select * from ossobject')
print(result.read())
```

## 输出示例 {#section_y1u_x5n_rwc .section}

您可以在OSS控制台上查看上传后的python\_select.csv及python\_select.json文件。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/895636/156289751651302_zh-CN.png)

CSV及Json的示例输出如下：

-   CSV示例输出

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/895636/156289751651303_zh-CN.png)

-   Json示例输出

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/895636/156289751651305_zh-CN.png)


## 常见SQL语句 {#section_uku_u8v_1ze .section}

常见的SQL应用场景及对应的SQL语句如下表所示：

|应用场景|SQL语句|
|----|-----|
|返回前10行数据|select \* from ossobject limit 10|
|返回第1列和第3列的整数，并且第1列大于第3列|select \_1, \_3 from ossobject where cast\(\_1 as int\) \> cast\(\_3 as int\)|
|返回第1列以'陈'开头的记录的个数|select count\(\*\) from ossobject where \_1 like '陈%' **说明：** 此处like之后的中文需要用UTF-8编码。

 |
|返回所有第2列时间大于2018-08-09 11:30:25且第3列大于200的记录|select \* from ossobject where \_2 \> cast\('2018-08-09 11:30:25' as timestamp\) and \_3 \> 200|
|返回第2列浮点数的平均值、总和、最大值、和最小值|select AVG\(cast\(\_2 as double\)\), SUM\(cast\(\_2 as double\)\), MAX\(cast\(\_2 as double\)\), MIN\(cast\(\_2 as double\)\)|
|返回第1列和第3列连接的字符串中以'Tom'为开头以’Anderson‘结尾的所有记录|select \* from ossobject where \(\_1 || \_3\) like 'Tom%Anderson'|
|返回第1列能被3整除的所有记录|select \* from ossobject where \(\_1 % 3\) = 0|
|返回第1列大小在1995到2012之间的所有记录|select \* from ossobject where \_1 between 1995 and 2012|
|返回第5列值为N、M、G、和L的所有记录|select \* from ossobject where \_5 in \('N', 'M', 'G', 'L'\)|
|返回第2列乘以第3列比第5列大100以上的所有记录|select \* from ossobject where \_2 \* \_3 \> \_5 + 100|

## 更多参考 {#section_iw9_7u6_ls6 .section}

有关Python SDK API的更多信息，请参考[GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/oss2/api.py)。

有关SelectObject的更多信息，请参考[SelectObject](../../../../cn.zh-CN/开发指南/管理文件/SelectObject.md#)。

