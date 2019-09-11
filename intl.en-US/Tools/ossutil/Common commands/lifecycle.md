# lifecycle {#concept_744987 .concept}

The lifecycle command is used to add, modify, query, or delete lifecycle configurations.

**Note:** For more information about lifecycle rules, see [Manage object lifecycle](../../../../reseller.en-US/Developer Guide/Object lifecycle/Manage lifecycle rules.md#).

## Command syntax {#section_2iw_zhv_dwe .section}

-   Add or modify lifecycle configurations

    ``` {#codeblock_g9q_6fi_lv6}
    ./ossutil lifecycle --method put oss://bucket local_xml_file [options]
    ```

    ossutil reads the configurations in local\_xml\_file, and adds the rules obtained from the configuration file for the bucket. If the rule ID exists in the configuration file, ossutil will overwrite the configurations of the existing rule ID.

    **Note:** A lifecycle configuration file is an XML file that describes the rules on handling expired objects, such as whether to delete the object or to change the storage class of expired objects. You can configure the rules based on your needs. The following code provides an example of a complete lifecycle configuration:

    ``` {#codeblock_zg9_mu0_50r}
    <? xml version="1.0" encoding="UTF-8"? >
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
          <StorageClass>StorageClass</StorageClass>
        </Transition>
        <AbortMultipartUpload>
          <Days>Days</Days>
        </AbortMultipartUpload>
      </Rule>
    </LifecycleConfiguration>
    ```

    For more information about lifecycle configurations, see [Lifecycle rule configuration examples](../../../../reseller.en-US/Developer Guide/Object lifecycle/Lifecycle rule configuration examples.md#).

-   Obtain lifecycle configurations

    ``` {#codeblock_gy0_orp_8t2}
    ossutil lifecycle --method get oss://bucket [local_file]
    ```

    The local\_file parameter specifies the name of the configuration file. If this parameter is specified, ossutil saves the obtained lifecycle configurations as a local file. If this parameter is not specified, ossutil displays the obtained lifecycle configurations.

-   Delete lifecycle configurations

    ``` {#codeblock_gxn_k9h_1qb}
    ./ossuitl lifzecycle --method delete oss://bucket
    ```


## Examples {#section_xo0_sqp_7f0 .section}

-   Add a lifecycle rule that changes the storage class of objects uploaded for 10 days and prefixed by test/ to IA

    ``` {#codeblock_7am_73d_kc4}
    ./ossutil lifecycle --method put oss://bucket1  /file/lifecycle.xml
    ```

    The content of the lifecycle.xml configuration file is as follows:

    ``` {#codeblock_y97_m4t_oy1}
    <? xml version="1.0" encoding="UTF-8"? >
    <LifecycleConfiguration>
      <Rule>
        <ID>1</ID>
        <Prefix>test/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>10</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
     </Rule>
    </LifecycleConfiguration>
    ```

-   Add a lifecycle rule that deletes objects modified before January 1, 2019 and prefixed by test/ 

    ``` {#codeblock_84x_m7p_1yf}
    ./ossutil lifecycle --method put oss://bucket1  /file/lifecycle.xml
    ```

    The content of the lifecycle.xml configuration file is as follows:

    ``` {#codeblock_x3u_77l_gpk}
    <? xml version="1.0" encoding="UTF-8"? >
    <LifecycleConfiguration>
      <Rule>
        <ID>2</ID>
        <Prefix>test/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <CreatedBeforeDate>2014-10-11T00:00:00.000Z</CreatedBeforeDate>
        </Expiration>
     </Rule>
    </LifecycleConfiguration>
    ```

-   Obtain lifecycle configurations

    ``` {#codeblock_mau_wb3_tsc}
    ./ossutil lifecycle --method get oss://bucket1  /file/lifecycle.xml
    ```

-   Delete lifecycle configurations

    ``` {#codeblock_bc7_d4w_ngs}
    ./ossutil lifecycle --method delete oss://bucket1  
    ```


## Common options {#section_632_cpr_dve .section}

The following table describes the options you can add to the lifecycle command.

|Option|Description|
|------|-----------|
|--method|Specifies the HTTP request method. Valid values: -   put: adds or modifies configurations
-   get: obtains configurations
-   delete: deletes configurations

 |
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain HTTP requests and responses.

 |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 proxies are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password of the proxy server. The default value is null.|

**Note:** For more information about common options, see [View all supported options](reseller.en-US/Tools/ossutil/View all supported options.md#).

