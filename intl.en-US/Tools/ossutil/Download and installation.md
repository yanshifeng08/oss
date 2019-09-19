# Download and installation {#concept_303829 .concept}

This topic describes how to download and install ossutil.

## Version and runtime environment {#section_g0u_8gm_953 .section}

-   Current version: 1.6.7
-   Runtime environment
    -   Windows/Linux/macOS
    -   Supported architectures: x86 \(32-bit and 64-bit\)

## Download URLs {#section_xla_1lk_s3b .section}

-   [Linux x86 32-bit](http://gosspublic.alicdn.com/ossutil/1.6.7/ossutil32)
-   [Linux x86 64-bit](http://gosspublic.alicdn.com/ossutil/1.6.7/ossutil64) 

    **Note:** When copying the preceding URLs into the wget command to download ossutil, delete the ? spm=xxxx section from the URLs.

-   [Windows x86 32-bit](http://gosspublic.alicdn.com/ossutil/1.6.7/ossutil32.zip)
-   [Windows x86 64-bit](http://gosspublic.alicdn.com/ossutil/1.6.7/ossutil64.zip)
-   [macOS x86 32-bit](http://gosspublic.alicdn.com/ossutil/1.6.7/ossutilmac32)
-   [macOS x86 64-bit](http://gosspublic.alicdn.com/ossutil/1.6.7/ossutilmac64)

## Quick installation {#section_j39_lrf_j5i .section}

Download the package based on your operating system and run the corresponding binary file.

-   Install ossutil in Linux \(64-bit Linux system is used as an example\)
    1.  Download the ossutil installation package:

        ``` {#codeblock_0ds_9yl_7rs}
        wget http://gosspublic.alicdn.com/ossutil/1.6.7/ossutil64                           
        ```

    2.  Modify the file execution permissions:

        ``` {#codeblock_u78_mby_4le}
        chmod 755 ossutil64
        ```

    3.  Generate a configuration file by following the interactive processing:

        ``` {#codeblock_yu9_f5g_3hf}
        ./ossutil64 config
        This command generates a configuration file to store configuration information. Enter the path of the configuration file. The default path is /home/user/.ossutilconfig. If you press Enter without specifying a path, the file is generated in the default path. If you want to generate the file in another path, set the --config-file option to the path. 
        If the path of the configuration file is not specified, the default path /home/user/.ossutilconfig is used. 
        The following parameters are ignored if you press Enter without configuring them. For more information about the parameters, run the help config command. 
        Enter the endpoint: http://oss-cn-shenzhen.aliyuncs.com 
        Enter the AccessKey ID: your AccessKey ID 
        Enter the AccessKey Secret: your AccessKey Secret
        Enter the STS token: 
        ```

        -   endpoint: specifies the domain name of the region to which the bucket belongs. For more information, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
        -   accessKeyID: For more information about how to view the AccessKey ID, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
        -   accessKeySecret: For more information about how to view the AccessKey secret, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
        -   stsToken: This option is required only when you use a temporary STS token to access the OSS bucket. Otherwise, you can leave this parameter unspecified. For more information about how to generate an STS token, see [Temporary access credential](../../../../reseller.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md#section_dvv_hkb_5db).
        **Note:** For more information about the configuration file, see [config](reseller.en-US/Tools/ossutil/Common commands/config.md#).

-   Install ossutil in Windows \(64-bit Windows system is used as an example\)
    1.  Download the ossutil installation package.
    2.  Decompress the package to a specified folder, and then double-click the ossutil.bat file.
    3.  Generate the configuration file. For more information about the parameters, see the configuration parameters described in the preceding Linux section.

        ``` {#codeblock_ji1_30j_54e}
        D:\ossutil>ossutil64.exe config
        ```

-   Install ossutil in macOS \(64-bit macOS system is used as an example\)
    1.  Download the ossutil installation package.

        ``` {#codeblock_zex_iy6_ta5}
        curl -o ossutilmac64 http://gosspublic.alicdn.com/ossutil/1.6.7/ossutilmac64
        ```

    2.  Modify the file execution permissions:

        ``` {#codeblock_piq_tze_p6y}
        chmod 755 ossutilmac64
        ```

    3.  Generate the configuration file. For more information about the parameters, see the configuration parameters described in the preceding Linux section.

        ``` {#codeblock_ur9_3sx_g1g}
        ./ossutilmac64 config
        ```


