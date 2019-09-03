# Monitoring service user guide {#concept_ods_34j_5db .concept}

## OSS monitoring entry {#section_rpm_k4j_5db .section}

The OSS monitoring service is available on the Alibaba Cloud Console. You can access the OSS monitoring service in either of the following ways:

-   Log on to the [OSS console](https://oss.console.aliyun.com/) and then click **Manage** on the right side of OSS overview page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935896368_en-US.png)

-   Log on to the [CloudMonitor console](https://cloudmonitor.console.aliyun.com) to view the OSS monitoring service.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935896369_en-US.png)


## OSS monitoring page {#section_ejs_r4j_5db .section}

The OSS monitoring page consists of the following three tabs:

-   Users
-   Bucket list
-   Alarm rules

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906370_en-US.jpg)

The OSS monitoring page does not support automatic refresh. You can click **Refresh** in the upper-right corner to display the latest data.

Click **Go to Object Storage Service Console** to log on to the OSS console.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906371_en-US.jpg)

Users

The Users page displays monitoring information at the user level, including User monitoring information, Latest month statistics and User-level metric indicators.

-   User monitoring information

    This module shows the total number of your buckets and related alarm rules.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906372_en-US.jpg)

    -   -   The parameters are as follows: Click the number next to **Bucket number** to display all the buckets you have created.
-   Click the number next to Alarm rules amount, In Alarm, Forbidden amount, or Alerted to display the following information: **Alarm rules amount** refers to total number of alarm rules you have set.
-   **In Alarm** refers to alarms in alarm state.
-   **Forbidden amount** refers to alarms that are currently disabled.
-   Alerted refers to alarms recently changed to alarm state
-   Monthly Statistics

    This module displays information about charged OSS resources that you have used during the period from 00:00 on the first day of the current month, to the metering acquisition cutoff time. The following indicators are displayed:

    -   Storage Size
    -   Internet Outbound Traffic
    -   Number of PUT Requests
    -   Number of GET Requests
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906374_en-US.jpg)

    The unit of each value is automatically adjusted by the order of magnitude. The exact value is displayed when you hover the cursor over the selected value.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906375_en-US.jpg)

-   User-level metric indicators

    This module displays user-level metric charts and tables and consists of Monitoring Service Overview and Request Status Detalls.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906376_en-US.jpg)

    You can select a pre-determined time range, or define a time range in the custom time boxes, to display the corresponding metric chart or table.

    -   The following time range options are available: 1 hour, 6 hours, 12 hours, 1 day, and 7 days. The default option is 1 hour.
    -   The custom time boxes allow the start time and the end time to be defined at the minute-level.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906377_en-US.jpg)

    Metric charts/tables support the following display modes:

    -   Legend hiding: You can click a legend to hide the corresponding indicator curve, as shown in the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906378_en-US.jpg)

    -   Click the ![](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/image/media/zoom_chart.jpg) icon in the upper-right corner of a metric chart to zoom in on the chart. Be note that tables cannot be zoomed in.
    -   Click the ![](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/image/media/alert_chart.jpg) icon in the upper-right corner of a metric chart to configure alarm rules for the displayed metric indicators. For more information, see the[Alarm Service User Guide](intl.en-US/Developer Guide/Monitoring service/Alert service.md#). Be note that you cannot set alarm rules for tables and metering reference indicators.
    -   Place the cursor inside the curve area of a chart, and press and hold the left button on the mouse while dragging the mouse to extend the time range. Click **Reset Zoom** to restore the original time range.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906379_en-US.jpg)

-   Monitoring Service Overview

    The Monitoring Service Overview page displays the following main metric charts:

    -   User-level availability/valid request rate, which includes two metric indicators: availability and percentage of valid requests.
    -   User-level requests/valid requests, which includes two metric indicators: total number of requests and number of valid requests.
    -   User-level traffic, which includes eight metric indicators: Internet outbound traffic, Internet inbound traffic, Intranet outbound traffic, Intranet inbound traffic, CDN outbound traffic, CDN inbound traffic, outbound traffic of cross-region replication, and inbound traffic of cross-region replication.
    -   User-level request state distribution, which is a table that displays the number and percentage of each type of requests within the selected time range.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935904334_en-US.jpg)

-   Request Status Detalls

    The "Request State Details" page shows the metric data of request state distribution through the following main metric charts:

    -   Number of Server Error Requests by User
    -   Server Error Request Proportion by User
    -   Number of Network Error Requests by User
    -   Network Error Request Proportion by User
    -   Number of Client Error Requests by User, which includes four metric indicators: number of error requests indicating resource not found, number of authorization error requests, number of client-site time-out error requests, and number of other client-site error requests
    -   Client Error Request Proportion by User, which includes four metric indicators: percentage of error requests indicating resource not found, percentage of authorization error requests, percentage of client-site time-out error requests, and percentage of other client-site error requests
    -   Number of Valid Requests by User, which includes two metric indicators: number of successful requests and number of redirect requests
    -   Valid Request Proportion by User, which includes two metric indicators: percentage of successful requests and percentage of redirect requests
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935906380_en-US.png)


Bucket List

