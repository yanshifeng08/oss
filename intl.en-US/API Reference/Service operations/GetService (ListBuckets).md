# GetService \(ListBuckets\) {#reference_ahf_k4t_tdb .reference}

Returns the information about all buckets owned by a user who sends a GET request to the OSS server, and "/" indicates the root directory.

## Request syntax {#section_glm_xkr_bz .section}

``` {#codeblock_lyv_r6z_7ic}
GET / HTTP/1.1
Host: oss.example.com
Date: GMT Date
Authorization: SignatureValue
```

## Request parameters {#section_z4x_zkr_bz .section}

When using GetService \(ListBuckets\), you can set the parameters described in the following table to limit the returned bucket list so that only a part of the results are returned.

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|prefix|String|No|Indicates that only the buckets with the specified prefix are returned. If this parameter is not specified, prefix information is not used to filter the returned buckets. Default value: None

 |
|marker|String|No|Indicates that the buckets whose names are after the marker in an alphabetical order are returned. If this parameter is not specified, all results are returned from the start. Default value: None

 |
|max-keys|String|No|Limits the maximum number of buckets returned for one request. If this parameter is not specified, the default value 100 is used. The value of this parameter cannot exceed 1,000. Default value: 100

 |

## Response elements {#section_fvy_clr_bz .section}

|Elements|Type|Description|
|--------|----|-----------|
|ListAllMyBucketsResult|Container|Indicates the container that stores the results returned for the GetService request. Sub-node: Owner and Buckets

 Parent node: None

 |
|Prefix|String|Indicates the prefix of the buckets returned for a request. This node is available only when not all buckets are returned. Parent node: ListAllMyBucketsResult

 |
|Marker|String|Indicates the start position of the current GetService \(ListBuckets\) operation. This node is available only when not all buckets are returned. Parent node: ListAllMyBucketsResult

 |
|Maxkeys|String|Indicates the maximum number of returned results for a request. This node is available only when not all buckets are returned. Parent node: ListAllMyBucketsResult

 |
|IsTruncated|Enumerated string|Indicates whether all results have been returned. “true” indicates that not all results are returned this time; “false” indicates that all results are returned this time. This node is available only when not all buckets are returned. Values: trueand false

 Parent node: ListAllMyBucketsResult

 |
|NextMarker|String|Indicates the marker for the next GetService\(ListBuckets\) request, which can be used to return the results that are not returned this time. This node is available only when not all buckets are returned. Parent node: ListAllMyBucketsResult

 |
|Owner|Container|Indicates the container that stores the information about the bucket owner. Parent node: ListAllMyBucketsResult

 |
|ID|String|Indicates the user ID of the bucket owner. Parent node: ListAllMyBucketsResult.Owner

 |
|DisplayName|String|Indicates the name of the bucket owner \(the same as ID currently\). Parent node: ListAllMyBucketsResult.Owner

 |
|Buckets|Container|Indicates the container that stores the information about multiple buckets. Sub-node: Bucket

 Parent node: ListAllMyBucketsResult

 |
|Bucket|Container|Indicates the container used to store the bucket information. Subnodes: Name, CreationDate, and Location

 Parent node: ListAllMyBucketsResult.Buckets

 |
|Name|String|Indicates the bucket name. Parent node: ListAllMyBucketsResult.Buckets.Bucket

 |
|CreateDate|Time \(format: yyyy-mm-ddThh:mm:ss.timezone, for example, 2011-12-01T12:27:13.000Z\)|Indicates the time when the bucket is created. Parent node: ListAllMyBucketsResult.Buckets.Bucket

 |
|Location|String|Indicates the data center in which a bucket is located. Parent node: ListAllMyBucketsResult.Buckets.Bucket

 |
|ExtranetEndpoint|String|Indicates the domain name used to access the bucket through the Internet. Parent node: ListAllMyBucketsResult.Buckets.Bucket

 |
|IntranetEndpoint|String|Indicates the domain name used by an ECS instance in the same region to access the bucket through the intranet. Parent node: ListAllMyBucketsResult.Buckets.Bucket

 |
|StorageClass|String|Indicates the storage class of the bucket. Value: Standard, IA, and Archive

**Note:** The Archive storage class is only supported in some regions.

 Parent node: ListAllMyBucketsResult.Buckets.Bucket

 |
|Comment|String|Indicates the comment on the bucket. Parent node: ListAllMyBucketsResult.Buckets.Bucket

 |

## Detail analysis {#section_ets_qlr_bz .section}

