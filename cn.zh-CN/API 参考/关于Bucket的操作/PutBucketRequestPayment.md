# PutBucketRequestPayment {#reference_1340164 .reference}

PutBucketRequestPayment接口用于设置请求者付费模式。

**说明：** 您可以指定Bucket的付费类型为BucketOwner或Requester。

-   一旦Bucket设置为Requester付费，匿名请求将被拒绝访问。
-   非Owner访问设置为Requester付费的Bucket时，需要携带请求头"x-oss-request-payer: requester"，表明请求者知晓付费策略，否则将被拒绝访问。服务器响应中将携带"x-oss-request-charged: requester"。

## 请求语法 {#section_mxt_j6z_hwk .section}

``` {#codeblock_uqr_3me_rwm}
PUT /?requestPayment HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue 
<?xml version="1.0" encoding="UTF-8"?>
<RequestPaymentConfiguration>
  <Payer>Requester</Payer>
</RequestPaymentConfiguration>
```

## 请求元素 {#section_wr6_4x0_9th .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|RequestPaymentConfiguration|容器|是|请求付费配置的容器。 子节点：Payer

 |
|Payer|字符串|是|指定Bucket付费类型。 取值：BucketOwner、Requester

 父节点：RequestPaymentConfiguration

 |

## 示例 {#section_uko_rct_vlp .section}

请求示例

``` {#codeblock_oua_n40_uzd}
PUT /?requestPayment
Content-Length: 83
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Tue, 23 Jul 2019 01:33:47 GMT
Authorization: OSS LTAIC***********:FsDgQiO+RMwLq***********
<RequestPaymentConfiguration><Payer>Requester</Payer></RequestPaymentConfiguration>
```

返回示例

``` {#codeblock_kne_n9k_q12}
200 (OK)
content-length: 0
x-oss-request-id: 5D3663FBB007B79097FC****
date: Tue, 23 Jul 2019 01:33:47 GMT
```

## SDK {#section_f2e_j6b_ihz .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../cn.zh-CN/SDK 示例/Java/请求者付费模式.md#)
-   [Python](../../../../cn.zh-CN/SDK 示例/Python/请求者付费模式.md#)
-   [Go](../../../../cn.zh-CN/SDK 示例/Go/请求者付费模式.md#)
-   [C++](../../../../cn.zh-CN/SDK 示例/C++/请求者付费模式.md#)

## 错误码 {#section_26y_dvm_ibb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|访问的Bucket不存在。|

