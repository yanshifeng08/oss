# Convert the storage class of an object {#concept_187350 .concept}

OSS provides the following storage classes to cover various data storage scenarios from hot data to cold data: Standard, Infrequent Access \(IA\), and Archive. This topic describes how to convert the storage class of an object.

For more information about these storage classes, see [Introduction to storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#) and [Convert between storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Convert between storage classes.md#) in the OSS Developer Guide.

The following code provides an example of how to convert the storage class of an object.

-   The following code provides an example of how to convert the storage class of an object from Standard or IA to Archive:

    ``` {#codeblock_r94_fpa_qga}
    package main
    
    import (
        "fmt"
        "os"
    
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        bucketName := "<yourBucketName>"
        objectName := "<yourObjectName>"
    
        // Obtain information about the bucket based on the bucket name.
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Modify the storage class of the object. Set the storage class to Archive.
        _, err = bucket.CopyObject(objectName, objectName, oss.ObjectStorageClass(oss.StorageArchive))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    }
    				
    ```

-   The following code provides an example of how to convert the storage class from Archive to Standard:

``` {#codeblock_048_yxv_tw4}
package main

import (
    "fmt"
    "os"
    "time"

    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    bucketName := "<yourBucketName>"
    objectName := "<yourObjectName>"

    // Obtain information about the bucket based on the bucket name.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Check whether the storage class of the source object is Archive. If the storage class of the source object is Archive, you must restore the object before you can modify the storage class.
    meta, err := bucket.GetObjectDetailedMeta(objectName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    fmt.Println("X-Oss-Storage-Class : ", meta.Get(oss.HTTPHeaderOssStorageClass))
    if meta.Get(oss.HTTPHeaderOssStorageClass) == string(oss.StorageArchive) {
        // Restore the object.
        err = bucket.RestoreObject(objectName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }

        // Wait for about one minute until the object is restored.
        meta, err = bucket.GetObjectDetailedMeta(objectName)
        for meta.Get("X-Oss-Restore") == "ongoing-request=\"true\"" {
            fmt.Println("X-Oss-Restore:" + meta.Get("X-Oss-Restore"))
            time.Sleep(1 * time.Second)
            meta, err = bucket.GetObjectDetailedMeta(objectName)
        }
    }

    //Modify the storage class of the restored object. Set the storage class to Standard.
    _, err = bucket.CopyObject(objectName, objectName, oss.ObjectStorageClass(oss.StorageStandard))
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
```


For more information about the storage classes and storage fees, see the [Storage fees](../../../../reseller.en-US/Pricing/Billing items.md#section_jmi_2z4_hka) section in Billing items.

