# Real-time log query {#concept_eyb_1n5_1gb .concept}

When you access OSS, a large number of access logs are generated. By combining OSS and [Log Service \(LOG\)](../../../../reseller.en-US/Product Introduction/What is Log Service.md#), the real-time log query feature allows you to query OSS access logs in the OSS console and implement operation audit, access statistics collection, exception backtracking, and troubleshooting accordingly. With this feature, you can improve your work efficiency and make decisions based on data.

## Comparison between real-time log query and access logging {#section_fgg_gq2_cgb .section}

-   Real-time log query:
    -   Pushes logs to Log Service instances within 3 minutes and allows you to view real-time logs in the OSS console.
    -   Provides the log analysis service and typical analysis reports so that you can easily query data.
    -   Allows you to query and analyze raw logs in real time and filter logs by bucket, object name, API operation, and time.
-   Access logging:
    -   Allows you to enable the logging feature for a bucket. Then, automatically generates an object by hour based on the predefined naming rules to store access logs for the bucket and writes the object to the specified bucket.
    -   Allows you to use Alibaba Cloud Data Lake Analytics or build a Spark cluster to analyze access logs.
    -   Allows you to set lifecycle management rules for the specified bucket to convert the storage class of log objects to Archive for long-term archiving.

## Configuration method {#section_nwv_k2v_1gb .section}

Console: [Enable real-time log query](../../../../reseller.en-US/Console User Guide/Manage logs/Real-time log query.md#)

## Query methods {#section_agq_z1v_1gb .section}

The real-time log query feature provides the following query methods:

-   Real-time raw log query

    You can specify the period and query statement for raw logs to easily perform the following operations:

    -   Analyze the distribution of a field, such as an API, within a period.
    -   Filter records to be analyzed by field for further queries. For example, filter the object deletion operations for the last day by bucket, object name, or API name, and query the deletion time and access IP address.
    -   Collect statistics on OSS access records, for example, the page view \(PV\), unique visitor \(UV\), or maximum latency of a bucket within a period.
-   Log report query

    The real-time log query feature provides four immediately available reports:

    -   **Access Center**: displays the overall operating status, including the PV, UV, traffic, and distribution of Internet access.
    -   **Audit Center**: displays statistics for object operations, including read, write, and delete operations on objects.
    -   **Operation Center**: displays statistics for access logs, including the number of requests and distribution of failed operations.
    -   **Performance Center**: displays statistics for performance, including the performance of download or upload through the Internet, the performance of transmission through different networks or with different object sizes, and the list of differences between stored and downloaded objects.
-   Query through the Log Service console

    You can view OSS access logs in the [Log Service console](https://sls.console.aliyun.com/).


## Billing method {#section_tjh_5cv_1gb .section}

The real-time log query feature of OSS allows you to query logs for the past seven days free of charge. If the log storage time that you set is longer than seven days, Log Service separately charges the excess days. Extra fees are charged when you read data from or write data to Log Service through the Internet.

For more information about the billing standards, see [Billing method](../../../../reseller.en-US/Pricing/Billing method.md#) of Log Service.

