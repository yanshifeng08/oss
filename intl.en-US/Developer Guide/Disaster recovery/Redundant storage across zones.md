# Redundant storage across zones {#concept_ufs_g5m_cfb .concept}

Data in OSS is separately stored in three zones within the same region. Users can access the data even if one of the three zones is unavailable. With this redundant storage mechanism, OSS achieves 99.9999999999% data reliability and 99.995% designed service availability.

The redundant storage mechanism provides OSS with the disaster recovery capability in the data center level, that is, OSS can provide services with strong consistency even if a data center is not available because of network disconnection, power outage, or other disaster events. During failover, services are switched without interruption and data loss, ensuring that the failover process is not perceived by users. With the disaster recovery capability, OSS can meet the strict requirement that the Recovery Time Objective \(RTO\) and Recovery Point Objective \(RPO\) of key services must be 0.

## Supported storage class {#section_oft_dn3_dfb .section}

Redundant storage across zones supports two storage classes: Standard and Infrequent Access \(IA\). The following table compares the two storage classes from different dimensions.

|Index|Standard|IA|
|:----|:-------|:-|
|Data reliability|99.9999999999%|99.9999999999%|
|Designed service availability|99.995%|99.995%|
|Storage measurement unit|Actual object size|64 KB|
|Minimum storage period|No limit|30 days|
|Data retrieval fee|No fee is charged for data retrieval.|Charged based on the size of retrieved data \(GB\).|
|Data access|Real-time access with latency of milliseconds|Real-time access with latency of milliseconds|
|Images processing|Supported|Supported|

## Enable redundant storage across zones {#section_a4c_p43_dfb .section}

You can enable redundant storage across zones for a bucket only when you create the bucket.

If you want to separately store data in an existing bucket in multiple zones for redundant storage, use data migration tools \(such as ossimport and SDK\) to copy the data to a bucket with the redundant storage across zones feature enabled.

