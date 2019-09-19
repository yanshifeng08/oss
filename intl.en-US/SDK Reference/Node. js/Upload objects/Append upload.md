# Append upload {#concept_508006 .concept}

This topic describes how to append data from a local file.

You can use AppendObject to append content to appendable objects that have been uploaded.

**Note:** 

-   Only objects that are created through append upload can have data appended to them. Therefore, objects created through simple upload, form upload, multipart upload, or resumable upload cannot have data appended to them.
-   You cannot perform CopyObject for appendable objects.

For more information about append upload, see [Append upload](../../../../reseller.en-US/Developer Guide/Objects/Upload files/Append upload.md#) in the OSS Developer Guide.

Run the following code for append upload:

``` {#codeblock_0xr_2hy_1fy}
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

async function append () {
  try {
    let result = await client.append('object-name', 'local-file');
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}

append();
```

**Note:** \[position\] \{String\} specifies the position calculated based on the content length of the object uploaded last time. For more information about how to configure other parameters, visit [GitHub](https://github.com/ali-sdk/ali-oss/blob/master/README.md#appendname-file-options).

