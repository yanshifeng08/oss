# Overview {#concept_jv4_ssb_wdb .concept}

osscmd is a Python 2.x-based command line tool. You can use this tool to manage buckets and objects.

**Note:** Commands supported by the osscmd tool have been integrated with the [ossutil](reseller.en-US/Tools/ossutil/Quick start.md#) tool. The osscmd tool is no longer available for downloads as of July 31, 2019.

## Scenarios {#section_vki_cp1_n4r .section}

You can use the osscmd tool in the following scenarios:

-   API-based development and debugging. You can use the osscmd tool to send a request in a specific format and perform multipart upload step by step.
-   Bucket-based configurations. You can use the osscmd tool to configure logging, website, and lifecycle rules for buckets.

## Limits {#section_jdd_w1r_d3f .section}

-   The osscmd tool supports Python versions 2.5, 2.6, and 2.7 only.
-   Only bugs of the osscmd tool can be fixed. You cannot use the tool to configure new features such as the storage class of infrequent access \(IA\) or Archive, cross-region replication \(CRR\), and back-to-origin.

## Use the osscmd tool {#section_wl4_qvb_wdb .section}

After you have downloaded and decompressed the Python SDK, run the `python osscmd + operation` command in the directory where the osscmd tool resides. For example, run the following command to upload a file to a bucket:

``` {#codeblock_xw4_g5l_md9}
python  osscmd  put  myfile.txt  oss://mybucket
```

**Note:** In the commands that are supported by the osscmd tool, oss://bucket specifies a bucket. oss://bucket/object specifies a bucket or an object. oss:// is only a format used to specify resources.

To obtain a detailed list of commands, run the `python osscmd` command.

To obtain a detailed list of command parameters, run the `python osscmd help` command.

