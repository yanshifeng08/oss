# Authorized access {#concept_303922 .concept}

This topic describes how to authorize access to OSS.

## Use STS for temporary access authorization {#section_iy3_bfe_7mn .section}

You can use Alibaba Cloud Security Token Service \(STS\) to authorize temporary access to OSS. STS is a Web service that provides a temporary access token to a cloud computing user. You can use STS to grant a third-party application or a RAM user \(whose user ID is managed by yourself\) an access credential with customized validity period and permissions. For more information about STS, see [STS introduction](../../../../reseller.en-US/API Reference (STS)/What is STS?.md#).

STS has the following advantages:

-   You can generate an access token and send it to the third-party application for authorization without exposing your long-term key \(AccessKey\). You can customize the access permissions and validity period of the token.
-   The access token automatically becomes invalid after expiration.

For more information about how to use STS to access OSS, see [Access control](../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#) in OSS Developer Guide.

Run the following code to create a signed request by using STS:

``` {#codeblock_kjr_4d5_c4q}
const express = require('express');
const { STS } = require('ali-oss');
const fs = require('fs');

const app = express();
const path = require('path');
const conf = require('./config');

app.get('/sts', (req, res) => {
  console.log(conf);
  let policy;
  if (conf.PolicyFile) {
    policy = fs.readFileSync(path.resolve(__dirname, conf.PolicyFile)).toString('utf-8');
  }

  const client = new STS({
    accessKeyId: conf.AccessKeyId,
    accessKeySecret: conf.AccessKeySecret
  });

  client.assumeRole(conf.RoleArn, policy, conf.TokenExpireTime).then((result) => {
    console.log(result);
    res.set('Access-Control-Allow-Origin', '*');
    res.set('Access-Control-Allow-METHOD', 'GET');
    res.json({
      AccessKeyId: result.credentials.AccessKeyId,
      AccessKeySecret: result.credentials.AccessKeySecret,
      SecurityToken: result.credentials.SecurityToken,
      Expiration: result.credentials.Expiration
    });
  }).catch((err) => {
    console.log(err);
    res.status(400).json(err.message);
  });
});

app.use('/static', express.static('public'));
app.get('/', (req, res) => {
  res.sendFile(path.resolve(__dirname, '../index.html'));
});

app.listen(9000, () => {
  console.log('App started.');
});
```

## Sign a URL to authorize temporary access {#section_j5o_0n2_9c6 .section}

-   Sign a URL

    You can provide a signed URL to a visitor for temporary access. When you sign a URL, you can specify the expiration time of the URL to restrict the period of access from visitors.

-   Sign a URL for an object

    **Note:** name \{String\} indicates the name of the target object. \[expires\] \{Number\} indicates the expiration period of the URL, which is 1,800 seconds by default. For more information about other parameters, see [Github](https://github.com/ali-sdk/ali-oss#signatureurlname-options).

    Run the following code to sign a URL for an object:

    ``` {#codeblock_nns_zte_fja}
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

-   Run the following code to sign a URL for image processing:

    ``` {#codeblock_zx2_8so_xlv}
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


