# PutBucketLifecycle {#reference_xlw_dbv_tdb .reference}

Configures the lifecycle rules for a bucket. After lifecycle rules are configured for a bucket, OSS automatically deletes the objects that conform to the lifecycle rules on a regular basis. Only the owner of a bucket can initiate a PutBucketLifecycle request.

**Note:** 

-   If no lifecycle rules have been configured for a bucket, the PutBucketLifecycle operation creates a new lifecycle rule. If a lifecycle rule is configured for the bucket, this operation overwrites the previous lifecycle rule.
-   You can perform the PutBucketLifecycle operation to set the expiration time of objects and parts that are not completely uploaded in multipart upload tasks.

## Request syntax {#section_bfn_rcw_bz .section}

```
PUT /?lifecycle HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<LifecycleConfiguration>
  <Rule>
    <ID>RuleID</ID>
    <Prefix>Prefix</Prefix>
    <Status>Status</Status>
    <Expiration>
      <Days>Days</Days>
    </Expiration>
    <Transition>
      <Days>Days</Days>
      <Storage
>StorageClass</StorageClass>
    </Transition>
    <AbortMultipartUpload>
      <Days>Days</Days>
    </AbortMultipartUpload>
  </Rule>
</LifecycleConfiguration>
```

## Request elements {#section_vrs_vcw_bz .section}

|Element|Type|Required?|Description|
|-------|----|---------|-----------|
|CreatedBeforeDate|String|One of Days and CreatedBeforeDate is required.| Specifies the time before which the rules take effect. The date must conform to the ISO8601 format and always be UTC 00:00. For example: 2002-10-11T00:00:00.000Z indicates that objects updated before 2002-10-11T00:00:00.000Z are deleted or converted to another storage class, and objects updated after this time \(including this time\) are not deleted or converted.

 Parent node: Expiration or AbortMultipartUpload

 |
|Days|Positive integer|One of Days and CreatedBeforeDate is required.|Specifies how many days after the object is updated for the last time until the rules take effect. Parent node: Expiration

 |
|Expiration|Container|No|Specifies the expiration attribute of the lifecycle rules for the object. **Note:** For an object in a bucket with the versioning function enabled, this element only specifies the expiration period of the current version of the object.

 Sub-node: Days, CreatedBeforeDate, or ExpiredObjectDeleteMarker

 Parent node: Rule

 |
|AbortMultipartUpload|Container|No|Specifies the expiration attribute of the multipart upload tasks that are not complete. Sub-node: Days or CreatedBeforeDate

 Parent node: Rule

 |
|ID|String|No|Indicates the unique ID of a lifecycle rule. An ID is composed of 255 bytes at most. If the value of ID is not specified or null, OSS automatically generates a unique ID for the rule. Sub-node: None

 Parent node: Rule

 |
|LifecycleConfiguration|Container|Yes|Specifies the container used to store lifecycle configurations, which can store a maximum of 1,000 rules. Sub-node: Rule

 Parent node: None

 |
|Prefix|String|Yes|Specifies the prefix applicable to a rule. Only objects with a matching prefix are affected by the rule. A prefix cannot be overlapped. Sub-node: None

 Parent node: Rule

 |
|Rule|Container|Yes|Expresses a rule. **Note:** 

-   You cannot create a rule to convert the storage class of an Archive bucket.
-   The expiration time of an object must be longer than the time period after which the object is converted into the IA or Archive storage class.

 Sub-nodes: ID, Prefix, Status, and Expiration

 Parent node: LifecycleConfiguration

 |
|Status|String|Yes| If the value of this parameter is Enabled, OSS executes this rule regularly. If this value of this parameter is Disabled, OSS ignores this rule.

 Parent node: Rule

 Valid value: Enabled, Disabled

 |
|StorageClass|String|Required if Transition or NoncurrentVersionTransition is configured.|Specifies the storage class that objects that conform to the rule are converted into. **Note:** The storage class of the objects in a bucket of the IA storage class can be converted into Archive but cannot be converted into Standard.

 Value: IA, Archive

 Parent node: Transition

 |
|Transition|Container|No|Specifies the time when an object is converted to the IA or archive storage class during a valid life cycle. **Note:** An object of the Standard storage class in a bucket of the same storage class can be converted into the IA or Archive storage class. However, the required storage period for the object before it is converted to the Archive storage class must be longer than the required storage period before it is converted to the IA storage class. For example, if the value of Transition for IA is set to 30 days, the value of Transition for Archive must be longer than 30 days.

 |
|Tag|Container|No|Specifies the object tag applicable to a rule. Multiple tags are supported. Parent node: Rule

 Sub-nodes: Key and Value

 |
|Key|String|Required is Tag is configured.|Indicates the tag key. Parent node: Tag

 |
|Value|String|Required is Tag is configured.|Indicates the tag value. Parent node: Tag

 |
|NoncurrentDays|String|Required if NoncurrentVersionTransition orNoncurrentVersionExpiration is configured.|Specifies how many days after the current version of an object becomes the historical version until the lifecycle rule takes effect. Parent node: NoncurrentVersionTransition, NoncurrentVersionExpiration

 |
