# 使用 Java SDK 的 SelectObject 查询 CSV 和 Json 文件 {#concept_1062344 .concept}

本文介绍如何使用 Java SDK 的 SelectObject 查询 CSV 和 Json 文件。

**说明：** 

本文示例由阿里云用户 [bin](https://yq.aliyun.com/u/apple1024) 提供。

以下代码用于查询 CSV 文件和 Json 文件：

``` {#codeblock_ox2_oj5_8wo}
 private static final String csvKey = "test.csv";

 // Endpoint 以杭州为例，其它 Region 请按实际情况填写。
 private static final String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
 // 阿里云主账号 AccessKey 拥有所有 API 的访问权限，风险很高。
 // 强烈建议您创建并使用 RAM 账号进行 API 访问或日常运维，请登录 https://ram.console.aliyun.com 创建 RAM 账号。
 private static final String accessKeyId = "<yourAccessKeyId>";
 private static final String accessKeySecret = "<yourAccessKeySecret>";
 private static final String bucketName = "<yourBucketName>";

    /**
    * CreateSelectObjectMeta API 用于获取目标文件总的行数，总的列数（对于 CSV 文件），以及 Splits 个数。
    */
    public static void createCsvSelectObjectMetadata() {
      // 创建 OSSClient 实例。
      OSS client = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

      String content = "name,school,company,age\r\n" +
           "Lora Francis,School A,Staples Inc,27\r\n" +
           "Eleanor Little,School B,\"Conectiv, Inc\",43\r\n" +
           "Rosie Hughes,School C,Western Gas Resources Inc,44\r\n" +
           "Lawrence Ross,School D,MetLife Inc.,24";

      client.putObject(bucketName, csvKey, new ByteArrayInputStream(content.getBytes()));

       SelectObjectMetadata selectObjectMetadata = client.createSelectObjectMetadata(
            new CreateSelectObjectMetadataRequest(bucketName, csvKey)
               .withInputSerialization(
                    new InputSerialization().withCsvInputFormat(
                        new CSVFormat().withHeaderInfo(CSVFormat.Header.Use)
                            .withRecordDelimiter("\r\n"))));
       //获取 csv 的总列数。
       System.out.println(selectObjectMetadata.getCsvObjectMetadata().getTotalLines());
       //获取 csv 的 splits 个数。
       System.out.println(selectObjectMetadata.getCsvObjectMetadata().getSplits());
       client.shutdown();
   }

    /**
    * 查询 csv。
    */
    public static void selectCsv() {
      // 创建 OSSClient 实例。
      OSS client = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

      try {
         //保存 Select 请求的容器。
         SelectObjectRequest selectObjectRequest =
             new SelectObjectRequest(bucketName, csvKey)
                 .withInputSerialization(
                      new InputSerialization().withCsvInputFormat(
                          new CSVFormat().withHeaderInfo(CSVFormat.Header.Use)
                              .withRecordDelimiter("\r\n")))
                 .withOutputSerialization(new OutputSerialization().withCsvOutputFormat(new CSVFormat()));

         // ossobject 不可修改，查询第 4 列值大于 40 的数据，会直接进行隐式转换。
         selectObjectRequest.setExpression("select * from ossobject where _4 > 40");
         OSSObject ossObject = client.selectObject(selectObjectRequest);

         writeToFile(ossObject.getObjectContent(), "result.csv");
        } catch (OSSException ex) {
           System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } catch (ClientException ex) {
           System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } finally {
            client.shutdown();
        }
    }

    /**
     * 查询简单 json。
     */
    public static void selectSimpleJson() {
       String key = "simple.json";
       // 创建 OSSClient 实例。
       OSS client = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

       final String content = "{\n" +
             "\t\"name\": \"Lora Francis\",\n" +
             "\t\"age\": 27,\n" +
             "\t\"company\": \"Staples Inc\"\n" +
             "}\n" +
             "{\n" +
             "\t\"name\": \"Eleanor Little\",\n" +
             "\t\"age\": 43,\n" +
             "\t\"company\": \"Conectiv, Inc\"\n" +
             "}\n" +
             "{\n" +
             "\t\"name\": \"Rosie Hughes\",\n" +
             "\t\"age\": 44,\n" +
             "\t\"company\": \"Western Gas Resources Inc\"\n" +
             "}\n" +
             "{\n" +
             "\t\"name\": \"Lawrence Ross\",\n" +
             "\t\"age\": 24,\n" +
             "\t\"company\": \"MetLife Inc.\"\n" +
             "}";

        try {
            client.putObject(bucketName, key, new ByteArrayInputStream(content.getBytes()));
            SelectObjectRequest selectObjectRequest =
                 new SelectObjectRequest(bucketName, key)
                     .withInputSerialization(new InputSerialization()
                          .withCompressionType(CompressionType.NONE)
                          .withJsonInputFormat(new JsonFormat().withJsonType(JsonType.LINES)))
                          .withOutputSerialization(new OutputSerialization()
                          .withCrcEnabled(true)
                          .withJsonOutputFormat(new JsonFormat()))
                     .withExpression("select * from ossobject as s where s.age > 40");

            OSSObject ossObject = client.selectObject(selectObjectRequest);
            writeToFile(ossObject.getObjectContent(), "result.simple.json");
        } catch (OSSException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } catch (ClientException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } finally {
            client.shutdown();
        }
    }

    /**
     * 查询复杂 json。
     */
    public static void selectComplexJson() {
        String key = "complex.json";
        // 创建 OSSClient 实例。
        OSS client = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

        String content = "{\n" +
             "  \"contacts\":[\n" +
             "{\n" +
             "  \"firstName\": \"John\",\n" +
             "  \"lastName\": \"Smith\",\n" +
             "  \"isAlive\": true,\n" +
             "  \"age\": 27,\n" +
             "  \"address\": {\n" +
             "    \"streetAddress\": \"21 2nd Street\",\n" +
             "    \"city\": \"New York\",\n" +
             "    \"state\": \"NY\",\n" +
             "    \"postalCode\": \"10021-3100\"\n" +
             "  },\n" +
             "  \"phoneNumbers\": [\n" +
             "    {\n" +
             "      \"type\": \"home\",\n" +
             "      \"number\": \"212 555-1234\"\n" +
             "    },\n" +
             "    {\n" +
             "      \"type\": \"office\",\n" +
             "      \"number\": \"646 555-4567\"\n" +
             "    },\n" +
             "    {\n" +
             "      \"type\": \"mobile\",\n" +
             "      \"number\": \"123 456-7890\"\n" +
             "    }\n" +
             "  ],\n" +
             "  \"children\": [],\n" +
             "  \"spouse\": null\n" +
             "}\n" +
             "]}";

        try {
            client.putObject(bucketName, key, new ByteArrayInputStream(content.getBytes()));
            SelectObjectRequest selectObjectRequest =
                new SelectObjectRequest(bucketName, key)
                   .withInputSerialization(new InputSerialization()
                        .withCompressionType(CompressionType.NONE)
                        .withJsonInputFormat(new JsonFormat().withJsonType(JsonType.LINES)))
                   .withOutputSerialization(new OutputSerialization()
                        .withCrcEnabled(true)
                        .withJsonOutputFormat(new JsonFormat()))
                   //返回所有 age 是 27 的记录，表达式可以根据 json 对象结构进行嵌套调用。
                   .withExpression("select * from ossobject.contacts[*] s where s.age = 27");

            OSSObject ossObject = client.selectObject(selectObjectRequest);
            writeToFile(ossObject.getObjectContent(), "result.complex.json");
        } catch (OSSException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } catch (ClientException ex) {
            System.out.println(ex.getErrorCode().concat(",").concat(ex.getErrorMessage()));
        } finally {
            client.shutdown();
        }
    }

    /**
     * 写入文件。
     *
     * @param in
     * @param file
     */
    private static void writeToFile(InputStream in, String file) {
        try {
            BufferedOutputStream outputStream = new BufferedOutputStream(new FileOutputStream(file));
            byte[] buffer = new byte[1024];
            int bytesRead;
            while ((bytesRead = in.read(buffer)) != -1) {
              outputStream.write(buffer, 0, bytesRead);
            }
            outputStream.close();
        } catch (FileNotFoundException ex) {
            ex.printStackTrace();
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }
```

SelectObject 的更多详情，请参考[SelectObject](../../../../cn.zh-CN/API 参考/关于Object操作/SelectObject.md#)。

