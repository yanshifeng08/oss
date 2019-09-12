# Security white paper {#concept_1322552 .concept}

This topic describes how to secure data in OSS by using the security features provided by OSS, such as encryption, access control, logging, monitoring, and data protection.

## Encryption {#section_zxi_uu1_rj8 .section}

The following encryption methods are available in OSS: server-side encryption, client-side encryption, and transmission encryption.

-   Server-side encryption

    OSS protects static data by encrypting it on the server side. This method is suitable for scenarios that require additional security or compliance for object storage. Examples of the scenarios include storage of deep learning samples and online collaborative documents.

    **Note:** For more information about the principle of server-side encryption, see [Principle](../../../../reseller.en-US/Developer Guide/Data encryption/Server-side encryption.md#section_c24_wbd_5gb).

    OSS allows you to implement server-side encryption in any of the following methods:

    -   SSE-KMS: Customer master keys \(CMKs\) are hosted by OSS.

        When sending a request to upload an object or modify the metadata of an object, you can include the `X-OSS-server-side-encryption` header in the request and specify its value as KMS without a specified CMK ID. In this method, OSS generates an individual key to encrypt each object by using the default managed CMK, and automatically decrypts the object when it is downloaded.

    -   SSE-KMS BYOK: This encryption method allows you to use bring your own key \(BYOK\).

        OSS supports using BYOK material for encryption. When sending a request to upload an object or modify the metadata of an object, you can include the `X-OSS-server-side-encryption` header in the request, specify its value as KMS, and specify the value of `X-oss-server-side-encryption-key-id` to a specified CMK ID. In this method. OSS generates an individual key to encrypt each object by using the specified CMK, and adds the CMK ID used to encrypt an object into the metadata of the object so that the object is automatically decrypted when it is downloaded by an authorized user

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4384/156828872638833_en-US.png)

    -   SSE-OSS: This encryption method is fully hosted by OSS.

        This encryption method is an attribute of objects. In this mode, OSS server-side encryption uses AES256 to encrypt objects with different data keys. AES256 uses master keys that are regularly rotated to encrypt data keys.

-   Client-side encryption

    Client-side encryption encrypts files on the client before they are uploaded to OSS. You can use either of the following methods to implement client-side encryption:

    -   Use CMKs hosted on KMS

        Encrypt files on the client using a CMK hosted on KMS. When using KMS, you need only to specify the CMK ID. You do not need to provide additional data keys. The following flow chart shows the encryption process in detail.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1054111/156828872660492_en-US.png)

    -   Manually manage data keys

        BYOK allows you to generate and manage data keys using your own tools. When you implement client-side encryption, you can upload your symmetric or asymmetric key to the client. The following flow chart shows the encryption process in detail.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1054111/156828872660493_en-US.png)

-   Transmission encryption

    OSS allows you to access resources in OSS over HTTP or HTTPS. You can set bucket-based authorization policies to only allow access to resources in OSS over HTTPS. Transport Layer Security \(TLS\) is a cryptographic protocol that provides end-to-end communications security over networks.


## Access control {#section_ukq_7bo_zg5 .section}

OSS has a variety of methods to control access to resources, such as ACL, RAM, and bucket-based authorization policies.

