# Restore an Archive object {#concept_525793 .concept}

This topic describes how to restore an Archive object.

You must restore an archive object before you read it. Do not call restoreObject for non-archive objects.

The state conversion process of an archive object is as follows:

1.  An archive object is in the frozen state.
2.  After you submit it for restoration, the server restores the object. The object is in the restoring state. It takes about 1 minute to restore the object. However, a restoration task may take 4 hours to complete at most.
3.  After being restored, the object is in the restored state. The restored state of the object lasts for 24 hours by default. If you call RestoreObject again during this period, the period of the restored state is automatically prolonged for 24 hours. You can call RestoreObject to restore an Archive object for 7 times in a restoration task so that the restored state of the object can be kept for 7 days in maximum.
4.  Once the period of the restored state expires, the object returns to the frozen state.

The following code provides an example of how to restore an Archive object:

**Note:** For more information about how to restore an Archive object, visit [GitHub](https://github.com/ali-sdk/ali-oss/blob/master/README.md#restorename-options).

``` {#codeblock_yat_tvt_76u}
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

client.restore('objectName').then((res) => {
    console.log(res);
}).catch(err => {
    console.log(err);
})
```

For more information about the Archive storage class, see [Introduction to storage classes](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#).

