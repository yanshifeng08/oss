# OSS Select {#task_c2l_sc1_ffb .task}

OSS Select allows you to use simple SQL statements to select content from an object in OSS to obtain only required data. In this way, OSS Select reduces the amount of data transmitted from OSS to improve the data retrieval efficiency.

-   OSS Select supports UTF-8 encoded CSV objects and JSON objects. Supported CSV objects and CSV-like objects \(such as TSV objects\) must conform to RFC 4180. You can customize row and column delimiters and quote characters in CSV objects.
-   You can use the console to extract a maximum of 40 MB of data from an object of no larger than 128 MB. To process an object larger than 128 MB or retrieve more records, call [SelectObject](../../../../reseller.en-US/API Reference/Object operations/SelectObject.md#).

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the left-side bucket list, click the name of the target bucket.
3.  In the overview page of the bucket, click the **Files** tab.
4.  On the right of the object that you want to select content from, click **More** \> **Select Content**.
5.  In the Select Content dialog box, set the following parameters: 
    -   File Type: Select an object format as required. Valid values: CSV and JSON.
    -   Delimiter: This parameter is set for CSV objects. You can click comma \(,\) or Custom.
    -   Title Line: This parameter is set for CSV objects. You can configure this option to specify whether the first row of the object contains a column heading.
    -   JSON Display Mode: This parameter is set for JSON objects. Select the display mode of your JSON object.
    -   Compression Format: Specify whether to compress the current object. Currently, only GZIP-based compression is supported.
6.  Click **Preview** to preview the object. 

    **Note:** Fees incur when you preview an object scanned by OSS Select.

    -   Standard: You will incur fees when you preview an object scanned by OSS Select.
    -   IA and Archive: You will incur fees when you preview an object scanned by OSS Select and through data retrieval.
7.  Click **Next Step**. Enter and execute an SQL statement. 

    **Note:** For more information about how to use the SQL statement, see [SelectObject](../../../../reseller.en-US/API Reference/Object operations/SelectObject.md#table_kh4_4z2_2fb).

8.  View the execution results. Click **Download** to download the selected content to the local device.

Assume that an object named People contains the following columns: Name, Company, and Age.

-   To query people who are more than 50 years old and whose names start with string "Lora", execute the following SQL statement. In the statement, \_1 and \_3 indicate the index of the first and third column separately.

    ``` {#codeblock_4ja_w9s_ovv}
    select * from ossobject where _1 like 'Lora*' and _3 > 50
    ```

-   To query the count of rows in the object, maximum age, and minimum age, execute the following SQL statement:

    ``` {#codeblock_otu_psk_y7o}
    select count(*), max(cast(_3 as int)), min(cast(_3 as int)) from ossobject
    ```


