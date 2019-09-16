# Single-connection bandwidth throttling {#concept_1013170 .concept}

OSS allows you to configure bandwidth throttling for upload, download, and copy operations on OSS to ensure sufficient bandwidth for other applications.

Upload and download operations on OSS may consume a large amount of bandwidth. If your client does not have bandwidth throttling features, other applications are affected. To avoid this problem, you can include the `x-oss-traffic-limit` parameter in your requests to configure bandwidth throttling when you call operations such as PutObject, AppendObject, PostObject, CopyObject, UploadPart, UploadPartCopy, and GetObject.

## Scenarios {#section_tbl_8dg_2bq .section}

Single-connection bandwidth throttling applies to the following scenarios:

-   You can use the single-connection bandwidth throttling feature for clients such as mobile terminals and Web terminals that do not have advanced throttling features. This can help ensure sufficient bandwidth for other applications.
-   When sharing your data on OSS with other parties, you can include the `x-oss-traffic-limit` parameter in the object URL to limit the download speed. This will ensure sufficient bandwidth for your other business or the access of other users. For example, to share an object file.zip in the root directory of a bucket named atest in the China \(Hangzhou\) region, you can share the object URL with others for downloads. If you want to set the bandwidth limit to 5 MB/s, the object URL must include the `x-oss-traffic-limit=41943040` parameter:

    ``` {#codeblock_6bg_so6_9qs}
    https://atest.oss-cn-hangzhou.aliyuncs.com/file.zip?Expires=156273****&OSSAccessKeyId=TMP.hW5bxvoEiX2BkVvrdBYrfdgc******12RLSb888DqdDZtBUbtHNBKgmn9ZtvTEBtBfATe4VJpnnmKd48UJomnp5JtoHRQXL2FuuS8JKR58RhnD5uRnas6h6ZVHg4tf.tmp&Signature=FIH8m%2FQRDhF%2Bc%2FHhucUogRUcr8U%3D&x-oss-traffic-limit=41943040  
    ```


## Implementation modes {#section_mlh_kyw_qt5 .section}

|Implementation mode|Description|
|-------------------|-----------|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Single-connection bandwidth throttling.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Single-connection bandwidth throttling.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Single-connection bandwidth throttling.md#)|
|[C++ SDK](../../../../reseller.en-US/SDK Reference/C++/Single-connection bandwidth throttling.md#)|

## Precautions {#section_z5v_wy4_t7l .section}

-   The `x-oss-traffic-limit` parameter can be included in headers, request parameters, and form fields for form upload. The parameter must be included in the string to be signed.
-   The value of the `x-oss-traffic-limit` parameter must be a number. It must be expressed in bit/s.
-   Valid values of the bandwidth limit is 819200 to 838860800, which is equivalent to 100 KB/s to 100 MB/s.

    **Note:** Formula for unit conversion: 1 MB = 1,024 KB =1,048,576 Bytes = 8,388,608 bits


