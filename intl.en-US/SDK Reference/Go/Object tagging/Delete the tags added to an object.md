# Delete the tags added to an object {#concept_711015 .concept}

This topic describes how to delete the tags added to an object.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

Run the following code to delete the tags added to an object:

``` {#codeblock_mge_ov4_90r}
package main

import (
  "fmt"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Obtains the bucket that stores the object.
  bucket, err := client.Bucket("<yourBucketName>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Deletes the tags added to the object.
  err = bucket.DeleteObjectTagging("<yourObjectName>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

For more information about how to delete the tags added to an object, see [DeleteObjectTagging](../../../../intl.en-US/API Reference/Object operations/DeleteObjectTagging.md#).

