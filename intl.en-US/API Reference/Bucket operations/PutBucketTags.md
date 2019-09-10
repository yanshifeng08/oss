# PutBucketTags {#concept_261782 .concept}

Adds tags for a bucket or modify the tags for a bucket.

## Request syntax {#section_xv4_vg0_dx2 .section}

``` {#codeblock_300_8y7_lrk}
PUT /?tagging HTTP/1.1
Date: GMT Date
Content-Length: ContentLengt
Authorization: SignatureValue
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<Tagging>
  <TagSet>
    <Tag>
      <Key>key1</Key>
      <Value>value1</Value>
    </Tag>
    <Tag>
      <Key>key2</Key>
      <Value>value2</Value>
    </Tag>
  </TagSet>
</Tagging>
```

## Request elements {#section_h8e_4wp_ykk .section}

|Element|Type|Required?|Description|
|-------|----|---------|-----------|
|Tagging|Container|Yes| Specifies the container used to configure the TagSet for the bucket.

 Sub-node: TagSet

 Parent node: None

 |
|TagSet|Container|Yes| Specifies the container used to store a set of tags for the bucket.

 Sub-node: Tag

 Parent node: Tagging

 |
|Tag|Container|Yes| Specifies the container used to configure a tag for the bucket.

 Sub-node: Key, Value

 Parent node: TagSet

 |
|Key|String|Yes| Specifies the key of a tag for the bucket.

-   The maximum size of a key is 64 bytes.
-   The key of a tag cannot be prefixed with `http://`, `https://`, or `Aliyun`.
-   The key of a tag must be UTF-8 encoded.
-   The key of a tag cannot be null.

 Sub-node: None

 Parent node: Tag

 |
|Value|String|No| Specifies the value of a tag for the bucket.

-   The maximum size of a tag value is 128 bytes.
-   The value of a tag must be UTF-8 encoded.
-   The value of a tag can be null.

 Sub-node: None

 Parent node: Tag

 |

## Detail analysis {#section_hj4_8jl_blm .section}

-   Only the bucket owner or authorized RAM users can add tags for a bucket. Otherwise, the 403 Forbidden error is returned with the error code: Access Denied.
-   You can add a maximum of 20 tags \(key-value pairs\) for a bucket.
-   If you call PutBucketTags to add tags for a bucket, the original tags added for the bucket are completely overwritten.

## Examples {#section_38w_mup_2lj .section}

-   Request example:

    ``` {#codeblock_8d1_xky_wrs}
    PUT /?tagging
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 20 Dec 2018 11:49:13 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    <Tagging>
      <TagSet>
        <Tag>
          <Key>testa</Key>
          <Value>testv1</Value>
        </Tag>
        <Tag>
          <Key>testb</Key>
          <Value>testv2</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```

-   Response example:

    ``` {#codeblock_78q_sw8_fo6}
    200 (OK)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5C1B138A109F4E405B2D8AEF
    date: Thu, 20 Dec 2018 11:59:06 GMT
    x-oss-server-time: 148
    connection: keep-alive
    ```


