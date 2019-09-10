# Configure object tagging {#task_1796814 .task}

OSS allows you to configure object tagging to classify objects. Object tagging uses a key-value pair to identify objects. You can perform operations on multiple objects that have the same tag. For example, you can configure lifecycle rules for objects that have the same tag.

Object tagging uses a key-value pair to identify objects. You can manage multiple objects that have the same tag. For example, you can configure lifecycle rules for objects that have the same tag and authorize RAM users to access objects that have the same tag. For more information, see [Object tagging](../reseller.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

-   A maximum of 10 tags can be set for each object. Tags associated with an object must have unique tag keys.
-   A tag key can be a maximum of 128 bytes in length. Each tag value can be a maximum of 256 bytes in length.
-   Keys and values are case-sensitive.
-   The key and value of the tag can contain letters, digits, spaces, and special characters such as

    + ‑ = . \_ : /

-   Only the bucket owner and authorized users have the read and write permissions on object tags. These permissions are independent of object ACLs.
-   During cross-region replication, object tags are also replicated to the destination bucket.

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the name of a bucket to go to the Overview tab.
3.  Click the **Files** tab.
4.  Move the pointer over **More** in the Actions column corresponding to an object and choose **Tagging** from the drop-down list.
5.  In the Tagging dialog box that appears, configure tags.
6.  Click **OK**.

