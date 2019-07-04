# GetBucketWebsite {#reference_wvy_s4w_tdb .reference}

Queries the static website hosting status and routing rules for a bucket.

## Request syntax {#section_jbk_tgw_bz .section}

``` {#codeblock_6yp_tbd_glr}
GET /? website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_aqn_ygw_bz .section}

|Element|Type|Description|
|WebsiteConfiguration|Container| Root node

 Parent node: None

 |
|IndexDocument|Container| Specifies the container for the default home page.

 Parent node: WebsiteConfiguration

 |
|Suffix|String| Specifies the default home page.

 Parent node: IndexDocument

 |
|ErrorDocument|Container| Specifies the container for the 404 page.

 Parent node: WebsiteConfiguration

 |
|Key|Container| 404 page

 Parent node: ErrorDocument

 |
|RoutingRules|Container| Specifies the container for the RoutingRule.

 Parent node: WebsiteConfiguration

 |
|RoutingRule|Container| Specifies routing rules or mirroring back-to-origin rules.

 Parent node: RoutingRules

 |
|RuleNumber|Positive integer| Specifies the sequence number used to match and execute routing rules. Routing rules are matched according to the sequence numbers. A routing rule that matches the number is executed and the following rules are not executed.

 Parent node: RoutingRule

 |
|Condition|Container| Specifies the matching conditions.

 If a routing rule meets all the conditions, it is executed. The elements in the bucket are in the AND relationship, that is, a routing rule must meet all the conditions before it can be considered matched.

 Parent node: RoutingRule

 |
|KeyPrefixEquals|String| Indicates that only objects that match the prefix can match the rule.

 Parent node: Condition

 |
|HttpErrorCodeReturnedEquals|HTTP status code| Indicates that the rule can be matched only when the object returns the specified status code when being accessed. If the routing rule is a mirroring back-to-source rule, this status code must be 404.

 Parent node: Condition

 |
|IncludeHeader|Container| Indicates that the routing rule can be matched only when the specified header is included in the request and the header equals the specified value. You can specify a maximum 5 of the same container.

 Parent node: Condition

 |
|Key|String| Indicates that the rule is matched only when this header is included in the request and the header value equals the value specified by Equals.

 Parent node: IncludeHeader

 |
|Equals|String| Indicates that the rule can be matched only when the header specified by Key is included in the request and the header value equals the specified value.

 Parent node: IncludeHeader

 |
|Redirect|Container| Specifies the actions to perform after the rule is matched.

 Parent node: RoutingRule

 |
|RedirectType|String| Specifies the redirecting type, which has the following available values:

 -   Mirror \(mirroring back-to-origin\)
-   External \(external redirection, that is, OSS returns a 3xx request which redirects the access to another IP address.\)
-   Internal \(internal redirection, that is, OSS redirects the access from object1 to object2 based on the rule. In this case, the user accesses object2 but not object1.\)
-   AliCDN \(AliCDN redirection, which is used for AliCDN. Unlike the External type, OSS adds an additional header to the request. After identifying the header, AliCDN redirects the access to the specified IP address and returns the obtained data but not the 3xx redirecting request to the user.\)

 Parent node: Redirect

 |
|PassQueryString|Bool| Indicates whether the request parameter is carried when the redirection or mirroring back-to-origin is performed.

 For example, if the parameter " ?a=b&c=d" is carried in a request to OSS and this element is set to true, this parameter is added to the Location header when the rule is 302 redirection. For example, if the request includes "Location:www.test.com ?a=b&c=d” and the redirecting type is mirroring back-to-origin, the parameter "a=b&c=d" is also carried in the back-to-origin request.

 Default value: false

 Parent node: Redirect

 |
|MirrorURL|String| Indicates the IP address of the origin site in the mirroring back-to-origin. This element takes effect only when the value of RedirectType is Mirror.

 If the MirrorURL starts with http:// or https://, it must be ended with a slash \(/\). OSS constructs the back-to-origin URL by adding the target object to the MirrorURL. For example, if MirrorURL is set to `http://www.test.com/` and the object to be accessed is "myobject", the back-to-origin URL is `http://www.test.com/dir1/myobject`. If MirrorURL is set to `http://www.test.com/dir1/`, the back-to-origin URL is `http://www.test.com/dir1/myobject`.

 Parent node: Redirect

 |
|MirrorPassQueryString|Bool| This element plays the same role as PassQueryString and has a higher priority than PassQueryString. However, this element take effects only when the RedirectType is Mirror.

 Default value: false

 Parent node: Redirect

 |
|MirrorFollowRedirect|Bool| Indicates whether the access is redirected to the specified Location if the origin site returns a 3xx status code when receiving a back-to-origin request.

 For example, the origin site returns a 302 status code and specifies the Location when receiving a mirroring back-to-origin request. In this case, if the value of MirrorFollowRedirect is true, OSS continues to send requests to the IP address specified by the Location. \(A request can be redirected for a maximum of 10 times. If the request is redirected for more than 10 times, a mirroring back-to-origin failure message is returned.\) If the value of MirrorFollowRedirect is false, OSS returns a 302 status code and passes through the Location. This element takes effect only when the value of RedirectType is Mirror.

 Default value: true

 Parent node: Redirect

 |
