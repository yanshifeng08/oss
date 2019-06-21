# List objects {#concept_88639_zh .concept}

Objects are listed alphabetically. You can use `Bucket.ListObjects` to list objects in a bucket. The following parameters can be used:

|Parameter|Description|
|:--------|:----------|
|delimiter|Specifies a delimiter used to group objects. The substring between the specified prefix and the first occurrence of the delimiter of a forward slash \(/\) is commonPrefixes.|
|prefix|Specifies the prefix you configure to list required objects.|
|maxKeys|Specifies the maximum number of objects that can be listed. The default value is 100. The maximum value is 1,000.|
|marker|Specifies the initial object in the list.|

For the complete code of ListObjects, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/list_objects.go).

## Simple list {#section_jx2_xtw_kfb .section}

You can use the default parameters to obtain the object list of a bucket. 100 objects can be listed by default. Run the following code to list objects in a bucket:

```language-go
package main

import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func HandleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        HandleError(err)
    }

    // Obtain the bucket.
    bucketName := "<yourBucketName>"
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        HandleError(err)
    }

    // List all objects.
    marker := ""
    for {
        lsRes, err := bucket.ListObjects(oss.Marker(marker))
        if err ! = nil {
            HandleError(err)
        }

        // Prints the object list. 100 records are returned by default. 
        for _, object := range lsRes.Objects {
            fmt.Println("Bucket: ", object.Key)
        }

        if lsRes.IsTruncated {
            marker = lsRes.NextMarker
        } else {
            break
        }
    }
}
			
```

## List a specified number of objects { .section}

Run the following code to list a specified number of objects:

```language-go
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

    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Configure the maximum number of lthe listed object and list objects.
    lsRes, err := bucket.ListObjects(oss.MaxKeys(200))
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Print the results.
    fmt.Println("Objects:", lsRes.Objects)
    for _, object := range lsRes.Objects {
        fmt.Println("Object:", object.Key)
    }
}
			
```

## List objects by a specified prefix { .section}

Run the following code to list objects with a specified prefix:

```language-go
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

    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // List objects with a specified prefix. 100 objects are listed by default.
    lsRes, err := bucket.ListObjects(oss.Prefix("my-object-"))
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Print the results.
    fmt.Println("Objects:", lsRes.Objects)
    for _, object := range lsRes.Objects {
        fmt.Println("Object:", object.Key)
    }
}
			
```

## List objects by marker { .section}

The marker parameter indicates the name of an object from which the listing begins. Run the following code to specify an object \(specified with marker\) after which the listing begins:

```language-go
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

    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // List objects placed after the object (specified with marker). 100 objects are listed by default.
    lsRes, err := bucket.ListObjects(oss.Marker("my-object-xx"))
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Print the results.
    fmt.Println("Objects:", lsRes.Objects)
    for _, object := range lsRes.Objects {
        fmt.Println("Object:", object.Key)
    }
}
			
```

## List all objects on one or more pages { .section}

Run the following code to list all objects on one or more pages. The maximum number of objects listed on each page is specified with maxKeys.

```language-go
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

    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // List all objects on multiple pages. 200 objects are listed in one page.
    marker := oss.Marker("")
    for {
        lsRes, err := bucket.ListObjects(oss.MaxKeys(200), marker)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        marker = oss.Marker(lsRes.NextMarker)

        fmt.Println("Objects:", lsRes.Objects)

        if ! lsRes.IsTruncated {
            break
        }
    }
}
			
```

## List objects by marker on one or more pages { .section}

Run the following code to specify an object \(specified with marker\) after which the listing begins on one or more pages: The maximum number of objects listed on each page is specified with maxKeys.

```language-go
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

    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // List objects placed after the object (specified with marker) on multiple pages. 50 objects are listed in one page.
    marker = oss.Marker("my-object-xx")
    for {
        lsRes, err := bucket.ListObjects(oss.MaxKeys(50), marker)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }

        marker = oss.Marker(lsRes.NextMarker)

        fmt.Println("Objects:", lsRes.Objects)

        if ! lsRes.IsTruncated {
            break
        }
    }
}
			
```

## List objects by a specified prefix on one or more pages { .section}

Run the following code to list objects by a specified prefix on one or more pages: The maximum number of objects listed on each page is specified with maxKeys.

```language-go
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

    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // List objects with a specified prefix on multiple pages. 80 objects are listed in one page.
    prefix := oss.Prefix("my-object-")
    marker := oss.Marker("")
    for {
        lsRes, err := bucket.ListObjects(oss.MaxKeys(80), marker, prefix)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }

        prefix = oss.Prefix(lsRes.Prefix)
        marker = oss.Marker(lsRes.NextMarker)

        fmt.Println("Objects:", lsRes.Objects)

        if ! lsRes.IsTruncated {
            break
        }
    }
}
			
```

