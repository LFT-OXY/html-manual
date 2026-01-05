# day07 OSS

## 一、分布式存储的解决方案

OSS（Object Storage Service）对象存储服务。对象可以是file，avi，mp3，images等，存储就是使用统一的一个方式来进行附件处理。Service说明，会提供一个服务（ip+端口），来通过链接分享我们的图片。

![image-20250410103812489](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504101038579.png)

> 理解OSS中的专业名词，至少保证，就业面试的时候，聊到OSS，要听得懂，而不是 嗯嗯嗯。你说得对。

## 二、专业术语

### OSS

Object Storage Service俗称对象存储，主要提供图片、文档、音频、视频等二进制文件的海量存储功能。目前除了公有云提供对象存储服务外，一般私有云比较关心一些开源的分布式对象存储解决方案，本文列举了一些常见的技术方案供参考。

### 块存储 b

通常SAN（Storage Area Network）结构的产品属于块存储，比如我们常见的硬盘、磁盘阵列等物理盘。

### 文件存储

一般NAS（Network Attached Storage）产品都是文件级存储，如Ceph的CephFS，另外GFS、HDFS等也属于文件存储。

### 对象存储

同时兼顾着SAN高速直接访问磁盘特点及NAS的分布式共享特点的一类存储，一般是通过RESTful接口访问。

### 常见解决方案

#### Swift

Swift 是 OpenStack 社区核心子项目，是一个**弹性可伸缩**、高可用的分布式对象存储系统，使用Python语言实现，采用 Apache 2.0 许可协议。

Swift 提供一个基于RESTful HTTP接口的 Object Storage API，用于创建，修改和获取对象和元数据。用户可以使用 Swift 高效、安全且廉价地存储大量数据。Swift 整体架构：

![image-20210718181203768](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206051406300.png)

总的来说，企业如果想要建立可扩展的分布式对象存储集群，可以考虑 Swift。

#### Ceph

Ceph是一种高性能、高可用、可扩展的分布式存储系统，统一的对外提供对象存储、块存储以及文件存储功能，底层使用C/C++语言。

其中对象存储功能支持 2 种接口：
1、兼容S3：提供了对象存储接口，兼容 S3 RESTful 接口的一个大子集。
2、兼容Swift：提供了对象存储接口，兼容 Openstack Swift 接口的一个大子集。

![image-20210718181304987](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206051406358.png)

Ceph是一个企业级分布式存储系统，功能强大，不仅可以为企业建立对象存储服务，还可以帮助企业建立自己的**云平台**，具有广泛的应用场景特别是在**云环境**(阿里云OSS推荐)下使用广泛。

#### Minio

Minio是一个企业级、兼容S3接口的对象存储系统。Minio基于 Apache 2.0 许可协议，采用Go语言实现，客户端支持Java、Python、Go等多种语言，是一种轻量级、高并发的开源解决方案，可以作为云存储方案用来保存海量的图片，视频，文档等。

