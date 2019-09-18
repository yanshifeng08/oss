# Restore an Archive object {#task_727806 .task}

This topic describes how to restore an Archive object in the OSS console.

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the name of the bucket in which the object to be restored is stored.
3.  In the overview page of the bucket, click the **Files** tab.
4.  Move the pointer over **More** in the Actions column corresponding to the object and select **Restore** from the drop-down list. You can also click the object name. On the View Details page that appears, click **Restore** \> **OK**.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/570229/156878484749531_en-US.png)

 

    **Note:** 

    -   The restoration takes about one minute. Then, the object is in the restored state.
    -   The object remains in the restored state for one day by default. You can use [ossutil](../../../../reseller.en-US/Tools/ossutil/Common commands/restore.md#) or [SDK](../../../../reseller.en-US/SDK Reference/Java/Manage objects/Restore an archive object.md#) to prolong this period to a maximum of seven days. When this period expires, the object returns to the frozen state.
    -   Data retrieval fees are incurred when you restore an Archive object. For more information, see [Billing items](../../../../reseller.en-US/Pricing/Billing items.md#section_ykd_nst_lgb).
    -   For more information about the Archive storage class, see [Archive](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#section_v3z_lxt_tdb).