|MirrorCheckMd5|Bool| Indicates whether OSS performs an MD5 check on the body of the response returned by the origin site.

 When the value of this element is true and the response returned by the origin site includes a Content-Md5 header, OSS checks whether the MD5 checksum of the obtained data matches the header. If not, OSS does not store the data. This element takes effect only when the value of RedirectType is Mirror.

 Default value: false

 Parent node: Redirect|
|MirrorHeaders|Container| Specifies the header carried in the response returned by the origin site. This element takes effect only when the value of RedirectType is Mirror.

 Parent node: Redirect

 |
|PassAll|Bool| Indicates whether OSS passes through all headers \(except for reserved headers and the headers starting with oss-/x-oss-/x-drs-\) to the origin site. This element takes effect only when the value of RedirectType is Mirror.

 Default value: false

 Parent node: MirrorHeaders

 |
|Pass|String| Specifies the headers that are passed through to the origin site. A maximum of 10 headers can be specified. The maximum length of a header is 1,024 bytes. The character set of this element is: 0-9, A-Z, a-z, and dash. This element takes effect only when the value of RedirectType is Mirror.

 Parent node: MirrorHeaders

 |
|Remove|String| Specifies the headers that cannot to be passed through to the origin site. A maximum of 10 headers can be specified \(including repeated headers\). This element is used together with PassAll. The maximum length of a header is 1,024 bytes. The character set of this element is the same as that of Pass. This element takes effect only when the value of RedirectType is Mirror.

 Parent node: MirrorHeaders

 |
|Set|Container| Specifies headers that are sent to the origin site. The specified headers are configured in the data returned by the origin site no matter whether they are carried in the request. A maximum of 10 groups of headers can be configured \(including repeated headers\). This element takes effect only when the value of RedirectType is Mirror.

 Parent node: MirrorHeaders

 |
|Key|String| Specifies the key of the header. The maximum length of a key is 1,024 bytes. The character set of this element is the same as that of Pass. This element takes effect only when the value of RedirectType is Mirror.

 Parent node: Set

 |
|Value|String| Specifies the value of the header. The maximum length of the value is 1,024 bytes. The character "\\r\\n" is not allowed in the element. This element takes effect only when the value of RedirectType is Mirror.

 Parent node: Set

 |
|Protocol|String| Specifies the protocol used for redirections. For example, the Location header is `https://www.test.com/test` if the requested object is test, the request is redirected to `www.test.com`, and the value of Protocol is https.

 This element takes effect only when the value of RedirectType is External or AliCDN.

 Values: http, https

 Parent node: Redirect

 |
|HostName|String| Specifies the domain name used in redirections, which must comply with the specifications for domain names. For example, the Location header is `https://www.test.com/test` if the requested object is test, the value of Protocol is https, and the Hostname is specified to `www.test.com`. This element takes effect only when the value of RedirectType is External or AliCDN.

 Parent node: Redirect

 |
|HttpRedirectCode|HTTP status code| Specifies the returned status code in redirections. This element takes effect only when the value of RedirectType is External or AliCDN.

 Values: 301, 302, 307

 Parent node: Redirect

 |
|ReplaceKeyPrefixWith|String| Indicates the string used to replace the prefix of the requested object name in redirections. If the prefix of the object name is empty, this string is added before the object name. The ReplaceKeyWith and ReplaceKeyPrefixWith elements cannot be set simultaneously.

 For example, if KeyPrefixEquals is set to abc/ and ReplaceKeyPrefixWith is set to def/, the Location header for an object named abc/test.txt is `http://www.test.com/def/test.txt`.

 This element takes effect only when the value of RedirectType is Internal, External, or AliCDN.

 Parent node: Redirect

 |
|ReplaceKeyWith|String| Indicates the string used to replace the requested object name in redirections. This element can be a variable. \(The $\{key\} variable indicating the object name in the request is supported.\) The ReplaceKeyWith and ReplaceKeyPrefixWith elements cannot be set simultaneously.

 For example, if ReplaceKeyWith is set to prefix/$\{key\}.suffix, the Location header for an object named test is `http://www.test.com/prefix/test.suffix`.

 This element takes effect only when the value of RedirectType is Internal, External, or AliCDN.

 Parent node: Redirect

 |

## Examples {#section_wld_hhw_bz .section}

Request example

``` {#codeblock_072_alf_n90}
    Get /? website HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com   
    Date: Thu, 13 Sep 2012 07:51:28 GMT
    Authorization: OSS qn6qrrqx******k53otfjbyc: BuG4rRK+zNh******1NNHD39zXw=
			
```

Response example with logging rules configured

