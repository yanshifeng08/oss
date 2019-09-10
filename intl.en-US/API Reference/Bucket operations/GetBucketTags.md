# GetBucketTags {#concept_261796 .concept}

Obtains the tags for a bucket.

## Request syntax {#section_acx_42n_i42 .section}

``` {#codeblock_sr6_uxd_3rb}
GET /?tagging
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_az8_q7a_rrd .section}

|Element|Type|Description|
|-------|----|-----------|
|Tagging|Container| Indicates the container used to store the returned tags for a bucket.

 Parent node: None

 |
|TagSet|Container| Indicates the container used to store the returned tags for a bucket.

 Parent node: Tagging

 |
|Tag|Container| Indicates the container used to store the returned tags for a bucket.

 Parent node: TagSet

 |
|Key|String| Indicates the key of a tag.

 Parent node: Tag

 |
|Value|String| Indicates the value of a tag.

 Parent node: Tag

 |

## Detail analysis {#section_p8w_w0l_r34 .section}

-   If the target bucket does not exist, the 404 No Content error is returned with the error code: NoSuchBucket.
-   Only the bucket owner and authorized RAM users can view the tags for a bucket. Otherwise, the 403 Forbidden error is returned with the error code: AccessDenied.
-   If no tags are added for the bucket, OSS returns an XML message body in which value of Tagging is null.

## Examples {#section_71p_87d_fek .section}

-   Request example:

    ``` {#codeblock_pwn_k0h_aww}
    GET /?tagging
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 20 Dec 2018 13:09:13 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    ```

-   Response example:

    ``` {#codeblock_qke_6wq_10u}
    200 (OK)
    content-length: 237
    server: AliyunOSS
    x-oss-request-id: 5C1B2D24B90AD5490CFE368E
    date: Thu, 20 Dec 2018 13:12:21 GMT
    content-type: application/xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Tagging>
      <TagSet>
        <Tag>
          <Key>testa</Key>
          <Value>value1-test</Value>
        </Tag>
        <Tag>
          <Key>testb</Key>
          <Value>value2-test</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```


