# Commands for operations on parts {#concept_bvd_chc_wdb .concept}

This topic describes commands that can be used to manage parts.

**Note:** Commands supported by the osscmd tool have been integrated with the [ossutil](reseller.en-US/Tools/ossutil/Quick start.md#) tool. The osscmd tool is no longer available for downloads as of July 31, 2019.

## init {#section_w2y_rhc_wdb .section}

Command:

`init oss://bucket/object`

Initialize an upload event to generate an upload ID. You can add this upload ID to the multiupload command to perform operations on parts.

Example:

 `python osscmd init oss://mybucket/myobject`

## listpart {#section_z2y_rhc_wdb .section}

Command:

`listpart oss://bucket/object --upload_id=xxx`

List the parts that are uploaded by using the upload ID of a specified object. For more information about related concepts, see [OSS API Reference](../../../../reseller.en-US/API Reference/Overview.md#). You must specify the upload ID.

Example:

``` {#codeblock_nr1_ofb_mc8}
python osscmd listpart oss://mybucket/myobject --upload_id=
          75835E389EA648C0B93571B6A46023F3
```

## listparts {#section_bfy_rhc_wdb .section}

Command:

`listparts oss://bucket`

List the objects and upload IDs of multipart upload events that have not been completed for a bucket. When you want to delete a bucket but the system prompts that the bucket is not empty, you can run this command to check whether there are fragments in the bucket.

Example:

 `python osscmd listparts oss://mybucket`

## getallpartsize {#section_dfy_rhc_wdb .section}

Command:

`getallpartsize oss://bucket`

List the total size of parts that are uploaded by using the existing upload IDs.

Example:

 `python osscmd getallpartsize oss://mybucket`

## cancel {#section_ffy_rhc_wdb .section}

Command:

`cancel oss://bucket/object --upload_id=xxx`

Terminate the multipart upload event that uses the upload ID.

Example:

``` {#codeblock_9nq_mu6_gb2}
python osscmd cancel oss://mybucket/myobject --upload_id=
          D9D278DB6F8845E9AFE797DD235DC576
```

## multiupload\(multi\_upload,mp\) {#section_hfy_rhc_wdb .section}

Command:

``` {#codeblock_ysj_y51_ohl}
multiupload(multi_upload,mp) localfile oss://bucket/object --check_md5=false
        --thread_num=10
```

Use multipart upload to upload a local file to OSS.

Example:

-   `python osscmd multiupload /tmp/localfile.txt oss://mybucket/object`
-   `python osscmd multiup_load /tmp/localfile.txt oss://mybucket/object`
-   `python osscmd mp /tmp/localfile.txt oss://mybucket/object`

Command:

``` {#codeblock_7pr_hs1_2md}
multiupload(multi_upload,mp) localfile oss://bucket/object --upload_id=xxx --thread_num=10
        --max_part_num=1000 --check_md5=false
```

Use multipart upload to upload a local file to OSS. The part count of the local file is defined by the max\_part\_num parameter. When this command is run, the system first determines whether the MD5 value of ETags of parts that use the upload ID is the same with the MD5 value of the local file. If their values are the same, the parts are uploaded. Generate an upload ID before this upload event is started. Add the upload ID to the command. If the upload fails, you can run the same multiupload command to upload the parts in the same way you use resumable upload. `--check_md5=false` indicates that Content-MD5 is not included in the request header and MD5 verification will not be performed. --check\_md5=true indicates that MD5 verification will be performed.

Example:

-   ``` {#codeblock_906_ji8_8a7}
python osscmd multiupload /tmp/localfile.txt oss://mybucket/object --upload_id=
          D9D278DB6F8845E9AFE797DD235DC576
```

-   ``` {#codeblock_73a_fdq_t3k}
python osscmd multiup_load /tmp/localfile.txt oss://mybucket/object
        --thread_num=5
```

-   `python osscmd mp /tmp/localfile.txt oss://mybucket/object --max_part_num=100`

## copylargefile {#section_ph2_12y_32b .section}

Command:

``` {#codeblock_ygg_9fg_kya}
copylargefile oss://source_bucket/source_object oss://target_bucket/target_object
        --part_size=10*1024*1024 --upload_id=xxx
```

To replicate an object that is larger than 1 GB, use multipart to replicate the object to the destination bucket. Ensure that the source bucket and destination bucket are in the same region. The upload\_id parameter is optional. If you need to resume the transmission of a multipart copy event, you can import the upload\_id parameter for the multipart copy event. The part\_size parameter is used to define the size of each part. A single part must be at least 100 KB in size. A maximum of 10,000 parts are supported for a multipart copy event. If the value of part\_size is smaller than 100 KB, the program automatically adjusts the part size.

Example:

``` {#codeblock_uyj_npa_noo}
python osscmd copylargefile oss://source_bucket/source_object
          oss://target_bucket/target_object --part_size=10*1024*1024
```

## uploadpartfromfile \(upff\) {#section_lfy_rhc_wdb .section}

Command:

``` {#codeblock_as5_yh0_vqw}
uploadpartfromfile (upff) localfile oss://bucket/object --upload_id=xxx
        --part_number=xxx
```

This command is used for tests only.

## uploadpartfromstring\(upfs\) {#section_mfy_rhc_wdb .section}

Command:

``` {#codeblock_bak_jds_7lm}
uploadpartfromstring(upfs) oss://bucket/object --upload_id=xxx --part_number=xxx
        --data=xxx
```

This command is used for tests only.

