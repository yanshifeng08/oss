# STS临时授权访问OSS {#concept_xzh_nzk_2gb .concept}

OSS可以通过阿里云STS \(Security Token Service\) 进行临时授权访问。阿里云STS是为云计算用户提供临时访问令牌的Web服务。通过STS，您可以为第三方应用或子用户（即用户身份由您自己管理的用户）颁发一个自定义时效和权限的访问凭证。

**说明：** 关于STS的更多信息，请参见[RAM和STS介绍](cn.zh-CN/开发指南/隐藏/权限管理/RAM和STS介绍.md#)。

## 使用场景 {#section_f3l_11l_2gb .section}

对于您本地身份系统所管理的用户，比如您的App的用户、您的企业本地账号、第三方App的用户 ，将这部分用户称为联盟用户。此外，联盟用户还可以是您创建的能访问您的阿里云资源应用程序的用户。这些联盟用户可能需要直接访问OSS资源。

对于这部分联盟用户，通过阿里云STS服务为阿里云账号（或RAM用户）提供短期访问权限管理。您不需要透露云账号（或RAM用户）的长期密钥（如登录密码、AccessKey），只需要生成一个短期访问凭证给联盟用户使用即可。这个凭证的访问权限及有效期限都可以由您自定义。您不需要关心权限撤销问题，访问凭证过期后会自动失效。

通过STS生成的凭证包括安全令牌（SecurityToken）、临时访问密钥（AccessKeyId，AccessKeySecret）。使用AccessKey方法与您在使用阿里云账户或RAM用户AccessKey发送请求时的方法相同。需要注意的是在每个向OSS发送的请求中必须携带安全令牌。

## 实现原理 {#section_ssr_k1l_2gb .section}

以一个移动 App 举例。假设您是一个移动App开发者，打算使用阿里云OSS服务来保存App的终端用户数据，并且要保证每个App用户之间的数据隔离，防止一个App用户获取到其它App用户的数据。你可以使用STS授权用户直接访问OSS。

使用STS授权用户直接访问OSS的流程如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4347/1564046207983_zh-CN.png)

1.  App用户登录。App用户和云账号无关，它是App的终端用户，AppServer支持App用户登录。对于每个有效的App用户来说，需要AppServer能定义出每个App用户的最小访问权限。
2.  AppServer请求STS服务获取一个安全令牌（SecurityToken）。在调用STS之前，AppServer需要确定App用户的最小访问权限（用Policy语法描述）以及授权的过期时间。然后通过扮演角色（AssumeRole）来获取一个代表角色身份的安全令牌。
3.  STS返回给AppServer一个有效的访问凭证，包括一个安全令牌（SecurityToken）、临时访问密钥（AccessKeyId，AccessKeySecret）以及过期时间。
4.  AppServer将访问凭证返回给ClientApp。ClientApp可以缓存这个凭证。当凭证失效时，ClientApp需要向AppServer申请新的有效访问凭证。比如，访问凭证有效期为1小时，那么ClientApp可以每30分钟向AppServer请求更新访问凭证。
5.  ClientApp使用本地缓存的访问凭证去请求Aliyun Service API。云服务会感知STS访问凭证，并会依赖STS服务来验证访问凭证，正确响应用户请求。

## 操作步骤 {#section_t4q_4zr_ggb .section}

假设有一个名为ram-test的Bucket用于存储用户数据，现将利用用户子账号结合STS实现OSS权限控制访问。

您可以通过OSS SDK与STS SDK的结合使用，实现使用STS临时授权访问OSS实例。

1.  创建用户子账号。
    1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。
    2.  在左侧导航栏的**人员管理**菜单下，单击**用户**。
    3.  单击**新建用户**。

        **说明：** 单击**添加用户**，可一次性创建多个RAM用户。

    4.  输入**登录名称**和**显示名称**。
    5.  在**访问方式**区域下，选择**编程访问**。
    6.  单击**确定**。
    7.  勾选目标RAM用户，单击**添加权限**，被授权主体会自动填入。
    8.  在添加权限页面，为已创建子账号添加**AliyunSTSAssumeRoleAccess**权限。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/156404620735411_zh-CN.jpg)

        **说明：** 尽量不要赋予子账号其他任意权限，因为在扮演角色的时候会自动获得被扮演角色的所有\(部分\)权限。

    9.  单击**确定**。
