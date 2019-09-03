# Configure object tagging {#concept_710957 .concept}

This topic describes how to configure object tagging.

The object tagging function uses a key-value pair to tag an object. For more information about the object tagging function, see [Object tagging](../../../../intl.en-US/Developer Guide/Objects/Manage files/Object tagging.md#).

**Note:** 

-   You can add tags to an object when uploading it or add tags for an uploaded object. If add tags to an object that already has tags, the original tags are overwritten. For more information about adding tags, see [PutObjectTagging](../../../../intl.en-US/API Reference/Object operations/PutObjectTagging.md#).
-   To add tags to an object, you must have the permission to call PutObjectTagging.
-   The Last-Modified value of an object is not updated when its tags are changed.
-   A tag can contain letters, numbers, spaces, and the following symbols: + ‑ = . \_ : /

## Add tags to an object when uploading it {#section_voi_7s8_xq2 .section}

-   Add tags to an object when uploading it by calling PutObject.

    Run the following code to add tags to an object when uploading it by calling PutObject:

    ``` {#codeblock_vo7_bt3_k5v}
    package main
    
    import (
      "fmt"
      "os"
      "strings"
    
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
    
      // Configures the tags to be added to the object and the tagging rule. 
      tag1 := oss.Tag{
        Key:   "key1",
        Value: "value1",
      }
      tag2 := oss.Tag{
        Key:   "key2",
        Value: "value2",
      }
      tagging := oss.Tagging{
        Tags: []oss.Tag{tag1, tag2},
      }
    
      // Adds tags to the object when uploading it.
      err = bucket.PutObject("<yourObjectKey>", strings.NewReader("<yourObjectValue>"), oss.SetTagging(tagging))
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
    }
    ```

-   Add tags to an object when uploading it in the multipart upload method.

    Run the following code to add tags to an object when uploading it in the multipart upload method:

    ``` {#codeblock_58d_5jl_r79}
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
    
        // Configures the tags to be added to the object and the tagging rule. 
      tag1 := oss.Tag{
        Key:   "key1",
        Value: "value1",
      }
      tag2 := oss.Tag{
        Key:   "key2",
        Value: "value2",
      }
      tagging := oss.Tagging{
        Tags: []oss.Tag{tag1, tag2},
      }
    
      // Obtains the name and path of the local file to be uploaded.
      fileName := "<youFileNameAndPath>"
    
      // Divides the life into 3 parts and uploads the parts. The number of parts are calculated based on the file size.
      chunks, err := oss.SplitFileByPartNum(fileName, 3)
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
    
      // Opens the file.
      fd, err := os.Open(fileName)
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
      defer fd.Close()
    
      // Initializes the multipart upload task and sets the tags to be added.
      imur, err := bucket.InitiateMultipartUpload("<yourObjectName>", oss.SetTagging(tagging))
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
    
      // Uploads the object part by part.
      var parts []oss.UploadPart
      for _, chunk := range chunks {
        fd.Seek(chunk.Offset, os.SEEK_SET)
        part, err := bucket.UploadPart(imur, fd, chunk.Size, chunk.Number)
        if err != nil {
          fmt.Println("Error:", err)
          os.Exit(-1)
        }
        parts = append(parts, part)
      }
      _, err = bucket.CompleteMultipartUpload(imur, parts)
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
    }
    ```

-   Add tags to an object when uploading it in the append upload method.

    Run the following code to add tags to an object when uploading it in the append upload method:

    ``` {#codeblock_r99_kem_53x}
    package main
    
    import (
      "fmt"
      "os"
      "strings"
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
    
        // Configures the tags to be added to the object and the tagging rule.
      tag1 := oss.Tag{
        Key:   "key1",
        Value: "value1",
      }
      tag2 := oss.Tag{
        Key:   "key2",
        Value: "value2",
      }
      tagging := oss.Tagging{
        Tags: []oss.Tag{tag1, tag2},
      }
    
      var nextPos int64
      // Uploads the file to the appendable object for the first time. Only the tags set when AppendObject is called for the first time.
      nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("<yourObjectValue>"), nextPos, oss.SetTagging(tagging))
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
      // Uploads the file for the second time.
      nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("<yourObjectAppandValue>"), nextPos)
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
    }
    ```

-   Add tags to an object when uploading it in the resumable upload method.

    Run the following code to add tags to an object when uploading it in the resumable upload method:

    ``` {#codeblock_8ii_woi_oi6}
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
    
        // Configures the tags to be added to the object and the tagging rule.
      tag1 := oss.Tag{
        Key:   "key1",
        Value: "value1",
      }
      tag2 := oss.Tag{
        Key:   "key2",
        Value: "value2",
      }
      tagging := oss.Tagging{
        Tags: []oss.Tag{tag1, tag2},
      }
    
      // Divides the object into multiple parts of 100 KB and uses three threads to upload the parts simultaneously. Tags are added to the object when it is being uploaded.
      err = bucket.UploadFile("<yourObjectName>", "<yourNeedPuttingFile>", 100*1024, oss.Routines(3), oss.SetTagging(tagging))
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
    }
    ```


## Add tags to an uploaded object or modify the tags added to an object {#section_hfk_ai1_2u0 .section}

Run the following code to add tags to an uploaded object or modify the tags added to an object:

``` {#codeblock_7jt_89p_w3v}
package main

import (
  "fmt"
  "os"
  "strings"

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

  // Configures the tags to be added to the object and the tagging rule. 
  tag1 := oss.Tag{
    Key:   "key1",
    Value: "value1",
  }
  tag2 := oss.Tag{
    Key:   "key2",
    Value: "value2",
  }
  tagging := oss.Tagging{
    Tags: []oss.Tag{tag1, tag2},
  }
  // Adds the tags to the object.
  err = bucket.PutObjectTagging("<yourObjectName>", tagging)
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

## Add tags to an object when copying it {#section_yrj_7cy_gag .section}

You can specify one of the following tagging rules when copying an object:

-   Copy \(default\): Copy the tags of the source object to the destination object.
-   Replace: Add the tags specified in the request to the destination object.

Examples of adding tags to objects of different sizes when copying them are described as follows:

-   Run the following code to add tags to an object smaller than 1 GB when directyly copying it:

``` {#codeblock_0no_93k_z7e}
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

    // Configures the tags to be added to the object and the tagging rule.

  tag1 := oss.Tag{
    Key:   "key1",
    Value: "value1",
  }
  tag2 := oss.Tag{
    Key:   "key2",
    Value: "value2",
  }
  tagging := oss.Tagging{
    Tags: []oss.Tag{tag1, tag2},
  }

  // If you only set the tagging parameter when copying the object, the tags are not added.
  _, err = bucket.CopyObject("<yourSourceObjectName>", "<yourDirectObjectName>", oss.SetTagging(tagging))
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // The tags are added only when the TaggingReplace and tagging parameters are both set.
  _, err = bucket.CopyObject("<yourSourceObjectName>", "<yourDirectObjectName>", oss.SetTagging(tagging), oss.TaggingDirective(oss.TaggingReplace))
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

-   Run the following code to add tags to an object larger than 1 GB when copying it in the multipart copy method:

    ``` {#codeblock_j9s_6yt_coo}
    package main
    
    import (
      "fmt"
      "os"
      "strings"
    
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
    
        // Configures the tags to be added to the object and the tagging rule.
      tag1 := oss.Tag{
        Key:   "key1",
        Value: "value1",
      }
      tag2 := oss.Tag{
        Key:   "key2",
        Value: "value2",
      }
      tagging := oss.Tagging{
        Tags: []oss.Tag{tag1, tag2},
      }
    
      // Uploads an object for copying.
      content := "this your object value"
      err = bucket.PutObject("<yourObjectName>", strings.NewReader(content))
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
    
      // Initializes an object named <desObjectName> as the destination object and sets the tags to be added to the object.
      imur, err := bucket.InitiateMultipartUpload("<desObjectName>", oss.SetTagging(tagging))
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
      // The yourSourObject indicates the source object. The source object is uploaded as one part.
      part, err := bucket.UploadPartCopy(imur, "<yourBucketName>", "<yourSourObject>", 0, int64(len(content)), 1)
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
      parts := []oss.UploadPart{part}
      _, err = bucket.CompleteMultipartUpload(imur, parts)
      if err != nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
      }
    }
    ```


## Add tags to a symbolic link object {#section_a0t_xj0_eev .section}

Run the following code to add tags to a symbolic link object:

``` {#codeblock_lqi_3j3_97d}
package main

import (
  "fmt"
  "os"
  "strings"

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

    // Configures the tags to be added to the object and the tagging rule.
  tag1 := oss.Tag{
    Key:   "key1",
    Value: "value1",
  }
  tag2 := oss.Tag{
    Key:   "key2",
    Value: "value2",
  }
  tagging := oss.Tagging{
    Tags: []oss.Tag{tag1, tag2},
  }

  err = bucket.PutObject("<yourObjectName>", strings.NewReader("<yourObjectValue>"))
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  err = bucket.PutSymlink("<yourObjectName>", "<yourObjectSymLinkName>", oss.SetTagging(tagging))
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

