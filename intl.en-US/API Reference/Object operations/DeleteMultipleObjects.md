# DeleteMultipleObjects {#reference_ydg_25v_wdb .reference}

Deletes multiple objects from the same bucket.

**Versioning**

You can use DeleteMultipleObjects to delete multiple objects in a bucket with versioning enabled. If you do not specify the versionId in the request, delete markers are added to the objects that you want to delete. If you specify the versionId in the request, the specified versions of the objects that you want to delete are permanently deleted.

## Request syntax {#section_h7y_zur_vt3 .section}

``` {#codeblock_xbq_tik_13a}
POST /?delete HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: ContentLength
Content-MD5: MD5Value
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<Delete>
  <Quiet>true</Quiet>
  <Object>
    <Key>key</Key>
  </Object>
…
</Delete>
```

## Request headers {#section_wks_emw_3l9 .section}

OSS verifies the received message body based on the following headers, and deletes the object only when the attributes of the message body conform to the headers.

|Header|Description|
|:-----|:----------|
|encoding-type|Specify the encoding type of the Key in the returned result. Currently, the URL encoding is supported. The Key adopts UTF-8 encoding, but the XML 1.0 Standard does not support parsing certain control characters, such as the characters with ASCII values from 0 to 10. In case that the Key contains control characters not supported by the XML 1.0 Standard, you can specify the encoding-type to encode the returned Key. Data type: String

 Default: None

 Optional value: url

 |

|Header|Type|Required?|Description|
|:-----|:---|:--------|:----------|
|Encoding-type|String|No| The Key parameter is UTF-8 encoded. If the Key parameter includes control characters which are not supported by the XML 1.0 standard, you can specify this header to encode the Key parameter in the returned result.

 Default value: None

 Valid value: url

 |
|Content-Length|String|Yes| Indicates the length of the HTTP message body.

 OSS verifies the received message body based on this header, and deletes the object only when the length of the message body is the same as this header.

 |
|Content-MD5|String|Yes| Content-MD5 is a string calculated with the MD5 algorithm. This header is used to check whether the content of the received message is consistent with that of the sent message. If this header is included in the request, OSS calculates the Content-MD5 of the received message body and compares it with the value of this header.

 **Note:** To obtain the value of this header, encrypt the message body of the DeleteMultipleObjects request using the MD5 algorithm to get a 128-bit byte array, and then base64-encode the byte array.

 |

## Request elements {#section_s7k_bt2_xym .section}

|Element|Type|Required?|Description|
|:------|:---|---------|:----------|
|Delete|Container|Yes|Specifies the container that stores the DeleteMultipleObjects request. Sub-node: One or more Objects, Quiet

 Parent node: None

 |
|Key|String|Yes|Specifies the name of the object to be deleted. Parent node: Object

 |
|Object|Container|Yes|Specifies the container that stores the information about the object. Sub-node: Key

 Parent node: Delete

 |
|Quiet|Enumerated string|Yes|Enables the Quiet response mode. DeleteMultipleObjects provides the following two response modes:

 -   Quiet: The message body of the response returned by OSS only includes objects that fail to be deleted. If all objects are deleted successfully, the response does not include a message body.
-   Verbose: The message body of the response returned by OSS includes the results of all deleted objects. This mode is used by default.

 Valid value: true\(enables Quiet mode\), false\(enables Verbose mode\)

 Default value: false

 Parent node: Delete

 |
|VersionId|String|No|Indicates the version of the object that you want to delete. Parent node: Object

 |

## Response elements {#section_dwg_xdl_x03 .section}

|Element|Type|Description|
|:------|:---|:----------|
|Deleted|Container|Specifies the container that stores the successfully deleted objects. Sub-node: Key

 Parent node: DeleteResult

 |
|DeleteResult|Container|Specifies the container that stores the returned results of the DeleteMultipleObjects request. Sub-node: Deleted

 Parent node: None

 |
|Key|String|Specifies the name of the deleted object. Parent node: Deleted

 |
|EncodingType|String|Specifies the encoding type for the returned results. If encoding-type is specified in the request, the Key is encoded in the returned result. Parent node: Container

 |
|DeleteMarker|Boolean|Indicates whether the specified version is a delete marker. Values:

-   true: The specified version is a delete marker.
-   false: The specified version is not a delete marker.

**Note:** This element is returned only when a delete marker is created or permanently deleted, and the value is true.

 |
|DeleteMarkerVersionId|String|Indicates the version ID of the delete marker. Parent node: Deleted

 |

## Examples {#section_6su_jeh_7vp .section}

