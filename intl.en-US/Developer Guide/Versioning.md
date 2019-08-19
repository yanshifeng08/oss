# Versioning {#concept_jdg_4rx_bgb .concept}

After versioning is enabled for a bucket, data that is overwritten or deleted is saved as a previous version. Versioning allows you to restore objects in a bucket to any previous point in time after you overwrite or delete the objects.

**Note:** Versioning is now available in India \(Mumbai\) where users are added to the whitelist. This feature will be available in other regions soon.

## Scenarios {#section_mjs_lg2_cgb .section}

-   Restore data that was accidentally deleted

    OSS does not provide functions of recycle bins. After data in OSS is deleted, deleted data cannot be restored. You must use a local or third party backup tool to restore deleted data.

-   Restore data that was overwritten

    Documents stored in online storage or online collaborative documents are frequently modified. In online office scenarios, a large number of temporary versions are generated when files are edited. Users need to find versions of a certain point in time.


## Principles {#section_wlb_pn2_cgb .section}

Versioning applies to all objects instead of specified objects in buckets. After you enable versioning for a bucket, all objects in the bucket are subject to versioning. Each version has a unique version ID.

-   Bucket versions can be in any of the following states:

    -   Unversioned \(default\)
    -   Versioning-enabled
    -   Versioning-suspended
    **Note:** After you have enabled versioning for a bucket, the status of the bucket version can only be suspended. The status can cannot be rolled back to unversioned.

-   The following users can configure versioning for buckets:
    -   Users who have root accounts
    -   RAM users or roles who have been granted OSSFullAccess
    -   RAM users or roles who have been granted PutBucketVersioning

**Note:** After versioning is enabled for a bucket, fees will be incurred when previous versions are generated for overwritten objects. You can configure lifecycle rules to automatically delete expired versions.

