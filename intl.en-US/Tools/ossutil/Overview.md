# Overview {#concept_cnr_3d4_vdb .concept}

ossutil is a command-line tool that is used in Windows, Linux, or macOS to manage data in OSS. This tool provides a wide range of convenient and simple commands to manage objects and buckets.

ossutil allows you to perform the following operations:

-   Bucket management, such as creating, listing, and deleting buckets.
-   Object management, such as uploading, downloading, listing, copying, and deleting objects.
-   Part management, such as listing and deleting parts.

## Install ossutil {#section_8h2_n0c_x70 .section}

For more information about how to download and install ossutil, see [Download and installation](intl.en-US/Tools/ossutil/Download and installation.md#).

## Commonly used commands {#section_ed3_zgm_9f1 .section}

The following table describes the commands that are supported in ossutil.

|Command|Description|
|-------|-----------|
|[appendfromfile](intl.en-US/Tools/ossutil/Common commands/appendfromfile.md#)|Uploads a local file to an appendable object in OSS.|
|[bucket-encryption](intl.en-US/Tools/ossutil/Common commands/bucket-encryption.md#)|Adds, modifies, queries, or deletes encryption configurations for a bucket.|
|[bucket-policy](intl.en-US/Tools/ossutil/Common commands/bucket-policy.md#)|Adds, modifies, queries, or deletes bucket policy configurations for a bucket.|
|[bucket-tagging](intl.en-US/Tools/ossutil/Common commands/bucket-tagging.md#)|Adds, modifies, queries, or deletes tagging configurations for a bucket.|
|[cat](intl.en-US/Tools/ossutil/Common commands/cat.md#)|Exports object content to ossutil.|
|[config](intl.en-US/Tools/ossutil/Common commands/config.md#)|Generates a configuration file to store OSS access information.|
|[cors](intl.en-US/Tools/ossutil/Common commands/cors.md#)|Adds, modifies, queries, or deletes cross-origin resource sharing \(CORS\) configurations for a bucket.|
|[cors-options](intl.en-US/Tools/ossutil/Common commands/cors-options.md#)|Tests whether a bucket allows a specified cross-origin request.|
|[cp](intl.en-US/Tools/ossutil/Common commands/cp.md#)|Uploads, downloads, or copies objects.|
|[create-symlink](intl.en-US/Tools/ossutil/Common commands/create-symlink.md#)|Creates a symbolic link \(also known as a soft link\).|
|[du](intl.en-US/Tools/ossutil/Common commands/du.md#)|Obtains the storage size of a specified bucket, object, or folder.|
|[getallpartsize](intl.en-US/Tools/ossutil/Common commands/getallpartsize.md#)|Obtains the total size of all parts in a bucket, and the size of each part that has not been uploaded.|
|[hash](intl.en-US/Tools/ossutil/Common commands/hash.md#)|Calculates the CRC64 or MD5 value of a local file.|
|[help](intl.en-US/Tools/ossutil/Common commands/help.md#)|Obtains help information about a command. We recommend that you use the help command to obtain information about a specified command.|
|[lifecycle](intl.en-US/Tools/ossutil/Common commands/lifecycle.md#)|Adds, modifies, queries, or deletes lifecycle configurations for a bucket.|
|[listpart](intl.en-US/Tools/ossutil/Common commands/listpart.md#)|Lists the parts that have not been uploaded for a specified object.|
|[logging](intl.en-US/Tools/ossutil/Common commands/logging.md#)|Adds, modifies, queries, or deletes logging configurations for a bucket.|
|[ls](intl.en-US/Tools/ossutil/Common commands/ls.md#)|Lists buckets, objects, or parts.|
|[mb](intl.en-US/Tools/ossutil/Common commands/mb.md#)|Creates a bucket.|
|[mkdir](intl.en-US/Tools/ossutil/Common commands/mkdir.md#)|Creates a directory in a bucket.|
|[object-tagging](intl.en-US/Tools/ossutil/Common commands/object-tagging.md#)|Adds, modifies, queries, or deletes tagging configurations for an object.|
|[probe](intl.en-US/Tools/ossutil/Common commands/probe.md#)|Monitors access to OSS, and troubleshoots problems caused during the upload and download process by network faults or incorrect parameter settings.|
|[read-symlink](intl.en-US/Tools/ossutil/Common commands/read-symlink.md#)|Reads the description of a symbolic link object.|
|[referer](intl.en-US/Tools/ossutil/Common commands/referer.md#)|Adds, modifies, queries, or deletes hotlink protection configurations for a bucket.|
|[restore](intl.en-US/Tools/ossutil/Common commands/restore.md#)|Restores a single object from the frozen state to the readable state.|
|[request-payment](intl.en-US/Tools/ossutil/Common commands/request-payment.md#)|Configures or queries pay-by-requester configurations for a bucket.|
|[rm](intl.en-US/Tools/ossutil/Common commands/rm.md#)|Deletes buckets, objects, or parts.|
|[set-acl](intl.en-US/Tools/ossutil/Common commands/set-acl.md#)|Configures the ACL for a bucket or object.|
|[set-meta](intl.en-US/Tools/ossutil/Common commands/set-meta.md#)|Configures the metadata of an uploaded object.|
|[sign](intl.en-US/Tools/ossutil/Common commands/sign.md#)|Generates signed URLs for third-party users to access objects in a bucket.|
|[stat](intl.en-US/Tools/ossutil/Common commands/stat.md#)|Obtains the description of a specified bucket or object.|
|[update](intl.en-US/Tools/ossutil/Common commands/update.md#)|Updates the ossutil version.|
|[website](intl.en-US/Tools/ossutil/Common commands/website.md#)|Adds, modifies, queries, or deletes static website hosting and back-to-origin configurations for a bucket.|