|NoncurrentVersionTransition|Container|No|Specifies when the storage class of a historical version of the specified object is converted to IA or Archive during the lifecycle of the object. **Note:** An object of the Standard storage class in a bucket of the same storage class can be converted into the IA or Archive storage class. However, the required storage period for the object before it is converted to the Archive storage class must be longer than the required storage period before it is converted to the IA storage class. For example, if the value ofNoncurrentVersionTransition for IA is set to 30 days, the value of Transition for Archive must be longer than 30 days.

 Sub-nodes: NoncurrentDays, StorageClass

 |
|NoncurrentVersionExpiration|Container|No|Specifies the expiration attributes of the lifecycle rules for the historical versions of the object. Sub-node: NoncurrentDays

 |
|ExpiredObjectDeleteMarker|String|No|Specifies whether expired delete markers are automatically deleted. Values:

-   true: Indicates that expired delete markers are automatically deleted.
-   false: Indicates that expired delete markers are not automatically deleted.

 Parent node: Expiration

 |

## Examples {#section_ox3_zcw_bz .section}

-   Request example when the versioning function for the bucket is not enabled:

    ``` {#codeblock_3g1_pg3_9yi}
    PUT /?lifecycle HTTP/1.1
    Host: oss-example.oss.aliyuncs.com
    Content-Length: 443
    Date: Thu , 8 Jun 2017 13:08:38 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:PYbzsdWSMrAIWAlMW8luWe****
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>delete objects and parts after one day</ID>
        <Prefix>logs/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <Days>1</Days>
        </Expiration>
        <AbortMultipartUpload>
          <Days>1</Days>
        </AbortMultipartUpload>
      </Rule>
      <Rule>
        <ID>transit objects to IA after 30, to Archive 60, expire after 10 years</ID>
        <Prefix>data/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>30</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
        <Transition>
          <Days>60</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
        <Expiration>
          <Days>3600</Days>
        </Expiration>
      </Rule>
      <Rule>
        <ID>transit objects to Archive after 60 days</ID>
        <Prefix>important/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>6</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
      </Rule>
      <Rule>
        <ID>delete created before date</ID>
        <Prefix>backup/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
        </Expiration>
        <AbortMultipartUpload>
          <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
        </AbortMultipartUpload>
      </Rule>
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

    Response example:

    ``` {#codeblock_j85_7es_oei}
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674A4D890*****
    Date: Thu , 8 Jun 2017 13:08:38 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Request example when the versioning function is enabled for the bucket:

    ``` {#codeblock_m72_r5t_dv7}
    PUT /?lifecycle HTTP/1.1
    Host: oss-example.oss.aliyuncs.com
    Content-Length: 336
    Date: Mon , 6 May 2019 15:23:20 GMT
    Authorization: OSSWnjl3fg9fdv8fg4b8sdf:Phuu8bBhS8dsff2a*****
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>delete example</ID>
        <Prefix>logs/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <ExpiredObjectDeleteMarker>true</ExpiredObjectDeleteMarker>
          <Days>1</Days>
        </Expiration>
        <NoncurrentVersionExpiration>
          <NoncurrentDays>5</NoncurrentDays>
        </NoncurrentVersionExpiration>
        <AbortMultipartUpload>
          <Days>1</Days>
        </AbortMultipartUpload>
      </Rule>
      <Rule>
        <ID>transit example</ID>
        <Prefix>data/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>30</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
        <NoncurrentVersionTransition>
          <NoncurrentDays>10</NoncurrentDays>
          <StorageClass>IA</StorageClass>
        </NoncurrentVersionTransition>
      </Rule>
    </LifecycleConfiguration>
    ```

    Response example:

    ``` {#codeblock_3np_xyl_mhq}
    HTTP/1.1 200 OK
    x-oss-request-id: 7D3435J59A9812B*****
    Date: Mon , 6 May 2019 15:23:20 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../intl.en-US/SDK Reference/Java/Manage lifecycle rules.md)
-   [Python](../../../../intl.en-US/SDK Reference/Python/Manage lifecycle rules.md)
-   [PHP](../../../../intl.en-US/SDK Reference/PHP/Manage lifecycle rules.md)
-   [Go](../../../../intl.en-US/SDK Reference/Go/Manage lifecycle rules.md)
-   [C](../../../../intl.en-US/SDK Reference/C/Manage lifecycle rules.md)
-   [.NET](../../../../intl.en-US/SDK Reference/. NET/Manage lifecycle rules.md)
-   [Node.js](../../../../intl.en-US/SDK Reference/Node. js/Manage lifecycle rules.md)
-   [Ruby](../../../../intl.en-US/SDK Reference/Ruby/Manage lifecycle rules.md)

## Error codes {#section_dsv_grs_qgb .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403|You do not have the permission to configure the lifecycle rules for a bucket. Only the owner of a bucket can initiate a PutBucketLifecycle request.|
|InvalidArgument|400| -   An object of the Standard storage class in a bucket of the same storage class can be converted into the IA or Archive storage class. You can configure individual rules for an object in a bucket of the Standard storage class at the same time to convert the object to the IA and Archive storage classes. However, the required storage period for the object before it is converted to the Archive storage class must be longer than the required storage period before it is converted to the IA storage class.
-   The expiration time of an object must be longer than the time period after which the object is converted into the IA or Archive storage class.

 |

