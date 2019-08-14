# cp {#concept_303810 .concept}

cp命令用于上传、下载、拷贝文件。

## 命令格式 {#section_ctf_575_0y4 .section}

-   上传文件

    ``` {#codeblock_io1_aci_2du}
    ./ossutil cp file_url cloud_url  [-r] [-f] [-u] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--snapshot-path=sdir] [--payer requester]
    ```

-   下载文件

    ``` {#codeblock_yf8_2j4_1qv}
    ./ossutil cp cloud_url file_url  [-r] [-f] [-u] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--range=x-y] [--payer requester]
    ```

-   拷贝文件

    ``` {#codeblock_d3r_c2a_pwy}
    ./ossutil cp cloud_url cloud_url [-r] [-f] [-u] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--payer requester]
    ```


## 使用示例 {#section_2ju_iy1_c1g .section}

-   上传文件
    -   上传单个文件

        ``` {#codeblock_xl5_ao3_7z6}
        ./ossutil cp a.txt oss://bucket/path
        ```

    -   上传单个文件并指定--meta选项

        上传文件的同时可以使用--meta选项设置文件的meta信息，其内容格式为`header:value#header:value...`，如下将当前目录下文件a.txt上传并设置其meta信息：

        ``` {#codeblock_rzp_rx5_zr7}
        ./ossutil cp a.txt oss://bucket/path --meta=Cache-Control:no-cache#Content-Encoding:gzip
        ```

        **说明：** 更多关于meta设置的信息请参见[set-meta](cn.zh-CN/常用工具/命令行工具ossutil/常用命令/set-meta.md#)。

    -   上传文件夹

        使用cp命令时增加-r选项可以将目标文件夹上传到OSS。命令如下:

        ``` {#codeblock_evm_q0u_a8r}
        ./ossutil cp -r dir oss://bucket/path
        ```

        **说明：** 若上传目标对象为符号链接（软链接），且指向本地文件夹，则使用cp命令上传时，应当给软链接加上正斜线（/）。

        ``` {#codeblock_xry_xgf_63l}
        ./ossutil cp -r symbolic_link/ oss://bucket/path
        ```

    -   上传文件/文件夹并设置限速

        上传文件时，指定--maxupspeed选项，可设置上传时的最高速度，单位为KB/s，缺省为0（不限速）。

        -   上传文件并设置限速为1MByte/s

            ``` {#codeblock_dnh_jd6_7e2}
            ./ossutil cp a.jpg oss://bucket/path --maxupspeed 1024
            ```

        -   上传文件夹并设置限速为1MByte/s

            ``` {#codeblock_pa6_262_eun}
            ./ossutil cp -r dir oss://bucket/path --maxupspeed 1024
            ```

    -   批量上传符合条件的文件

        您可以使用--include/--exclude，在cp操作时批量选择对应条件的文件。

        --include/--exclude选项支持格式：

        -   \*：通配符，匹配所有字符。例如：\*.txt表示匹配所有txt格式的文件。
        -   ?：匹配单个字符，例如：abc?.jpg表示匹配所有文件名为abc+任意单个字符的jpg格式的文件，如 abc1.jpg
        -   \[sequence\]：匹配序列的任意字符，例如：abc\[1-5\].jpg表示匹配文件名为abc1.jpg~abc5.jpg的文件
        -   \[!sequence\]：匹配不在序列的任意字符，例如：abc\[!0-7\].jpg，表示匹配文件名不为abc0.jpg~abc7.jpg的文件。
        **说明：** 

        -   不支持带目录的格式，例如：`--include "/usr/test/.jpg"`。
        -   指定--include/--exclude选项时，需同时指定--recursive（-r）选项。
        -   一条规则中可以包含多个include（包含）和exclude（排除）条件，且每个文件从左到右逐一运用每个规则，直至最后才能最终确定匹配的结果。

            示例：当指定生效的文件夹中包含名为test.txt的文件，匹配不同的规则产生的结果如下。

            -   规则一：`--include "*test*" --exclude "*.txt"`，当规则匹配到`--include "*test*"`，匹配的结果为test.txt符合条件；当规则继续匹配到`--exclude "*.txt"`时，因test.txt为txt文件，所以被排除。则匹配的最终结果为test.txt不符合条件。
            -   规则二：`--exclude "*.txt" --include "*test*"`，当规则匹配到`--exclude "*.txt"`，匹配的结果为test.txt不符合条件；当规则继续匹配到`--include "*test*"`，因test.txt文件名包含test，所以符合条件。则匹配的最终结果为test.txt符合条件。
            -   规则三：`--include "*test*" --exclude "*.txt" --include "te?t.txt"`，当规则匹配到`--include "*test*"`，匹配的结果为test.txt符合条件；当规则继续匹配到`--exclude "*.txt"`时，因test.txt为txt文件，所以被排除；当规则最后匹配到`--include "te?t.txt"`时，因test.txt符合条件，所以被包含。则匹配的最终结果为test.txt符合条件。
        示例：

        -   上传所有文件格式为txt的文件：

            ``` {#codeblock_gud_nx6_5nk}
            ./ossutil cp dir/ oss://my-bucket/path --include "*.txt" -r
            ```

        -   上传所有文件名包含abc且不是jpg和txt格式的文件：

            ``` {#codeblock_jmd_jy7_i5f}
            ./ossutil cp dir/ oss://my-bucket/path --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
            ```

    -   上传文件并指定存储类型

        您可以在上传文件时，通过--meta选项修改对象存储类型。

        -   上传单个文件并指定存储类型为IA（低频访问）类型

            ``` {#codeblock_9ln_1s4_4nz}
            ./ossutil cp dir/sys.log  oss://my-bucket/path --meta X-oss-Storage-Class:IA
            ```

        -   上传文件目录并指定存储类型为IA类型：

            ``` {#codeblock_1xg_9p4_fjl}
            ./ossutil cp dir/ oss://my-bucket/path --meta X-oss-Storage-Class:IA -r
            ```

    -   上传文件并指定服务器端加密方式

        您可以在上传文件时指定文件的服务器端加密方式，将文件加密后保存在Bucket内，关于服务器端加密功能介绍请参见[服务器端加密编码](../../../../cn.zh-CN/开发指南/数据加密/服务器端加密.md#)。

        -   上传文件并指定加密方式为AES256

            ``` {#codeblock_ydq_m6o_m0q}
            ./ossutil cp a.txt oss://my-bucket/path --meta=x-oss-server-side-encryption:AES256
            ```

        -   上传文件并指定加密方式为KMS

            ``` {#codeblock_yat_ubu_zkq}
            ./ossutil cp a.txt oss://my-bucket/path --meta=x-oss-server-side-encryption:KMS
            ```

            使用KMS加密时，OSS会为这个文件在KMS平台上创建一个主密钥，会产生少量KMS密钥API调用费用，详情请参见[KMS计费标准](../../../../cn.zh-CN/产品定价/计费方式.md#section_br1_k3j_kfb)。

    -   上传文件夹并跳过已有文件

        批量上传时，若指定--update（可缩写为-u）选项，只有当目标文件不存在，或源文件的最后修改时间晚于目标文件时，ossutil才会执行上传操作。命令如下：

        ``` {#codeblock_o0a_ogd_2ux}
        ./ossutil cp -r dir oss://bucket1/path -u
        ```

        该选项可用于当批量上传失败重传时，跳过已经成功的文件，实现增量上传。

    -   上传文件夹并生成快照信息

        批量上传时，若指定--snapshot-path选项，ossutil在指定的目录下生成文件上传的快照信息，在下一次指定该选项上传时，ossutil会读取指定路径下的快照信息进行增量上传。命令如下：

        ``` {#codeblock_bdv_w7p_qke}
        ./ossutil cp -r dir oss://bucket1/path --snapshot-path=path                                
        ```

        **说明：** 

        -   --snapshot-path选项用于在某些场景下加速增量上传/下载批量文件（拷贝不支持该选项）。例如，文件数较多且两次上传期间没有其他用户更改OSS上对应的Object。
        -   --snapshot-path选项通过在本地记录成功上传/下载的文件的本地lastModifiedTime，从而在下次上传/下载时通过比较lastModifiedTime来决定是否跳过相同文件，所以在使用该选项时，请确保两次上传/下载期间没有其他用户更改了OSS上的对应Object。当不满足该场景时，如果想要增量上传/下载批量文件，请使用--update选项。
        -   ossutil不会主动删除snapshot-path下的快照信息，为了避免快照信息过多，当您确定快照信息无用时，请自行清理snapshot-path。
        -   由于读写snapshot信息需要额外开销，当要批量上传/下载的文件数比较少或网络状况比较好或有其他用户操作相同Object时，并不建议使用该选项。可以使用--update选项来增量上传/下载。
        -   --update选项和--snapshot-path选项可以同时使用，ossutil 会优先根据snapshot-path信息判断是否跳过此文件，如果不满足跳过条件，再根据--update判断是否跳过此文件。
    -   上传文件到开通了请求者付费模式的Bucket

        ``` {#codeblock_smd_e4w_ye0}
        ./ossutil cp dir/test.mp4 oss://payer/ --payer=requester
        ```

-   下载文件
    -   下载单个文件

        ``` {#codeblock_kya_rxk_w7g}
        ./ossutil cp oss://my-bucket/path/test1.txt /dir
        ```

    -   指定范围下载文件

        下载文件时，可以通过--range选项进行指定范围下载。

        示例：将test1.txt的第10到第20个字符作为一个文件载到本地。

        ``` {#codeblock_0v6_4rn_gfx}
        ./ossutil cp oss://my-bucket/path/test1.txt /dir  --range=10-20
        Succeed: Total num: 1, size: 11. OK num: 1(download 1 objects).
        0.290769(s) elapsed
        ```

    -   从开通了请求者付费模式的 Bucket 下载文件

        ``` {#codeblock_tpm_s74_3g0}
        ./ossutil cp oss://payer/test.mp4 dir/ --payer=requester
        ```

    -   下载文件夹

        ``` {#codeblock_kac_2fx_8ok}
        ./ossutil cp -r oss://my-bucket/path /dir 
        ```

    -   下载文件夹并指定--update选项

        批量下载时，若指定--update选项，只有当目标文件不存在，或源文件的最后修改时间晚于目标文件时，ossutil才会执行下载操作。命令如下：

        ``` {#codeblock_urt_ycz_ovh}
        ./ossutil cp -r oss://bucket/path  /dir  --update                           
        ```

        该选项可用于当批量下载失败重传时，跳过已经下载成功的文件，实现增量下载。

    -   下载文件夹并生成快照信息

        批量下载时，若指定--snapshot-path选项，ossutil在指定的目录下生成文件下载的快照信息，在下一次指定该选项下载时，ossutil会读取指定路径下的快照信息进行增量下载。详情请参见[--snapshot-path选项](#li_6j6_cu4_r76)。命令如下：

        ``` {#codeblock_3ky_k67_iyz}
        ./ossutil cp -r oss://bucket1/path dir --snapshot-path=path                                
        ```

    -   批量下载符合指定条件的文件

        您可以使用--include/--exclude参数，在下载时选定符合条件的文件。详情请参见[过滤选项](#li_1ff_wwf_29y)。

        -   下载所有文件格式不为jpg的文件

            ``` {#codeblock_ty3_obn_5pd}
            ./ossutil cp oss://my-bucket/path dir/ --exclude "*.jpg" -r
            ```

        -   下载所有文件名包含abc且不是jpg和txt格式的文件

            ``` {#codeblock_iv7_oel_agp}
            ./ossutil cp oss://my-bucket1/path dir/ --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
            ```

-   拷贝文件

    目前只支持拷贝文件，不支持拷贝未完成的Multipart，不支持跨region拷贝文件。

    -   拷贝单个文件

        ``` {#codeblock_v7v_vtc_bsd}
        ./ossutil cp oss://bucket/path1/a oss://bucket/path2/                                 
        ```

    -   拷贝单个文件并重命名

        ``` {#codeblock_19v_w9p_3lh}
        ./ossutil cp oss://bucket/path1/a oss://bucket/path2/b                           
        ```

    -   批量拷贝符合指定条件的文件

        您可以使用--include/--exclude，在拷贝时选定符合条件的文件。详情请参见[过滤选项](#li_1ff_wwf_29y)。

        -   拷贝所有文件格式不为jpg的文件

            ``` {#codeblock_2fo_jjt_7r8}
            ./ossutil cp oss://my-bucket1/path oss://my-bucket2/path --exclude "*.jpg" -r
            ```

        -   拷贝所有文件名包含abc且不是jpg和txt格式的文件

            ``` {#codeblock_hpf_6rc_dgk}
            ./ossutil cp oss://my-bucket1/path oss://my-bucket2/path --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
            ```

    -   拷贝文件并修改文件存储类型
        -   修改已有文件的存储类型为Archive（归档存储）类型

            ``` {#codeblock_pd4_2sc_v3j}
            ./ossutil cp oss://my-bucket/path/0104_6.jpg oss://my-bucket/path/0104_6.jpg --meta X-oss-Storage-Class:Archive
            ```

        -   修改已有文件目录下所有文件的文件类型为Standard（标准存储）类型

            ``` {#codeblock_ixj_tlj_5gu}
            ./ossutil cp oss://my-bucket/path/ oss://my-bucket/path/ --meta X-oss-Storage-Class:Standard -r
            ```

            **说明：** 

            -   存储类型为归档类型的文件不能通过cp命令直接转换成其他类型，必须先通过[restore](cn.zh-CN/常用工具/命令行工具ossutil/常用命令/restore.md#)命令将该文件解冻，之后再使用cp命令转换文件类型。
            -   使用cp覆写文件时涉及到数据覆盖操作。如果**低频访问型**或**归档型**文件分别在创建后30和60天内被覆盖，则它们会产生提前删除费用。详情请参见[计量项和计费项](../../../../cn.zh-CN/计量计费/计量项和计费项.md#table_v24_5ft_lgb)。
    -   拷贝文件并指定服务器端加密方式

        您可以在拷贝文件时指定文件的服务器端加密方式，将文件加密后保存在Bucket内，关于服务器端加密功能介绍请参见[服务器端加密编码](../../../../cn.zh-CN/开发指南/数据加密/服务器端加密.md#)。

        -   拷贝文件并指定加密方式为AES256

            ``` {#codeblock_hqm_cne_g9s}
            ./ossutil cp oss://bucket/path1/a oss://bucket/path2/ --meta=x-oss-server-side-encryption:AES256
            ```

        -   拷贝文件并指定加密方式为KMS

            ``` {#codeblock_9ed_gyy_5an}
            ./ossutil cp oss://bucket/path1/a oss://bucket/path2/ --meta=x-oss-server-side-encryption:KMS
            ```

            使用KMS加密时，OSS会为这个文件在KMS平台上创建一个主密钥，会产生少量KMS密钥API调用费用，详情请参见[KMS计费标准](../../../../cn.zh-CN/产品定价/计费方式.md#section_br1_k3j_kfb)。

    -   拷贝单个文件并指定--meta选项

        拷贝文件的同时可以使用 --meta 选项设置 Object 的 meta 信息，其内容格式为`header:value#header:value...`。

        ``` {#codeblock_lmd_r1c_msf}
        ./ossutil cp oss://bucket/path1/a oss://bucket/path2/ --meta=Cache-Control:no-cache
        ```

    -   同region不同Bucket之间的文件拷贝

        拷贝文件时，搭配 -r 选项可以实现批量文件拷贝功能。

        ``` {#codeblock_hsa_ciy_o9u}
        ./ossutil cp oss://your_src_bucket/path1/ oss://your_dest_bucket/path2/ -r                                   
        ```

    -   同region不同Bucket之间的增量文件拷贝

        批量拷贝时，若指定--update选项，只有当目标文件不存在，或源文件的最后修改时间晚于目标文件时，ossutil才会执行拷贝操作。命令如下：

        ``` {#codeblock_8ua_g9e_xlc}
        ./ossutil cp oss://your_src_bucket/path1/ oss://your_dest_bucket/path2/ -r --update
        ```

        该选项可用于当批量拷贝失败重传时，跳过已经拷贝成功的文件，实现增量拷贝。

    -   从开通了请求者付费模式的Bucket拷贝文件到普通Bucket

        ``` {#codeblock_f9p_ckp_yu7}
        ./ossutil cp oss://payer/test.mp4 oss://my-bucket/path  --payer=requester
        ```

    -   从普通Bucket拷贝数据到开通了请求者付费模式的Bucket

        ``` {#codeblock_x1t_lo7_lg9}
        ./ossutil cp oss://my-bucket/path/test.mp4 oss://payer --payer=requester
        ```

-   上传/下载/拷贝文件的性能调优

    在cp命令中，通过--jobs项和--parallel项控制并发数。当ossutil自行设置的默认并发达不到用户的性能需求时，您可以自行调整该两个选项来升降性能。

    -   使用--jobs项来控制多个文件上传/下载/拷贝时，文件间启动的并发数。
    -   使用--parallel项来控制分片上传/下载/拷贝一个大文件时，每一个大文件启动的并发数。
    默认情况下，ossutil会根据文件大小来计算parallel个数（该选项对于小文件不起作用，进行分片上传/下载/拷贝的大文件文件阈值可由`--bigfile-threshold`选项来控制）。当进行批量大文件的上传/下载/拷贝时，实际的并发数为jobs个数乘以parallel个数。

    **警告：** 

    -   通常情况下，当ECS虚拟机或者服务器在网络、内存、CPU等资源不是特别大的情况下，建议将并发数调整到100以下。如果网络、内存、CPU等资源没有占满，可以适当增加并发数。
    -   如果并发数调得太大，由于线程间资源切换及抢夺等，ossutil上传/下载/拷贝性能可能会下降。并发数过大可能会产生EOF错误。所以请根据实际的机器情况调整--jobs和--parallel选项的数值。如果要进行压测，可在一开始时调低这两项数值，然后逐渐调大直至找到最优值。

## 常用选项 {#section_m6m_5iu_wvw .section}

您可以在使用cp命令时附加如下选项：

|选项名称|描述|
|----|--|
|-r，--recursive|递归进行操作。当指定该选项时，命令会对Bucket下所有符合条件的Object进行操作，否则只对指定的单个Object进行操作。|
|-f，--force|强制操作，不进行询问提示。|
|-u，--update|只有当目标文件不存在，或源文件的最后修改时间晚于目标文件时，ossutil才会执行上传/下载/拷贝操作。|
|--output-dir|指定输出文件所在的目录，输出文件目前包含：cp命令批量拷贝文件出错时所产生的report文件。默认值为：当前目录下的ossutil\_output目录。|
|--bigfile-threshold|开启大文件断点续传的文件大小阀值，单位为Byte，默认值：100MByte，取值范围：0-9223372036854775807（Byte）。|
|--part-size|分片大小，单位为Byte。默认情况下ossutil根据文件大小自行计算合适的分片大小值。如果有特殊需求或者需要性能调优，可以设置该值，取值范围：1-9223372036854775807（Byte）。|
|--checkpoint-dir|checkpoint目录的路径（默认值为：.ossutil\_checkpoint），断点续传操作失败时，ossutil会自动创建该目录，并在该目录下记录checkpoint信息，操作成功会删除该目录。如果指定了该选项，请确保所指定的目录可以被删除。|
|--range|下载文件时，指定文件下载的范围，格式为：3-9或3-或-9。|
|--encoding-type|输入或者输出的文件名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示文件名未经过编码。Bucket名不支持url编码。|
|--include|包含对象匹配模式，如：\*.jpg。|
|--exclude|不包含对象匹配模式，如：\*.txt。|
|--meta|设置Object的meta为\[header:value\#header:value...\]，如：Cache-Control:no-cache\#Content-Encoding:gzip。更多详情请参见[set-meta](cn.zh-CN/常用工具/命令行工具ossutil/常用命令/set-meta.md#)。|
|--acl|设置Object的访问权限，默认为default。可配置项为： -   default：继承Bucket
-   private：私有
-   public-read：公共读
-   public-read-write：公共读写

 |
|--snapshot-path|批量上传/下载时，若指定--snapshot-path选项，ossutil在指定的目录下生成文件上传/下载的快照信息，在下一次指定该选项上传/下载时，ossutil会读取指定路径下的快照信息进行增量上传/下载。 **说明：** 

-   --snapshot-path选项用于在某些场景下加速增量上传/下载批量文件（拷贝不支持该选项）。例如，文件数较多且两次上传期间没有其他用户更改OSS上对应的Object。
-   --snapshot-path命令通过在本地记录成功上传/下载的文件的本地lastModifiedTime，从而在下次上传/下载时通过比较lastModifiedTime来决定是否跳过相同文件，所以在使用该选项时，请确保两次上传/下载期间没有其他用户更改了OSS上的对应Object。当不满足该场景时，如果想要增量上传/下载批量文件，请使用--update选项。
-   ossutil不会主动删除snapshot-path下的快照信息，为了避免快照信息过多，当您确定快照信息无用时，请自行清理snapshot-path。
-   由于读写snapshot信息需要额外开销，当要批量上传/下载的文件数比较少或网络状况比较好或有其他用户操作相同Object时，并不建议使用该选项。可以使用--update选项来增量上传/下载。
-   --update选项和--snapshot-path选项可以同时使用，ossutil 会优先根据snapshot-path信息判断是否跳过此文件，如果不满足跳过条件，再根据--update判断是否跳过此文件。

 |
|--disable-crc64|该选项关闭CRC64，默认情况下，ossutil进行数据传输都打开CRC64校验。|
|--maxupspeed|最大上传速度，单位：KB/s，缺省值为0（不受限制）。|
|--payer|请求的支付方式，如果为请求者付费模式，可以将该值设置成requester。|
|--partition-download|分区下载使用。一个ossutil命令下载一个分区，其值格式为“分区编号：总分区数”，例如1：5，表示当前ossutil下载分区1，总共有5个分区，分区编号从1开始，Object的分区规则由工具内部算法决定。利用该选项，将待下载的Object分成多个区，可以由多个ossutil命令同时下载，每个ossutil命令下载各自的分区，多个ossutil命令可以并行在不同机器上执行。|
|-j，--jobs|多文件操作时的并发任务数，默认值：3，取值范围：1-10000。|
|--parallel|单文件内部操作的并发任务数，取值范围：1-10000，默认将由ossutil根据操作类型和文件大小自行决定。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。

 |
|--retry-times|当错误发生时的重试次数，默认值：10，取值范围：1-500。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如https://120.79.128.211:3128、 socks5://120.79.128.211:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md#)。

