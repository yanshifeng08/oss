# sign {#concept_303817 .concept}

The sign command is used to generate signed URLs for third-party users to access objects in buckets.

**Note:** For more information about object URLs, see .

## Command syntax {#section_67z_ip4_e5t .section}

``` {#codeblock_a7g_7j1_yc1}
./ossutil sign oss://bucket/object [--timeout time] [--trafic-limit limitSpeed]
```

-   --timeout: specifies the timeout value of the object URL. Valid values: non-negative integers. Default value: 60. Unit: seconds.
-   --trafic-limit: specifies the bandwidth that is used to access the object over HTTP. Valid values: 819200 to 838860800 \(100 KByte/s to 100 MByte/s\). Default value: 0 \(unlimited\). Unit: bit/s.

## Examples {#section_m41_ssr_ipw .section}

-   Generate an object URL that has a default timeout value of 60 seconds

    ``` {#codeblock_m0c_vjl_ef3}
    ./ossutil sign oss://bucket/path/object                       
    ```

-   Generate an object URL with a specified timeout value of 3600 seconds

    ``` {#codeblock_itd_hmn_x76}
    ./ossutil sign oss://bucket/path/object --timeout 3600
    ```

-   Generate an object URL with the bandwidth of 1 MByte/s

    ``` {#codeblock_2ub_9fm_lso}
    ./ossutil sign oss://bucket/path/object --trafic-limit 8388608
    ```

    **Note:** Formula for unit conversion: 1 MB = 1,024 KB =1,048,576 Bytes = 8,388,608 bits


## Common options {#section_x2p_nzu_vex .section}

The following table describes the options you can add to the sign command.

|Option|Description|
|------|-----------|
|--timeout|Specifies the timeout value of the object URL. Valid values: non-negative integers. Default value: 60. Unit: seconds.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--trafic-limit|Specifies the bandwidth that is used to access the object over HTTP. Valid values: 819200 to 838860800 \(100 KByte/s to 100 MByte/s\). Default value: 0 \(unlimited\). Unit: bit/s.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

