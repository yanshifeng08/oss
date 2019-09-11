# bucket-encryption {#concept_354612 .concept}

The bucket-encryption command is used to add, modify, query, or delete encryption configurations for a bucket.

**Note:** For more information about bucket encryption, see [Server-side encryption](../../../../reseller.en-US/Developer Guide/Data encryption/Server-side encryption.md#).

## Command syntax {#section_qz7_p38_o8w .section}

-   Add or modify bucket encryption configurations

    ``` {#codeblock_3im_uy8_y74}
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm algorithmName [--kms-masterkey-id  keyid]
    ```

    -   --sse-algorithm: specifies the encryption method for the bucket. Valid values: KMS and AES256.
    -   --kms-masterkey-id: specifies the CMK ID for encryption. When --sse-algorithm is set to AES256, do not add the -kms-masterkey-id option. When the --sse-algorithm parameter is set to KMS, add this option as required.

        **Note:** The use of a specified CMK ID for encryption is under public preview. To enable this feature, contact [After-sales technical support](https://selfservice.console.aliyun.com/ticket/createIndex).

    If the bucket has no encryption configurations, you can run this command to add a specified encryption method for this bucket. If the bucket has encryption configurations, you can run this command to overwrite the existing encryption configurations.

-   Obtain bucket encryption configurations

    ``` {#codeblock_icr_n19_4ff}
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   Delete bucket encryption configurations

    ``` {#codeblock_ixq_txk_rpz}
    ./ossuitl bucket-encryption --method delete oss://bucket
    ```


## Examples {#section_23p_qal_6on .section}

-   Add or modify bucket encryption configurations

    ``` {#codeblock_aud_92w_uuz}
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm KMS --kms-masterkey-id 9468da86-3509-4f8d-a61e-6eab1eac****
    ```

-   Obtain bucket encryption configurations

    ``` {#codeblock_eut_5dv_ks8}
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   Delete bucket encryption configurations

    ``` {#codeblock_w70_afy_cwg}
    ./ossuitl bucket-encryption --method delete oss://bucket
    ```


## Common options {#section_v4k_n4k_j0a .section}

The following table describes the options you can add to the bucket-encryption command.

|Option|Description|
|------|-----------|
|--sse-algorithm|Specifies the server-side encryption algorithm. Valid values: KMS and AES256.|
|--kms-masterkey-id|Specifies the CMK ID for encryption. When `--sse-algorithm` is set to AES256, do not add the -kms-masterkey-id option. When the --sse-algorithm parameter is set to KMS, add this option as required.|
|--method|Specifies the HTTP request method. Valid values: -   put: adds or modifies bucket encryption configurations.
-   get: obtains bucket encryption configurations.
-   delete: deletes bucket encryption configurations.

 |
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

