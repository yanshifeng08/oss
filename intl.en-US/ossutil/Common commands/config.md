# config {#concept_627783 .concept}

The config command is used to create a configuration file to store OSS access information. You can use the `-c` option with other commands to provide OSS access information.

## Command syntax {#section_dm4_tyc_q0l .section}

``` {#codeblock_wx2_okq_0oc}
./ossutil config [-e endpoint] [-i id] [-k key] [-t token] [-L language] [--output-dir outdir] [-c file]
```

**Note:** This command can be used in both interactive and non-interactive modes. We recommend that you run the command in interactive mode to ensure security.

## Examples {#section_v7p_yo3_c1b .section}

-   Generate a configuration file in interactive mode.

    ``` {#codeblock_lro_daw_alt}
    ./ossutil config
    This command generates a configuration file. Enter the path of the configuration file. The default path is /home/user/.ossutilconfig. If you press Enter without specifying a different destination, the file is generated in the default path. If you want to generate the file in another path, set the --config-file option to the path. 
    If the path of the configuration file is not entered, the default configuration file /home/user/.ossutilconfig is used. 
    The following parameters are ignored if you press Enter without configuring them. For more information about the parameters, run the help config command. 
    Enter the endpoint: http://oss-cn-shenzhen.aliyuncs.com 
    Enter the AccessKey ID: yourAccessKeyID 
    Enter the AccessKey Secret: yourAccessKeySecret
    Enter the STS token: 
    ```

    -   **endpoint**: the endpoint of the region to which the bucket belongs. For more information, see [Regions and endpoints](../../../../intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
    -   **accessKeyID**: For more information about how to view the AccessKey ID, see [Create an AccessKey](../../../../intl.en-US/General Reference/Create an AccessKey.md#).
    -   **accessKeySecret**: For more information about how to view the AccessKey Secret, see [Create an AccessKey](../../../../intl.en-US/General Reference/Create an AccessKey.md#).
    -   **stsToken**: This option is required only when you use a temporary STS token to access the OSS bucket. Otherwise, you can leave this parameter blank. For more information about how to generate an STS token, see [Temporary access credential](../../../../intl.en-US/Developer Guide/Upload files/Authorized third-party upload.md#section_dvv_hkb_5db).
-   Generate a configuration file in non-interactive mode.

    ``` {#codeblock_15f_6m2_yo5}
    ./ossutil config -e oss-cn-beijing.aliyuncs.com -i LTAIbZcdVCmQ**** -k D26oqKBudxDRBg8Wuh2EWDBrM0****  -L CH -c /myconfig
    ```

    If you run the config command with options other than `--language` and `--config-file`, the non-interactive mode is used, and all configuration items are specified using options.


## Configuration file format {#section_6ws_r5m_bes .section}

You can directly modify OSS access information in the generated configuration file. The configuration file is in the following format:

``` {#codeblock_uac_5oj_gnt}
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

-   Bucket-Endpoint: specifies an individual endpoint for each specified bucket. If you configure the Bucket-Endpoint option, ossutil searches for the endpoint specified for a bucket when you perform operations on the bucket. If the specified endpoint exists, ossutil manages the bucket through that endpoint. Otherwise, ossutil manages the bucket through the endpoint specified in the Credentials option.
-   Bucket-Cname: specifies an individual CDN acceleration domain name for each specified bucket. If you configure the Bucket-Cname option, ossutil searches for the CDN acceleration domain name specified for a bucket when you perform operations on the bucket. If the specified CDN acceleration domain name exists, ossutil replaces the endpoints specified in the Bucket-Endpoint and Credentials options with the domain name. For more information about CDN acceleration domain names, see [CDN-based OSS acceleration](../../../../intl.en-US/Quick Start/Quick start.md#substeps_9my_37d_bc3).
-   AkService: This parameter is not configured by default. If you want to use OSS by using a RAM user associated with an ECS instance, add this parameter into the configuration file by replacing EcsRamRoleTesting with the RAM user associated with your ECS instandce. If you configure this parameter in the configuration file, accessKeyID and accessKeySecret cannot be configured. However, if you already configure accessKeyID and accessKeySecret, authentication is performed based on the configured accessKeyID and accessKeySecret. For more information about RAM users associated with ECS instances, see [Use the instance RAM role in the console](../../../../intl.en-US/Security/Instance RAM roles/Use the instance RAM role in the console.md#)。

**Note:** 

-   In the new version of ossutil, the Bucket-Endpoint and Bucket-Cname options do not need to be specified in interactive mode. However, the two options still take effect in the configuration file. You can specify an endpoint or CDN acceleration domain name for each bucket in the configuration file.
-   ossutil can specify endpoints for multiple regions. The priorities of all of the endpoint configurations are as follows: endpoint specified by the `--endpoint` option in commands \> endpoint specified in Bucket-Cname \> endpoint specified in Bucket-Endpoint \> endpoint specified in Credentials. If you specify the `--endpoint` \(abbreviated as `-e`\) option when running a command, the value of the `--endpoint` option takes precedence. If this option is not specified, ossutil searches for the endpoints specified in Bucket-Cname, Bucket-Endpoint, and Credentials in the configuration file. The endpoint with the highest priority is used.

## Common options {#section_ta0_zab_6ka .section}

You can run the config command with the following options to generate the specified configuration items.

|Option|Description|
|------|-----------|
|`-e, --endpoint`|Specifies the endpoint in the \[Credentials\] section of the configuration file.|
|`-i, --access-key-id`|Specifies the AccessKey ID in the \[Credentials\] section of the configuration file.|
|`-k, --access-key-secret`|Specifies the AccessKey Secret in the \[Credentials\] section of the configuration file.|
|`-t, --sts-token`|Optional. This option specifies the STS token in the \[Credentials\] section of the configuration file.|
|`--output-dir`|Specifies the directory in which output files are located. Output files include report files generated due to errors that occur when you use the cp command to batch copy files. The default value is the ossutil\_output directory in the current directory.|
|`-L, --language`|Specifies the language of the ossutil tool. Valid values: CH and EN. Default value: CH. If you plan to set this option to CH, ensure that your system supports UTF-8 encoding.|
|`--loglevel`|Specifies the log level. The default value is null and specifies that no log file is generated. Valid values: -   info: generates prompt logs.
-   debug: generates detail logs and their corresponding HTTP request and response information.

 |

**Note:** For more information about common options, see [View all supported options](intl.en-US/ossutil/View all supported options.md#).