-   Request example with Quiet mode disabled:

    ``` {#codeblock_9y4_rni_vkx}
    POST /?delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 29 Feb 2012 12:26:16 GMT
    Content-Length:151
    Content-MD5: ohhnqLBJFiKkPSBO1eNaUA==
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:+z3gBfnFAxBcBDgx27Y/jEfb****
    <?xml version="1.0" encoding="UTF-8"?>
    <Delete> 
      <Quiet>false</Quiet>  
      <Object> 
        <Key>multipart.data</Key> 
      </Object>  
      <Object> 
        <Key>test.jpg</Key> 
      </Object>  
      <Object> 
        <Key>demo.jpg</Key> 
      </Object> 
    </Delete>
    ```

    Response example:

    ``` {#codeblock_4xe_y5c_8qm}
    HTTP/1.1 200 OK
    x-oss-request-id: 78320852-7eee-b697-75e1-b6db0f4849e7
    Date: Wed, 29 Feb 2012 12:26:16 GMT
    Content-Length: 244
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <DeleteResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Deleted>
           <Key>multipart.data</Key>
        </Deleted>
        <Deleted>
           <Key>test.jpg</Key>
        </Deleted>
        <Deleted>
           <Key>demo.jpg</Key>
        </Deleted>
    </DeleteResult>
    ```

