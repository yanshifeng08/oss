# update {#concept_303828 .concept}

The update command is used to update the ossutil version.

## Command syntax {#section_2fu_n1f_agm .section}

``` {#codeblock_w45_f1b_5gz}
./ossutil update [-f]
```

This command checks the current and latest versions of ossutil and generates their version numbers. If an update is available, this command asks whether you want to upgrade ossutil to the latest version. If the --force \(abbreviated as-f\) option is specified, the command upgrades ossutil without asking for confirmation when an update is available.

## Examples {#section_qjs_oz2_mx7 .section}

Upgrade the version of ossutil.

``` {#codeblock_rgf_izs_9af}
./ossutil update 
The current version is 1.5.1 and the latest version is 1.6.0.
Are you sure you want to update the version (y or N)? y
Updated.
```

## Common options {#section_le1_8r7_kp2 .section}

The following table describes the options you can add to the update command.

|Option|Description|
|------|-----------|
|-f, --force|Forces an operation without prompting the user for confirmation.|
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