-   [ACL](reseller.en-US/Developer Guide/Access and control/Access control based on ACLs.md#): OSS provides Access Control List \(ACL\) for access control. An ACL is set based on resources. You can set ACLs for buckets or objects. You can set an ACL for a bucket when you create the bucket or for an object when you upload the object to OSS. You can also modify the ACL for a created bucket or an uploaded object at anytime.
-   [RAM Policy](reseller.en-US/Developer Guide/Access and control/Access control based on RAM Policy/RAM policy.md#): Resource Access Management \(RAM\) is a service provided by Alibaba Cloud for resource access control. RAM policies are configured based on users. By configuring RAM policies, you can manage multiple users in a centralized manner and control the resources that can be accessed by the users. For example, you can control the permission of a user so that the user can only read a specified bucket. A RAM user belongs to the Alibaba Cloud account under which it was created, and does not own any actual resources. That is, all resources belong to the corresponding Alibaba Cloud account.
-   [Bucket Policy](reseller.en-US/Developer Guide/Access and control/Bucket policy.md#): Bucket policies are configured based on resources. Compared with RAM policies, bucket policies can be directly configured on the graphical console. By configuring bucket policies, you can authorize users to access your bucket even you do not have permissions for RAM operations. By configuring bucket policies, you can grant access permissions to RAM users under other Alibaba Cloud accounts, and to anonymous users who access your resources from specified IP addresses or IP ranges.

## Logs and monitoring {#section_6o9_phi_2cc .section}

OSS can store and query logs, allowing you to gain insight into resource access. Additionally, OSS provides the monitoring service to allow you to gain insights into the running status of OSS, perform diagnostics, and troubleshoot problems.

-   Query access logs

    A large number of logs are generated when OSS resources are accessed. After you enable and configure logging for a bucket, an object with a specified prefix is generated on an hourly basis to record access logs of the bucket. You can use Alibaba Cloud Data Lake Analytics or build a Spark cluster to analyze these access logs. You can also configure lifecycle management rules for the log object to convert its storage class to Archive. This way, the log can be retained for a long time. For more information about OSS access logs, see [Access logging](../../../../reseller.en-US/Developer Guide/Manage logs/访问日志存储.md#).

-   Query logs in real time

    Real-time log query integrates OSS with Log Service. This feature allows you to query and collect statistics on OSS access logs, audit access to OSS, track exception events, and troubleshoot problems in the OSS console. This way, you can improve efficiency at work and make better decisions based on logs. For more information about real-time log query, see [Real-time log query](../../../../reseller.en-US/Developer Guide/Manage logs/Real-time log query.md#).

-   Monitoring

    The monitoring service of OSS provides metrics that describe basic system operating statuses, performance, and metering. The monitoring service also provides a custom alert service to help you track requests, analyze usage, collect statistics on business trends, and promptly discover and diagnose system problems. For more information about the monitoring service, see [Monitoring service overview](../../../../reseller.en-US/Developer Guide/Monitoring service/Monitoring service overview.md#).


## Data protection {#section_k9c_s7t_vv6 .section}

OSS provides the compliance retention policy, zone-redundant storage, and versioning to guarantee data security of OSS.

-   Compliance retention policy

    The Write Once Read Many strategy is supported in OSS. This feature prevents objects from being deleted or overwritten for a specified period of time.

    OSS provides strong compliant policies. You can configure time-based compliant retention policies for buckets. After a compliant retention policy is locked, you can read objects from or upload objects to buckets. However, no one can delete the pretected objects or compliant retention policies within the retention period. You can delete objects only after their retention period ends. The WORM strategy is suitable for industries such as the financing, insurance, health care, and security. OSS allows you to build a "compliant bucket in the cloud."

    For more information about the compliance retention policy, see [Compliant retention policy](../../../../reseller.en-US/Developer Guide/Compliant retention policy.md#).

-   Zone-redundant storage

    OSS uses the multi-zone mechanism to distribute user data across three zones within the same region. If one zone becomes unavailable, the data will still be accessible. OSS zone-redundant storage provides durability of 99.9999999999% \(twelve 9's\) and a guaranteed data availability of 99.95% in the SLA.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1054111/156828872660495_en-US.jpg)

    The redundant storage mechanism provides OSS with the disaster recovery capability in the data center level, that is, OSS can provide services with strong consistency even if a data center is not available because of network disconnection, power outage, or other disaster events. During failover, services are switched without interruption and data loss, ensuring that the failover process is not perceived by users. With the disaster recovery capability, OSS can meet the strict requirement that the Recovery Time Objective \(RTO\) and Recovery Point Objective \(RPO\) of key services must be 0.

    For more information about zone-redundant storage, see [Redundant storage across zones](../../../../reseller.en-US/Developer Guide/Disaster recovery/Redundant storage across zones.md#).

-   Versioning

    After versioning is enabled for a bucket, data that is overwritten or deleted is saved as a previous version. Versioning allows you to restore objects in a bucket to any previous point in time after you overwrite or delete the objects.

    **Note:** Versioning will be available soon.

    Versioning applies to all objects in buckets. After you enable versioning for a bucket, all objects in the bucket are subject to versioning. Each version has a unique version ID.

    Fees are incurred when previous versions are generated for overwritten objects. You can configure lifecycle rules to automatically delete expired versions.


