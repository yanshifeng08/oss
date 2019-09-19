# 基于Ubuntu的ECS实例实现OSS反向代理 {#concept_f45_wcn_qgb .concept}

阿里云OSS的存储空间（Bucket）访问地址会随机变换，您可以通过在ECS实例上配置OSS的反向代理，实现通过固定IP地址访问OSS的存储空间。

## 背景信息 {#section_av4_kdn_qgb .section}

阿里云OSS通过Restful API方式对外提供服务。最终用户通过OSS默认域名或者绑定的自定义域名方式访问，但是在某些场景下，用户需要通过固定的IP地址访问OSS：

-   某些企业由于安全机制，需要在出口防火墙配置策略，以限制内部员工和业务系统只能访问指定的公网IP，但是OSS的Bucket访问IP会随机变换，导致需要经常修改防火墙策略。
-   金融云环境下，因金融云网络架构限制，金融云内网类型的Bucket只能在金融云内部访问，不支持在互联网上直接访问金融云内网类型Bucket。

以上问题可以通过在ECS实例上搭建反向代理的方式访问OSS。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123246/156888749038572_zh-CN.png)

## 配置步骤 {#section_o22_12n_qgb .section}

1.  创建一个和对应Bucket相同地域的Ubuntu系统的ECS实例，本文演示系统为Ubuntu 18.04 64位系统。创建过程可参考[创建ECS实例](../../../../cn.zh-CN/个人版快速入门/创建ECS实例.md#)。
2.  使用root用户登录ECS实例，并更新apt源：

    ``` {#codeblock_zbd_mi8_sk2}
    root@test:~# apt-get update
    ```

3.  安装Nginx：

    ``` {#codeblock_1jf_uk7_92g}
    root@test:~# apt-get install nginx
    ```

    **说明：** Nginx默认安装位置：

    ``` {#codeblock_fco_ctq_8ji}
     /usr/sbin/nginx       主程序 
     /etc/nginx            存放配置文件 
     /usr/share/nginx      存放静态文件 
     /var/log/nginx        存放日志
    ```

4.  打开Nginx配置文件：

    ``` {#codeblock_q0f_i3r_ajt}
    root@test:~# vi /etc/nginx/nginx.conf
    ```

5.  在config文件中的http模块添加如下内容：

    ``` {#codeblock_enm_z4e_xjy}
    server {
            listen 80;
            server_name 47.**.**.73; #对外提供反向代理服务的IP，即ECS实例的外网地址;
    
            location / {
                proxy_pass http://bucketname.oss-cn-beijing-internal.aliyuncs.com; #填写Bucket的内网访问域名，如果ECS实例与Bucket不在同一个地域，需填写外网域名;
           }
    }
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123246/156888749038579_zh-CN.png)

    **说明：** 

    -   本文为演示环境，实际环境中，为了您的数据安全，建议配置https模块，配置方法可参考[反向代理配置](https://help.aliyun.com/knowledge_detail/39544.html)。
    -   如果遇到签名错误问题，请在 nginx 的配置中添加 `proxy_set_header Host $host;`。
    -   此种配置方式只能代理一个bucket的访问。
6.  进入Nginx主程序文件夹，启动Nginx：

    ``` {#codeblock_dkp_5sg_dvn}
    root@test:~# cd /usr/sbin/
    root@test:~# ./nginx
    ```

7.  测试使用ECS外网地址加文件访问路径访问OSS资源。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/123246/156888749038580_zh-CN.png)


## 更多参考 {#section_d2t_53n_qgb .section}

 [基于CentOS的ECS实例实现OSS反向代理](cn.zh-CN/最佳实践/使用ECS实例反向代理OSS/基于CentOS的ECS实例实现OSS反向代理.md#)

