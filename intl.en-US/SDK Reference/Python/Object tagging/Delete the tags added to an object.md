# Delete the tags added to an object {#concept_645158 .concept}

This topic describes how to delete the tags added to an object.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

Run the following code to delete the tags added to an object:

``` {#codeblock_ddw_q1r_bkg}
# -*- coding: utf-8 -*-

import oss2
from oss2.models import Tagging, TaggingRule

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Deletes the tags added to the object.
result = bucket.delete_object_tagging('<yourObjectName>')
print('http response status: ', result.status)
```

For more information about how to delete the tags added to an object, see [DeleteObjectTagging](../../../../intl.en-US/API Reference/Object operations/DeleteObjectTagging.md#).