大数据集成方面，Minio支持各种常见的查询计算引擎，比如Spark、Presto、Hive以及Flink等，可以使用这些处理框架查询分析对象数据，此外，Minio支持Parquet，Json、Csv格式等多种文件存储格式，包括压缩与编码。更多特性可以参考官网 地址[https://min.io。Minio架构：](https://min.io.xn--minio/)

![img](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206051406866.png)

Minio主要为人工智能、机器学习而设计，并适用于其他大数据负载。从架构与功能方面考虑，Minio是一个比较好的开源对象存储解决方案。

#### HBase MOB

这是利用HBase的MOB特性支持对象存储功能。Apache HBase2.0 版本开始支持中等对象存储（Medium Object Storage，简称 MOB），这个特性使得HBase能够非常良好的存储大小在100KB-10M的图片、文档、音频、短视频等二进制数据。

![image-20210718181615972](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206051406797.png)

架构如上，HBase MOB的设计类似于HBase + HDFS的方式，中等对象在写入HDFS之前同样是先写入MemStore，但是刷写与其他写入数据不同，MOB数据被刷写到MOB File中，MOB File被存放在特殊的Region中。

MOB特性在Apache HBase 2.0、CDH 5.4.x 或 HDP 2.5.x 及以上版本支持，用户可以基于HBase MOB特性设计自己的对象存储服务。

## 三、Minio的使用(重点)

[官方文档](https://www.minio.org.cn/docs/minio/windows/operations/install-deploy-manage/deploy-minio-single-node-single-drive.html)

### 1. 安装启动

#### [2) 为 MinIO 准备数据路径](https://www.minio.org.cn/docs/minio/windows/operations/install-deploy-manage/deploy-minio-single-node-single-drive.html#id16)

确保数据路径为空，且不包含任何现有文件，包括隐藏或Windows系统文件。

如果指定了一个不为MinIO专用的驱动器，考虑创建一个专用于存储MinIO数据的专用文件夹，例如 `D:/Minio`。

#### [3) 启动 MinIO 服务器](https://www.minio.org.cn/docs/minio/windows/operations/install-deploy-manage/deploy-minio-single-node-single-drive.html#id17)

打开命令提示符或PowerShell，并执行以下命令来在该会话中启动MinIO SNSD 部署：

```
minio.exe server F:\miniodata --console-address ":9001"
```



输出应类似于以下内容：

```
Status:         1 Online, 0 Offline.
API: http://192.168.2.100:9000  http://127.0.0.1:9000
Console: http://192.168.2.100:9001 http://127.0.0.1:9001

Command-line: https://minio.org.cn/docs/minio/linux/reference/minio-mc.html
   $ mc alias set myminio http://10.0.2.100:9000 minioadmin minioadmin

Documentation: https://minio.org.cn/docs/minio/linux/index.html
```

`API` 列出了客户端可以访问MinIO S3 API的网络接口和端口。 `Console` 列出了网络接口和端口，客户端可以通过这些接口和端口访问MinIO网页端的控制台。

![image-20250410113402264](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504101134341.png)

### 2. 访问后台 webUI

![image-20250410113534396](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504101135547.png)

创建桶之后，按下图来测试是否可以上传。

![sp20250108_151237_821](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202501081513340.gif)

![1](http://192.168.0.70:9001/api/v1/download-shared-object/aHR0cDovLzEyNy4wLjAuMTo5MDAwL2phdmExMTIvaGFja2VyLmpwZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPURIT05aTUI0Q0RVRk5EMkhIR1pXJTJGMjAyNTA0MTAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNDEwVDAzMzcyNFomWC1BbXotRXhwaXJlcz00MzIwMCZYLUFtei1TZWN1cml0eS1Ub2tlbj1leUpoYkdjaU9pSklVelV4TWlJc0luUjVjQ0k2SWtwWFZDSjkuZXlKaFkyTmxjM05MWlhraU9pSkVTRTlPV2sxQ05FTkVWVVpPUkRKSVNFZGFWeUlzSW1WNGNDSTZNVGMwTkRJNU9USTJPQ3dpY0dGeVpXNTBJam9pYldsdWFXOWhaRzFwYmlKOS5QNzUybFR0S0R0Mm5jMTBYUnR0QlFyU2M3Mm5VNTJENEw0RE03a1JxZTNEM0VDb3lYVFU1WS1lMUNBZVFaYTNESmk4Yjc1TWtxYnduNEhIQUhSTE9HdyZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmdmVyc2lvbklkPW51bGwmWC1BbXotU2lnbmF0dXJlPTZiMmQ1ZDMwMzUzNDkzODI5ZTNjYzA5YTQ3YjcwNWEwOWNmNTdkMGU0MjU0ODI3ZDg4ZmU2NjdmYmZkMzY4NmY)

简单测试后，可以看见本地通过内部分项的链接，就可以成功访问minio的图片。

## 四、在微服务项目中使用

### 1.引入依赖

https://minio.org.cn/docs/minio/linux/developers/java/minio-java.html

根据文档配置依赖即可.

```java
<!--     minio的依赖   -->
        <dependency>
            <groupId>io.minio</groupId>
            <artifactId>minio</artifactId>
            <version>8.5.17</version>
        </dependency>
        <dependency>
            <groupId>com.squareup.okhttp3</groupId>
            <artifactId>okhttp</artifactId>
            <version>4.11.0</version>
        </dependency>
```

### 2. 官方代码的演示

```java
package com.woniuxy.utils;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/10/星期四 11:45
 * @Component: 成都蜗牛学苑
 **/
import io.minio.BucketExistsArgs;
import io.minio.MakeBucketArgs;
import io.minio.MinioClient;
import io.minio.UploadObjectArgs;
import io.minio.errors.MinioException;
import java.io.IOException;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;

public class MinioUtils {
    public static void main(String[] args)
            throws IOException, NoSuchAlgorithmException, InvalidKeyException {
        try {
            // Create a minioClient with the MinIO server playground, its access key and secret key.
            MinioClient minioClient =
                    MinioClient.builder()
                            .endpoint(" http://192.168.0.70:9000")
                            .credentials("OokEzumDglCZNUlDMvF4", "U85WGo9lbOC4U1x66bi8FbsJhfh7NOsOe06Z6vRH")
                            .build();

            // Make 'asiatrip' bucket if not exist.
            boolean found =
                    minioClient.bucketExists(BucketExistsArgs.builder().bucket("java112").build());
            if (!found) {
                // Make a new bucket called 'asiatrip'.
                minioClient.makeBucket(MakeBucketArgs.builder().bucket("java112").build());
            } else {
                System.out.println("Bucket 'java112' already exists.");
            }

            // Upload '/home/user/Photos/asiaphotos.zip' as object name 'asiaphotos-2015.zip' to bucket
            // 'asiatrip'.
            minioClient.uploadObject(
                    UploadObjectArgs.builder()
                            .bucket("java112")
                            .object("蜗牛桌面.jpg")
                            .filename("C:\\Users\\ASUS\\Pictures\\微信图片_20240329110804.jpg")
                            .build());
//            System.out.println(
//                    "'/home/user/Photos/asiaphotos.zip' is successfully uploaded as "
//                            + "object 'asiaphotos-2015.zip' to bucket 'asiatrip'.");
        } catch (MinioException e) {
            System.out.println("Error occurred: " + e);
            System.out.println("HTTP trace: " + e.httpTrace());
        }
    }
}
```

### 3.创建accessKey和SecriteKey

![image-20250410114745145](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504101147344.png)

![image-20250410114819682](https://woniumd.oss-cn-hangzhou.aliyuncs.com/security/mayuhang/202504101148763.png)



### 3.创建一个OSS模块 8010

nacos配置中心,如果想要用就用

```yml
server:
  port: 8010
spring:
  application:
    name: myh-oss
minio:
  endpoint: http://192.168.1.101:9000
  accessKey: OokEzumDglCZNUlDMvF4
  secretKey: U85WGo9lbOC4U1x66bi8FbsJhfh7NOsOe06Z6vRH
```



### 4.commons中创建图片响应实体类

> 其他模块都要用到这个实体类,才能拿到oss模块返回出来的资源地址.

```java
package com.woniuxy.entity;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/10/星期四 12:09
 * @Component: 成都蜗牛学苑
 **/
@Data
@AllArgsConstructor
@NoArgsConstructor
public class UploadResult {
    private String fileName;
    private String fileUrl;
}
```

### 5. OSS模块下开发图片minio上传配置客户端

```java
package com.woniuxy.config;

import io.minio.MinioClient;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/10/星期四 11:45
 * @Component: 成都蜗牛学苑
 **/
@Configuration
public class MinioConfiguration {
    @Value("${minio.endpoint}")
    String endpoint;
    @Value("${minio.accessKey}")
    String accessKey;
    @Value("${minio.secretKey}")
    String secretKey;
    @Bean
    public MinioClient minioClient(){
        return MinioClient.builder()
                .endpoint(endpoint)
                .credentials(accessKey,secretKey)
                .build();
    }
}
```



### 6. oss模块下,开发controller接口

```java
package com.woniuxy.controller;

import com.woniuxy.entity.UploadResult;
import io.minio.GetPresignedObjectUrlArgs;
import io.minio.MinioClient;
import io.minio.ObjectWriteResponse;
import io.minio.PutObjectArgs;
import io.minio.http.Method;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;
import java.io.InputStream;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/04/10/星期四 12:13
 * @Component: 成都蜗牛学苑
 **/
@RestController
@RequestMapping("/oss")
public class OssController {
    @Autowired
    MinioClient minioClient;
    @PostMapping("/upload")
    public UploadResult upload(@RequestParam("file") MultipartFile file) {
        String fileName = file.getOriginalFilename();
        long size = file.getSize();
        try {
            InputStream inputStream = file.getInputStream();
            //通过stream流来上传图片
            ObjectWriteResponse oss = minioClient.putObject(
                            PutObjectArgs.builder()
                                    .stream(inputStream,size,-1l)
                                    .object(fileName)
                                    .bucket("java112")
                                    .build()
            );
            //获取文件的访问路径 share分享的url地址
            String url = minioClient.getPresignedObjectUrl(
                    GetPresignedObjectUrlArgs.builder()
                            .method(Method.GET)
                            .bucket("java112")
                            .object(fileName)
                            .build());
            return new UploadResult(oss.object(),url);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

> 如果要完善细节,请配置nacos,gateway,然后单独指定断言.
>
> 只要是上传图片,都直接访问oss服务.

## 五、阿里云oss流程(不强制要求)

### 利用阿里云OSS实现文件存储

阿里云对象存储*OSS*（Object Storage Service）是阿里云提供的海量、安全、低成本、高持久的云存储服务。其数据设计持久性不低于99.9999999999%（12个9），服务可用性…有关*阿里云存储服务*的客户案例、解决方案等，请参见阿里云存储产品家族。

访问地址：https://www.aliyun.com/

#### 开通阿里云OSS服务

点击“产品”->“存储”->“对象存储OSS”

![image-20220628233641509](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282336709.png)

点击“立即开通”，后进入登录页面，自己注册登录，实名认证。

![image-20220628233759545](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282337876.png)

勾选协议，点击立即开通

![image-20220628234310775](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282343871.png)

开通完毕，进入控制台！

![image-20220628234334654](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282343752.png)

#### 创建桶

bucket列表->创建bucket

![image-20220628234600892](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282346986.png)

指定桶名字以及存储区域

![image-20220628234850809](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282348896.png)

![image-20220628235009369](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282350450.png)

#### 设置AccessKey

![image-20220628235228837](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282352922.png)

点击“开始使用子用户AccessKey”

![image-20220628235324570](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282353656.png)

点击“创建用户”

![image-20220628235404103](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282354186.png)

指定登录名和显示名称，选择“编程访问”，点击“确定创建”，验证身份后创建成功。

![image-20220628235538032](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282355127.png)

**创建完毕之后，在用户列表下游用户信息和accesskey id和accesskey secret，请立即将access key保存**

![image-20220628235901300](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206282359471.png)

#### 指定用户权限

返回用户菜单，点击用户信息

![image-20220629000034733](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206290000825.png)

选择”权限管理“选项卡，点击”添加权限“

![image-20220629000116221](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206290001322.png)

授予全部权限，显示授权成功后，就随便看看吧。

![image-20220629000227450](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/mayuhang/202206290002518.png)