2.  创建权限策略。
    1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。
    2.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。
    3.  单击**新建权限策略**。
    4.  填写**策略名称**和**备注**。
    5.  配置模式选择**可视化配置**或**脚本配置**。

        以脚本配置为例，对ram-test添加ListObjects与GetObject等只读权限，在**策略内容**中配置脚本如下：

        ``` {#codeblock_epa_gf1_f17}
        {
            "Version": "1",
            "Statement": [
             {
                   "Effect": "Allow",
                   "Action": [
                     "oss:ListObjects",
                     "oss:GetObject"
                   ],
                   "Resource": [
                     "acs:oss:*:*:ram-test",
                     "acs:oss:*:*:ram-test/*"
                   ]
             }
            ]
        }
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/156404620835423_zh-CN.jpg)

3.  创建角色。
    1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。
    2.  在左侧导航栏，单击**RAM角色管理**。
    3.  单击**新建RAM角色**，选择可信实体类型为**阿里云账号**，单击**下一步**。
    4.  在新建RAM角色页面，填写**RAM角色名称**和**备注**，本示例RAM角色名称为RamOssTest。
    5.  **选择云账号**为**当前云账号**。
    6.  单击**完成**。
    7.  单击**为角色授权**，被授权主体会自动填入。
    8.  在添加权限页面，选择**自定义权限策略**，添加步骤2中创建的策略Ramtest。

        添加策略后，页面如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/156404620835437_zh-CN.jpg)

        **说明：** ARN代表需要扮演角色的ID。

4.  通过STS API获取STS AK与SecurityToken。

    通过调用STS服务接口[AssumeRole](../../cn.zh-CN/API 参考（STS）/操作接口/AssumeRole.md#)来获取有效访问凭证。STS SDK的安装及使用详见[STS Java SDK安装及使用](../../cn.zh-CN/SDK参考/SDK参考（STS）/（隐藏）旧版 SDK 参考（STS）/安装.md#)。

    以STS Java SDK为例：

    ``` {#codeblock_svb_z5v_ass}
    public class StsServiceSample {
        public static void main(String[] args) {
            String endpoint = "sts.aliyuncs.com";
            String accessKeyId = "<access-key-id>";
            String accessKeySecret = "<access-key-secret>";
            String roleArn = "<role-arn>";
            String roleSessionName = "session-name";
            String policy = "{\n" +
                    "    \"Version\": \"1\", \n" +
                    "    \"Statement\": [\n" +
                    "        {\n" +
                    "            \"Action\": [\n" +
                    "                \"oss:*\"\n" +
                    "            ], \n" +
                    "            \"Resource\": [\n" +
                    "                \"acs:oss:*:*:*\" \n" +
                    "            ], \n" +
                    "            \"Effect\": \"Allow\"\n" +
                    "        }\n" +
                    "    ]\n" +
                    "}";
            try {
                // 添加endpoint（直接使用STS endpoint，前两个参数留空，无需添加region ID）
                DefaultProfile.addEndpoint("", "", "Sts", endpoint);
                // 构造default profile（参数留空，无需添加region ID）
                IClientProfile profile = DefaultProfile.getProfile("", accessKeyId, accessKeySecret);
                // 用profile构造client
                DefaultAcsClient client = new DefaultAcsClient(profile);
                final AssumeRoleRequest request = new AssumeRoleRequest();
                request.setMethod(MethodType.POST);
                request.setRoleArn(roleArn);
                request.setRoleSessionName(roleSessionName);
                request.setPolicy(policy); // 若policy为空，则用户将获得该角色下所有权限
                request.setDurationSeconds(1000L); // 设置凭证有效时间
                final AssumeRoleResponse response = client.getAcsResponse(request);
                System.out.println("Expiration: " + response.getCredentials().getExpiration());
                System.out.println("Access Key Id: " + response.getCredentials().getAccessKeyId());
                System.out.println("Access Key Secret: " + response.getCredentials().getAccessKeySecret());
                System.out.println("Security Token: " + response.getCredentials().getSecurityToken());
                System.out.println("RequestId: " + response.getRequestId());
            } catch (ClientException e) {
                System.out.println("Failed：");
                System.out.println("Error code: " + e.getErrCode());
                System.out.println("Error message: " + e.getErrMsg());
                System.out.println("RequestId: " + e.getRequestId());
            }
        }
    }
    ```

    参数说明如下：

    -   AccessKeyId、AccessKeySecret：子账号AK信息。
    -   RoleArn：需要扮演的角色ID。
    -   RoleSessionName：用来标识临时凭证的名称，建议使用不同的应用程序用户来区分。
    -   Policy：在扮演角色的时候额外添加的权限限制。

        **说明：** 这里传入的Policy是用来限制扮演角色之后的临时凭证的权限。临时凭证最后获得的权限是角色的权限和这里传入的Policy的交集。扮演角色的时候传入Policy的原因是为了灵活性，比如上传文件的时候可以根据不同的用户添加对于上传文件路径的限制等。

    -   DurationSeconds：设置临时凭证的有效期，单位是s，最小为900，最大为3600。
5.  通过STS AK与SecurityToken访问OSS。

    获取STS AK与SecurityToken后，您可以使用STS凭证构造签名请求。

    以下是Java SDK的示例，其他示例请参见[Python](../cn.zh-CN/SDK 示例/Python/授权访问.md#section_zx1_55k_kfc)、[PHP](../cn.zh-CN/SDK 示例/PHP/授权访问.md#section_m2g_jwr_kfc)、[Go](../cn.zh-CN/SDK 示例/Go/授权访问.md#section_ygd_qxw_kfc) SDK授权访问的“使用STS进行临时授权”一节。

    ``` {#codeblock_3up_1g0_m6e}
    // Endpoint以杭州为例，其它Region请按实际情况填写。
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 https://ram.console.aliyun.com 创建RAM账号。
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String securityToken = "<yourSecurityToken>";
    
    // 用户拿到STS临时凭证后，通过其中的安全令牌（SecurityToken）和临时访问密钥（AccessKeyId和AccessKeySecret）生成OSSClient。
    // 创建OSSClient实例。注意，这里使用到了上一步生成的STS凭证
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, securityToken);
    
    // OSS操作。
    
    // 关闭OSSClient。
    ossClient.shutdown();
    ```


