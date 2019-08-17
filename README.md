# Mikilin 介绍

该框架是对象的属性核查框架，通过引入黑白名单和匹配器机制，在可用和不可用两个方面对对象的属性进行拦截，黑名单匹配上是不可用，而白名单是只可用。所有的拦截都是基于基本的类型，而复杂类型（集合、map或者自定义类型）又都是由基本类型组成的，框架支持对复杂类型进行拆解并核查内部的基本类型进而对复杂类型进行拦截。该框架具有以下特性：

### 功能性：
- 全类型：可以核查所有类型，基本类型，复杂类型，集合和Map等各种有固定属性（泛型暂时不支持）的类型
- 匹配器：对类型的匹配机制：分组、值列表、属性class、指定模型类型、正则表达式、系统回调、枚举类型、范围判决（支持时间范围）和表达式语言判决
- 黑白机制：核查机制引入正反的黑白名单机制，白名单表示只要的值，黑名单表示禁用的值

### 非功能性：
- 零侵入：对代码无侵入，仅作为一个工具类存在
- 易使用：使用超级简单，一个类，两种函数，三种注解，多种匹配器
- 高性能：所有的核查均是内存直接调用，第一次构建匹配树后，后面就无须重建
- 可扩展：针对一些不好核查的属性，可以设置自定义匹配，可以通过自定义匹配器类，也可以使用spring的Bean作为系统匹配器类

项目对应的jar包已经发布到maven中央仓库中，可以直接使用，目前最低版本是1.4.0，引入即可

```xml
<dependency>
  <groupId>com.github.simonalong</groupId>
  <artifactId>mikilin</artifactId>
  <version>1.4.0</version>
</dependency>
```

该工具在使用方面，采用一个类，两种函数（核查函数和检测函数），三个注解的方式，使用超级简单，但是功能却很多，所有的功能都提供在注解中，下面先简单介绍下。

源码：https://github.com/SimonAlong/mikilin