# Object tagging {#concept_zxf_jpy_pgb .concept}

OSS allows you to use object tagging to classify objects. You can configure lifecycle rules for objects based on tags.

**Note:** You are charged when using object tagging. For more information, see [OSS pricing](https://www.aliyun.com/price/product?#/oss/detail).

Object tagging uses a key-value pair to identify objects. You can add tags to objects when and after you upload the objects.

-   A maximum of 10 tags can be set for each object. Tags associated with an object must have unique tag keys.
-   A tag key can be a maximum of 128 bytes in length. Each tag value can be a maximum of 256 bytes in length.
-   Keys and values are case-sensitive.
-   The key and value of the tag can contain letters, digits, spaces, and special characters such as

    + ‑ = . \_ : /

-   Only the bucket owner and authorized users have the read and write permissions on object tags. These permissions are independent of object ACLs.
-   During cross-region replication, object tags are also replicated to the destination bucket.

## Scenarios {#section_sfw_lxy_pgb .section}

Object tags are not limited to folders. You can perform the following operations on multiple objects that have a specific tag:

-   Configure lifecycle rules based on specific tags. For example, when you upload objects, you can configure tags for temporary objects that are periodically generated. After you configure lifecycle rules, you can delete these objects based on specific tags.
-   Use RAM to grant permissions to access objects that have specific tags.

## Implementation mode {#section_6n1_zde_54u .section}

|Implementation mode|Description|
|-------------------|-----------|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Object tagging/Configure object tagging.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Object tagging/Configure object tagging.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Object tagging/Configure object tagging.md#)|
|[C++ SDK](../../../../reseller.en-US/SDK Reference/C++/Object tagging/Configure object tagging.md#)|

## Instructions {#section_utm_dxd_qgb .section}

-   API operations related to object tagging
    -   [PutObjectTagging](../../../../reseller.en-US/API Reference/Object operations/PutObjectTagging.md#): Adds a tag to an object. If the target object already has a tag, the original tag is overwritten.
    -   [GetObjectTagging](../../../../reseller.en-US/API Reference/Object operations/GetObjectTagging.md#): reads tags of an object.
    -   [DeleteObjectTagging](../../../../reseller.en-US/API Reference/Object operations/DeleteObjectTagging.md#): deletes tags that are associated with an object.
    -   [PutObject](../../../../reseller.en-US/API Reference/Object operations/PutObject.md#): You can use the `x‑oss‑tagging` request header to specify tags when you upload an object.
    -   [InitiateMultipartUpload](../../../../reseller.en-US/API Reference/Multipart upload operations/InitiateMultipartUpload.md#): You can use the `x‑oss‑tagging` request header to specify tags when you initialize a multipart upload task.
    -   [CopyObject](../../../../reseller.en-US/API Reference/Object operations/CopyObject.md#): You can use the `x-oss-tagging-directive` request header to specify whether to replicate tags of source objects. You can use the `x‑oss‑tagging` request header to specify tags of destination objects.
    -   [GetObject](../../../../reseller.en-US/API Reference/Object operations/GetObject.md#): If you have permissions to read the object tags, the count of tags is included in the `x‑oss‑tagging‑count` response header.
    -   [HeadObject](../../../../reseller.en-US/API Reference/Object operations/HeadObject.md#): If you have permissions to read the object tags, the count of tags is included in the `x‑oss‑tagging‑count` response header.
-   Permission description

    Users, roles, or services that perform operations on tags must have the following permissions:

    -   GetObjectTagging: the permission to obtain object tags. If you have this permission, you can view existing tags of objects.
    -   PutObjectTagging: the permission to replace tags on objects. If you have this permission, you can replace tags on objects.
    -   DeleteObjectTagging: the permission to delete object tags. If you have this permission, you can delete object tags.

## Object tagging and lifecycle management {#section_bmt_f4t_9p7 .section}

When you configure lifecycle rules, you can specify a filter to select a subset of objects to which the rule applies. You can specify a filter based on the key name prefixes, object tags, or both.

-   If you set tag conditions, the rule only applies to objects that meet the tag key and value conditions.
-   If you set object key prefixes and multiple object tags in one lifecycle rule, the rule only applies to objects that meet the object key prefixes and object tags.

Examples:

``` {#codeblock_rvy_l3x_3fi}
<LifecycleConfiguration>
<Rule>
<ID>r1</ID>
<Prefix>rule1</Prefix>
<Tag><Key>xx</Key><Value>1</Value></Tag>
<Tag><Key>yy</Key><Value>2</Value></Tag>
<Status>Enabled</Status>
<Expiration>
<Days>30</Days>
</Expiration>
</Rule>
<Rule>
<ID>r2</ID>
<Prefix>rule2</Prefix>
<Tag><Key>xx</Key><Value>1</Value></Tag>
<Status>Enabled</Status>
<Transition>
<Days>60</Days>
<StorageClass>Archive</StorageClass>
</Transition>
</Rule>
</LifecycleConfiguration>
```

In the preceding rules,

-   Objects that are prefixed with rule1 and whose tag configurations are xx=1 and yy=2 are deleted after 30 days.
-   The storage class of objects that are prefixed with rule2 and whose tag configuration is xx=1 is converted to Archive after 60 days.

**Note:** For more information, see [Manage object lifecycle](reseller.en-US/Developer Guide/Object lifecycle/Manage lifecycle rules.md#).

## Object tagging and RAM policies {#section_p6p_nw9_al7 .section}

You can authorize RAM users to manage object tags. You can also authorize RAM users to manage objects that have specific tags.

-   Authorize RAM users to manage object tags

    You can authorize RAM users to manage tags of all or specific objects. If user A is authorized to set object tagging to allow=yes, this user can add the tag configuration of allow=yes for objects. An example of the RAM policy is as follows:

    ``` {#codeblock_w2m_hmo_h7v}
    {
      "Version": "1",
      "Statement": [
        {
          "Action": "oss:PutObjectTagging",
          "Effect": "Allow",
          "Resource": "*",
          "Condition": {
            "StringEquals": {
              "oss:RequestObjectTag/allow": [
                "yes"
              ]
            }
          }
        }
      ]
    }
    ```

    **Note:** After the RAM user is authorized to configure tags for specified objects, this user can configure tags only for existing objects.

-   Authorize RAM users to manage objects that have specific tags

    You can authorize RAM users to manage all objects that have specific tags. For example, you can authorize user A to access all objects that have the tag configuration of allow=yes. An example of the RAM policy is as follows:

    ``` {#codeblock_m3w_d0n_l9a}
    {
      "Version": "1",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": "oss:GetObject",
          "Resource": "*",
          "Condition": {
            "StringEquals": {
              "oss:ExistingObjectTag/allow": [
                "yes"
              ]
            }
          }
        }
      ]
    }
    ```