-   Only authenticated users can call the GetService API.
-   If a request does not include authentication information about the user \(that is, an anonymous access\), the 403 Forbidden error is returned with the error code “AccessDenied”.
-   When all buckets are returned, the XML file included in the response does not contain the following elements: Prefix, Marker, MaxKeys, IsTruncated, and NextMarker. If some results are not returned yet, the preceding elements are included, in which NextMarker is used to assign the marker for the successive query.

## Examples {#section_tm3_slr_bz .section}

-   Example 1

    Request example

    ``` {#codeblock_nwc_ald_2v8}
    GET / HTTP/1.1
    Date: Thu, 15 May 2014 11:18:32 GMT
    Host: oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS nxj7dtl******hcyl5hpvnhi:COS3OQkfQPnKmYZTEHYv2qUl5jI=
    ```

    Response example

    ``` {#codeblock_40p_78p_lzr}
    HTTP/1.1 200 OK
    Date: Thu, 15 May 2014 11:18:32 GMT
    Content-Type: application/xml
    Content-Length: 556
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    <?xml version="1.0" encoding="UTF-8"?>
    <ListAllMyBucketsResult>
      <Owner>
        <ID>51264</ID>
        <DisplayName>51264</DisplayName>
      </Owner>
      <Buckets>
        <Bucket>
          <CreationDate>2015-12-17T18:12:43.000Z</CreationDate>
          <ExtranetEndpoint>oss-cn-shanghai.aliyuncs.com</ExtranetEndpoint>
          <IntranetEndpoint>oss-cn-shanghai-internal.aliyuncs.com</IntranetEndpoint>
          <Location>oss-cn-shanghai</Location>
          <Name>app-base-oss</Name>
          <StorageClass>Standard</StorageClass>
          <Comment>app</Comment>
        </Bucket>
        <Bucket>
          <CreationDate>2014-12-25T11:21:04.000Z</CreationDate>
          <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
          <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
          <Location>oss-cn-hangzhou</Location>
          <Name>atestleo23</Name>
          <StorageClass>IA</StorageClass>
          <Comment></Comment>
        </Bucket>
      </Buckets>
    </ListAllMyBucketsResult>
    ```

-   Example 2

    Request example

    ``` {#codeblock_ar5_moh_699}
    GET /? prefix=xz02tphky6fjfiuc&max-keys=1 HTTP/1.1
    Date: Thu, 15 May 2014 11:18:32 GMT
    Host: oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS nxj7dtl******hcyl5hpvnhi:COS3OQkfQPnKmYZTEHYv2qUl5jI=
    ```

    Response example

    ``` {#codeblock_6rz_wyz_ux1}
    HTTP/1.1 200 OK
    Date: Thu, 15 May 2014 11:18:32 GMT
    Content-Type: application/xml
    Content-Length: 545
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D75
    <?xml version="1.0" encoding="UTF-8"?>
    <ListAllMyBucketsResult>
      <Prefix>xz02tphky6fjfiuc</Prefix>
      <Marker></Marker>
      <MaxKeys>1</MaxKeys>
      <IsTruncated>true</IsTruncated>
      <NextMarker>xz02tphky6fjfiuc0</NextMarker>
      <Owner>
        <ID>ut_test_put_bucket</ID>
        <DisplayName>ut_test_put_bucket</DisplayName>
      </Owner>
      <Buckets>
        <Bucket>
          <CreationDate>2014-05-15T11:18:32.000Z</CreationDate>
          <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
          <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
          <Location>oss-cn-hangzhou</Location>
          <Name>xz02tphky6fjfiuc0</Name>
          <StorageClass>Standard</StorageClass>
          <Comment>test</Comment>
        </Bucket>
      </Buckets>
    </ListAllMyBucketsResult>
    ```


## SDK {#section_egl_m2c_5gb .section}

The SDKs of this API are as follows:

-   [Java](../../../../reseller.en-US/SDK Reference/Java/Manage a bucket.md#section_vsh_fjf_2gb)
-   [Python](../../../../reseller.en-US/SDK Reference/Python/Bucket.md#)
-   [PHP](../../../../reseller.en-US/SDK Reference/PHP/Bucket.md#)
-   [Go](../../../../reseller.en-US/SDK Reference/Go/Bucket.md#ul_ofr_nbx_kfb)
-   [C](../../../../reseller.en-US/SDK Reference/C/Bucket.md#)
-   [.NET](../../../../reseller.en-US/SDK Reference/. NET/Manage a bucket.md#)
-   [iOS](../../../../reseller.en-US/SDK Reference/iOS/Manage a bucket.md#ul_xpj_skk_zgb)
-   [Node.js](../../../../reseller.en-US/SDK Reference/Node. js/Manage a bucket.md#section_l4m_ppk_lfb)
-   [Ruby](../../../../reseller.en-US/SDK Reference/Ruby/Manage buckets.md#)

