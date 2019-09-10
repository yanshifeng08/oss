# Compliant retention policy {#concept_lnj_4rl_cfb .concept}

OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a period of time. This strategy can be used in environments that are subject to regulations of the U.S. Securities and Exchange Commission \(SEC\) and Financial Industry Regulatory Authority, Inc. \(FINRA\).

**Note:** 

-   The compliant retention policy function is in the beta testing phase. To use this function, contact [After-sales technical support](https://selfservice.console.aliyun.com/ticket/createIndex).
-   In OSS, you can only configure a compliant retention policy for buckets.
-   OSS is the only cloud service in China that has passed the audit and certification of Cohasset Associates and can meet specific requirements for electronic data storage. OSS configured with compliant retention policies can be used in environments that are subject to regulations such as SEC Rule 17a-4\(f\), CFTC Rule 1.31\(c\)-\(d\), and FINRA Rule 4511\(c\). For more information, see [OSS Cohasset Assessment Report](http://gosspublic.alicdn.com/OSSCohassetAssessmentReport.pdf).

OSS provides strong compliant policies. You can configure time-based compliant retention policies for buckets. After a compliant retention policy is locked, you can read objects from or upload objects to buckets. However, no one can delete the pretected objects or compliant retention policies within the retention period. You can delete objects only after their retention period ends. The WORM strategy is suitable for industries such as the financing, insurance, health care, and security. OSS allows you to build a "compliant bucket in the cloud."

**Note:** During the protection period of objects in buckets, you can [Set lifecycle rules](reseller.en-US/Developer Guide/Object lifecycle/Manage lifecycle rules.md#) to convert the storage class to minimize costs while ensuring compliance.

## Implementation mode {#section_gq3_zz5_cfb .section}

Console: [Set a compliant retention policy](../../../../reseller.en-US/Console User Guide/Manage buckets/Configure a compliant retention policy.md#)

## Rules {#section_nk2_fz5_cfb .section}

You can add only one time-based compliant retention policy that has a protection period ranging from one day to 70 years.

Assume that you created a bucket named examplebucket on June 1, 2013, and uploaded the file1.txt, file2.txt, and file3.txt objects to the bucket at different points in time. Then, you created a compliant retention policy that has a protection period of five years on July 1, 2014. The following table describes the dates on which these objects were uploaded and are to expire.

|Object|Upload date|Expiration date|
|:-----|:----------|:--------------|
|file1.txt|June 1, 2013|May 31, 2018|
|file2.txt|July 1, 2014|June 30, 2019|
|file3.txt|September 30, 2018|September 29, 2023|

-   Implementation rule

    After a time-based compliant retention policy is created for a bucket, the policy is in the InProgress state by default, which is valid for 24 hours. Within the validity period, the retention policy protects the resources in the bucket.

    -   Within 24 hours after the compliant retention policy is enabled: If the retention policy is not locked, the bucket owner and authorized users can delete this policy. If the retention policy is locked, no one can delete this policy or shorten the protection period. The protection period can only be prolonged.
    -   24 hours after the compliant retention policy is enabled: If the retention policy is not locked, the policy becomes invalid.
    -   If you attempt to delete or modify data in the protected bucket, the OSS API returns `409 FileImmutable`.
-   Deletion rules
    -   A time-based compliant retention policy is a metadata property of a bucket. When a bucket is deleted, the compliant retention policy and ACL of the bucket are also deleted. To delete the retention policy of a bucket, you can delete the bucket when the bucket is empty.
    -   Within 24 hours after a retention policy is created for a bucket, if the retention policy is still not locked, the bucket owner and authorized users can delete the policy.
    -   If a bucket stores objects that are within the protection period, you cannot delete the bucket or its compliant retention policy.

## FAQ {#section_7vh_26q_rp3 .section}

-   What are the advantages of a compliant retention policy?

    The compliant retention policy provides compliant storage for your data. No one can delete or modify data within the protection period of the compliant retention policy. However, data protected through RAM policies and bucket policies may be modified and deleted.

-   What scenarios are suitable for a compliant retention policy?

    You can use the compliant retention policy when you want to store infrequently accessed important data that cannot be modified or deleted. Such data includes medical records, technical documents, and contracts. You can store these archive objects in a specified bucket and enable the compliant retention policy for the bucket.

-   Does a compliant retention policy apply to individual objects?

    Only buckets can have the compliant retention policy enabled. Folders and individual objects cannot have the compliant retention policy enabled.

-   How to delete a bucket protected by a compliant retention policy?
    -   If no object is stored in a bucket, the bucket can be deleted.
    -   If the bucket stores objects that are no longer within the protection period, the bucket cannot be deleted. In this case, you can delete all objects in the bucket before you delete the bucket.
    -   If the bucket stores objects that are within the protection period, the bucket cannot be deleted.
-   Are objects within the protection period of the compliant retention policy retained if my account has overdue OSS payments?

    Alibaba Cloud retains data in an overdue account based on the terms and conditions of your contract.

-   Can an authorized RAM user configure a compliant retention policy?

    All API operations related to the compliant retention policy are available. Related API operations support RAM policies. RAM users authorized through RAM policies can create or delete the compliant retention policy through the console, API, or SDK.


