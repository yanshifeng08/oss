# GetVodPlaylist {#concept_rqq_gw2_cgb .concept}

GetVodPlaylist接口用于查看指定LiveChannel在指定时间段内推流生成的播放列表。

## 请求语法 {#section_b12_mw2_cgb .section}

``` {#codeblock_idv_c1m_lec}
GET /ChannelName?vod&endTime=EndTime&startTime=StartTime HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 请求头 {#section_e5x_4w2_cgb .section}

|名称|类型|是否必选|描述|
|--|--|----|--|
|ChannelName|字符串|是|指定LiveChannel名称。该LiveChannel必须存在。|
|StartTime|整数|是|指定查询ts文件的起始时间，格式为Unix timestamp。|
|EndTime|整数|是|指定查询ts文件的终止时间，格式为Unix timestamp。 **说明：** EndTime必须大于 StartTime，且时间跨度不能大于1天。

 |

## 示例 {#section_ewp_bxf_cgb .section}

请求示例

``` {#codeblock_hqp_5xy_k5v}
GET /test-channel?vod&endTime=1472020226&startTime=1472020031 HTTP/1.1
Date: Thu, 25 Aug 2016 07:13:26 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:ABIigvnLtCHK+7fMHLeRlOUn****
```

返回示例

``` {#codeblock_srp_d01_vv0}
HTTP/1.1 200
content-length: 312
server: AliyunOSS
connection: close
etag: "9C6104DD9CF1A0C4D0CFD21F43905D59"
x-oss-request-id: 57BE9A96B92475920B002359
date: Thu, 25 Aug 2016 07:13:26 GMT
Content-Type: application/x-mpegURL
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-TARGETDURATION:13
#EXTINF:7.120,
1543895706266.ts
#EXTINF:5.840,
1543895706323.ts
#EXTINF:6.400,
1543895706356.ts
#EXTINF:5.520,
1543895706389.ts
#EXTINF:5.240,
1543895706428.ts
#EXTINF:13.320,
1543895706468.ts
#EXTINF:5.960,
1543895706538.ts
#EXTINF:6.520,
1543895706561.ts
#EXT-X-ENDLIST
```

## SDK {#section_in6_5nh_o19 .section}

[Java](../../../../cn.zh-CN/用户实践/Java SDK 的 LiveChannel 常见操作.md#)

