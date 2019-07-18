# bucket-encryption {#concept_627780 .concept}

The bucket-encryption command is used to add encryption settings to a bucket or modify, query, or delete the added encryption settings of a specified bucket.

**Note:** For more information about bucket encryption, see [Server-side encryption](https://www.alibabacloud.com/help/doc-detail/119320.html).

## Command syntax {#section_e65_hqy_hpm .section}

-   Add encryption settings to a bucket or modify the encryption settings added to a bucket.

    ``` {#codeblock_1yn_lfz_g1u}
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm algorithmName [--kms-masterkey-id  keyid]
    ```

    -   `--sse-algorithm`: specifies the encryption method for the bucket, which can be set to KMS or AES256.
    -   `--kms-masterkey-id`: specifies the CMK ID used for encryption. Set this parameter as required if the value of `--sse-algorithm` is KMS. Do not set this parameter if the value of --sse-algorithm is AES256.
    The preceding command either sets or modifies the encryption method of a bucket depending on whether an encryption method is already set for the bucket.

-   Obtain the encryption settings for a bucket.

    ``` {#codeblock_oms_yix_l2h}
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   Delete the encryption settings for a bucket.

    ``` {#codeblock_f0j_qnt_x22}
    ./ossuitl bucket-encryption --method delete oss://bucket
    ```


## Examples {#section_olq_pl3_3cm .section}

-   Add encryption settings to a bucket or modify the encryption settings added to a bucket.

    ``` {#codeblock_nh9_qn9_a3c}
    ./ossutil bucket-encryption --method put oss://bucket --sse-algorithm KMS --kms-masterkey-id 9468da86-3509-4f8d-a61e-6eab1eac****
    ```

-   Obtain the encryption settings for a bucket.

    ``` {#codeblock_mz7_shl_bpd}
    ./ossutil bucket-encryption --method get oss://bucket
    ```

-   Delete the encryption settings for a bucket.

    ``` {#codeblock_5l8_ux8_mtn}
    ./ossuitl bucket-encryption --method delete oss://bucket
    ```


## Common options {#section_glm_p7r_0j5 .section}

You can run the bucket-encryption command with the following options.

|Option|Description|
|------|-----------|
|`--sse-algorithm`|Specifies the server-side encryption algorithm. Valid values: KMS and AES256.|
|`--kms-masterkey-id`|Specifies the CMK ID for encryption. Set this parameter as required if the value of `--sse-algorithm` is KMS. Do not set this parameter if the value of --sse-algorithm is AES256.|
|`--method`|Specifies the HTTP request method. Valid values: -   put: adds or modifies configurations.
-   get: obtains configurations.
-   delete: deletes configurations.

 |
|`--loglevel`|Specifies the log level. The default value is null and specifies that no log file is generated. Valid values: -   info: generates prompt logs.
-   debug: generates detail logs and their corresponding HTTP request and response information.

 |

**Note:** For more information about common options, see [View all supported options](intl.en-US/ossutil/View all supported options.md#).

