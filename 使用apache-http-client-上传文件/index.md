# 使用Apache Http Client上传文件

<!--more-->

本文实现的是将一个文件从web页面上传至服务端，再从服务端使用Apache httpclient上传到另外一个第三方服务的功能。代码片段是验证通过的使用Apache httpclient上传到另外一个第三方服务完整代码。

## Maven dependency

```
<dependency>
	<groupId>org.apache.httpcomponents</groupId>
	<artifactId>httpclient</artifactId>
	<version>4.5.3</version>
</dependency>
<dependency>
	<groupId>org.apache.httpcomponents</groupId>
	<artifactId>httpmime</artifactId>
	<version>4.5.3</version>
</dependency>

```

## Snippet

```
private String uploadToEkuiper(MultipartFile f){
        CloseableHttpClient httpClient = null;
        CloseableHttpResponse response = null;

        try{
            httpClient = HttpClients.createDefault();
            HttpPost httpPost = new HttpPost(ekuiperEdgeUploadUrl);
            httpPost.setConfig(RequestConfig.custom().setConnectTimeout(1000).build());
            byte [] fileBytes = f.getBytes();
            ByteArrayBody bab = new ByteArrayBody(fileBytes,f.getOriginalFilename());
            HttpEntity reqEntity = MultipartEntityBuilder.create()
                    // 相当于<input type="file" name="file"/>
                    .addPart("uploadFile", bab)
                    .build();
            httpPost.setEntity(reqEntity);
            //发起请求
            response = httpClient.execute(httpPost);
            //获取响应对象
            HttpEntity resEntity = response.getEntity();
            if(resEntity != null){
                return EntityUtils.toString(resEntity, Charset.forName("UTF-8"));
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            try {
                if(response != null){
                    response.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }

            try {
                if(httpClient != null){
                    httpClient.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return null;
    }
```
