# 使用爱组搭实现阿里云OSS上传文件服务


<!--more-->

编程的过程中，如果有好的轮子，我会优先考虑使用，其次是自己从0-1去实现。这次项目中有个需求是通过前端上传文件到阿里云对象存储中供后续逻辑使用。

系统的后端框架用的微服务，文件服务中已经有一部分上传功能了，实现了文件上传到服务所在的机器磁盘，不支持上传对象存储。看到这个需求时我想到了爱组搭。爱组搭是提供SpringBoot第三方组件地代码平台，它的初衷是开发者可以使用它进行低代码甚至无代码构建自己的应用。目前来看的话，只能做到低代码。


这是爱组搭官网：<http://doc.aizuda.com/>


言归正传，本次解决对象存储的需求我用的是爱组搭的OSS文件存储组件。主要步骤就是引入依赖和实现接口逻辑。

## 引入依赖

```
  <!--爱组搭对象存储组件-->
  <dependency>
        <groupId>com.aizuda</groupId>
        <artifactId>aizuda-oss</artifactId>
        <version>1.0.4</version>
  </dependency>

   <!--阿里云对象存SDK-->
  <dependency>
        <groupId>com.aliyun.oss</groupId>
        <artifactId>aliyun-sdk-oss</artifactId>
        <version>3.15.0</version>
  </dependency>
```

爱组搭起到的作用是对阿里云的SDK进行了再封装，简化开发。如果只引入爱组搭对象存储组件服务无法启动。

## 基础配置

配置信息直接写到注册中心nacos配置文件中即可

```
# 配置存储平台 ，为默认存储平台
aizuda:
  oss:
    aliyun-oss:
      platform: aliyun
      endpoint: xxx
      accessKey: xxx
      secretKey: xxx
      bucketName: xxx
      fileName: xxx/
```

## Bean方式注入

```
Configuration
public class OssConfig {
    @Value("${aizuda.oss.aliyun-oss.end-point}")
    private String endpoint;

    @Value("${aizuda.oss.aliyun-oss.accessKey}")
    private String accessKey;

    @Value("${aizuda.oss.aliyun-oss.secretKey}")
    private String secretKey;

    @Value("${aizuda.oss.aliyun-oss.bucketName}")
    private String bucketName;


    @Bean
    public IFileStorage aliyun() {
        // 注入一个自定义存储平台
        OssProperty ossProperty = new OssProperty();
        ossProperty.setPlatform(StoragePlatform.aliyun);
        ossProperty.setBucketName(bucketName);
        ossProperty.setEndpoint(endpoint);
        ossProperty.setAccessKey(accessKey);
        ossProperty.setSecretKey(secretKey);
        return new AliyunOss(ossProperty);
    }
}
```

## 接口实现

```

@Slf4j
@Service(value = "aliyunOssService")
public class AliyunOssFileInfoServiceImpl implements ISysFileInfoService {

    @Reference
    private IFileStorage fileStorage;

    @Value("${aizuda.oss.aliyun-oss.fileName}")
    private String fileName;

    @Value("${aizuda.oss.aliyun-oss.end-point}")
    private String endpoint;

    @Value("${aizuda.oss.aliyun-oss.bucketName}")
    private String bucketName;



    @Override
    public void upload(MultipartFile[] files, String businessType) {
        if (files == null) {
            return new ArrayList<>();
        }

        Arrays.asList(files).stream().map(file -> aliyunUpload(file))).collect(Collectors.toList());
     
    }

    private String aliyunUpload(MultipartFile f){
        ByteArrayInputStream os = null;
        OssResult ossResult = null;
        AtomicReference<String> finalURL = new AtomicReference<>();
        try{
            String objectName = fileName+"module/Custom/"+f.getOriginalFilename();
            byte [] fileBytes = f.getBytes();
            os = new ByteArrayInputStream(fileBytes);
            //上传
            OSS.fileStorage("aliyun").upload(os, f.getOriginalFilename(),objectName);
            finalURL.set(String.format("https://%s.%s/%s", bucketName,endpoint, objectName));
            return finalURL.get();
        }catch (Exception e){
           
        }
    }


}

```
> Tips : 接口实现是主要逻辑，其他无关代码都已经做了删除。 finalURL.set(String.format("https://%s.%s/%s", bucketName,endpoint, objectName)); 这行代码是为了解决组件提供的预览连接失效问题，此处需要手动拼接，供后续逻辑使用。使用这个连接的前提是阿里云OSS中药对bucket权限设置为公开读。
