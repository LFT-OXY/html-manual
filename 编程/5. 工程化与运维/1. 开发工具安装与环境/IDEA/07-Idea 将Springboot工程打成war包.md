## Idea将SpringBoot工程打成War包

#### 将pom.xml中，打包方式修改为war

```xml
    <!--该项目最终的打包方式为：war-->
    <packaging>war</packaging>
```

![image-20201224171816605](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224171816.png)



#### 剔除内置的tomcat

这部很重要，如果不剔除，内置tomcat将会与外部的tomcat产生冲突

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!-- 将tomcat从WEB启动器中移除 -->
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

#### 导入servlet.api

```xml
		<!--引入servlet api相关Jar包 -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
```

#### 修改启动类

SpringBootExtendApplication 继承 SpringBootServletInitializer

```java
import org.springframework.boot.autoconfigure.SpringBootApplication;

import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;
import org.springframework.scheduling.annotation.EnableScheduling;

/**
 * @EnableScheduling 开启定时任务注解
 */
@EnableScheduling
@SpringBootApplication
public class SpringBootExtendApplication extends SpringBootServletInitializer {

    /**
     * 将SpringBoot的全部内容，向Tomcat中去构建
     */
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        // TODO Auto-generated method stub
        return builder.sources(SpringBootExtendApplication.class); 	//启动加载项
    }
}
```

#### 修改maven插件

```xml
    <build>
        <!--设置war的名称-->
        <finalName>boots</finalName>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <!--此处配置：程序的主启动类-->
                    <mainClass>com.woniuxy.springbootextend.SpringBootExtendApplication</mainClass>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <!-- 导入war包插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <!-- 配置忽略web.xml -->
                <configuration>
                    <failOnMissingWebXml>false</failOnMissingWebXml>
                </configuration>
            </plugin>

        </plugins>
    </build>
```

可以修改的位置：

1、设置war的名称

![image-20201224172950859](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224172950.png)

2、设置主启动类的路径

![image-20201224173032394](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224173032.png)



#### 打war包

一定要先clean，再package

![image-20201224173227357](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224173228.png)

最后结果是：

![image-20201224173612293](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224173612.png)

#### war包位置

不管是jar包，还是war包，构建成功之后， 都是在target目录下

![image-20201224173705635](https://woniumd.oss-cn-hangzhou.aliyuncs.com/java/puxubo/20201224173705.png)