# ossfs使用问题排查案例 {#concept_e4y_dzj_ggb .concept}

本文介绍在使用 ossfs 时遇到的一些问题案例及解决方案。

ossfs 的报错都会有明显的 message。排查问题时，需要收集这些 message，并根据 message 判断问题。例如，socket 建连失败、HTTP 响应的状态码4xx、5xx等，使用前先将 debug-log 功能开启。

-   403错误是因权限不足，导致访问被拒绝。
-   400错误是用户的操作方法有误。
-   5xx错误一般和网络抖动以及客户端业务有关系。

**说明：** 如果使用 ossfs 不满足业务需求时，可以考虑使用 ossutil。

-   ossfs 是将远端的 OSS 挂载到本地磁盘，如果对文件读写性能敏感的业务，不建议使用 ossfs 。
-   ossfs 的操作不是原子性，存在本地操作成功，但 oss 远端操作失败的风险。

如果发现 ossfs 在 ls 目录文件时很慢，可以增加调优参数，通过 `-omax_stat_cache_size=xxx` 参数增大 stat cache 的 size，修改参数之后，第一次 ls 会较慢，但是后续 ls 速度会提高。

## 案例：安装依赖库 fuse 报错![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83242/156654067136502_zh-CN.png)

 {#section_hbv_2zj_ggb .section}

问题分析：这种问题是 fuse 的版本不满足 ossfs 的要求。

解决方案：手动下载 fuse 最新版本安装，不要使用 yum 安装。详情请参考：[fuse](https://github.com/libfuse/libfuse)。

## 案例：ossfs 偶尔出现断开的情况![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83242/156654067236507_zh-CN.png)

 {#section_t1v_2zj_ggb .section}

排查步骤：

1.  开启 ossfs 的 debug 日志，加上 `-d -o f2` 参数，ossfs 会把日志写入到系统 `/var/log/message`。
2.  分析日志，发现 ossfs 在 `listbucket`、`listobject` 申请内存过多，触发了系统的 oom。

    **说明：** listobject 是发起 http 请求到 OSS 获取文件的 meta 信息，如果客户的文件很多，ls 会消耗系统大量内存来获取文件的 meta。


解决方案：

-   通过 `-omax_stat_cache_size=xxx` 参数增大 stat cache 的 size，这样第一次 ls 会较慢，但是后续的 ls 速度会提高，因为文件的元数据都在本地 cache 中。这个值默认是1000，约消耗 4MB 内存，请根据您机器内存大小调整为合适的值。
-   ossfs 在读写时会占用磁盘写大量的 temp cache ，和 nginx 差不多，可能会导致磁盘可用空间不足，需要经常清理。
-   使用 osstuil 替代 ossfs ，非线上敏感业务可以使用 ossfs ，要求可靠性、稳定性的建议使用 ossutil。

## 案例：访问出现“THE bucket you are attempting to access must be addressed using the specified endpoint”报错![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83242/156654067236503_zh-CN.png)

 {#section_x1v_2zj_ggb .section}

问题分析：根据 Message 判断，错误原因是 Endpoint 指定错误，有以下两种可能性：

-   Bucket 和 Endpoint 不匹配。
-   Bucket 所属 UID 和实际的 Accesskey 对应的 UID 不一致。

解决方案：确认正确的配置信息并修改。

## 案例：cp 时出现 “input/output error”报错![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83242/156654067236504_zh-CN.png)

 {#section_bbv_2zj_ggb .section}

问题分析：input/output error 都是捕获到系统磁盘的错误而产生的报错，可以查看出现报错时，磁盘读写是否存在高负载的情况。

解决方案：可以增加分片参数，控制文件读写。使用 ossfs -h命令可以查看分片参数。

## 案例：使用 rsync 同步时出现 “input/output error”报错![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83242/156654067236509_zh-CN.png)

 {#section_ebv_2zj_ggb .section}

问题分析：ossfs 与 rsync 同步使用本身会出现问题，此案例中，用户是对一个 141GB的大文件进行 cp 操作，使磁盘读写处于非常高的负载状态，从而产生此报错。

解决方案：如果想要将 oss 文件下载到本地 ECS ，或者本地上传到 ECS ，可以通过 ossutil 的分片上传、下载进行操作。

## 案例：上传大文件时出现“there is no enough disk space for used as cache\(or temporary\)”报错![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83242/156654067336505_zh-CN.png)

 {#section_kbv_2zj_ggb .section}

问题分析：ossfs 上传大文件时，是通过分片来上传的。分片大小默认为 10MB，分片最大数量为1000个。

ossfs 在上传文件时会写一些临时缓存文件到 /tmp 目录下，在写这些文件之前需要先判断 /tmp 目录所在的磁盘可用空间是否小于用户上传的文件总量，若判断磁盘可用空间小于用户上传文件总量，就会出现本地磁盘可用空间不足的报错。以下场景会导致磁盘可用空间不足的报错：

-   场景一：磁盘可用空间本身小于用户上传文件总量。例如，磁盘可用空间是200GB，上传的文件是300GB。
-   场景二：分片大小和上传线程数量的参数设置错误。例如：磁盘可用空间是300GB，需上传的文件是100GB。因操作错误，multipart\_size 被设置成了100GB，上传线程数量是5。此时 ossfs 判断上传的文件就是100GB\*5=500GB，超过磁盘安全空间了。

解决方案：

-   场景一：增大磁盘可用空间。
-   场景二：分片大小正常单位是 MB，最大数量是1000，不要将分片大小设置过大。

## 案例：OSS 挂载到本地，touch 文件时出现403报错![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83242/156654067336506_zh-CN.png)

 {#section_obv_2zj_ggb .section}

问题分析：403错误通常是访问权限问题导致的。以下情况会导致 touch 一个文件出现403报错。

-   该文件为归档类型文件，touch 时会出现403报错。
-   登录的 Accesskey 无该存储空间的操作权限。

解决方案：

-   归档类文件问题：将文件解冻后访问。
-   权限问题：为登录的 Accesskey 对应的账号配置正确的权限。

