# Delete a bucket {#concept_bcz_sbz_5db .concept}

You can delete a bucket that is no longer need to save storage cost.

## Prerequisites {#section_q5s_sfz_5db .section}

Before deleting a bucket, ensure that all objects \(including fragments generated in multipart upload tasks\) in the bucket are deleted. Otherwise, the bucket cannot be deleted.

**Note:** 

-   If you want to delete all objects in a bucket, we recommend that you [Set lifecycle](reseller.en-US/Console User Guide/Manage buckets/Set lifecycle.md#) for the bucket.
-   For more information about how to delete fragments, see [Manage fragments](reseller.en-US/Console User Guide/Manage fragments.md#).

## Procedure {#section_azm_ybz_5db .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the bucket that you want to delete, and then click the **Basic Settings** tab.
3.  In the **Bucket Management** area, click **Delete Bucket**.
4.  In the displayed dialog box, click **OK**.

    **Warning:** Delete a bucket with caution because a deleted bucket cannot be restored.


