# Real-time log query {#concept_ttq_vfq_bgb .concept}

A large number of access logs are generated when you access OSS. By integrating OSS and [Log Service](../../../../reseller.en-US/Product Introduction/What is Log Service.md#) \(SLS\), the real-time log query function allows you to query OSS access logs in the OSS console so that you can monitor OSS operations, measure OSS access statistics, trace exceptions, and locate problems with ease, improving efficiency and helping you make data-based decisions.

For more information, see [Real-time log query](../../../../reseller.en-US/Developer Guide/Manage logs/Real-time log query.md#) in the OSS developer guide.

## Enable real-time log query {#section_ify_c32_cgb .section}

You can enable the real-time log query function using either of the following two methods:

-   Method 1: Enable real-time log query when creating a bucket.
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  Click **Create Bucket**.
    3.  In the **Create Bucket** dialog box, configure the parameters of the bucket you want to create.

        Select **Enable** for **Real-time Log Query**. For the configuration of other parameters, see [Create a bucket](reseller.en-US/Console User Guide/Manage buckets/Create a bucket.md#).

    4.  Click **OK**.
-   Method 2: Enable real-time log query in the **Log Overview** page.
    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
    2.  In the left-side bucket list, click the bucket that you want to enable real-time log query.
    3.  Click **Log Overview**, and then click **Activate Now** in the displayed page.

        **Note:** If Log Service is not activated, you are asked to authorize Log Service so that it can access OSS. After Log Service is authorized, a Log Service activation dialog box is displayed. Click **Activate Now**. After activating Log Service, follow step 3 to enable real-time log query.


**Note:** By using real-time log query, you can query logs recorded in the latest seven days for free. You can click **Config Log Retention Time** in the upper right of the **Log Overview** page to modify the log storage period. If the log storage period is set to a value larger than seven days, logs generated outside the scope of the latest seven days are charged individually. Additional fees are incurred if you use Log Service through the Internet.

For more information about the fees, see [Billing method](../../../../reseller.en-US/Pricing/Billing method.md#).

## Query real-time logs {#section_cp4_f32_cgb .section}

You can query real-time logs using any of the following three methods:

-   Query real-time logs in the **Original Log** page.

    When querying real-time logs, you can specify the time range and query statements. For example, you can quickly analyze the distribution of a specified field \(such as an API operation\) in a specified time range.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78515/156263984934106_en-US.png)

-   Query real-time logs in the **Dashboard** page.

    In the **Dashboard** page, the following four system-generated reports are provided:

    -   **Access Center**: displays general operation information, including PV, UV, traffics, and Internet access distribution in maps.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78515/156263984950993_en-US.png)

    -   **Audit Center**: displays the statistics about object operations, including object reading, writing, and deleting.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78515/156263984950995_en-US.png)

    -   **Operation Center**: displays the statistics about access logs, including the request number and the distribution of failed operations.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78515/156263984950996_en-US.png)

    -   **Performance Center**: displays the statistics about performance, including the distribution of Internet download and upload performance, transmission performance of different networks and object sizes, and the list of download performance for different objects.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78515/156263984950997_en-US.png)

-   Query real-time logs in the Log Service console.

    You can view OSS access logs in the [Log Service console](https://sls.console.aliyun.com/).


## Reference {#section_zww_hgq_bgb .section}

To store OSS access logs, see [Set logging](reseller.en-US/Console User Guide/Manage logs/Set logging.md#).

