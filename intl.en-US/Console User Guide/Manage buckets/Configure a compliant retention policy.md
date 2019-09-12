# Configure a compliant retention policy {#concept_lnq_csm_cfb .task}

OSS uses the compliant retention policy \(Write Once Read Many strategy\) to specify the protection period of objects in a bucket. No one can modify or delete a protected object during the protection period.

The compliant retention policy is in the beta testing phase. To use this policy, contact [After-sales technical support](https://selfservice.console.aliyun.com/ticket/createIndex).

**Note:** For more information about the compliant retention policy, see [Compliant retention policy](../../../../reseller.en-US/Developer Guide/Compliant retention policy.md#).

## Video tutorial {#section_ied_f4o_53r .section}

The following video provides a quick introduction to the compliance retention policy.  

## Procedure {#section_4qe_7zw_6ae .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  On the left-side bucket list, click the target bucket to open the overview page of the bucket.
3.  Click the **Basic Settings** tab, find the **Retention Policy** section, and click **Configure**.
4.  Click **Create Policy**. The Create Policy dialog box appears.
5.  Set **Retention Period** for the compliant retention policy. Valid values of **Retention Period** range from one day to 70 years.
6.  Click **OK**. After the policy is created, the status indicates "IN\_PROGRESS." You can click **Lock** or **Delete**.
7.  Click **Lock**.
8.  In the message that appears, click **OK**. 

    **Note:** 

    -   The policy status becomes LOCKED. You cannot delete the policy or shorten the protection period. However, you can click **Edit** to prolong the protection period for objects.
    -   During the protection period, data in the bucket is protected. If you attempt to delete or modify the data, error message `The operation failed. The object has been locked.` is displayed.

## Calculate the expiration time for objects {#section_cfe_g6m_js4 .section}

You can calculate the expiration time by adding the retention period and last update time of objects in buckets. For example, bucket A is configured with the compliant retention policy in which the retention period is 10 days. The last update time of an object in the bucket is 12:00 on March 1, 2019. The object expires at 12:01 on March 11, 2019. For more information about the rules of the compliant retention policy, see [Rules](reseller.en-US/Developer Guide/Compliant retention policy.md#ul_imr_nz5_cfb).

