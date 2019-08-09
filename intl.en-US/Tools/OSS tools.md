# OSS tools {#concept_owg_knn_vdb .concept}

In addition to the OSS console, you can use the following tools to use OSS more efficiently.

|Tool|Description|
|:---|:----------|
|[ossbrowser](reseller.en-US/Tools/ossbrowser/Quick start.md#)|A graphical object management tool. -   Provides an easy-to-use graphical interface.
-   Provides features similar to those of Windows Explorer.
-   Allows you to browse objects.
-   Allows you to upload and download folders.
-   Allows you to use concurrent upload and resumable upload to upload files.
-   Allows you to configure authorization policies and grant RAM users permissions.
-   Supports Windows, Linux, and Mac.

 Limits:

-   The transmission speed and performance of ossbrower are not as good as those of ossutil because ossbrower is a graphical tool.
-   Only files smaller than 5 GB can be uploaded or replicated.

 |
|[ossutil](reseller.en-US/Tools/ossutil/Quick start.md#)|A command line tool that is used to manage objects and buckets. -   Provides a wide range of convenient and simple commands to manage objects and buckets while ensuring good operation performance.
-   Allows you to use concurrent upload and resumable upload to upload files.
-   Allows you to upload and download folders.
-   Supports typical bucket management-related commands.

 |
|[osscmd \(unavailable\)](reseller.en-US/Tools/osscmd/Quick installation.md#)|A command line tool that is used to manage objects and buckets. -   Provides a wide range of commands to manage objects and buckets.
-   Supports Windows and Linux.

 Limits:

-   The osscmd tool is compatible with Python versions 2.5, 2.6, and 2.7 only.
-   You cannot use the tool to configure new features such as the storage class of infrequent access \(IA\) or Archive, cross-region replication \(CRR\), and back-to-origin.

 **Note:** Commands supported by the osscmd tool have been integrated with the [ossutil](reseller.en-US/Tools/ossutil/Quick start.md#) tool. The osscmd tool is no longer available for downloads as of July 31, 2019.

 |
|[ossfs](reseller.en-US/Tools/ossfs/Quick installation.md#)|A tool that is used to attach a bucket to the local file system. After you attach OSS buckets to the local file system of Linux, you can perform operations on the objects in OSS through the local file system to access or share these objects.

 -   Supports most functions of POSIX-based file systems, including file read/write, directories, soft links, permissions, UID or GID, and extended attributes.
-   Allows you to use multipart upload to upload large files.
-   Supports MD5 verification to ensure data integrity.

 Limits: -   You cannot attach an Archive bucket.
-   If you use ossfs tool to edit an uploaded file, the file is uploaded again.
-   The performance of metadata-related operations is compromised when you run the `list directory` command. In this case, you must remotely access the OSS server.
-   Errors may occur when you rename an object or a folder. Operation failures may cause data inconsistency.
-   The ossfs tool is not suitable for scenarios where read and write operations are highly concurrent.
-   You must maintain data consistency when an OSS bucket is attached to multiple clients. For example, you must schedule object use to prevent it from being written by multiple clients at the same time.
-   Hard links are not supported.

 |
|[ossftp](reseller.en-US/Tools/ossftp/Quick installation for OSS FTP.md#)|An FTP-based tool that is used to manage objects in OSS. -   You can use FTP clients such as FileZilla, WinSCP, and FlashFXP to manage data in OSS.
-   The ossftp tool is an FTP server that receives FTP requests and performs operations on objects and folders in OSS.
-   The ossftp tool is based on Python V2.7 and later.
-   This tool supports Windows, Linux, and Mac.

 |
|[ossimport](reseller.en-US/Tools/ossimport/Architecture and configuration.md#)|A tool that is used to synchronize data to OSS. -   Allows you to use the ossimport tool to synchronize data from a third-party data source to OSS.
-   Supports the distributed deployment mode. You can use multiple servers to migrate multiple data simultaneously.
-   Supports the migration of 1 TB of data at least.
-   Supports Windows and Linux.
-   Applies to Java V1.7 and later.

 |
|[Visual signature tool](https://bbs.aliyun.com/read/233851.html)|A third-party visual signature tool. -   This tool is used to generate a signed URL for operations on data in OSS.
-   You can debug the system to resolve problems you encounter when the signed URL is generated. If you encounter any problems when the signed URL is generated, compare the signed URL with the signature of the tool to troubleshoot the problems.
-   The browser edition of this tool supports the following browsers: Google Chrome, Firefox, and Safari.

 |
|[RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html)|A tool that is used to achieve automatic generation of OSS-related authorization policies. -   This tool can automatically generate authorization policies based on specified information. You can add the generated policies to the [custom policy](https://ram.console.aliyun.com/policies/new) in the RAM console.
-   This tool supports the following browsers: Google Chrome, Firefox, and Safari.

 **Note:** We recommend that you use this tool to generate custom authorization policies.

 |
|[oss-emulator](https://github.com/aliyun/oss-emulator)|A lightweight simulator of OSS. -   The oss-emulator tool provides the same API as that of OSS. This tool can be used to the debug and test OSS functions.
-   This tool is based on Ruby V2.2.8 and later.
-   This tool supports Windows and Linux.

 |