-   Request example with Quiet mode enabled:

    ``` {#codeblock_mpv_fsj_m3f}
    POST /?delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 29 Feb 2012 12:33:45 GMT
    Content-Length:151
    Content-MD5: ohhnqLBJFiKkPSBO1eNaUA==
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:WuV0Jks8RyGSNQrBca64kEEx****
    <?xml version="1.0" encoding="UTF-8"?>
    <Delete> 
      <Quiet>true</Quiet>  
      <Object> 
        <Key>multipart.data</Key> 
      </Object>  
      <Object> 
        <Key>test.jpg</Key> 
      </Object>  
      <Object> 
        <Key>demo.jpg</Key> 
      </Object> 
    </Delete>
    ```

    Response example:

    ``` {#codeblock_ixy_0in_xdo}
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    Date: Wed, 29 Feb 2012 12:33:45 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Request example when the versionId is not specified in the DeleteMultipartObject request:

    ``` {#codeblock_pij_r5d_plq}
    POST /?delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 04:20:03 GMT
    Content-MD5: xSLOYWaPC86RPwWXNiFeXg==
    Authorization: OSS xq39jyxyrddzvvh:/G5kxMIw1ilMQjPp2HkJEp5q****
    <?xml version="1.0" encoding="UTF-8"?>
    <Delete> 
      <Quiet>false</Quiet>  
      <Object> 
        <Key>multipart.data</Key>
      </Object>  
      <Object> 
        <Key>test.jpg</Key> 
      </Object>
    </Delete>
    ```

    Response example:

    ``` {#codeblock_1bn_4z6_ive}
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC1D73B7AEADE01700044F
    Date: Tue, 09 Apr 2019 04:20:03 GMT
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <DeleteResult>
        <Deleted>
           <Key>multipart.data</Key>
           <DeleteMarker>true</DeleteMarker>
           <DeleteMarkerVersionId>CAEQMhiBgIDXiaaB0BYiIGQzYmRkZGUxMTM1ZDRjOTZhNjk4YjRjMTAyZjhl****</DeleteMarkerVersionId>
        </Deleted>
        <Deleted>
           <Key>test.jpg</Key>
           <DeleteMarker>true</DeleteMarker>
           <DeleteMarkerVersionId>CAEQMhiBgIDB3aWB0BYiIGUzYTA3YzliMzVmNzRkZGM5NjllYTVlMjYyYWEy****</DeleteMarkerVersionId>
        </Deleted>
    </DeleteResult>
    ```

    **Note:** In the preceding example, the version IDs of the two objects to be deleted \(multipart.dat and test.jpg\) are not specified. Therefore, OSS adds delete markers for the two objects, and returns `<DeleteMarker>true</DeleteMarker>` and `<DeleteMarkerVersionId>XXXXXX</DseleteMarkerVersionId>`.

-   Request example when the versionId is specified to delete the specified version of the object:

    You must specify the key of the object when specifying the versionId.

    ``` {#codeblock_4og_fcl_dpq}
    POST /?delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:09:34 GMT
    Content-Length:151
    Content-MD5: 2Tpk+dL/tGyuSA+YCEuSVg==
    Authorization: OSS qac50zy2vbvbq4z:/pingCxyqfxc0+50Bfi2SX9c****
    <?xml version="1.0" encoding="UTF-8"?>
    <Delete> 
      <Quiet>false</Quiet>  
      <Object> 
        <Key>multipart.data</Key>
        <VersionId>CAEQNRiBgIDyz.6C0BYiIGQ2NWEwNmVhNTA3ZTQ3MzM5ODliYjM1ZTdjYjA4****</VersionId>
      </Object>
    </Delete>
    ```

    Response example:

    ``` {#codeblock_wby_mtr_tdc}
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC371EB7AEADE0170004BF
    Date: Tue, 09 Apr 2019 06:09:34 GMT
    Content-Length: 244
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <DeleteResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Deleted>
           <Key>multipart.data</Key>
           <VersionId>CAEQNRiBgIDyz.6C0BYiIGQ2NWEwNmVhNTA3ZTQ3MzM5ODliYjM1ZTdjYjA4****</VersionId>
        </Deleted>
    </DeleteResult>
    ```

    **Note:** In the preceding example, the key and the version ID of the object to be deleted are returned.

-   Request example when the versionId is specified to delete a delete marker:

    ``` {#codeblock_jpz_qb4_944}
    POST /?delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:14:50 GMT
    Content-Length: 178
    Content-MD5: dX9IFePFgYhmINvAhG30Bg==
    Authorization: OSS 9dfwrokza5gf1p7:iyl6IU6TcfFXcu5p0ds5dUdo****
    <?xml version="1.0" encoding="UTF-8"?>
    <Delete> 
      <Quiet>false</Quiet>  
      <Object> 
        <Key>multipart.data</Key>
        <VersionId>CAEQNRiBgICEoPiC0BYiIGMxZWJmYmMzYjE0OTQ0ZmZhYjgzNzkzYjc2NjZk****</VersionId>
      </Object>
    </Delete>
    ```

    Response example:

    ``` {#codeblock_xma_fno_10g}
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC385AB7AEADE01700052F
    Date: Tue, 09 Apr 2019 06:14:50 GMT
    Content-Length: 364
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <DeleteResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Deleted>
           <Key>demo.jpg</Key>
           <VersionId>CAEQNRiBgICEoPiC0BYiIGMxZWJmYmMzYjE0OTQ0ZmZhYjgzNzkzYjc2NjZk****</VersionId>
           <DeleteMarker>true</DeleteMarker>
           <DeleteMarkerVersionId>111111</DeleteMarkerVersionId>
        </Deleted>
    </DeleteResult>
    ```

    **Note:** 

    -   In the preceding example, the returned Key indicates the key of the deleted object and the returned VersionId indicates the deleted version of the object.
    -   DeleteMarker indicates that the deleted version is a delete marker. DeleteMarkerVersionId indicates the ID of the deleted version. In this case, the values of VersionId and DeleteMarkerVersionId are the same, and DeleteMarker and DeleteMarkerVersionId are returned together.

## SDK {#section_3wk_kgo_xn0 .section}

The SDKs of this API are as follows:

-   [Java](../../../../reseller.en-US/SDK Reference/Java/Manage objects/Delete objects.md)
-   [Python](../../../../reseller.en-US/SDK Reference/Python/Manage objects/Delete objects.md)
-   [PHP](../../../../reseller.en-US/SDK Reference/Python/Manage objects/Delete objects.md)
-   [Go](../../../../reseller.en-US/SDK Reference/Go/Manage objects/Delete objects.md)
-   [C](../../../../reseller.en-US/SDK Reference/C/Manage objects/Delete objects.md)
-   [.NET](../../../../reseller.en-US/SDK Reference/. NET/Manage objects/Delete objects.md)
-   [iOS](../../../../reseller.en-US/SDK Reference/iOS/Manage objects/Overview.md)
-   [Node.js](../../../../reseller.en-US/SDK Reference/Node. js/Manage objects/Overview.md)
-   [Browser.js](../../../../reseller.en-US/SDK Reference/Browser.js/Manage objects.md)
-   [Ruby](../../../../reseller.en-US/SDK Reference/Ruby/Manage objects.md)

## Error codes {#section_cno_jag_k67 .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidDigest|400|If you specify the Content-MD5 header in the request, OSS calculates the Content-MD5 of the message body and compares it with this header. If the two values are different, this error code is returned.|
|MalformedXML|400| -   A DeleteMultipleObjects request can contain a message body of up to 2 MB. If the size of the message body exceeds 2 MB, this error code is returned.
-   A DeleteMultipleObjects request can be used to delete up to 1,000 objects at a time. If the number of objects to be deleted at a time exceeds 1,000, this error code is returned.

 |

