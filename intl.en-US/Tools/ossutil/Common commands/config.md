# config {#concept_303826 .concept}

The config command is used to create a configuration file to store OSS access information. You can add the -c option to other commands to provide OSS access information.

## Command syntax {#section_tuj_b37_78e .section}

``` {#codeblock_981_qo0_qkw}
./ossutil config [-e endpoint] [-i id] [-k key] [-t token] [-L language] [--output-dir outdir] [-c file]
```

**Note:** This command can be used in both interactive and non-interactive modes. We recommend that you run the command in interactive mode to ensure security.

## Examples {#section_nf0_j38_6jj .section}

-   Generate a configuration file in interactive mode

    ``` {#codeblock_qq8_qh0_fdh}
    ./ossutil config
    This command generates a configuration file. Enter the path of the configuration file. The default path is /home/user/.ossutilconfig. If you press Enter without specifying a different destination, the file is generated in the default path. If you want to generate the file in another path, set the --config-file option to the path. 
    If no configuration file path is entered, the default configuration file path /home/user/.ossutilconfig is used. 
    The following parameters are ignored if you press Enter without configuring them. For more information about the parameters, run the help config command. 
    Enter the endpoint: http://oss-cn-shenzhen.aliyuncs.com 
    Enter the AccessKey ID: yourAccessKeyID 
    Enter the AccessKey secret: yourAccessKeySecret
    Enter the STS token: 
    ```

    -   endpoint: specifies the domain name of the region to which the bucket belongs. For more information, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
    -   accessKeyID: For more information about how to view the AccessKey ID, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
    -   accessKeySecret: For more information about how to view the AccessKey secret, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
    -   stsToken: This option is required only when you use a temporary STS token to access the OSS bucket. Otherwise, you can leave this parameter unspecified. For more information about how to generate an STS token, see [Temporary access credential](../../../../reseller.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md#section_dvv_hkb_5db).
-   Generate a configuration file in non-interactive mode

    ``` {#codeblock_shk_b57_kv7}
    ./ossutil config -e oss-cn-beijing.aliyuncs.com -i LTAIbZcdVCmQ**** -k D26oqKBudxDRBg8Wuh2EWDBrM0****  -L CH -c /myconfig
    ```

    If you run the config command with options other than --language and --config-file, the non-interactive mode is used. All configuration items are specified using options.


## Configuration file format {#section_ow0_k3g_v3j .section}

You can modify OSS access information in the generated configuration file. The configuration file is in the following format:

``` {#codeblock_wyz_1cl_r7p}
[Credentials]
        language = CH
        endpoint = oss.aliyuncs.com
        accessKeyID = your_key_id
        accessKeySecret = your_key_secret
        stsToken = your_sts_token
        outputDir = your_output_dir
[Bucket-Endpoint]
        bucket1 = endpoint1
        bucket2 = endpoint2
        ...
[Bucket-Cname]
        bucket1 = cname1
        bucket2 = cname2
        ...
[AkService]
        ecsAk=http://100.100.100.200/latest/meta-data/Ram/security-credentials/EcsRamRoleTesting
```

-   Bucket-Endpoint: specifies an individual endpoint for each specified bucket. If you configure the Bucket-Endpoint option, ossutil searches for the specified endpoint when you perform operations on the bucket. If the specified endpoint exists, ossutil manages the bucket through that endpoint. Otherwise, ossutil manages the bucket through the endpoint specified in the Credentials option.
-   Bucket-Cname: specifies an individual origin domain for each specified bucket. If you configure the Bucket-Cname option, ossutil searches for the specified origin domain when you perform operations on the bucket. If the specified origin domain exists, ossutil replaces the endpoints specified in the Bucket-Endpoint and Credentials options with the origin domain. For more information about origin domains, see [Configure CNAME](../../../../reseller.en-US/Quick Start/Overview.md#substeps_9my_37d_bc3).
-   AkService: This option is not added by default. This option is required if you need to use a RAM role bound to an ECS instance to perform operations on OSS. When you configure this option, you need only to set EcsRamRoleTesting to the RAM role bound to the ECS instance. After configuring the AkService option, you do not need to configure the accessKeyID, accessKeySecret, and stsToken options. If these options are configured, the configurations of these options instead of the AkService option take effect. For more information about how to bind a RAM role to an ECS instance, see Use RAM roles in the console. For more information about how to bind a RAM role to an ECS instance, see [Use the instance RAM role in the console](../../../../reseller.en-US/Security/Instance RAM roles/Use the instance RAM role in the console.md#).

**Note:** 

-   In the new version of ossutil, the Bucket-Endpoint and Bucket-Cname options do not need to be specified in interactive mode. However, the two options still take effect in the configuration file. You can specify an endpoint or origin domain for each bucket in the configuration file.
-   ossutil can specify endpoints for multiple regions. The priorities of all endpoint configurations are as follows: endpoint specified by the `--endpoint` option in commands \> endpoint specified in Bucket-Cname \> endpoint specified in Bucket-Endpoint \> endpoint specified in Credentials. If you specify the `--endpoint` \(abbreviated as `-e`\) option when running a command, the value of the `--endpoint` option takes precedence. If this option is not specified, ossutil searches for the endpoints specified in Bucket-Cname, Bucket-Endpoint, and Credentials in the configuration file. The endpoint with the highest priority is used.

## Common options {#section_8ea_9nz_khm .section}

The following table describes the options you can add to the config command to generate the specified configuration items.

|Option|Description|
|------|-----------|
|-e, --endpoint|Specifies the endpoint in the \[Credentials\] section of the configuration file.|
|-i, --access-key-id|Specifies the AccessKey ID in the \[Credentials\] section of the configuration file.|
|-k, --access-key-secret|Specifies the AccessKey secret in the \[Credentials\] section of the configuration file.|
|-t, --sts-token|Optional. This option specifies the STS token in the \[Credentials\] section of the configuration file.|
|--output-dir|Specifies the directory in which output objects are located. Output objects include report objects generated due to errors that occur when you use the cp command to copy multiple objects. The default value is the ossutil\_output directory in the current directory.|
|-L, --language|Specifies the language ossutil uses. Valid values: CH and EN. Default value: CH. If you plan to set this option to CH, ensure that your system supports UTF-8 encoding.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