``` {#codeblock_2k1_rl7_g5e}
HTTP/1.1 200
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:51:28 GMT
Connection: keep-alive
Content-Length: 218  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<WebsiteConfiguration xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
<IndexDocument>
<Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>error.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```

Return example with logging rules not set

``` {#codeblock_3i8_edj_gke}
HTTP/1.1 404 
x-oss-request-id: 534B371674E88A4D8906008B
Date: Thu, 13 Sep 2012 07:56:46 GMT
Connection: keep-alive
Content-Length: 308  
Server: AliyunOSS

<? xml version="1.0" encoding="UTF-8"? >
<Error xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Code>NoSuchWebsiteConfiguration</Code>
    <Message>The specified bucket does not have a website configuration. </Message>
    <BucketName>oss-example</BucketName>
    <RequestId>505191BEC4689A033D00236F</RequestId>
    <HostId>oss-example.oss-cn-hangzhou.aliyuncs.com</HostId>
</Error>
```

Complete code

``` {#codeblock_4ym_qcx_3wc}
GET /? website HTTP/1.1
Date: Fri, 27 Jul 2018 09:07:41 GMT
Host: test.oss-cn-hangzhou-internal.aliyuncs.com
Authorization: OSS a1nBN******QMf8u:0Jzamofmy******sU9HUWomxsus=
User-Agent: aliyun-sdk-python-test/0.4.0

HTTP/1.1 200 OK
Server: AliyunOSS
Date: Fri, 27 Jul 2018 09:07:41 GMT
Content-Type: application/xml
Content-Length: 2102
Connection: keep-alive
x-oss-request-id: 5B5AE0DD2F7938C45FCED4BA
x-oss-server-time: 47

<? xml version="1.0" encoding="UTF-8"? >
<WebsiteConfiguration>
<IndexDocument>
<Suffix>index.html</Suffix>
</IndexDocument>
<ErrorDocument>
<Key>error.html</Key>
</ErrorDocument>
<RoutingRules>
<RoutingRule>
<RuleNumber>1</RuleNumber>
<Condition>
<KeyPrefixEquals>abc/</KeyPrefixEquals>
<HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
</Condition>
<Redirect>
<RedirectType>Mirror</RedirectType>
<PassQueryString>true</PassQueryString>
<MirrorURL>http://www.test.com/</MirrorURL>
<MirrorPassQueryString>true</MirrorPassQueryString>
<MirrorFollowRedirect>true</MirrorFollowRedirect>
<MirrorCheckMd5>false</MirrorCheckMd5>
<MirrorHeaders>
<PassAll>true</PassAll>
<Pass>myheader-key1</Pass>
<Pass>myheader-key2</Pass>
<Remove>myheader-key3</Remove>
<Remove>myheader-key4</Remove>
<Set>
<Key>myheader-key5</Key>
<Value>myheader-value5</Value>
</Set>
</MirrorHeaders>
</Redirect>
</RoutingRule>
<RoutingRule>
<RuleNumber>2</RuleNumber>
<Condition>
<IncludeHeader>
<Key>host</Key>
<Equals>test.oss-cn-beijing-internal.aliyuncs.com</Equals>
</IncludeHeader>
<KeyPrefixEquals>abc/</KeyPrefixEquals>
<HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
</Condition>
<Redirect>
<RedirectType>AliCDN</RedirectType>
<Protocol>http</Protocol>
<HostName>www.test.com</HostName>
<PassQueryString>false</PassQueryString>
<ReplaceKeyWith>prefix/${key}.suffix</ReplaceKeyWith>
<HttpRedirectCode>301</HttpRedirectCode>
</Redirect>
</RoutingRule>
</RoutingRules>
</WebsiteConfiguration>
```

## SDK {#section_dr0_713_b1y .section}

The SDKs of this API are as follows:

-   [Java](../../../../reseller.en-US/SDK Reference/Java/Static website hosting.md)
-   [Python](../../../../reseller.en-US/SDK Reference/Python/Static website hosting.md)
-   [PHP](../../../../reseller.en-US/SDK Reference/PHP/Static website hosting.md)
-   [Go](../../../../reseller.en-US/SDK Reference/Go/Static website hosting.md)
-   [C++](../../../../reseller.en-US/SDK Reference/C++/Static website hosting.md)
-   [C](../../../../reseller.en-US/SDK Reference/C/Static website hosting.md)
-   [.NET](../../../../reseller.en-US/SDK Reference/. NET/Static website hosting.md)
-   [Node.js](../../../../reseller.en-US/SDK Reference/Node. js/Static website hosting.md)
-   [Ruby](../../../../reseller.en-US/SDK Reference/Ruby/Static website hosting.md)

## Error codes {#section_7yh_lsk_jbh .section}

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The target bucket does not exist.|
|AccessDenied|403|You do not have the permission to view the static website hosting status of the bucket. Only the owner of a bucket can view the static website hosting status of the bucket.|
|NoSuchWebsiteConfiguration|404|Static website hosting is not configured for the target bucket.|

