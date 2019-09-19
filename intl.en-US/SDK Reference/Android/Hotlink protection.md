# Hotlink protection {#concept_2118548 .concept}

This topic describes how to use hotlink protection.

To configure hotlinking protection, you need to set the following parameters:

-   Referer Whitelist: the referer whitelist that specifies the domains that are allowed to access OSS resources.
-   Allow Empty Referer: indicates whether the Referer field can be left empty in a request. If the Referer field cannot be left empty, users must include the Referer field in the HTTP or HTTPS request header so as to access OSS resources.

For more information about hotlink protection, see [Configure hotlink protection](../../../../reseller.en-US/Developer Guide/Buckets/Configure hotlinking protection.md#).

## Configure hotlink protection {#section_jxy_lh4_wxx .section}

``` {#codeblock_ovl_v25_kgs}
PutBucketRefererRequest request = new PutBucketRefererRequest();
request.setBucketName("yourBucketName");
// Add a Referer whitelist. You can use an asterisk (*) or a question mark (?) as a wildcard character to set the Referer parameter.
ArrayList<String> referers = new ArrayList<String>();
referers.add("http://www.aaa.com");
referers.add("http://www. *.com");
referers.add("http://www.?.com");
request.setReferers(referers);

OSSAsyncTask task = oss.asyncPutBucketReferer(request, new OSSCompletedCallback<PutBucketRefererRequest, PutBucketRefererResult>() {
    @Override
    public void onSuccess(PutBucketRefererRequest request, PutBucketRefererResult result) {
        OSSLog.logInfo("code: " + result.getStatusCode());
    }
    @Override
    public void onFailure(PutBucketRefererRequest request, ClientException clientException, ServiceException serviceException) {
        OSSLog.logError("error: "+serviceException.getRawMessage());
    }
});
task.waitUntilFinished();
```

## Obtain hotlink protection information {#section_qvl_ybs_723 .section}

``` {#codeblock_ibe_hyf_ut8}
GetBucketRefererRequest request = new GetBucketRefererRequest();
request.setBucketName("yourBucketName");
OSSAsyncTask task = oss.asyncGetBucketReferer(request, new OSSCompletedCallback<GetBucketRefererRequest, GetBucketRefererResult>() {
    @Override
    public void onSuccess(GetBucketRefererRequest request, GetBucketRefererResult result) {
        // Obtain the Referer whitelist of the bucket.
        ArrayList<String> list = result.getReferers();
        for (String ref : list){
            OSSLog.logInfo("info: " + ref);
        }
    }
    @Override
    public void onFailure(GetBucketRefererRequest request, ClientException clientException, ServiceException serviceException) {
        OSSLog.logError("error: "+serviceException.getRawMessage());
    }
});
task.waitUntilFinished();
```

## Clear hotlink protection configurations {#section_2di_3qx_rqj .section}

``` {#codeblock_fxs_76j_oi7}
PutBucketRefererRequest request = new PutBucketRefererRequest();
request.setBucketName("yourBucketName");
request.setAllowEmpty(true);
// You cannot clear hotlink protection configurations directly. To clear hotlink protection configurations, you must create a rule that allows an empty Referer field to replace the existing rule.
ArrayList<String> referers = new ArrayList<String>();

request.setReferers(referers);
OSSAsyncTask task = oss.asyncPutBucketReferer(request, new OSSCompletedCallback<PutBucketRefererRequest, PutBucketRefererResult>() {
    @Override
    public void onSuccess(PutBucketRefererRequest request, PutBucketRefererResult result) {
        OSSLog.logInfo("code: " + result.getStatusCode());

    }
    @Override
    public void onFailure(PutBucketRefererRequest request, ClientException clientException, ServiceException serviceException) {
        OSSLog.logError("error: "+serviceException.getRawMessage());

    }
});
task.waitUntilFinished();
```

