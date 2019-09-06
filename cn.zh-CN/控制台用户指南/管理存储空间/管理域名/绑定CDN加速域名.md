# 绑定CDN加速域名 {#concept_eyf_fvh_wfb .task}

对象存储 OSS 的文件读取支持开启阿里云 CDN 加速服务，能够将 OSS 的存储空间（Bucket）作为源站，将源内容发布到边缘节点。阿里云 CDN 配合精准的调度系统，将用户的请求分配至最适合的节点，使终端用户以最快的速度读取到所需的内容，有效解决 Internet 网络拥塞状况，提高用户访问的响应速度。

要启用 CDN 加速服务，需要将您的用户域名指向阿里云 CDN 分配的 CDN 加速域名，这样访问用户域名的请求才能转发到 CDN 节点上，达到加速效果。

**说明：** 

-   开启 CDN 加速后，通过 CDN 域名访问 OSS 的数据会产生 CDN 的费用。详情请参见[基础服务计费](../../../../cn.zh-CN/产品定价/计费方式/基础服务计费.md#)。
-   通过 CDN 加速域名访问 OSS 数据时，若 CDN 缓存中没有指定的信息，需要回源到 OSS 中读取，此时会产生 CDN 回源流量费用和请求费用。详情请参见[计量项与计费项](../../../../cn.zh-CN/计量计费/计量项和计费项.md#section_gcg_nqt_lgb)。

您可以通过以下两种方式开启阿里云 CDN 加速服务：

-   先将您的用户域名绑定到 OSS 域名，同时开启 CDN 加速。具体操作方法请参见[方式一：通过 OSS 控制台开启 CDN 加速服务](#section_6de_13t_9cr)。
-   先将 OSS 域名指向 CDN 加速域名，再将用户域名绑定到 CDN 加速域名（CNAME）。具体操作方式请参见方式[方式二：通过 CDN 控制台开启 CDN 加速服务](#section_g4l_xvs_422)。

## 操作视频 {#section_iqr_61b_xun .section}

观看以下视频快速了解如何绑定 CDN 加速域名：  

## 方式一：通过 OSS 控制台开启 CDN 加速服务 {#section_6de_13t_9cr .section}

1.  登录 [OSS 管理控制台](https://oss.console.aliyun.com/overview)。
2.  在左侧存储空间列表中，单击目标存储空间名称。
3.  单击**域名管理** \> **绑定用户域名**，打开绑定用户域名对话框。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/63857/156775429732011_zh-CN.png)

    -   用户域名：用于输入要绑定的域名名称，例如：hello-world.com，最大输入63字符。
    -   阿里云 CDN 加速：开启 [CDN 加速](../../../../cn.zh-CN/开发指南/隐藏/CDN加速OSS.md#)服务。
    -   自动添加 CNAME 记录：添加的域名是您当前阿里云账号下管理的域名，可以自动添加 CNAME 记录；非本账号下的域名，您需要在您的域名解析商处手动配置云解析，详情请参考[手动添加 CNAME 记录](#section_g9y_nes_oms)。
4.  输入您的用户域名，开启**阿里云 CDN 加速**，并开启自动添加 CNAME 记录。
5.  域名信息更新约需要一分钟时间。更新完成后，您可单击**域名绑定配置**，查看**阿里云 CDN 域名**和 **OSS 访问域名**。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/156775429732592_zh-CN.png)


## 方式二：通过 CDN 控制台开启 CDN 加速服务 {#section_g4l_xvs_422 .section}

1.  进入 [CDN 管理控制台](https://cdn.console.aliyun.com/overview)。
2.  单击**域名管理** \> **添加域名**。
3.  填写加速域名，并选择需要加速的 OSS Bucket 作为源站。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/156775429732707_zh-CN.png)

 

    |参数|说明|
    |:-|:-|
    |**加速域名**|填写您的域名，如：ch.aliyun.com。|
    |**资源分组**|选择默认资源组。|
    |**业务类型**|不同的业务类型有不同的流量分配，按照您存储的内容及使用情况选择合适的业务类型。|
    |**源站信息**|选择您需要加速的 OSS 域名。|
    |**端口**|根据您的需求选择访问端口。|
    |**加速区域**|根据您的情况选择需要加速的区域。|

4.  单击**下一步**。 成功添加加速域名后，会生成一个 CNAME 值，您还需要将这个 CNAME 值添加到域名解析内，CDN 加速服务才会生效。具体操作请参见[手动添加 CNAME 记录](#section_g9y_nes_oms)。

## 手动添加 CNAME 记录 {#section_g9y_nes_oms .section}

已自动添加 CNAME 记录请跳过此步骤。

您需要在您的域名解析商处添加域名解析，这里以阿里云的域名添加域名解析为例，配置步骤如下：

1.  登录[CDN 管理控制台](https://cdn.console.aliyun.com/overview)。
2.  单击**域名管理**，在域名列表中找到需要添加解析的域名，复制对应的 CNAME 值。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/156775429834242_zh-CN.png)


3.  登录[云解析 DNS 控制台](https://dns.console.aliyun.com/#/dns/domainList)。
4.  在域名解析列表中，单击目标域名右侧的**解析设置**。
5.  单击**添加记录**，填写域名解析信息。 

    |参数|说明|
    |:-|:-|
    |**记录类型**|选择域名指向的类型。 此次选择 CNAME。

 |
    |**主机记录**|根据域名前缀填写主机记录，例如：     -   www.aliyun.com 填写 www；
    -   aliyun.com 填写 @；
    -   abc.aliyun.com 填写 abc；
    -   所有二级域名，如 a.aliyun.com、b.aliyun.com 等，填写星号（\*）。
 |
    |**解析线路**|解析域名时使用的线路。 建议选择默认，系统自动选择最佳线路。

 |
    |**记录值**|根据记录类型填写。 此处填写[步骤 2](#step_3q4_3ci_0k1) 复制的 CNAME 值。

 |
    |**TTL**|域名的更新周期，保持默认即可。|

6.  单击**确定**。 

    **说明：** 

    -   新增 CNAME 记录实时生效，修改 CNAME 记录最多 72 小时生效。
    -   配置完 CNAME 后，由于状态更新约有 10 分钟延迟，阿里云 CDN 控制台的域名列表页可能仍提示**未配置 CNAME**，请忽略。

若您的域名未转入阿里云，需在对应的域名解析商处配置云解析，如腾讯云解析（原 DNSPod）或新网，具体操作请参见[DNSPod 配置 CNAME 流程](https://help.aliyun.com/document_detail/27145.html)或[新网配置 CNAME 流程](https://help.aliyun.com/document_detail/27146.html)。

## 验证配置是否生效 {#section_vqp_m69_fog .section}

配置 CNAME 后，不同的域名服务商 CNAME 配置生效的时间也不同。您可以 ping或 lookup 您所添加的域名，如果被转向 \*.\*kunlun\*.com，即表示 CNAME 配置已经生效。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/156775429834256_zh-CN.png)

## 验证域名所有权 {#section_7eb_4a2_ocy .section}

当您的域名被其他用户绑定时，您可以通过验证域名所有权，强制解绑此域名。

**说明：** 此步骤仅在[绑定自定义域名](#d7e90)提示域名冲突时查看。

1.  单击获取TXT，系统根据您的信息生成对应的TXT记录。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/63870/156775429832020_zh-CN.png)


2.  在您的域名解析商处添加对应的 TXT 记录。已添加到阿里云的域名，**添加记录**请参见[\#d7e139](#d7e139)。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/63870/156775429832022_zh-CN.png)

 
    -   记录类型选择：TXT
    -   主机记录：@
    -   记录值：填写绑定用户域名时生成的记录值
    -   其他设置保持默认
3.  回到绑定用户域名对话框，单击**我已添加 TXT 验证文件，继续提交**。若系统检测信息无误，验证通过。

## 开启 CDN 缓存自动刷新 {#section_6g3_2mx_jgh .section}

1.  登录 [OSS 管理控制台](https://oss.console.aliyun.com/overview)。
2.  在左侧存储空间列表中，单击目标存储空间名称。
3.  单击**域名管理**页签。
4.  在您已经绑定域名的记录上，可以看到**CDN 缓存自动刷新**的开关，打开即可。

以上操作完成后，如果 Object 有更新，OSS 会自动将更新后的 Object 刷新到 CDN 的缓存上，从而实现文件更新后实时刷新缓存的功能。

**说明：** 当您解除 Bucket 与用户域名之间的绑定关系后，OSS 控制台将不支持 CDN 缓存自动刷新的操作，但您可以前往阿里云 CDN 控制台内进行配置。具体操作请参见[CDN 刷新缓存](../../../../cn.zh-CN/常见问题/缓存FAQ/使用CDN后，文件与源文件不一致，如何刷新缓存？.md#)。

## 访问网站时报错 AccessDenied {#section_50z_uc6_x15 .section}

绑定用户域名后，您可以使用用户域名加上具体的资源路径来访问 OSS 上的资源，例如 http://mydomain.cn/test/1.jpg。如果您直接访问用户域名，如 http://mydomain.cn，则会提示错误 AccessDenied，这是因为您还未配置 OSS 静态网站的默认首页。OSS 静态网站默认首页的配置方法请参见[设置静态网站托管](../../../../cn.zh-CN/最佳实践/存储空间管理/静态网站托管.md#)。

