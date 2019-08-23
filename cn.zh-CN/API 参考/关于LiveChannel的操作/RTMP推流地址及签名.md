# RTMP推流地址及签名 {#reference_tkt_5bd_xdb .reference}

本文介绍RTMP推流地址及其签名规则。

**说明：** 仅当Bucket ACL为非public-read-write时，推流地址需要签名后才可以使用。签名方法与OSS的URL签名类似。

RTMP推流地址组成规则为`rtmp://${bucket}.${host}/live/${channel}?${params}`，例如rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel。

-   `live`：RTMP协议的app名称，OSS固定使用live。
-   `params`：推流参数，格式与HTTP请求的query string相同，即形如`varA=valueA&varB=valueB`。

## RTMP推流支持的URL参数 {#section_psn_zbd_xdb .section}

RTMP推流支持的URL参数及描述如下表所示。

|名称|描述|
|:-|:-|
|playlistName|用来指定生成的m3u8文件名称，其值涵盖LiveChannel中的配置。 **说明：** 生成的m3u8文件名称仍被添加`${channel_name}/`前缀。

 |

## 推流地址的签名规则 {#section_vzf_bcd_xdb .section}

带签名的推流地址形如：`rtmp://${bucket}.${host}/live/${channel}?OSSAccessKeyId=xxx&Expires=yyy&Signature=zzz&${params}`。

推流地址的签名规则中包含的参数及描述如下表所示。

|参数名称|描述|
|:---|:-|
|OSSAccessKeyId|与OSS的HTTP签名的AccessKeyId意义相同。|
|Expires|过期时间戳，格式采用Unit timestamp。|
|Signature|签名字符串。|
|params|其他参数。 **说明：** 所有的参数都需要经过签名。

 |

Signature的计算规则如下。

``` {#codeblock_knp_b5b_e7j}
base64(hmac-sha1(AccessKeySecret,
    + Expires + "\n"
    + CanonicalizedParams
    + CanonicalizedResource))
```

Signature计算规则中涉及的参数及描述如下表所示。

|名称|描述|
|:-|:-|
|CanonicalizedResource|格式为`/BucketName/ChannelName`。|
|CanonicalizedParams|按照param key字典序将所有参数拼接`key:value\n`。 **说明：** 

-   如果参数个数为0，那此项为空。
-   参数中不包含SecurityToken、OSSAccessKeyId和Expire以及Signature。
-   每一个param key只能出现一次。

 |

