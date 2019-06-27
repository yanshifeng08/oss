# 图片处理报错<Code\>NoSuchKey</Code\> {#concept_ep3_jjf_xdb .concept}

本文介绍在使用图片处理服务时出现NoSuchKey报错应该如何处理。

使用OSS的图片处理服务增加一个OSS图片处理style， 增加成功后在浏览器中输入Bucket中某Object的地址， 能直接显示， 但是地址加上stype（@aaa）后， 提示NoSuchKey 。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4855/15616257043552_zh-CN.png)

在浏览器中输入Bucket中图片的地址，图片是存在的。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4855/15616257043553_zh-CN.png)

文件存在，样式也设置好了，图中wanyan.ethnicity.cn为绑定的域名。该域名是OSS的buket属性的域名，而不是OSS图片处理的域名。OSS图片处理的域名和buket的域名是有区别的（不能相同），这里需要将源站填写为图片服务的域名，如下在图片处理服务的控制台可以看到这个域名。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4855/15616257043555_zh-CN.png)

再次访问图片正常。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4855/15616257043556_zh-CN.png)

域名解析的验证方式如下（OSS的图片处理的域名是需要带img标签的，比如xxxxx-aliyun.img-cn-hangzhou.aliyuncs.com）。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4855/15616257043557_zh-CN.png)

如果问题还未能解决，请联系[售后技术支持](https://selfservice.console.aliyun.com/ticket/createIndex.htm)。

