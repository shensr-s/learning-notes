# SpringBoot自定义Starter

## Starter工程的命名

1. 官方定义的Starter命名格式：`spring-boot-starter-{name}`
2. 非官方定义的Starter命名格式：`{name}-spring-boot-starter`

## 手写Starter

### （1） 需求

下面我们自定义一个我们自己的 Starter，实现的功能是：为用户提供的字符串添加前辍与后辍，而前辍与后辍定义在 yml 或 properties 配置文件中。例如，用户输入的字符串为 China，application.yml 配置文件中配置的前辍为---，后辍为+++，则最终生成的字符串为
​---China+++。

### （2） 实现

项目结构

![image-20200711180324527](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201903.png)

**A 、 创建工程**
创建一个 Spring Boot 工程，命名为 wrap-spring-boot-starter，并导入 `Configuration Processor` 与 `Lombok` 依赖。

![image-20200711170201584](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201904.png)

![image-20200711170318449](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201905.png)

导入的配置处理器依赖为：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

#### **B 、 定义一个 Service 类**

该 Service 类是当前 Starter 功能的`核心类`。其核心功能就是在这个类中完成的。
该类中的成员变量可以随意命名，但一般与在 Spring Boot 中使用的属性名同名。

```java
package com.example.wrap.service;

import lombok.AllArgsConstructor;

/**
 * 核心业务类：字符串加前缀和后缀
 **/
@AllArgsConstructor
public class WrapService {
    /**
     * 前缀
     */
    private String prefix;
    /**
     * 后缀
     */
    private String suffix;

    public String wrap(String word) {
        return prefix + word + suffix;
    }

}

```

#### **C 、 定义配置属性封装类**

我们指定当前类用于封装来自于 Spring Boot 核心配置文件中的以 wrap.service 开头的prefix与 suffix属性值。即用于封装配置文件中的如下属性值：

- wrap.service.prefix

- wrap.service.suffix

该类的对象是由系统自动创建，所以无需将其将给 Spring 容器管理。

```java
package com.example.wrap.config;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;

/**
 * 配置文件中属性的封装类
 * 要读取并封装如下属性：
 *      wrap.service.prefix
 *      wrap.service.suffix
 **/
@Data
@ConfigurationProperties(prefix = "wrap.service")
public class WrapServiceProperties {
    /**
     * 前缀
     */
    private String prefix;
    /**
     * 后缀
     */
    private String suffix;
}

```

#### **D 、 定义自动配置类**

为了加深大家对“自动配置类与配置文件属性关系”的理解，这里再增加一个功能：为wrap.service 再增加一个组装开关，一个 `boolean` 属性 enable，当 enable 属性值为 true 时，或没有设置 `wrap.service.enable` 属性时才进行组装，若 enable 为 false，则不进行组装。

```java
package com.example.wrap.config;

import com.example.wrap.service.WrapService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 自动配置类
 *
 * @author shensr
 * @date 2020/7/11
 **/

@Configuration
@ConditionalOnClass(WrapService.class)
@EnableConfigurationProperties(WrapServiceProperties.class)
public class WrapServiceAutoConfiguration {
    @Autowired
    private WrapServiceProperties properties;

     /**
     * 1.注意两个方法顺序问题，
     * 2.matchIfMissing：如果没有写这个属性的，默认值为什么；即默认开启该功能
     * 3.havingValue的默认值不是false
     */
    @Bean
    @ConditionalOnProperty(name = "wrap.service.enable",
            havingValue = "true",
            matchIfMissing = true)
    public WrapService wrapService() {
        return new WrapService(properties.getPrefix(), properties.getSuffix());
    }

    @Bean
    @ConditionalOnMissingBean
    public WrapService wrapService2() {
        return new WrapService("", "");
    }
}

```

#### **E 、 创建 spring.factories 文件**

在 `resources/META-INF` 目录下创建一个名为 `spring.factories` 的文件。该配置文件是一个`键值对文件`，**键是固定的**，为 EnableAutoConfiguration 类的全限定性类名，而值则为我们自定义的自动配置类。

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=com.example.wrap.config.WrapServiceAutoConfiguration
```

![image-20200711175247085](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201906.png)

### （3）使用自定义的 Starter

在本地要使用我们自定义的 Starter，首先要保证其已经被 Install 到了本地 Maven 库。

#### **A. 创建工程**

创建一个 Spring Boot 工程，仅需要一个 web 依赖。

![image-20200711175412527](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201907.png)

#### **B.导入自定义 Starter 依赖**

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>wrap-spring-boot-starter</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
```

#### **C.定义 properties.yml**

自定义 Starter 中的属性在配置文件中也是有自动提示功能的(需要导入`Configuration Processor`依赖)。

```yaml
wrap:
  service:
    prefix: ----  #前缀
    suffix: ++++  #后缀
    enable: true  #开关状态
```

#### **D. 义 定义 Controller**

```java
@Autowired
private WrapService wrapService;

@ResponseBody
@GetMapping("wrap/{word}")
public String wrap(@PathVariable String word){
    return wrapService.wrap(word);
}
```

#### **E.运行访问**

启动类无需修改，直接启动运行即可

![image-20200711180054429](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201908.png)

如果wrap.service.enable不为true，则结果如下

![image-20200711180232557](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201909.png)