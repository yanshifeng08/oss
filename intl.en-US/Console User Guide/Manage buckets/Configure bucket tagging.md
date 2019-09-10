# Configure bucket tagging {#task_1925920 .task}

OSS allows you to configure bucket tagging to classify and manage buckets. For example, you can configure this feature to list only buckets that have specific tags. This topic describes how to configure bucket tagging in the OSS console.

Bucket tagging uses a key-value pair to identify buckets. You can manage multiple buckets that have the same tag.

-   Only the bucket owner can configure tagging for the bucket. If other users attempt to configure tagging for the bucket, 403 Forbidden is returned with error code AccessDenied.
-   You can configure a maximum of 20 tags for a bucket.
-   The tag key is required. The tag key can be a maximum of 64 characters in length and cannot start with `http://`, `https://`, or `Aliyun`.
-   The tag value is optional. The tag value can be a maximum of 128 characters in length.
-   The key and value of the tag must be encoded in UTF-8.

For more information about bucket tagging, see [Bucket tagging](../../../../reseller.en-US/Developer Guide/Buckets/Bucket tagging.md#).

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  On the left-side bucket list, click the target bucket to open the overview page of the bucket.
3.  Click the **Basic Settings** tab, find the **Bucket Tag** section.
4.  Click **Configure** and add tags.
5.  Click **Save**.

