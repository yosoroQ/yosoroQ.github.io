# SpringCloud微服务

![image-20221110162725672](http://qny.expressisland.cn/schoolOpens/image-20221110162725672.png)

# 微服务技术栈

![image-20221110163654420](http://qny.expressisland.cn/schoolOpens/image-20221110163654420.png)

![image-20221110163711939](http://qny.expressisland.cn/schoolOpens/image-20221110163711939.png)

![image-20221110163751747](http://qny.expressisland.cn/schoolOpens/image-20221110163751747.png)

![image-20221110163925218](http://qny.expressisland.cn/schoolOpens/image-20221110163925218.png)

# 分层次教学

![image-20221110164220704](http://qny.expressisland.cn/schoolOpens/image-20221110164220704.png)

# 微服务框架 —— SpringCloud微服务架构

## 认识微服务

### 服务架构演变

#### 单体架构

* 单体架构：将业务的所有功能集中在一个项目中开发，打成一个包部署。
* 优点：架构简单、部署成本低。
* 缺点：耦合度高。

#### 分布式架构

* 分布式架构：根据业务功能对系统进行拆分，每个业务模块作为独立项目开发，称为一个服务。
* 优点：降低服务耦合、有利于服务升级拓展。

#### 服务治理

* 分布式架构的要考虑的问题：
  * 服务拆分粒度如何？
  * 服务集群地址如何维护？
  * 服务之间如何实现远程调用？
  * 服务健康状态如何感知？
* ![image-20221110165157549](http://qny.expressisland.cn/schoolOpens/image-20221110165157549.png)

#### 微服务

* 微服务是一种经过良好架构设计的分布式架构方案。
* 微服务架构特征：
  * 单一职责：微服务拆分粒度更小，每一个服务都对应唯一的业务能力，做到单一职责，避免重复业务开发。
  * 面向服务：微服务对外暴露业务接口。
  * 自治：团队独立、技术独立、数据独立、部署独立。
  * 隔离性强：服务调用做好隔离、容错、降级，避免出现级联问题。
* ![image-20221110165526281](http://qny.expressisland.cn/schoolOpens/image-20221110165526281.png)

#### 总结

![image-20221110165600770](http://qny.expressisland.cn/schoolOpens/image-20221110165600770.png)

### 微服务技术对比

![image-20221110170210022](http://qny.expressisland.cn/schoolOpens/image-20221110170210022.png)

#### 企业需求

![image-20221110170343306](http://qny.expressisland.cn/schoolOpens/image-20221110170343306.png)

### SpringCloud

* SpringCloud是目前国内使用最广泛的微服务框架。
* 官网地址: https://spring.io/projects/spring-cloud
* SpringCloud集成了各种微服务功能组件，并基于SpringBoot实现了这些组件的自动装配，从而提供了良好的开箱即用体验。
* ![image-20221110170541392](http://qny.expressisland.cn/schoolOpens/image-20221110170541392.png)

#### SpringCloud与SpringBoot的版本兼容关系

* ![image-20221110170725920](http://qny.expressisland.cn/schoolOpens/image-20221110170725920.png)
* 例如我们学习的版本是Hoxton.SR10，因此对应的SpringBoot版本是2.3.x版本。

### 

## 分布式服务架构案例

### 服务拆分及远程调用

#### 服务拆分注意事项

1．不同微服务，不要重复开发相同业务。
2．微服务数据独立，不要访问其它微服务的数据库。
3．微服务可以将自己的业务暴露为接口，供其它微服务调用。

#### 导入"服务拆分Demo（cloud-demo）"

##### 项目结构

* ![image-20221110171311239](http://qny.expressisland.cn/schoolOpens/image-20221110171311239.png)

#### sql（同样新建两个数据库）

* ![image-20221110171340485](http://qny.expressisland.cn/schoolOpens/image-20221110171340485.png)
* ![image-20221113215003474](http://qny.expressisland.cn/schoolOpens/image-20221113215003474.png)

##### cloud-user.sql

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : local
 Source Server Type    : MySQL
 Source Server Version : 50622
 Source Host           : localhost:3306
 Source Schema         : heima

 Target Server Type    : MySQL
 Target Server Version : 50622
 File Encoding         : 65001

 Date: 01/04/2021 14:57:18
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_user
-- ----------------------------
DROP TABLE IF EXISTS `tb_user`;
CREATE TABLE `tb_user`  (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `username` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '收件人',
  `address` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '地址',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `username`(`username`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 109 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;

-- ----------------------------
-- Records of tb_user
-- ----------------------------
INSERT INTO `tb_user` VALUES (1, '柳岩', '湖南省衡阳市');
INSERT INTO `tb_user` VALUES (2, '文二狗', '陕西省西安市');
INSERT INTO `tb_user` VALUES (3, '华沉鱼', '湖北省十堰市');
INSERT INTO `tb_user` VALUES (4, '张必沉', '天津市');
INSERT INTO `tb_user` VALUES (5, '郑爽爽', '辽宁省沈阳市大东区');
INSERT INTO `tb_user` VALUES (6, '范兵兵', '山东省青岛市');

SET FOREIGN_KEY_CHECKS = 1;
```

##### cloud-order.sql

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : local
 Source Server Type    : MySQL
 Source Server Version : 50622
 Source Host           : localhost:3306
 Source Schema         : heima

 Target Server Type    : MySQL
 Target Server Version : 50622
 File Encoding         : 65001

 Date: 01/04/2021 14:57:18
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_order
-- ----------------------------
DROP TABLE IF EXISTS `tb_order`;
CREATE TABLE `tb_order`  (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '订单id',
  `user_id` bigint(20) NOT NULL COMMENT '用户id',
  `name` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '商品名称',
  `price` bigint(20) NOT NULL COMMENT '商品价格',
  `num` int(10) NULL DEFAULT 0 COMMENT '商品数量',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `username`(`name`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 109 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;

-- ----------------------------
-- Records of tb_order
-- ----------------------------
INSERT INTO `tb_order` VALUES (101, 1, 'Apple 苹果 iPhone 12 ', 699900, 1);
INSERT INTO `tb_order` VALUES (102, 2, '雅迪 yadea 新国标电动车', 209900, 1);
INSERT INTO `tb_order` VALUES (103, 3, '骆驼（CAMEL）休闲运动鞋女', 43900, 1);
INSERT INTO `tb_order` VALUES (104, 4, '小米10 双模5G 骁龙865', 359900, 1);
INSERT INTO `tb_order` VALUES (105, 5, 'OPPO Reno3 Pro 双模5G 视频双防抖', 299900, 1);
INSERT INTO `tb_order` VALUES (106, 6, '美的（Midea) 新能效 冷静星II ', 544900, 1);
INSERT INTO `tb_order` VALUES (107, 2, '西昊/SIHOO 人体工学电脑椅子', 79900, 1);
INSERT INTO `tb_order` VALUES (108, 3, '梵班（FAMDBANN）休闲男鞋', 31900, 1);

SET FOREIGN_KEY_CHECKS = 1;
```

#### 启动服务 - 测试

##### 访问http://localhost:8080/order/101

* ![image-20221113220209470](http://qny.expressisland.cn/schoolOpens/image-20221113220209470.png)

##### 访问http://localhost:8081/user/1

* ![image-20221113220219632](http://qny.expressisland.cn/schoolOpens/image-20221113220219632.png)

#### 服务间远程调用

##### 案例 —— 根据订单id查询订单功能

* ![image-20221113220507237](http://qny.expressisland.cn/schoolOpens/image-20221113220507237.png)

##### 远程调用方式分析

* ![image-20221113220630765](http://qny.expressisland.cn/schoolOpens/image-20221113220630765.png)

##### 步骤 —— 1）注册RestTemplate

* 在order-service的OrderApplication中注册RestTemplate。

###### OrderApplication

```java
@MapperScan("cn.itcast.order.mapper")
@SpringBootApplication
public class OrderApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderApplication.class, args);
    }

//    创建RestTemplate并注入Spring容器
    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }

}
```

##### 步骤 —— 2）服务远程调用RestTemplate

* 修改order-service中的OrderService的queryOrderByld方法。

```java
@Service
public class OrderService {

    @Autowired
    private OrderMapper orderMapper;
    @Autowired
    private RestTemplate restTemplate;

    public Order queryOrderById(Long orderId) {
        // 1.查询订单
        Order order = orderMapper.findById(orderId);

//        2.利用RestTemplate发起http请求，查询用户
        //2.1 url路径
        String url = "http://localhost:8081/user/" + order.getUserId();
        // 2.2 发起http请求，实现远程调用
        User user = restTemplate.getForObject(url, User.class);
        // 3. 封装user到order
        order.setUser(user);

        // 4.返回
        return order;
    }
}
```

##### 测试

* 访问http://localhost:8080/order/101
* 可以正常显示user信息。
* ![image-20221113221856675](http://qny.expressisland.cn/schoolOpens/image-20221113221856675.png)

##### 总结

* 微服务调用方式
  * 基于RestTemplate发起的http请求实现远程调用。
  * http请求做远程调用是与语言无关的调用，只要知道对方的ip、端口、接口路径、请求参数即可。

##### 提供者与消费者

* 服务提供者：一次业务中，被其它微服务调用的服务。（提供接口给其它微服务）
* 服务消费者：一次业务中，调用其它微服务的服务。（调用其它微服务提供的接口）

* ![image-20221113222308110](http://qny.expressisland.cn/schoolOpens/image-20221113222308110.png)

##### 思考

* 服务A调用服务B，服务B调用服务C，那么服务B是什么角色？

* 提供者与消费者角色其实是相对的，**一个服务可以同时是服务提供者和服务消费者**。

## Eureka注册中心

### 远程调用的问题

* 在此之前采用了硬编码的方式，将地址写死了。
* 那服务消费者该如何获取服务提供者的地址信息？
* 如果有多个服务提供者，消费者该如何选择？
* 消费者如何得知服务提供者的健康状态？

### Eureka的作用（原理）

* ![image-20221113223448564](http://qny.expressisland.cn/schoolOpens/image-20221113223448564.png)

##### 消费者该如何获取服务提供者具体信息？

* 服务提供者启动时向eureka注册自己的信息。
* eureka保存这些信息。
* 消费者根据服务名称向eureka拉取提供者信息。

##### 如果有多个服务提供者，消费者该如何选择？

* 服务消费者利用负载均衡算法，从服务列表中挑选一个。

##### 消费者如何感知服务提供者健康状态？

* 服务提供者会每隔30秒向EurekaServer发送心跳请求，报告健康状态。
* eureka会更新记录服务列表信息，心跳不正常会被剔除。
* 消费者就可以拉取到最新的信息。

##### 总结

* ![image-20221113224942604](http://qny.expressisland.cn/schoolOpens/image-20221113224942604.png)

### 动手实践

* ![image-20221113225130353](http://qny.expressisland.cn/schoolOpens/image-20221113225130353.png)

### 搭建EurekaServer

#### （1）创建项目，引入spring-cloud-starter-netflix-eureka-server的依赖

```xml
<!--eureka服务端-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

#### （2）编写启动类，添加@EnableEurekaServer注解

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@EnableEurekaServer
@SpringBootApplication
public class EurekaApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaApplication.class, args);
    }
}
```

#### （3）添加application.yml文件，编写下面的配置

```yaml
server:
  port: 10086 # 服务端口
spring:
  application:
    name: eurekaserver # eureka的服务名称
eureka:
  client:
    service-url:  # eureka的地址信息
      defaultZone: http://127.0.0.1:10086/eureka
```

#### 访问Eureka管理端

* http://localhost:10086/
* ![image-20221113232211600](http://qny.expressisland.cn/schoolOpens/image-20221113232211600.png)

### 服务注册

https://www.bilibili.com/video/av717242269/?p=12

### 服务发现

## Ribbon负载均衡原理

## Nacos注册中心
