# Object tagging and lifecycle rules {#concept_645503 .concept}

Lifecycle rules can take effect on objects with specified prefixes or tags. You can also specify prefixes and tags as the condition of a lifecycle rule at the same time.

**Note:** If you set a specified tag as the condition of a lifecycle rule, the rule applies to an object only when the key and value in the tag of the object both match the specified tag. If you specify a prefix and multiple tags as the condition of a lifecycle rule, the rule applies to an object only when the prefix and tags of the object match the prefix and all tags specified in the rule.

## Set tags as the matching condition of a lifecycle rule {#section_4hq_wf5_45h .section}

Run the following code to set tags as the matching condition of a lifecycle rule:

``` {#codeblock_cdx_key_2pa}
# -*- coding: utf-8 -*-
import oss2
import datetime
from oss2.models import (LifecycleExpiration, LifecycleRule, 
                        BucketLifecycle,AbortMultipartUpload, 
                        TaggingRule, Tagging, StorageTransition)

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Sets an expiration rule for objects so that objects are expired three days after they are modified for the last time.
rule1 = LifecycleRule('rule1', 'tests/',
                      status=LifecycleRule.ENABLED,
                      expiration=LifecycleExpiration(days=3))

# Sets an expiration rule for objects so that objects created before the specified date are expired.
rule2 = LifecycleRule('rule2', 'logging-',
                      status=LifecycleRule.DISABLED,
expiration=LifecycleExpiration(created_before_date=datetime.date(2018, 12, 12)))

# Sets an expiration rules for parts so that parts are expired 3 days after they are generated.
rule3 = LifecycleRule('rule3', 'tests1/',
                      status=LifecycleRule.ENABLED,
            abort_multipart_upload=AbortMultipartUpload(days=3))

# Sets an expiration rules for parts so that parts generated before the specified date are expired.
rule4 = LifecycleRule('rule4', 'logging1-',
                      status=LifecycleRule.DISABLED,
                      abort_multipart_upload = AbortMultipartUpload(created_before_date=datetime.date(2018, 12, 12)))

# Sets tags as the matching condition.
tagging_rule = TaggingRule()
tagging_rule.add('key1', 'value1')
tagging_rule.add('key2', 'value2')
tagging = Tagging(tagging_rule)

# Sets a storage class conversion rule for objects so that the storage class of an object is converted to Archive 365 days after it is modified for the last time.
# Different from the preceding rules, rule5 includes tags that are specified as the matching condition, so that it only applies to objects with the key1=value1 and key2=value2 tags at the same time.
rule5 = LifecycleRule('rule5', 'logging2-',
                      status=LifecycleRule.ENABLED,
                      storage_transitions=[StorageTransition(days=356, storage_class=oss2.BUCKET_STORAGE_CLASS_ARCHIVE)],
                      tagging = tagging)

lifecycle = BucketLifecycle([rule1, rule2, rule3, rule4, rule5])

bucket.put_bucket_lifecycle(lifecycle)
```

## View the tags set as the matching condition of a lifecycle rule {#section_5wx_5e4_cri .section}

Run the following code to view the tags set as the matching condition of a lifecycle rule:

``` {#codeblock_k5v_30z_aps}
# -*- coding: utf-8 -*-
import oss2

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Views the lifecycle rules set for a bucket.
lifecycle = bucket.get_bucket_lifecycle()

for rule in lifecycle.rules:
    # Views the expiration rules for parts.
    if rule.abort_multipart_upload is not None:
        print('id={0}, prefix={1}, tagging={2}, status={3}, days={4}, created_before_date={5}'
                .format(rule.id, rule.prefix, rule.tagging, rule.status,
                    rule.abort_multipart_upload.days,
                    rule.abort_multipart_upload.created_before_date))

    # Views the expiration rules for objects
    if rule.expiration is not None:
        print('id={0}, prefix={1}, tagging={2}, status={3}, days={4}, created_before_date={5}'
                .format(rule.id, rule.prefix, rule.tagging, rule.status,
                    rule.expiration.days,
                    rule.expiration.created_before_date))
  # Views the storage class conversion rule.
    if len(rule.storage_transitions) > 0: 
        storage_trans_info = ''
        for storage_rule in rule.storage_transitions:
            storage_trans_info += 'days={0}, created_before_date={1}, storage_class={2} **** '.format(
                storage_rule.days, storage_rule.created_before_date, storage_rule.storage_class)

        print('id={0}, prefix={1}, tagging={2}, status={3},, StorageTransition={4}'
                .format(rule.id, rule.prefix, rule.tagging, rule.status, storage_trans_info))
```

