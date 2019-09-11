# request-payment {#concept_1614439 .concept}

The request-payment command is used to configure the pay-by-requester mode for buckets, or query the pay-by-requester configurations for buckets.

**Note:** For more information about the pay-by-requester mode, see [Enable the pay-by-requester mode](../../../../reseller.en-US/Developer Guide/Buckets/Enable the pay-by-requester mode.md#).

## Command syntax {#section_48t_2df_dn7 .section}

-   Set the pay-by-requester mode

    ``` {#codeblock_5wo_nwo_qec}
    ./ossutil request-payment --method put oss://bucket payment_parameter
    ```

    Valid values of payment\_parameter are Requester and BucketOwner.

    -   Requester: enables the pay-by-requester mode. Requesters pay the cost of requests and the data download from buckets.
    -   BucketOwner: disables the pay-by-requester mode. Bucket owners pay the cost of requests and the data download from their buckets.
-   Query the pay-by-requester configurations

    ``` {#codeblock_y0k_0rp_kyh}
    ./ossutil request-payment --method get oss://bucket
    ```


## Examples {#section_6vs_7tl_6gl .section}

-   Enable the pay-by-requester mode

    ``` {#codeblock_eqq_6yn_e1x}
    ./ossutil request-payment --method put oss://bucket1 Requester
    ```

-   Disable the pay-by-requester mode

    ``` {#codeblock_8q3_39h_r0p}
    ./ossutil request-payment --method put oss://bucket1 BucketOwner
    ```

-   Query the pay-by-requester configurations

    ``` {#codeblock_3d0_9la_dc6}
    ./ossutil request-payment --method get oss://bucket1
    BucketOwner
    0.178036(s) elapsed
    ```


## Common options {#section_bca_bzv_629 .section}

The following table describes the options you can add to the request-payment command.

|Option|Description|
|------|-----------|
|--method|Specifies the HTTP request method. Valid values: -   put: enables or disables the pay-by-requester mode.
-   get: obtains pay-by-requester configurations.

 |
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

