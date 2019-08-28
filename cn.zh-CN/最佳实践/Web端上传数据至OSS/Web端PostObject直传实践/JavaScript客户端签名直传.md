# JavaScript客户端签名直传 {#concept_frd_4gy_5db .concept}

本文主要介绍如何基于POST Policy的使用规则在客户端通过JavaScript代码完成签名，然后通过表单直传数据到OSS。

## 步骤 1：下载应用服务器代码 {#section_ugn_1ky_5db .section}

[下载地址](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-direct.zip)

本示例采用Plupload直接提交表单数据（即[PostObject](../../../../cn.zh-CN/API 参考/关于Object操作/PostObject.md#)）到OSS，可以运行于PC浏览器、手机浏览器、微信等。您可以同时选择多个文件上传，并设置上传到指定目录和设置上传文件名称是随机文件名还是本地文件名。您还可以通过进度条查看上传进度。

## 步骤 2：修改配置文件 {#section_jty_1tm_q2b .section}

将下载包解压后，修改upload.js文件：

``` {#codeblock_x5s_mom_wwt}
accessid= '<yourAccessKeyId>';
accesskey= <yourAccessKeySecrety>';
.....
new_multipart_params = {
        ....
        'OSSAccessKeyId': accessid, 
        ....
    };

//如果是STS方式====
accessid = 'STS.ACCESSKEYID';
accesskey = 'STS.ACCESSSECRET';
token = 'STS.token';

.....
new_multipart_params = {
        ....
        'OSSAccessKeyId': accessid, 
        'x-oss-security-token':token
        ....
    };
//===========
host = 'http://post-test.oss-cn-hangzhou.aliyuncs.com';
```

-   accessid：您的AccessKeyId。
-   accesskey：您的AcessKeySecret。
-   token：您的STS token。使用STS方式验证时，您需要通过STS API获取STS AccessKeyId、STS AcessKeySecret、SecurityToken，详情请参见[STS临时授权访问OSS](../../../../cn.zh-CN/开发指南/身份认证/STS临时授权访问OSS.md#)。
-   host：您的OSS访问域名，格式为`BucketName.Endpoint`，例如`post-test.oss-cn-hangzhou.aliyuncs.com`。 关于OSS访问域名的介绍请参见[OSS访问域名使用规则](../../../../cn.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md#)。

## 步骤 3：设置CORS {#section_jgc_3mk_p2b .section}

客户端进行表单直传到OSS时，会从浏览器向OSS发送带有`Origin`的请求消息。OSS对带有`Origin`头的请求消息会进行跨域规则（CORS）的验证，因此需要为Bucket设置跨域规则以支持Post方法。操作步骤请参见[设置跨域访问](../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)，配置如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/156697703812308_zh-CN.png)

**说明：** 实际使用时，为了您的数据安全，**来源**栏建议您填写自己需要的域名。

## 步骤 4：体验JavaScript客户端签名直传 {#section_ypv_ytm_q2b .section}

1.  将应用服务器代码zip包解压到Web根目录下。
2.  在Web浏览器中输入<Web应用服务器地址\>/oss-h5-upload-js-direct/index.html，例如http://abc.com:8080/oss-h5-upload-js-direct/index.html。
3.  选择一个或多个文件进行上传。
4.  上传成功后，通过控制台查看上传结果。

## 核心代码解析 {#section_vyn_czq_p2b .section}

因为OSS支持POST协议，所以只要在Plupload发送POST请求时带上OSS签名即可。核心代码如下：

``` {#codeblock_nlt_ipg_t93}
var uploader = new plupload.Uploader({
    runtimes : 'html5,flash,silverlight,html4',
    browse_button : 'selectfiles',
    //runtimes : 'flash',
    container: document.getElementById('container'),
    flash_swf_url : 'lib/plupload-2.1.2/js/Moxie.swf',
    silverlight_xap_url : 'lib/plupload-2.1.2/js/Moxie.xap',
    url : host,
    multipart_params: {
        'Filename': '${filename}',
        'key' : '${filename}',
        'policy': policyBase64,
        'OSSAccessKeyId': accessid,
        'success_action_status' : '200', //让服务端返回200，不设置则默认返回204
        'signature': signature,
        'x-oss-security-token':token
    },
　....
}
```

上述代码中，`’Filename’: ‘${filename}’`表示上传后保持原来的文件名。如果您想上传到特定目录如abc下，且文件名不变，请修改代码如下：

``` {#codeblock_kzg_4e3_ag3}
multipart_params: {
        'Filename': 'abc/' + '${filename}',
        'key' : '${filename}',
        'policy': policyBase64,
        'OSSAccessKeyId': accessid,
        'success_action_status' : '200', //让服务端返回200，不设置则默认返回204
        'signature': signature,
    },
```

-   设置成随机文件名

    如果想在上传时固定设置成随机文件名，后缀保持跟客户端文件一致，可以将函数改为：

    ``` {#codeblock_cva_u37_pox}
    function check_object_radio() {
        g_object_name_type = 'random_name';
    }
    ```

-   设置成用户的文件名

    如果想在上传时固定设置成用户的文件名，可以将函数改为：

    ``` {#codeblock_rqm_gjt_st5}
    function check_object_radio() {
        g_object_name_type = 'local_name';
    }
    ```

-   设置上传目录

    您可以将文件上传到指定目录下。下面的代码是将上传目录改成abc/，注意目录必须以正斜线（/）结尾。

    ``` {#codeblock_54y_94w_5v2}
    function get_dirname()
    {
        g_dirname = "abc/"; 
    }
    ```

-   上传签名

    上传签名主要是对policyText进行签名，最简单的例子如下：

    ``` {#codeblock_ilk_ysy_x83}
    var policyText = {
        "expiration": "2020-01-01T12:00:00.000Z", // 设置Policy的失效时间，如果超过失效时间，就无法通过此Policy上传文件
        "conditions": [
        ["content-length-range", 0, 1048576000] // 设置上传文件的大小限制，如果超过限制，文件上传到OSS会报错
        ]
    }
    ```


## 总结 {#section_gzz_dnm_q2b .section}

在客户端通过JavaScript代码完成签名，无需过多配置，即可实现直传，非常方便。但是客户端通过JavaScript把AccesssKeyID和AccessKeySecret写在代码里面有泄露的风险，建议采用[服务端签名后直传](cn.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/服务端签名后直传.md#)。

