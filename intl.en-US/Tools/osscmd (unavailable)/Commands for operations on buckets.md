# Commands for operations on buckets {#concept_av1_r1c_wdb .concept}

This topic describes commands that can be used to manage buckets.

**Note:** Commands supported by the osscmd tool have been integrated with the [ossutil](reseller.en-US/Tools/ossutil/Quick start.md#) tool. The osscmd tool is no longer available for downloads as of July 31, 2019.

## config {#section_bbx_w1c_wdb .section}

Command:

``` {#codeblock_ttv_ayl_16n}
config --id=[accessid] --key=[accesskey] --host=[host] --sts_token=[sts_token]
```

Example:

-   `python osscmd config --id=your_id --key=your_key`
-   ``` {#codeblock_tg3_w21_tou}
python osscmd config --id=your_id --key=your_key
        --host=oss-internal.aliyuncs.com
```


## getallbucket\(gs\) {#section_alb_gbc_wdb .section}

Command:

`getallbucket(gs)`

Obtain created buckets. gs is short for get allbucket. You can run the gs or allbucket command to obtain a list of created buckets.

Example:

-   `python osscmd getallbucket`
-   `python osscmd gs`

## createbucket\(cb,mb,pb\) {#section_uhk_mbc_wdb .section}

Command:

`createbucket(cb,mb,pb) oss://bucket --acl=[acl]`

Create a bucket.

-   cb is short for create bucket. mb is short for make bucket. pb is short for put bucket.
-   You can set oss://bucket to specify a bucket name.
-   The acl parameter is optional.

Example:

-   `python osscmd createbucket oss://mybucket`
-   `python osscmd cb oss://myfirstbucket --acl=public-read`
-   `python osscmd mb oss://mysecondbucket --acl=private`
-   `python osscmd pb oss://mythirdbucket`

## deletebucket\(db\) {#section_hzk_rbc_wdb .section}

Command:

`deletebucket(db) oss://bucket`

Delete a bucket. db is short for delete bucket.

Example:

-   `python osscmd deletebucket oss://mybucket`
-   `python osscmd db oss://myfirstbucket`

## deletewholebucket {#section_cgy_5bc_wdb .section}

**Warning:** All data is deleted if you run this command. Deleted data cannot be recovered. Exercise caution when you run this command.

Command:

`deletewholebucket oss://bucket`

Delete a bucket, and all objects and fragments in the bucket.

Example:

 `python osscmd deletewholebucket oss://mybucket`

## getacl {#section_mmg_zbc_wdb .section}

Command:

`getacl oss://bucket`

Obtain the bucket ACL.

Example:

 `python osscmd getacl oss://mybucket`

## setacl {#section_wly_kcc_wdb .section}

Command:

`setacl oss://bucket --acl=[acl]`

Modify the bucket ACL. You can set the bucket ACL to private, public-read, or public-read-write.

Example:

 `python osscmd setacl oss://mybucket --acl=private`

## putlifecycle {#section_mjs_4cc_wdb .section}

Command:

`putlifecycle oss://mybucket lifecycle.xml`

Set lifecycle rules. In the command, lifecycle.xml indicates a file that is used to configure lifecycle rules. For more information, see [API Reference](../../../../reseller.en-US/API Reference/Bucket operations/PutBucketLifecycle.md#).

Example:

 `python osscmd putlifecycle oss://mybucket lifecycle.xml` 

Example:

``` {#codeblock_n0i_2me_1ti}
<LifecycleConfiguration>
    <Rule>
        <ID>1125</ID>
        <Prefix>log_backup/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
            <Days>2</Days>
        </Expiration>
    </Rule>
</LifecycleConfiguration>
```

## getlifecycle {#section_cnj_wcc_wdb .section}

Command:

`osscmd getlifecycle oss://bucket`

Obtain lifecycle rules of a bucket.

Example:

 `python osscmd getlifecycle oss://mybucket`

## deletelifecycle {#section_zym_zcc_wdb .section}

Command:

`osscmd deletelifecycle oss://bucket`

Delete all lifecycle rules of a bucket.

Example:

 `python osscmd deletelifecycle oss://mybucket`

## putreferer {#section_rqv_cdc_wdb .section}

Command:

``` {#codeblock_a88_oyw_xe9}
osscmd putreferer oss://bucket --allow_empty_referer=[true|false]
        --referer=[referer]
```

Set hotlinking protection rules. The `allow_empty_referer` parameter is required and is used to specify whether an empty Referer field is allowed. The `referer` parameter is used to set the Referer whitelist. For example, you can add www.test1.com,www.test2.com to the Referer whitelist. To add multiple domain names, separate the domain names with commas \(,\). For more information about configuration rules, see [Configure hotlinking protection](../../../../reseller.en-US/Developer Guide/Buckets/Configure hotlinking protection.md#).

Example:

``` {#codeblock_osy_iaf_zpr}
python osscmd putreferer oss://mybucket --allow_empty_referer=true
          --referer="www.test1.com,www.test2.com"
```

## getreferer {#section_nts_gdc_wdb .section}

Command:

`osscmd getreferer oss://bucket`

Obtain the hotlinking protection rule of the bucket.

Example:

-   `python osscmd getreferer oss://mybucket`

## putlogging {#section_v22_jdc_wdb .section}

Command:

`osscmd putlogging oss://source_bucket oss://target_bucket/[prefix]`

source\_bucket specifies the bucket that is accessed. target\_bucket specifies the bucket that is used to store the log of access to the source bucket. You can set a prefix for the log that is generated to record access to the source bucket and facilitate log queries.

Example:

 `python osscmd getlogging oss://mybucket`

## getlogging {#section_igc_rdc_wdb .section}

Command:

`osscmd getlogging oss://bucket`

Obtain the access log setting rule of the bucket.

Example:

 `python osscmd getlogging oss://mybucket`

