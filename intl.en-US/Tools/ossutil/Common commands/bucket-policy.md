# bucket-policy {#concept_1614438 .concept}

The bucket-policy command is used to add, modify, query, or delete bucket policy configurations for a bucket.

**Note:** For more information about bucket policies, see [Bucket policy](../../../../reseller.en-US/Developer Guide/Access and control/Bucket policy.md#).

## Command syntax {#section_h9z_aby_1m4 .section}

-   Add or modify bucket policy configurations

    ``` {#codeblock_p26_t9n_ijd}
    ./ossutil bucket-policy --method put oss://bucket local_json_file [options]
    ```

    ossutil reads the local\_json\_file configuration file. If the bucket has no bucket policy configurations, ossutil writes the configurations to this configuration file. If the bucket has bucket policy configurations, new configurations will overwrite the existing configurations.

    **Note:** The local\_json\_file configuration file is in the JSON format as follows:

    ``` {#codeblock_25z_jog_yba}
     {
         "Version": "1",
         "Statement": [
             {
                 "Effect": "Allow",
                 "Action": [
                     "ram:ListObjects"
                 ],
                 "Principal": [
                     "1234567"
                 ],
                 "Resource": [
                     "*"
                 ],
                 "Condition": {}
             }
         ]
    }
    ```

-   Obtain bucket policy configurations

    ``` {#codeblock_dpp_rnk_zyf}
    ./ossutil bucket-policy --method get oss://bucket [local_json_file] [options]
    ```

    The local\_json\_file parameter specifies the name of the configuration file. If this parameter is specified, bucket policy configurations will be saved as a local file. If this parameter is not specified, ossutil displays the bucket policy configurations.

-   Delete bucket policy configurations

    ``` {#codeblock_pg6_brq_ou7}
    ./ossuitl bucket-policy --method delete oss://bucket [options]
    ```


## Examples {#section_pll_drx_psv .section}

-   Add bucket policies to allow anonymous users with specified IP addresses to access all resources in a bucket

    ``` {#codeblock_xee_aca_dbo}
    ./ossutil bucket-policy --method put oss://bucket1 /file/policy.json
    ```

    The content of the policy.json configuration file is as follows:

    ``` {#codeblock_hkq_9n8_q96}
    {
    "Version": "1",
    "Statement": [
            {
                "Action": [
                    "oss:*"
                ],
                "Effect": "Allow",
                "Principal": [
                    "*"
               ],
                "Resource": [
                    "acs:oss:*:174649585760****:bucket1",
                    "acs:oss:*:174649585760****:bucket1/*"
                ],
                "Condition": {
                    "IpAddress": {
                        "acs:SourceIp": [
                            "10.10.10.10"
                        ]
                    }
                }
            }
        ]
    }
    ```

-   Obtain bucket policy configurations

    ``` {#codeblock_x3d_oji_ecv}
    ./ossutil.exe bucket-policy --method get oss://bucket1
    {
        "Version": "1",
        "Statement": [
            {
                "Action": [
                    "oss:*"
                ],
                "Effect": "Allow",
                "Principal": [
                    "*"
                ],
                "Resource": [
                    "acs:oss:*:174649585760****:bucket1",
                    "acs:oss:*:174649585760****:bucket1/*"
                ],
                "Condition": {
                    "IpAddress": {
                        "acs:SourceIp": [
                            "10.10.10.10"
                        ]
                    }
                }
            }
        ]
    }
    ```

-   Delete bucket policy configurations

    ``` {#codeblock_zwv_019_axj}
    ./ossutil.exe bucket-policy --method delete oss://bucket1
    ```


## Common options {#section_5i8_352_n8p .section}

The following table describes the options you can add to the bucket-policy command.

|Option|Description|
|------|-----------|
|--method|Specifies the HTTP request method. Valid values: -   put: adds or modifies bucket policy configurations.
-   get: obtains bucket policy configurations.
-   delete: deletes bucket policy configurations.

 |
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is https://120.79.128.211:3128 or socks5://120.79.128.211:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

