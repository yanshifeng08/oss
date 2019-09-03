# Obtain the tags added to an object {#concept_711007 .concept}

This topic describes how to obtain the tags added to an object.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

Run the following code to obtain the tags added to an object:

``` {#codeblock_5th_9fj_1wi}
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

  // Obtains the tags added to the object.
  taggingResult, err := bucket.GetObjectTagging("<yourObjectName>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  fmt.Printf("Object Tagging: %v\n", taggingResult)
}
```

For more information about how to obtain the tags added to an object, see [GetObjectTagging](../../../../intl.en-US/API Reference/Object operations/GetObjectTagging.md#).

