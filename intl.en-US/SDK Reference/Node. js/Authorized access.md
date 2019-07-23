# Authorized access {#concept_32077_zh .concept}

This topic describes how to authorize temporary access to OSS.

## Use STS for temporary access authorization {#section_zkq_3rq_dhb .section}

You can use Alibaba Cloud STS to authorize temporary access to OSS. STS is a Web service that provides a temporary access token to a cloud computing user. You can use STS to assign a third-party application or a federated user \(you can manage the user ID\) an access credential with a custom validity period and permissions. For more information, see [STS introduction](../../../../reseller.en-US/API Reference (STS)/What is STS?.md#).

STS benefits:

-   You do not need to expose your long-term key \(AccessKey\) to a third-party application. You need only to generate an access token and send the access token to the third-party application. You can customize the access permissions and validity period of this token.
-   The access token automatically expires after the validity period.

For more information about how to use STS to access OSS, see the "[Use STS to authorize temporary access to OSS](../../../../reseller.en-US/Developer Guide/Identity authentication/Access OSS with a temporary access token provided by STS.md#)" section in OSS Developer Guide.

To use STS to access OSS, you must set the stsToken parameter. Example:

``` {#codeblock_o17_vrf_mzr}
let OSS = require('ali-oss');
let STS = OSS.STS;
let sts = new STS({
  accessKeyId: '<The AccessKey ID of the RAM user>',
  accessKeySecret: '<The AccessKey Secret of the RAM user>'
});
async function assumeRole () {
  try {
    let token = await sts.assumeRole(
    '<role-arn>', '<policy>', '<expiration>', '<session-name>');
    let client = new OSS({
      region: '<region>',
      accessKeyId: token.credentials.AccessKeyId,
      accessKeySecret: token.credentials.AccessKeySecret,
      stsToken: token.credentials.SecurityToken,
      bucket: '<bucket-name>'
    });
  } catch (e) {
    console.log(e);
  }
}
assumeRole();
```

You can customize an STS policy when applying for a temporary token from STS. The temporary permission you apply for is owned by your role and specified by the policy at the same time. The following example specifies the STS policy used to apply for the read-only permission on `my-bucket`, and sets the validity period of the temporary token to 15 minutes:

``` {#codeblock_an9_4d5_erh}
let OSS = require('ali-oss');
let STS = OSS.STS;
let sts = new STS({
  accessKeyId: '<The AccessKey ID of the RAM user>',
  accessKeySecret: '<The AccessKey Secret of the RAM user>'
});
let policy = {
  "Statement": [
    {
      "Action": [
        "oss:Get*"
      ],
      "Effect": "Allow",
      "Resource": ["acs:oss:*:*:my-bucket/*"]
    }
  ],
  "Version": "1"
};
async function assumeRole () {
  try {
    let token = await sts.assumeRole(
    '<role-arn>', policy, 15 * 60, '<session-name>');
    let client = new OSS({
      region: '<region>',
      accessKeyId: token.credentials.AccessKeyId,
      accessKeySecret: token.credentials.AccessKeySecret,
      stsToken: token.credentials.SecurityToken,
      bucket: '<bucket-name>'
    });
  } catch (e) {
    console.log(e);
  }
}
assumeRole();
```

## Use a signed URL to authorize temporary access {#section_ubj_mtq_dhb .section}

-   Generate a signed URL

    You can provide a signed URL to authorize temporary access for a visitor. When a signed URL is generated, you can specify the validity period for the URL to restrict the period of access from visitors.

-   Generate a signed URL for an object

    Use the following code to generate a signed URL for an object:

    **Note:** name \{String\} indicates the name of the object stored in OSS. \[expires\] \{Number\} indicates the validity period of the URL. The default value is 1,800 seconds. For more information about other parameters, see [GitHub](https://github.com/ali-sdk/ali-oss#signatureurlname-options).

    ``` {#codeblock_yg6_uqh_70i}
    let OSS = require('ali-oss');
    let store = new OSS({
        bucket: '<your bucket>',
        region: '<your region>',
        accessKeyId: '<your accessKeyId>',
        accessKeySecret: '<your accessKeySecret>'
    })
    const url = store.signatureUrl('ossdemo.txt');
    console.log(url);
    // --------------------------------------------------
    const url = store.signatureUrl('ossdemo.txt', {
      expires: 3600,
      method: 'PUT'
    });
    console.log(url);
    
    //  put object with signatureUrl
    // -------------------------------------------------
    
    const url = store.signatureUrl('ossdemo.txt', {
      expires: 3600,
      method: 'PUT',
      'Content-Type': 'text/plain; charset=UTF-8',
    });
    console.log(url);
    
    // --------------------------------------------------
    const url = store.signatureUrl('ossdemo.txt', {
      expires: 3600,
      response: {
        'content-type': 'text/custom',
        'content-disposition': 'attachment'
      }
    });
    console.log(url);
    
    // put operation
    ```

-   Generated a signed URL for IMG

    ``` {#codeblock_70b_sp4_5fd}
    let OSS = require('ali-oss');
    let store = new OSS({
        bucket: '<your bucket>',
        region: '<your region>',
        accessKeyId: '<your accessKeyId>',
        accessKeySecret: '<your accessKeySecret>'
    })
    const url = store.signatureUrl('ossdemo.png', {
      process: 'image/resize,w_200'
    });
    console.log(url);
    // --------------------------------------------------
    const url = store.signatureUrl('ossdemo.png', {
      expires: 3600,
      process: 'image/resize,w_200'
    });
    console.log(url);
    ```


