# Set a compliant retention policy {#concept_lnq_csm_cfb .concept}

A compliant retention policy is used to specify the protection period of objects in a bucket. No one can modify or delete a protected object during the protection period.

**Note:** 

-   For more information about compliant retention policies, see [Introduction](intl.en-US/Developer Guide/Compliant retention strategy/Introduction.md#).
-   The compliant retention policy feature is in the beta testing phase in the China South 1 \(Shenzhen\) region and will be supported in all regions.

## Procedure {#section_loz_0pl_8hk .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the left-side bucket list, click the bucket that you want to set a compliant retention policy for.
3.  Click the **Basic Settings** tab, locate Retention Policy area, and then click **Configure**.
4.  Click **Create Policy**.
5.  Set the **Retention Period** for the compliant retention policy. The value range of **Retention Period** is 1 day to 70 years.
6.  Click **OK**.

    **Note:** After a compliant retention policy is created, it is in the IN\_PROGRESS state. You can **Lock** or **Delete** a compliant retention policy in the IN\_PROGRESS state.

7.  Click**Lock**.
8.  Confirm the compliant retention policy and click **OK**.

    **Note:** 

    -   After this step, the policy is in the LOCKED state. You can click **Edit** to extend the retention period.
    -   If you try to delete or modify the data in a bucket protected by a compliant retention policy in the console, the `The file is locked and cannot be operated` error message is returned.

## Calculate the retention time for an object {#section_sxh_avf_fed .section}

You can calculate the retention time for a protected object by adding the last update time of the object and the retention period of the compliant retention policy. For example, a compliant retention policy is set for a bucket, and the retention period of the policy is 10 days. If the last update time of an object in the bucket is 01/03/2019 12:00, the object is protected until 11/03/2019 12:01. For more information about the compliant retention policy, see [Introduction](intl.en-US/Developer Guide/Compliant retention strategy/Introduction.md#ul_imr_nz5_cfb)

## Video {#section_ryr_7xc_pt7 .section}

You can see the following video to get familiar with the compliant retention policy.  

