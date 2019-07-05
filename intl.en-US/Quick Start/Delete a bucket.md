# Delete a bucket {#concept_pkl_qdp_tdb .concept}

You can delete a bucket that is no longer need to save storage cost.

**Warning:** Delete a bucket with caution because a deleted bucket cannot be restored.

## Prerequisites {#section_xsk_tl2_chb .section}

Before deleting a bucket, ensure that all objects \(including fragments generated in multipart upload tasks\) in the bucket are deleted. For more information, see [Delete an object](reseller.en-US/Quick Start/Delete an object.md#).

## Delete a bucket in the OSS console {#section_s1s_hfk_bhb .section}

Follow these steps to delete a bucket in the OSS console:

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the bucket that you want to delete, and then click the **Basic Settings** tab.
3.  In the **Bucket Management** area, click **Delete Bucket**.
4.  In the displayed dialog box, click **OK**.

## Delete a bucket by using ossutil {#section_oqf_34k_bhb .section}

You can also use ossutil, which is a command line tool, to delete a bucket. For more information, see [Delete a bucket](../../../../reseller.en-US/Tools/ossutil/Bucket-related commands.md#section_wmp_z2l_xgb).

## Delete a bucket by using ossbrowser {#section_qhm_r4k_bhb .section}

You can also use ossbrowser, which is a graphical management tool, to delete a bucket. For more information, see [Quick start](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#).

## Delete a bucket by using OSS API/SDK {#section_ofw_xkk_bhb .section}

You can use OSS API and SDKs in different languages to delete a bucket. For more information, see the following URLs:

-   API: [DeleteBucket](../../../../reseller.en-US/API Reference/Object operations/DeleteObject.md#)
-   Java SDK: [Delete a bucket](../../../../reseller.en-US/SDK Reference/Java/Manage a bucket.md#section_ftd_mjf_2gb)
-   Python SDK: [Delete a bucket](../../../../reseller.en-US/SDK Reference/Python/Bucket.md#)
-   PHP SDK: [Delete a bucket](../../../../reseller.en-US/SDK Reference/PHP/Bucket.md#)
-   Go SDK: [Delete a bucket](../../../../reseller.en-US/SDK Reference/Go/Bucket.md#)
-   C SDK: [Delete a bucket](../../../../reseller.en-US/SDK Reference/C/Bucket.md#)

For SDK demos in more languages, see [Introduction](Introduction../DNOSS11814329/EN-US_TP_22258.dita#concept_dcn_tp1_kfb).