-   Bucket list information

    The Bucket list tab page displays the information including bucket name, region, creation time, metering statistics of the current month, and related operations.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935916381_en-US.jpg)

    -   Display parameters are as follows: The metering statistics of the current month display the storage size, Internet outbound traffic, Put request count, and Get request count for each bucket.
    -   Click Monitoring chart or the corresponding bucket name to go to the bucket monitoring view page.
    -   Click Alarm rules next to your expected bucket, or go to the Alarm rules tab to display all alarm rules of the bucket.
    -   Enter the expected bucket name in the search box in the upper left-corner to display the bucket \(fuzzy match is supported\).
    -   Select the check boxes before the expected bucket names and click Setting custom monitor alarm rules to batch set alarm rules. For more information, see the [Alarm Service User Guide](intl.en-US/Developer Guide/Monitoring service/Alert service.md#).
-   Bucket-level monitoring view

    Click **Monitoring chart** next to the expected bucket name in the bucket list to go to the bucket monitoring view.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935916382_en-US.png)

    The bucket monitoring view displays metric charts based on the following six indicator groups:

    -   -   Monitoring Service Overview
-   Request Status Details
-   Measurement Reference
-   Average Latency
-   Maximum Latency
-   Successful Request Category
    Except measurement reference, other indicators are displayed with an aggregation granularity of 60s. The default time range for bucket-level metric charts is of the previous six hours, whereas that for user-level metric charts is of the previous hour. Click **Back to bucket list** in the upper-left corner to return to the Bucket list tab.

    -   Monitoring Service Overview

        This indicator group is similar to the service monitoring overview at the user level, but the former displays metric data at the bucket level. The main metric charts include:

        -   Request Valid Availability, which includes two metric indicators: availability and percentage of valid requests
        -   Total/Valid request, which includes two metric indicators: total number of requests and number of valid requests
        -   Overflow, which includes eight metric indicators: Internet outbound traffic, Internet inbound traffic, intranet outbound traffic, intranet inbound traffic, CDN outbound traffic, CDN inbound traffic, outbound traffic of cross-region replication, and inbound traffic of cross-region replication
        -   Request status count, which is a table that displays the number and percentage of each type of requests within the selected time range.
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935916383_en-US.jpg)

    -   Request Status Details

        This indicator group is similar to the request state details at the user level, but the former displays metric data at the bucket level. The main metric charts include:

        -   Server error count
        -   Server error rate
        -   Network error count
        -   Network error request rate
        -   Client error request count, which includes four metric indicators: number of error requests indicating resource not found, number of authorization error requests, number of client-site time-out error requests, and number of other client-site error requests
        -   Client error request percent, which includes four metric indicators: percentage of error requests indicating resource not found, percentage of authorization error requests, percentage of client-site time-out error requests, and percentage of other client-site error requests
        -   Redirect request count, which includes two metric indicators: number of successful requests and number of redirect requests
        -   Success redirect rate, which includes two metric indicators: percentage of successful requests and percentage of redirect requests
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935914331_en-US.jpg)

    -   Measurement Reference

        The metering reference group shows metering indicators with an hourly collection and representation granularity, as shown in the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935911210_en-US.png)

        The metering metric charts include:

        -   Quota size
        -   Overflow
        -   Billing requests, which includes the Get request count and Put request count.
        After a bucket is created, new data is collected in the next hour on the hour following the current time point, and the collected data will be displayed within 30 minutes.

    -   Average Latency

        This indicator group contains the average latency indicators of API monitoring. The metric charts include:

        -   getObject Average Latency
        -   headObject Average Latency
        -   putObject Average Latency
        -   postObject Average Latency
        -   append Object Average Latency
        -   upload Part Average Latency
        -   upload Part Copy Average Latency
        Each metric chart shows the corresponding average E2E latency and average server latency. See the figure below:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935911212_en-US.png)

    -   Maximum Latency

        This indicator group contains the maximum latency indicators of API monitoring. The metric charts include:

        -   getObject Max Latency\(Millisecond\)
        -   headObject Max Latency
        -   putObject Max Latency
        -   postObject Max Latency
        -   append Object Max Latency
        -   upload Part Max Latency
        -   upload Part Copy Max Latency
        Each metric chart shows the corresponding maximum E2E latency and maximum server latency. See the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935911213_en-US.png)

    -   Successful Request Category

        This indicator group contains the successful request count indicators of API monitoring. The metric charts include:

        -   getObject Success Count
        -   headObject Success Count
        -   putObject Success Count
        -   post Object Success Count
        -   append Object Success Count
        -   upload Part Success Count
        -   upload Part Copy Success Count
        -   delete Object Success Count
        -   deleteObjects Success Count
        See the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935921214_en-US.png)


Alarm Rules

The Alarm rules tab page allows you to view and manage all your alarm rules, as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4392/15674935921215_en-US.png)

For the description and usage of the "Alarm Rules" tab page, see the [Alarm Service User Guide](intl.en-US/Developer Guide/Monitoring service/Alert service.md#).

## Additional links {#section_vpm_tvj_5db .section}

For more information regarding the important points and user guide of the monitoring service, see the related chapter in [Monitoring, diagnosis, and troubleshooting](intl.en-US/Developer Guide/Monitoring service/Service monitoring, diagnosis, and troubleshooting.md#).

