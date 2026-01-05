# day09 ELK

## 一、项目背景与目标

springboot和我们的ELK结合起来，可以实现一个强大的日志收集、分析和可视化系统。本项目旨在搭建这样一个系统，以满足企业内部对日志数据的需求，提升运维效率和问题诊断能力。具体来说，我们将使用Spring Boot作为后端服务，负责接收、处理和存储日志数据；Elasticsearch作为搜索引擎，用于索引和搜索日志数据；而Kibana则作为可视化工具，用于创建各种图表和仪表盘，方便用户分析和理解日志数据。

### 1. Springboot整合



```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

#### 实体类

```java
package com.woniuxy.entity;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.springframework.data.annotation.Id;
import org.springframework.data.elasticsearch.annotations.Document;
import org.springframework.data.elasticsearch.annotations.Field;
import org.springframework.data.elasticsearch.annotations.FieldType;

import java.math.BigDecimal;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/01/09/0009 16:41
 * @Component: 成都蜗牛学苑
 **/
@Data
@AllArgsConstructor
@NoArgsConstructor
@Document(indexName = "woniu")
public class Goods {
    @Id
    @Field(name="id", type = FieldType.Keyword)
    private String id;
    @Field(type = FieldType.Text,analyzer = "ik_max_word",searchAnalyzer = "ik_smart")
    private String name;
    @Field(type = FieldType.Text)
    private String goodsno;
    @Field(type = FieldType.Text,analyzer = "ik_max_word",searchAnalyzer = "ik_smart")
    private String publisher;
    @Field
    private String pubtime;
    @Field(type = FieldType.Text,analyzer = "ik_max_word",searchAnalyzer = "ik_smart")
    private String description;
    @Field
    private BigDecimal salesprice;
    @Field
    private int salenums;
}
```



#### FieldType类型

| 类型              | 解释                                                         |
| ----------------- | ------------------------------------------------------------ |
| FieldType.Text    | 当一个字段是要被全文搜索的，比如Email内容、产品描述，应该使用text类型。设置text类型以后，字段内容会被分析，在生成倒排索引以前，字符串会被分析器分成一个一个词项。text类型的字段不用于排序，很少用于聚合（termsAggregation除外）。 |
| FieldType.Keyword | keyword类型适用于索引结构化的字段，比如email地址、主机名、状态码和标签。如果字段需要进行过滤(比如查找已发布博客中status属性为published的文章)、排序、聚合。keyword类型的字段只能通过精确值搜索到。 |
| FieldType.Integer | 整数类型                                                     |

#### dao层

```java
package com.wonixy.springes.dao;

import com.wonixy.springes.entity.Goods;
import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;

import java.util.List;

/**
 * @Author: 马宇航
 * @Todo: TODO
 * @DateTime: 25/01/09/0009 16:45
 * @Component: 成都蜗牛学苑
 **/
public interface GoodsDao extends ElasticsearchRepository<Goods,String> {
    List<Goods> findAllByDescription(String content);
}
```

#### 自定义查询

ElasticsearchRepository接口提供给我们的方法比较少，一般情况下很难满足我们的需求，但同时ElasticsearchRepository也为我们提供了自定义接口的方法，但是自定义的方法必须满足elasticsearch相关规范，如下表所示：

| 关键字              | 使用示例                           | 等同于的ES查询                                               |
| :------------------ | :--------------------------------- | :----------------------------------------------------------- |
| And                 | findByNameAndPrice                 | {“bool” : {“must” : [ {“field” : {“name” : “?”}}, {“field” : {“price” : “?”}} ]}} |
| Or                  | findByNameOrPrice                  | {“bool” : {“should” : [ {“field” : {“name” : “?”}}, {“field” : {“price” : “?”}} ]}} |
| Is                  | findByName                         | {“bool” : {“must” : {“field” : {“name” : “?”}}}}             |
| Not                 | findByNameNot                      | {“bool” : {“must_not” : {“field” : {“name” : “?”}}}}         |
| Between             | findByPriceBetween                 | {“bool” : {“must” : {“range” : {“price” : {“from” : ?,”to” : ?,”include_lower” : true,”include_upper” : true}}}}} |
| LessThanEqual       | findByPriceLessThan                | {“bool” : {“must” : {“range” : {“price” : {“from” : null,”to” : ?,”include_lower” : true,”include_upper” : true}}}}} |
| GreaterThanEqual    | findByPriceGreaterThan             | {“bool” : {“must” : {“range” : {“price” : {“from” : ?,”to” : null,”include_lower” : true,”include_upper” : true}}}}} |
| Before              | findByPriceBefore                  | {“bool” : {“must” : {“range” : {“price” : {“from” : null,”to” : ?,”include_lower” : true,”include_upper” : true}}}}} |
| After               | findByPriceAfter                   | {“bool” : {“must” : {“range” : {“price” : {“from” : ?,”to” : null,”include_lower” : true,”include_upper” : true}}}}} |
| Like                | findByNameLike                     | {“bool” : {“must” : {“field” : {“name” : {“query” : “? *”,”analyze_wildcard” : true}}}}} |
| StartingWith        | findByNameStartingWith             | {“bool” : {“must” : {“field” : {“name” : {“query” : “? *”,”analyze_wildcard” : true}}}}} |
| EndingWith          | findByNameEndingWith               | {“bool” : {“must” : {“field” : {“name” : {“query” : “*?”,”analyze_wildcard” : true}}}}} |
| Contains/Containing | findByNameContaining               | {“bool” : {“must” : {“field” : {“name” : {“query” : “?”,”analyze_wildcard” : true}}}}} |
| In                  | findByNameIn(Collectionnames)      | {“bool” : {“must” : {“bool” : {“should” : [ {“field” : {“name” : “?”}}, {“field” : {“name” : “?”}} ]}}}} |
| NotIn               | findByNameNotIn(Collectionnames)   | {“bool” : {“must_not” : {“bool” : {“should” : {“field” : {“name” : “?”}}}}}} |
| True                | findByAvailableTrue                | {“bool” : {“must” : {“field” : {“available” : true}}}}       |
| False               | findByAvailableFalse               | {“bool” : {“must” : {“field” : {“available” : false}}}}      |
| OrderBy             | findByAvailableTrueOrderByNameDesc | spring:   elasticsearch:     uris: http://192.168.206.129:9200/{“sort” : [{ “name” : {“order” : “desc”} }],”bool” : {“must” : {“field” : {“available” : true}}}} |

#### 配置配置yaml

```yml
spring:
  elasticsearch:
    uris: http://192.168.206.129:9200
```

自己去测试一下，可以快速搭建一个简单的ELK系统，并熟悉各个组件之间的协作关系。



```java
package com.woniuxy;

import com.woniuxy.dao.WoniuMallDao;
import com.woniuxy.entity.Goods;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.math.BigDecimal;
import java.util.List;

@SpringBootTest
class SpringboootEsApplicationTests {
    @Autowired
    private WoniuMallDao woniuMallDao;

    @Test
    void contextLoads() {
        woniuMallDao.findAll().forEach(System.out::println);
    }
    @Test
    void contextLoads1() {
        List<Goods> goodsList = woniuMallDao.findAllByNameLike("我的IT技能还不错");
        goodsList.forEach(System.out::println);
    }
    @Test
    void contextLoads2() {
        Goods save = woniuMallDao.save(new Goods("1003", "我的IT技能还不错", "1234567890",
                "马宇航", "2025-01-01", "我的IT技能还不错", new BigDecimal(100), 100));
        System.out.println(save);
    }
}
```

