# Storage Class {#concept_fcn_3xt_tdb .concept}

OSS provides three storage classes: Standard, infrequent access \(IA\), and Archive, to cover various data storage scenarios from hot data to cold data.

## Standard {#section_r3z_lxt_tdb .section}

OSS Standard storage provides highly reliable, highly available, and high-performance object storage services that support frequent data access. The high-throughput and low-latency service response capability of OSS can effectively support access to hotspot data. Standard storage is the right choice for storing various images for social networking and sharing, and storing data for audio and video applications, large websites, and big data analysis.

The Standard storage class has the following features:

-   Designed for 99.9999999999% data reliability.
-   Designed for 99.995% service availability.
-   Delivers high-throughput and low-latency access performance.
-   Supports HTTPS-based transmission.
-   Supports Image Processing.

## IA {#section_t3z_lxt_tdb .section}

OSS IA storage is suitable for storing long-lived, but less-frequently accessed data \(an average of once or twice per month\). With a storage unit price lower than Standard storage, IA storage is suitable for long-term backup of various mobile applications, smart device data, and enterprise data. It also supports real-time data access. Objects of the IA storage class have a minimum storage duration. OSS charges a fee if you delete objects that are stored for less than 30 days. Objects of the IA storage class have a minimum billable size. Objects smaller than 64 KB are charged as 64 KB. Data retrieval incurs charges.

The IA storage class has the following features:

-   Designed for 99.9999999999% data reliability.
-   Designed for 99.995% service availability.
-   Supports real-time access.
-   Supports HTTPS-based transmission.
-   Supports Image Processing.
-   Requires a minimum storage duration and minimum billable size.

## Archive {#section_v3z_lxt_tdb .section}

OSS Archive storage has the lowest price among the three storage classes. It is suitable for storing archival data for a long time \(more than half a year recommended\), such as medical images, scientific materials, and video footage. The data is infrequently accessed during the storage period. It takes about 1 minute to restore the data from the frozen status to the readable status. Objects of the Archive storage class have a minimum storage duration. OSS charges a fee if you delete objects that are stored for less than 60 days. Objects of the Archive storage class have a minimum billable size. Objects smaller than 64 KB are charged as 64 KB. Data retrieval incurs charges.

The Archive storage class has the following features:

-   Designed for 99.999999999% data reliability.
-   Designed for 99.99% service availability.
-   Takes about 1 minute to restore the stored data from the frozen status to the readable status.
-   Supports HTTPS-based transmission.
-   Supports Image Processing, but data needs to be restored first.
-   Requires a minimum storage duration and minimum billable size.

## Comparison of storage classes {#section_ugg_pbr_g2u .section}

|Item|Standard|IA|Archive|
|:---|:-------|:-|:------|
|Data reliability|99.9999999999%|99.9999999999%|99.999999999%|
|Service availability|99.995%|99.995%|99.99% \(restored data\)|
|Minimum billable size of objects|Actual size of objects|64 KB|64 KB|
|Minimum storage duration|N/A|30 days|60 days|
|Data retrieval fee|No data retrieval fee|Charged by the size of retrieved data. Unit: GB|Charged by the size of restored data. Unit: GB|
|Data access|Real-time access with a latency in milliseconds|Real-time access with a latency in milliseconds|One minute after data is restored from the frozen status to the readable status|
|Image Processing|Supported|Supported|Supported after data is restored from the frozen status to the readable status|

**Note:** 

OSS charges a data retrieval fee based on the size of data read from the underlying distributed storage system. The data transmitted on the Internet is billed as part of the outbound traffic.

## Supported APIs {#section_pfz_1yt_tdb .section}

|API|Standard|IA|Archive|
|:--|:-------|:-|:------|
|Bucket creation, deletion, and query|
|PutBucket|Supported|Supported|Supported|
|GetBucket|Supported|Supported|Supported|
|DeleteBucket|Supported|Supported|Supported|
|Bucket ACL|
|PutBucketAcl|Supported|Supported|Supported|
|GetBucketAcl|Supported|Supported|Supported|
|Bucket logging| | | |
|PutBucketLogging|Supported|Supported|Supported|
|GetBucketLogging|Supported|Supported|Supported|
|Bucket static website hosting|
|PutBucketWebsite|Supported|Supported|Not supported|
|GetBucketWebsite|Supported|Supported|Not supported|
|Bucket hotlinking protection| | | |
|PutBucketReferer|Supported|Supported|Supported|
|GetBucketReferer|Supported|Supported|Supported|
|Bucket lifecycle|
|PutBucketLifecycle|Supported|Supported|Supported for data deletion only|
|GetBucketLifecycle|Supported|Supported|Supported|
|DeleteBucketLifecycle|Supported|Supported|Supported|
|Cross-region replication| | | |
|PutBucketReplication|Supported|Supported|Supported|
|Cross-origin resource sharing \(CORS\)|
|PutBucketcors|Supported|Supported|Supported|
|GetBucketcors|Supported|Supported|Supported|
|DeleteBucketcors|Supported|Supported|Supported|
|Object operations|
|PutObject|Supported|Supported|Supported|
|PutObjectACL|Supported|Supported|Supported|
|GetObject|Supported|Supported|Supported after data is restored from the frozen state to the readable state|
|GetObjectACL|Supported|Supported|Supported|
|GetObjectMeta|Supported|Supported|Supported|
|HeadObject|Supported|Supported|Supported|
|CopyObject|Supported|Supported|Supported|
|OptionObject|Supported|Supported|Supported|
|DeleteObject|Supported|Supported|Supported|
|DeleteMultipleObjects|Supported|Supported|Supported|
|PostObject|Supported|Supported|Supported|
|PutSymlink|Supported|Supported|Supported|
|GetSymlink|Supported|Supported|Supported|
|RestoreObject|Not supported|Not supported|Supported|
|Multipart operations|
|InitiateMultipartUpload|Supported|Supported|Supported|
|UploadPart|Supported|Supported|Supported|
|UploadPartCopy|Supported|Supported|Supported|
|CompleteMultipartUpload|Supported|Supported|Supported|
|AbortMultipartUpload|Supported|Supported|Supported|
|ListMultipartUpload|Supported|Supported|Supported|
|ListParts|Supported|Supported|Supported|
|Image Processing|Supported|Supported|Supported|

