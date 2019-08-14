# bucket-policy {#concept_1614438 .concept}

bucket-policy命令用于添加、修改、查询、删除存储空间（Bucket）的Bucket policy配置。

**说明：** Bucket policy的功能介绍请参见[基于Bucket Policy的权限控制](../../../../cn.zh-CN/开发指南/权限控制/基于Bucket Policy的权限控制.md#)。

## 命令格式 {#section_h9z_aby_1m4 .section}

-   添加/修改Bucket Policy配置

    ``` {#codeblock_p26_t9n_ijd}
    ./ossutil bucket-policy --method put oss://bucket local_json_file [options]
    ```

    ossutil从配置文件local\_json\_file中读取Bucket Policy配置，并在Bucket中添加对应规则。若Bucket已有Bucket Policy配置，新的配置将覆盖原有配置。

    **说明：** local\_json\_file是一个Json格式的文件，配置举例如下：

    ``` {#codeblock_25z_jog_yba}
     {
         "Version": "1",
         "Statement": [
             {
                 "Effect": "Allow",
                 "Action": [
                     "ram:ListObjects"
                 ],
                 "Principal": [
                     "1234567"
                 ],
                 "Resource": [
                     "*"
                 ],
                 "Condition": {}
             }
         ]
    }
    ```

-   获取Bucket Policy配置

    ``` {#codeblock_dpp_rnk_zyf}
    ./ossutil bucket-policy --method get oss://bucket [local_json_file] [options]
    ```

     local\_json\_file为文件路径参数。若填写，则将Bucket Policy的配置保存为本地文件；若置空，则将Bucket Policy的配置输出到屏幕上。

-   删除Bucket Policy配置

    ``` {#codeblock_pg6_brq_ou7}
    ./ossuitl bucket-policy --method delete oss://bucket [options]
    ```


## 使用示例 {#section_pll_drx_psv .section}

-   添加Bucket Policy配置，允许指定IP的匿名用户访问Bucket内所有资源

    ``` {#codeblock_xee_aca_dbo}
    ./ossutil bucket-policy --method put oss://bucket1 /file/policy.json
    ```

     policy.json文件内容为：

    ``` {#codeblock_hkq_9n8_q96}
    {
    "Version": "1",
    "Statement": [
            {
                "Action": [
                    "oss:*"
                ],
                "Effect": "Allow",
                "Principal": [
                    "*"
               ],
                "Resource": [
                    "acs:oss:*:174649585760****:bucket1",
                    "acs:oss:*:174649585760****:bucket1/*"
                ],
                "Condition": {
                    "IpAddress": {
                        "acs:SourceIp": [
                            "10.10.10.10"
                        ]
                    }
                }
            }
        ]
    }
    ```

-   获取Bucket Policy配置

    ``` {#codeblock_x3d_oji_ecv}
    ./ossutil.exe bucket-policy --method get oss://bucket1
    {
        "Version": "1",
        "Statement": [
            {
                "Action": [
                    "oss:*"
                ],
                "Effect": "Allow",
                "Principal": [
                    "*"
                ],
                "Resource": [
                    "acs:oss:*:174649585760****:bucket1",
                    "acs:oss:*:174649585760****:bucket1/*"
                ],
                "Condition": {
                    "IpAddress": {
                        "acs:SourceIp": [
                            "10.10.10.10"
                        ]
                    }
                }
            }
        ]
    }
    ```

-   删除Bucket Policy配置

    ``` {#codeblock_zwv_019_axj}
    ./ossutil.exe bucket-policy --method delete oss://bucket1
    ```


## 常用选项 {#section_5i8_352_n8p .section}

您可以在使用bucket-policy命令时，附加如下选项：

|选项名称|描述|
|----|--|
|--method|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。

 |
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如https://120.79.128.211:3128、 socks5://120.79.128.211:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

