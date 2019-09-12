# 教程示例：基于Bucket Policy实现跨账号访问OSS {#concept_kxn_fgs_2gb .task}

阿里云 OSS 的资源默认都是私有的，若您希望您的合作伙伴可以访问您的 OSS 资源，可以通过 Bucket Policy 授予合作伙伴访问 Bucket 的权限。

案例：公司 A 希望其合作公司 B 可以访问自己的 OSS 资源，但又不方便开放子账号给 B 公司。此时，A 公司可以通过 Bucket Policy 授予合作伙伴访问 Bucket 的权限。B 公司账号获得授权之后，可以在控制台添加 A 公司 OSS 资源的访问路径进行访问。

## 添加 Bucket Policy {#section_oz7_8s1_dex .section}

-   B 公司账号：
    1.  登录 [RAM 访问控制台](https://ram.console.aliyun.com)，创建 RAM 子账号。

        详细配置方法请参见[创建 RAM 用户](../../../../cn.zh-CN/快速入门/（隐藏）旧版快速入门/创建 RAM 用户.md#)。

    2.  在 RAM 访问控制台，单击**用户**。
    3.  单击刚刚创建的 RAM 用户的用户名，查看并记录 RAM 用户的 UID 号。
-   A 公司账号
    1.  登录阿里云[OSS 控制台](https://oss.console.aliyun.com)。
    2.  在左侧菜单栏选择您需要授权访问的 Bucket。
    3.  单击**文件管理** \> **授权** \> **新增授权**。
    4.  在**新增授权**的对话框，填写授权策略。其中，**授权用户**选择**其他账号**，并填写 B 公司子账号的 UID 号。其他参数请参考[Bucket Policy](../../../../cn.zh-CN/控制台用户指南/上传、下载和管理文件/使用Bucket Policy授权其他用户访问OSS资源.md#section_nbp_by4_j2b)。
    5.  单击**确定**。

## 登录 RAM 子账号并添加访问路径 {#section_7zz_3d0_lym .section}

Bucket Policy 添加完成之后，您还需要登录 B 公司子账号，添加 A 公司的 Bucket 访问路径。配置步骤如下：

1.  通过 [RAM 账号登录链接](http://signin.aliyun.com)登录 B 公司子账号。
2.  打开[OSS 控制台](https://oss.console.aliyun.com)。
3.  单击左侧菜单栏**我的访问路径**后的加号（+），添加 A 公司授权访问的 Bucket 路径信息。 
    -   **区域**：下拉选择 A 公司允许访问的 Bucket 所在地域。
    -   **访问路径**：添加 A 公司允许访问的 Bucket 访问路径，格式为「bucket/object-prefix」。例如，仅允许访问名为 aliyun 的 Bucket 下的 abc 文件夹，则添加 aliyun/abc。

您也可以[创建子账号的 AccessKey](../../../../cn.zh-CN/通用参考/创建AccessKey.md#)，并通过 AccessKey 使用 [ossutil](../../../../cn.zh-CN/常用工具/命令行工具ossutil/概述.md#)、[ossbrowser](../../../../cn.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md#) 等工具访问被授权的 Bucket。

## 参考文档 {#section_nif_8jz_0e5 .section}

[教程示例：基于 RAM 角色实现跨账号访问 OSS](https://help.aliyun.com/document_detail/69011.html)

