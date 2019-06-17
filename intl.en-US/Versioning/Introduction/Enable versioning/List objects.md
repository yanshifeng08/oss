# List objects {#concept_wb2_zgj_dgb .concept}

In a bucket with versioning enabled, you can call the GetBucketVersions \(ListObjectVersions\) interface to list the information about all versions of the objects in the bucket, including the delete markers.

**Note:** 

-   Unlike GetBucketVersions \(ListObjectVersions\), the GetBucket \(ListObject\) interface returns only the current version of the objects \(which is not the delete marker\) in the bucket.
-   A maximum of 1,000 object versions can be returned for a request. To list more versions of all objects, you must initiate multiple requests.

    For example, a bucket includes two keys: example.jpg and photo.jpg. The example.jpg key has 900 versions and the photo.jpg key has 500 versions. If you initiate a GetBucketVersions request, the results are returned in the alphabetical order of the keys and the creation time order of the versions, that is, the 900 versions of the example.jpg key are returned first in the creation time, followed by the latest 100 versions of the photo.jpg key.


The following figure describes the differences between the results of GetBucketVersions and GetBucket.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79968/156073950240404_en-US.jpg)

-   The GetBucketVersions interface returns all versions of all objects in the bucket with versioning enabled, including the objects of which the current version is a delete marker.
-   The GetBucket interface returns only the current version \(which is not a delete marker\) of the objects in the bucket.

