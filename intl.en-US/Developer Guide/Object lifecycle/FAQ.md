# FAQ {#concept_263590 .concept}

This topic answers the problems that may occur when you use lifecycle rules to manage objects.

## Is there a minimum storage period for a lifecycle rule if the rule is set to convert storage class or delete an expired object? {#section_szp_4av_s3i .section}

If a lifecycle rule is set to convert the storage class \(from Standard to IA or Archive or from IA to Archive\) of objects, no minimum storage period is required for the rule. However, a lifecycle rule set to delete expired objects requires a minimum storage period. The minimum storage period is 30 days for objects of the IA storage class and 60 days for objects of the Archive storage class.

The minimum storage period for a lifecycle rule set to delete an expired object indicates the time period from the time when the object is created until the time when the object is deleted. If the object is modified \(such as by CopyObject or AppendObject operations\) during this period, the minimum storage period is calculated from the last modified date.

Example 1: The storage class of an object is converted from IA to Archive 10 days after it is created. The creation time of the object does not change. The converted object must be stored for at least 50 days.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4377/156819260934477_en-US.png)

Example 2: If the storage class of an object is converted from IA to Archive through a CopyObject operation 10 days after it is created, fees of 20 days are incurred for the deletion. The creation time of the object updates. The converted object must be stored for at least 60 days.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4377/156819260934481_en-US.png)

## Billing logic for requests {#section_hsj_utc_47n .section}

The conversion of storage class or the deletion of expired objects performed according to lifecycle rules generate requests. The requests are charged by OSS. For example:

-   If the storage class of 1000 objects are converted from Standard to Archive according to a lifecycle rule, 1000 POST requests are generated.
-   If 1000 expired objects are deleted according to a lifecycle rule, 1000 Delete requests are generated.

For more information, see [Billing methods](../../../../reseller.en-US/Pricing/Billing items.md#).

## Are the conversion of storage class or the deletion of expired objects performed according to lifecycle rules recorded? {#section_nee_gs8_jx3 .section}

The conversion of storage class or the deletion of expired objects performed according to lifecycle rules are recorded in logs. Fields in the logs are described as follows:

-   Operation
    -   CommitTransition: Indicates that the rule is set to convert the storage class of object.
    -   ExpireObject: Indicates that the rule is set to delete expired objects.
-   Sync Request

    lifecycle: Indicates the operation to be performed by the lifecycle rule.


