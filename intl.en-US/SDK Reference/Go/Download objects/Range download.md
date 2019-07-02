# Range download {#concept_943856 .concept}

If you want to download a part of data in an object, you can specify a download range to download only the data that you want.

Run the following code to download data within a specified range:

``` {#codeblock_fab_j8x_8mx}
package main

import (
  "fmt"
  "os"
  "io/ioutil"
  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Obtains the bucket.
  bucket, err := client.Bucket("<yourBucketName>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Obtains data between the starting byte value of 15 and the maximum byte value of 35 inclusive (that is, a total of 21 bytes).
  // If the specified value range is invalid, for example, the specified range includes a negative number, or the specified value is larger than the object size, the entire object is downloaded.
  body, err := bucket.GetObject("<yourObjectName>", oss.Range(15, 35))
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  // If you do not close the reader after the data is read, connection leaks may occur. Consequently, no connections are available and an exception occurs.
  defer body.Close()

  data, err := ioutil.ReadAll(body)
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  fmt.Println("data:", string(data))
}
```

