# Java SDK的LiveChannel常见操作 {#concept_995188 .concept}

本文介绍Java SDK的LiveChannel常见操作，如创建LiveChannel、列举LiveChannel及删除LiveChannel等。

**说明：** 

本文示例由阿里云用户bin提供。

## 创建LiveChannel {#section_u82_slj_n23 .section}

通过RTMP协议上传音视频数据前，必须先调用该接口创建一个LiveChannel。调用PutLiveChannel接口会返回RTMP推流地址，以及对应的播放地址。

**说明：** 您可以使用返回的地址进行推流、播放，您还可以根据该LiveChannel的名称来发起相关的操作，如查询推流状态、查询推流记录、禁止推流等。

以下代码用于创建LiveChannel：

``` {#codeblock_51j_cwq_44q}
public static void createLiveChannel() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录https://ram.console.aliyun.com创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String liveChannelName = "<yourLiveChannelName>";

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        CreateLiveChannelRequest request = new CreateLiveChannelRequest(bucketName,
                liveChannelName, "desc", LiveChannelStatus.Enabled, new LiveChannelTarget());
        CreateLiveChannelResult result = oss.createLiveChannel(request);

        //获取推流地址。
        List<String> publishUrls = result.getPublishUrls();
        for (String item : publishUrls) {
            System.out.println(item);
        }

        //获取播放地址。
        List<String> playUrls = result.getPlayUrls();
        for (String item : playUrls) {
            System.out.println(item);
        }

        oss.shutdown();
    }
```

