# Lifecycle rule configuration examples {#concept_262491 .concept}

This topic provides examples of lifecycle rule configuration.

You can set lifecycle rules for objects in a bucket through OSS APIs. A lifecycle rule is in XML format, as shown in the following example:

``` {#codeblock_iua_ct4_2k8}
<LifecycleConfiguration>
<Rule>
<ID>delete logs after 10 days</ID>
<Prefix>logs/</Prefix>
<Status>Enabled</Status>
<Expiration>
<Days>10</Days>
</Expiration>
</Rule>
<Rule>
<ID>delete doc</ID>
<Prefix>doc/</Prefix>
<Status>Disabled</Status>
<Expiration>
<CreatedBeforeDate>2017-12-31T00:00:00.000Z</CreatedBeforeDate>
</Expiration>
</Rule>
<Rule>
<ID>delete xx=1</ID>
<Prefix>rule2</Prefix>
<Tag><Key>xx</Key><Value>1</Value></Tag>
<Status>Enabled</Status>
<Transition>
<Days>60</Days>
<StorageClass>Archive</StorageClass>
</Transition>
</Rule>
</LifecycleConfiguration>
		
```

In the preceding example, the following three lifecycle rules are set:

-   The first rule indicates that objects that are prefixed with logs/ and were modified 10 days ago are deleted.
-   The second rule indicates that objects that are prefixed with doc/ and were modified before December 31, 2014 are deleted. However, the rule does not take effect because it is in Disabled status.
-   The third rule indicates that the storage class of objects that are tagged with "xx=1" and were modified 60 days ago is converted to Archive.

You must configure the following elements when setting lifecycle rules:

-   <ID\>: Indicates the unique identifier for a rule.
-   <Status\>: Indicate the status of the lifecycle rule with two values: Enabled or Disabled. Only rules with the Enabled status is applied.
-   <Prefix\>: Indicates that the rule applies only to objects with the specified prefix.
-   <Expiration\>: Indicates operation to be performed on expired objects. The sub-element <CreatedBeforeDate\> and <Days\> indicates the absolute and relative expiration time respectively.
    -   <CreatedBeforeDate\>: Specifies an expiration date and the operation to be performed on expired objects. An object modified before the specified date expires, and the specified operation is performed on the object.
    -   <Days\>Specifies an expiration period \(N days\) and the operation to be performed on expired objects. An object expires N days after it is modified for the last time, and the specified operation is performed on the object.

