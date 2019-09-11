# website {#concept_744988 .concept}

The website command is used to add, modify, query, or delete static website hosting and back-to-origin configurations.

**Note:** 

-   For more information about static website hosting, see [Configure static website hosting](../../../../reseller.en-US/Developer Guide/Static website hosting/Configure static website hosting.md#).
-   For more information about back-to-origin, see [Manage back-to-origin configurations](../../../../reseller.en-US/Developer Guide/Objects/Manage files/Manage back-to-origin configurations.md#).

## Command syntax {#section_hjy_b97_mk6 .section}

-   Add or modify website-related configurations

    ``` {#codeblock_kzk_p1k_b9p}
    ./ossutil website --method put oss://bucket  local_xml_file
    ```

    ossutil is used to read or write website-related configurations from or to local\_xml\_file. If the bucket has website-related configurations, new configurations will overwrite the existing configurations.

    **Note:** The local\_xml\_file configuration file is in the XML format as follows:

    -   The IndexDocument field is used to specify the default homepage.
    -   The ErrorDocument field is used to specify the default 404 page.
    -   The RoutingRules field is used to configure back-to-origin rules in the redirection or mirroring mode.
    You can set the preceding fields as required. The following code provides an example of how to set these fields:

    ``` {#codeblock_i9m_4qi_8uc}
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
                   <KeyPrefixEquals>abc/</KeyPrefixEquals>
                   <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
                   <IncludeHeader>
                     <Key>host</Key>
                     <Equals>test.oss-cn-beijing-internal.aliyuncs.com</Equals>
                   </IncludeHeader>
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

-   Obtain website-related configurations

    ``` {#codeblock_4lk_pjs_8kr}
    ./ossutil website --method get oss://bucket  [local_xml_file]
    ```

    The local\_xml\_file parameter specifies the name of the configuration file. If this parameter is specified, website-related configurations will be saved as a local file. If this parameter is not specified, ossutil will display website-related configurations.

-   Delete website-related configurations

    ``` {#codeblock_030_rsw_66i}
    ./ossutil website --method delete oss://bucket
    ```


## Examples {#section_y39_vtq_vpj .section}

-   Add default homepage and default 404 page configurations for static website hosting

    ``` {#codeblock_6s2_in1_tfn}
    ./ossutil website --method put oss://bucket1  /file/website.xml
    ```

    The content of the website.xml file is as follows:

    ``` {#codeblock_1cu_k6e_il0}
    <? xml version="1.0" encoding="UTF-8"? >
     <WebsiteConfiguration>
         <IndexDocument>
             <Suffix>test.html</Suffix>
         </IndexDocument>
         <ErrorDocument>
             <Key>errortest.html</Key>
         </ErrorDocument>
    </WebsiteConfiguration>
    ```

-   Add mirroring-based back-to-origin configurations

    ``` {#codeblock_3hq_89g_ihl}
    ./ossutil website --method put oss://bucket1  /file/website.xml
    ```

    The content of the website.xml file is as follows:

    ``` {#codeblock_c4u_hb5_5a1}
    <? xml version="1.0" encoding="UTF-8"? >
    <WebsiteConfiguration>
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
                     <MirrorURL>http://www.aliyun.com/</MirrorURL>
                     <MirrorPassQueryString>true</MirrorPassQueryString>
                     <MirrorFollowRedirect>true</MirrorFollowRedirect>
                     <MirrorCheckMd5>false</MirrorCheckMd5>
                     <MirrorHeaders>
                       <PassAll>true</PassAll>
                       <Pass>myheader1</Pass>
                       <Pass>myheader2</Pass>
                       <Remove>myheader3</Remove>
                       <Remove>myheader4</Remove>
                       <Set>
                         <Key>myheader5</Key>
                         <Value>myheader5</Value>
                       </Set>
                     </MirrorHeaders>
                 </Redirect>
             </RoutingRule>
    <RoutingRules>
    </WebsiteConfiguration>
    ```

-   Obtain website-related configurations

    ``` {#codeblock_nw7_u3z_gyz}
    ./ossutil website --method get oss://bucket1  /file/website.xml
    ```

-   Delete website-related configurations

    ``` {#codeblock_hi6_ruk_117}
    ./ossutil website --method delete oss://bucket1  
    ```


## Common options {#section_bi3_ezw_cgm .section}

The following table describes the options you can add to the website command.

|Option|Description|
|------|-----------|
|--method|Specifies the HTTP request method. Valid values: -   put: creates or modifies configurations.
-   get: obtains configurations.
-   delete: deletes configurations.

 |
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

