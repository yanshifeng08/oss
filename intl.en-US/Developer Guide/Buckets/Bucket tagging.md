# Bucket tagging {#concept_1925905 .concept}

OSS allows you to configure bucket tagging to classify and manage buckets. For example, you can configure this feature to list only buckets that have specific tags.

Bucket tagging uses a key-value pair to identify buckets. You can manage multiple buckets that have specific tags.

-   Only the bucket owner and authorized users can configure tagging for the bucket. If other users attempt to configure tagging for the bucket, 403 Forbidden is returned with error code AccessDenied.
-   You can configure a maximum of 20 tags for a bucket.
-   The tag key is required. The tag key can be a maximum of 64 bytes in length and cannot start with `http://`, `https://`, or `Aliyun`.
-   The tag value is optional. The tag value can be a maximum of 128 bytes in length.
-   The key and value of the tag must be encoded in UTF-8.

## Implementation modes {#section_emm_sg4_9f0 .section}

|Implementation mode|Description|
|-------------------|-----------|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Bucket tagging.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Bucket tagging.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Bucket tagging.md#)|

## Instructions {#section_ldy_yd7_jrr .section}

After you add tags to buckets, you can manage multiple buckets that have the same tag. For example, you can list buckets that have the same tag and authorize RAM users to manage buckets that have the same tag.

-   Lists buckets that have specific tags

    You can list buckets that have specific tags. For more information, see the following SDK demos:

    -   [Java SDK](../../../../reseller.en-US/SDK Reference/Java/Bucket tagging.md#section_nt0_gl3_aey)
    -   [Python SDK](../../../../reseller.en-US/SDK Reference/Python/Bucket tagging.md#section_nt0_gl3_aey)
    -   [Go SDK](../../../../reseller.en-US/SDK Reference/Go/Bucket tagging.md#section_vzl_qwy_q7p)
-   Authorize RAM users to manage buckets that have specific tags

    When you have a large number of buckets, you can classify buckets based on tags and use the RAM policy to authorize specific users to manage buckets that have specific tags. For example, you can authorize user A to list buckets that have the tag configuration of keytest=valuetest. The RAM policy is as follows:

    ``` {#codeblock_tvs_ynp_e8o}
    {
        "Version": "1",
        "Statement": [
            {
                "Action": [
                    "oss:ListBuckets"
                ],
                "Resource": [
                    "acs:oss:*:1932487924256138:*"
                ],
                "Effect": "Allow",
                "Condition": {
                    "StringEquals": {
                        "oss:BucketTag/keytest": "valuetest"
                    }
                }
            }
        ]
    }
    ```


