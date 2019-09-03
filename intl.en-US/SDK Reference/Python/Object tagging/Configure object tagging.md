# Configure object tagging {#concept_644876 .concept}

This topic describes how to configure object tagging.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

**Note:** 

-   You can add tags to an object when uploading it or add tags for an uploaded object. If add tags to an object that already has tags, the original tags are overwritten. For more information about adding tags, see [PutObjectTagging](../../../../intl.en-US/API Reference/Object operations/PutObjectTagging.md#).
-   To add tags to an object, you must have the permission to call PutObjectTagging.
-   The Last-Modified value of an object is not updated when its tags are changed.
-   A tag can contain letters, numbers, spaces, and the following symbols: + ‑ = . \_ : /

## Add tags to an object when uploading it {#section_chb_rrt_z4g .section}

-   Add tags to an object when uploading it by calling put\_object.

    Run the following code to add tags to an object when uploading it by calling put\_object:

    ``` {#codeblock_tg8_afo_7ox}
    # -*- coding: utf-8 -*-
    
    import oss2
    from oss2.headers import OSS_OBJECT_TAGGING
    
    # It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    object_name = '<yourObjectName>'
    
    # Configures the tags in the HTTP headers.
    headers = dict()
    headers[OSS_OBJECT_TAGGING] = "k1=v1&k2=v2&k3=v3"
    
    # Set the headers of put_object so that the tags are added to the object to be uploaded. 
    result = bucket.put_object(object_name, 'content', headers=headers)
    print('http response status: ', result.status)
    
    # View the tags added to the uploaded object.
    result = bucket.get_object_tagging('yourObjectName')
    for key in result.tag_set.tagging_rule:
        print('tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))
    ```

-   Add tags to an object when uploading it by calling multipart\_upload.

    Run the following code to add tags to an object when uploading it by calling multipart\_upload:

    ``` {#codeblock_zco_we3_nqx}
    # -*- coding: utf-8 -*-
    import os
    import oss2
    from oss2 import SizedFileAdapter, determine_part_size
    from oss2.models import PartInfo
    from oss2.headers import OSS_OBJECT_TAGGING
    
    # It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    object_name = '<yourObjectName>'
    filename = '<yourLocalFile>'
    
    total_size = os.path.getsize(filename)
    # Uses the determine_part_size method to determine the part size.
    part_size = determine_part_size(total_size, preferred_size=100 * 1024)
    
    # Configures the tags in the HTTP headers.
    headers = dict()
    headers[OSS_OBJECT_TAGGING] = 'k1=v1&k2=v2'
    
    # Initializes the multipart upload task.
    # Set the headers of init_multipart_upload so that the tags are added to the object to be uploaded.
    upload_id = bucket.init_multipart_upload(object_name, headers=headers).upload_id
    parts = []
    
    # Uploads the parts one by one.
    with open(filename, 'rb') as fileobj:
        part_number = 1
        offset = 0
        while offset < total_size:
            num_to_upload = min(part_size, total_size - offset)
        # SizedFileAdapter(fileobj, size) generates a new file object and recalculates the position where the next upload task starts.
            result = bucket.upload_part(object_name, upload_id, part_number,
                                        SizedFileAdapter(fileobj, num_to_upload))
            parts.append(PartInfo(part_number, result.etag))
    
            offset += num_to_upload
            part_number += 1
    
    # Completes the multipart upload task.
    result = bucket.complete_multipart_upload(object_name, upload_id, parts)
    print('http response status: ', result.status)
    
    # Views the tags added to the object.
    result = bucket.get_object_tagging(object_name)
    for key in result.tag_set.tagging_rule:
        print('tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))
    
    # Verifies the result of the multipart upload task.
    with open(filename, 'rb') as fileobj:
        assert bucket.get_object(object_name).read() == fileobj.read()
    ```

-   Add tags to an object when uploading it by calling append\_object.

    Run the following code to add tags to an object when uploading it by calling append\_object:

    ``` {#codeblock_4k3_suq_sno}
    # -*- coding: utf-8 -*-
    
    import oss2
    from oss2.headers import OSS_OBJECT_TAGGING
    
    # It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    object_name = '<yourObjectName>'
    
    # Configures the tags in the HTTP headers.
    headers = dict()
    headers[OSS_OBJECT_TAGGING] = "k1=v1&k2=v2&k3=v3"
    
    # Uploads the object by calling append_object. Set the headers of append_object so that the tags are added to the object to be uploaded.
    # Only the tags added when the object is uploaded by calling append_object for the first time take effect.
    result = bucket.append_object(object_name, 0, '<yourContent>', headers=headers)
    
    # Views the tags added to the object.
    result = bucket.get_object_tagging('yourObjectName')
    for key in result.tag_set.tagging_rule:
        print('tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))
    ```

-   Add tags to an object when uploading it by calling resumable\_upload.

    Run the following code to add tags to an object when uploading it by calling resumable\_upload:

    ``` {#codeblock_m08_mke_pj2}
    # -*- coding: utf-8 -*-
    import oss2
    from oss2.headers import OSS_OBJECT_TAGGING
    
    # It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    object_name = '<yourObjectName>'
    local_file = '<yourLocalFileName>'
    
    # Configures the tags in the HTTP headers.
    headers = dict()
    headers[OSS_OBJECT_TAGGING] = "k1=v1&k2=v2&k3=v3"
    
    # An object is uploaded by calling resumable_upload when the object size is equal to or larger than the value of the optional parameter multiple_threshold (10 MB by default). If the store parameter is not specified, a .py-oss-upload directory is created under HOME to store breakpoint information.
    # Set the headers of resumable_upload so that the tags are added to the object to be uploaded.
    
    
    oss2.resumable_upload(bucket, object_name, local_file, headers=headers)
    
    result = bucket.get_object_tagging(object_name)
    for key in result.tag_set.tagging_rule:
        print('object tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))
    ```


## Add tags to an uploaded object or modify the tags added to an object {#section_2wo_wfs_w1k .section}

Run the following code to add tags to an uploaded object or modify the tags added to an object:

``` {#codeblock_9mj_ybj_ngl}
# -*- coding: utf-8 -*-

import oss2
from oss2.models import Tagging, TaggingRule

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Creates a tagging rule.
rule = TaggingRule()
rule.add('key1', 'value1')
rule.add('key2', 'value2')

# Creates a tag.
tagging = Tagging(rule)

# Adds the tag to the object.
result = bucket.put_object_tagging('<yourObjectName>', tagging)
# Views the returned HTTP status code.
print('http response status:', result.status)
```

## Add tags to an object when copying it {#section_v75_lzs_v7a .section}

You can specify one of the following tagging rules when copying an object:

-   Copy \(default\): Copy the tags of the source object to the destination object.
-   Replace: Add the tags specified in the request to the destination object.

Examples of adding tags to objects of different sizes when copying them are described as follows:

-   Run the following code to add tags to an object smaller than 1 GB when directly copying it:

``` {#codeblock_il7_vw1_87y}
# -*- coding: utf-8 -*-

import oss2
from oss2.headers import OSS_OBJECT_TAGGING, OSS_OBJECT_TAGGING_COPY_DIRECTIVE

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Specifies the source object.
src_object_name = '<yourtSrcObjectName>'
# Specifies the destination object 1.
dest_object_name1 = '<yourtDestObjectName1>'
# Specifies the destination object 2.
dest_object_name2 = '<yourtDestObjectName2>'

# Specifies OSS_OBJECT_TAGGING_COPY_DIRECTIVE to COPY or keeps the default value in the HTTP headers, so that the tags added to the source object is also added to the destination object 1.
headers=dict()
headers[OSS_OBJECT_TAGGING_COPY_DIRECTIVE] = 'COPY'
bucket.copy_object(bucket.bucket_name, src_object_name, dest_object_name1, headers=headers)

# Specifies OSS_OBJECT_TAGGING_COPY_DIRECTIVE to REPLACE in the HTTP headers, so that the tags specified in OSS_OBJECT_TAGGING are added to the destination object 2.
headers[OSS_OBJECT_TAGGING_COPY_DIRECTIVE] = 'REPLACE'
headers[OSS_OBJECT_TAGGING] = "key4=value4&key5=value5"
bucket.copy_object(bucket.bucket_name, src_object_name, dest_object_name2, headers=headers)

# Views the tags added to the objet src_object_name.
result = bucket.get_object_tagging(src_object_name)
for key in result.tag_set.tagging_rule:
    print('src tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))

# Views the tags added to the destination object 1. The tags are the same as thos of the source object.
result = bucket.get_object_tagging(dest_object_name1)
for key in result.tag_set.tagging_rule:
    print('dest1 object tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))

# Views the tags added to the destination object 2. The added tags are specified in OSS_OBJECT_TAGGING.
result = bucket.get_object_tagging(dest_object_name2)
for key in result.tag_set.tagging_rule:
    print('dest2 object tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))
```

-   Run the following code to add tags to an object larger than 1 GB when copying it by calling multipart\_upload:

    ``` {#codeblock_uk7_pkv_q3m}
    # -*- coding: utf-8 -*-
    import os
    import oss2
    from oss2 import determine_part_size
    from oss2.models import PartInfo
    from oss2.headers import OSS_OBJECT_TAGGING
    
    # It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # Specifies the source object.
    src_object_name = '<yourSrcObject>'
    # Specifies the destination object.
    dest_object_name = '<yourDestObject>'
    
    # Obtains the size of the source object.
    head_info = bucket.head_object(src_object_name)
    total_size = head_info.content_length
    print('src object size:', total_size)
    
    # Calls determin_part_size to determine the part size. 
    part_size = determine_part_size(total_size, preferred_size=100 * 1024)
    print('part_size:', part_size)
    
    # Configures the tags in the HTTP headers.
    headers = dict()
    headers[OSS_OBJECT_TAGGING] = 'k3=v3'
    
    # Initializes a multipart upload task.
    # Set the headers of multipart_upload so that the tags are added to the destination object.
    upload_id = bucket.init_multipart_upload(dest_object_name, headers=headers).upload_id
    parts = []
    
    # Uploads the parts one by one.
    part_number = 1
    offset = 0
    while offset < total_size:
        num_to_upload = min(part_size, total_size - offset)
        end = offset + num_to_upload - 1;
        result = bucket.upload_part_copy(bucket.bucket_name, src_object_name, (offset, end), dest_object_name, upload_id, part_number)
        # Saves thee part information.
        parts.append(PartInfo(part_number, result.etag))
    
        offset += num_to_upload
        part_number += 1
    
    # Completes the multipart upload task.
    result = bucket.complete_multipart_upload(dest_object_name, upload_id, parts)
    
    # Obtains the metadata of the destination object.
    head_info = bucket.head_object(dest_object_name)
    
    # Views the size of the destination object.
    dest_object_size = head_info.content_length
    print('dest object size:', dest_object_size)
    
    # Compares the size of the destination object with that of the source object.
    assert dest_object_size == total_size
    
    # Views the tags added to the source object.
    result = bucket.get_object_tagging(src_object_name)
    for key in result.tag_set.tagging_rule:
        print('src tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))
    
    # Views the tags added to the destination object.
    result = bucket.get_object_tagging(dest_object_name)
    for key in result.tag_set.tagging_rule:
        print('dest tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))
    ```


## Add tags to a symbolic link object {#section_uoh_9p0_h6b .section}

Run the following code to add tags to a symbolic link object:

``` {#codeblock_40x_9yk_o9w}
# -*- coding: utf-8 -*-

import oss2
from oss2.headers import OSS_OBJECT_TAGGING

# It is highly risky to log on with the AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the China East 1 (Hangzhou) endpoint. Specify an actual endpoint based on your requirements.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

object_name = '<yourObjectName>'
symlink_name = '<yourSymlinkName>'

# Configures the tags in the HTTP headers.
headers = dict()
headers[OSS_OBJECT_TAGGING] = "k1=v1&k2=v2&k3=v3"

# Adds a symbolic link to the object.
# Set the headers of put_symbolic so that the tags are added to the symbolic link object.
result = bucket.put_symlink(object_name, symlink_name, headers=headers)
print('http response status: ', result.status)

# Views the tags added to the symbolic link object.
result = bucket.get_object_tagging(symlink_name)
for key in result.tag_set.tagging_rule:
    print('tagging key: {}, value: {}'.format(key, result.tag_set.tagging_rule[key]))
```