创建LiveChannel的更多详情，请参考[PutLiveChannel](../cn.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannel.md#)。

## 列举LiveChannel {#section_pzl_4ja_r5n .section}

以下代码用于罗列指定的LiveChannel：

``` {#codeblock_eb8_d4r_fwe}
public static void listLiveChannels() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录https://ram.console.aliyun.com创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        ListLiveChannelsRequest request = new ListLiveChannelsRequest(bucketName);
        LiveChannelListing liveChannelListing = oss.listLiveChannels(request);
        System.out.println(JSON.toJSONString(liveChannelListing));
        oss.shutdown();
    }
```

列举LiveChannel详情，请参考[ListLiveChannel](../cn.zh-CN/API 参考/关于LiveChannel的操作/ListLiveChannel.md#)。

## 删除LiveChannel {#section_idc_4m0_g6y .section}

**说明：** 

-   当有客户端正在向LiveChannel推流时，删除请求会失败。
-   DeleteLiveChannel接口只会删除LiveChannel本身，不会删除推流生成的文件。

以下代码用于删除指定的LiveChannel：

``` {#codeblock_8zz_qzs_htl}
public static void deleteLiveChannel() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录https://ram.console.aliyun.com创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        LiveChannelGenericRequest request = new LiveChannelGenericRequest(bucketName, liveChannelName);

        try {
            oss.deleteLiveChannel(request);
        } catch (OSSException ex) {
            ex.printStackTrace();
        } catch (ClientException ex) {
            ex.printStackTrace();
        } finally {
            oss.shutdown();
        }
    }
```

删除LiveChannel详情，请参考[DeleteLiveChannel](../cn.zh-CN/API 参考/关于LiveChannel的操作/DeleteLiveChannel.md#)。

## 设置LiveChannel状态 {#section_pxw_mw0_3pr .section}

LiveChannel有enabled和disabled两种状态供您选择。

以下代码用于设置LiveChannel状态：

``` {#codeblock_dqq_use_yxg}
public static void setLiveChannelStatus() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录https://ram.console.aliyun.com创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String liveChannelName = "<yourLiveChannelName>";
        String bucketName = "<yourBucketName>";

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        try {
           oss.setLiveChannelStatus(bucketName, liveChannelName, LiveChannelStatus.Enabled);
        } catch (OSSException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } catch (ClientException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } finally {
            oss.shutdown();
        }
    }
```

设置LiveChannel状态更多详情，请参考[PutLiveChannelStatus](../cn.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannelStatus.md#)。

## 获取LiveChannel状态信息 {#section_c1r_0uf_kit .section}

以下代码用于获取指定LiveChannel的推流状态信息。

``` {#codeblock_xyb_0jz_ufe}
public static void getLiveChannelStat() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 https://ram.console.aliyun.com 创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String liveChannelName = "<yourLiveChannelName>";
        String bucketName = "<yourBucketName>";

         // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        LiveChannelStat liveChannelStat = oss.getLiveChannelStat(bucketName, liveChannelName);
        System.out.println(liveChannelStat.toString());
        oss.shutdown();
    }
```

获取LiveChannel状态信息更多详情，请参考[GetLiveChannelStat](../cn.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelStat.md#)。

## 获取LiveChannel配置信息 {#section_pso_80s_rft .section}

以下代码用于获取指定LiveChannel的配置信息：

``` {#codeblock_705_px0_x5g}
public static void getLiveChannelInfo() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录https://ram.console.aliyun.com创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String liveChannelName = "<yourLiveChannelName>";

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        LiveChannelInfo liveChannelInfo = oss.getLiveChannelInfo(bucketName, liveChannelName);
        System.out.println(JSON.toJSONString(liveChannelInfo));
        oss.shutdown();
    }
```

获取LiveChannel配置信息更多详情，请参考[GetLiveChannelInfo](../cn.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelInfo.md#)。

## 生成LiveChannel播放列表 {#section_i90_q7m_aad .section}

PostVodPlaylist接口用于为指定的LiveChannel生成一个点播用的播放列表。OSS会查询指定时间范围内由该LiveChannel推流生成的ts文件，并将其拼装为一个m3u8播放列表。

以下代码用于生成LiveChannel播放列表：

``` {#codeblock_pqp_82r_xyz}
public static void postVodPlaylist() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录https://ram.console.aliyun.com创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String liveChannelName = "<yourLiveChannelName>";
        String bucketName = "<yourBucketName>";
        String playListName = "<yourPlayListName>";

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        long startTime = getUnixTimestamp("2019-06-27 23:00:00");
        long endTIme = getUnixTimestamp("2019-06-28 22:00:00");

        try {
            oss.generateVodPlaylist(bucketName, liveChannelName, playListName, startTime, endTIme);
        } catch (OSSException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } catch (ClientException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } finally {
            oss.shutdown();
        }
    }

    private static long getUnixTimestamp(String time) {
        DateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        try {
            Date date = format.parse(time);
            return date.getTime() / 1000;
        } catch (ParseException e) {
            e.printStackTrace();
            return 0;
        }
    }
```

生成LiveChannel播放列表更多详情，请参考[PostVodPlaylist](../cn.zh-CN/API 参考/关于LiveChannel的操作/PostVodPlaylist.md#)。

## 查看LiveChannel播放列表 {#section_zb5_4dk_jx7 .section}

以下代码用于查看指定LiveChannel推流生成的、且指定时间段内的播放列表：

``` {#codeblock_uf7_rjt_4ea}
public static void getVodPlaylist() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 https://ram.console.aliyun.com 创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String liveChannelName = "<yourLiveChannelName>";
        String bucketName = "<yourBucketName>";

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        long startTime = getUnixTimestamp("2019-06-27 23:00:00");
        long endTIme = getUnixTimestamp("2019-06-28 22:00:00");

        try {
            OSSObject ossObject = oss.getVodPlaylist(bucketName, liveChannelName, startTime, endTIme);
            System.out.println(ossObject.toString());
        } catch (OSSException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } catch (ClientException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } finally {
            oss.shutdown();
        }
    }

    private static long getUnixTimestamp(String time) {
        DateFormat format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        try {
            Date date = format.parse(time);
            return date.getTime() / 1000;
        } catch (ParseException e) {
            e.printStackTrace();
            return 0;
        }
    }
```

查看LiveChannel播放列表更多详情，请参考[GetVodPlaylist](../cn.zh-CN/API 参考/关于LiveChannel的操作/GetVodPlaylist.md#)。

## 获取LiveChannel推流记录 {#section_4qu_ohd_n0k .section}

GetLiveChannelHistory接口用于获取指定LiveChannel的推流记录。使用GetLiveChannelHistory接口最多会返回指定LiveChannel最近的10次推流记录。

以下代码用于获取LiveChannel推流记录：

``` {#codeblock_4gs_g7h_db9}
public void getLiveChannelHistory() {
        String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
        // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。
        // 强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 https://ram.console.aliyun.com 创建RAM账号。
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String liveChannelName = "<yourLiveChannelName>";

        // 创建OSSClient实例。
        OSS oss = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        List<LiveRecord> list = oss.getLiveChannelHistory(bucketName, liveChannelName);
        System.out.println(JSON.toJSONString(list));
        oss.shutdown();
    }
```

获取LiveChannel推流记录详情，请参考[GetLiveChannelHistory](../cn.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelHistory.md#)。

