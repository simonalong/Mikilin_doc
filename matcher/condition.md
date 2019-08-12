# condition
是条件表达式语句，该条件表达式中支持Java的所有运算符和java.lang.math的所有函数，且也支持类自定义的两个占位符。现在列举如下：

<h3 id="运算符">运算符</h3>

| 类型 | 运算符 |
| ------ | ------ |
| 算术运算符 | +、-、*、/、%、++、-- |
| 关系运算符 | ==、!=、>、<、>=、<= |
| 位运算符 | &、&#124;、^、~、<<、>>、>>> |
| 逻辑运算符 | &&、&#124;&#124;、! |
| 赋值运算符 | =、+=、-=、*=、/=、(%)=、<<=、>>=、&=、^=、&#124;= |
| 其他运算符 | 条件运算符（?:）、instanceof运算符 |

<h3 id="math的函数">math的函数</h3>
除了支持运算符构成的条件表达式之外，这里也支持`java.lang.math`中的所有函数，比如：`min`、`max`和`abs`等等函数

<h3 id="自定义占位符">自定义占位符</h3>
自定义占位符有两个：


| 占位符 | 详解 |
| ------ | ------ |
| #root | 表示属性所在类的对象值 |
| #current | 表示当前属性的值 |

添加自定义占位符主要是用于在对象的属性间有相互约束的时候用，比如，属性a和属性b之和大于属性c，这种就可以使用自定义占位符使用。

<h3 id="用例">用例</h3>

```java
/**
 * @author zhouzhenyong
 * @since 2019/4/14 下午12:09
 */
@Data
@Accessors(chain = true)
public class ConditionEntity1 {

    @FieldWhiteMatcher(condition = "#current + #root.num2 > 100")
    private Integer num1;

    @FieldWhiteMatcher(condition = "#current < 20")
    private Integer num2;

    @FieldWhiteMatcher(condition = "(++#current) >31")
    private Integer num3;
}
```

```java
@Data
@Accessors(chain = true)
public class ConditionEntity2 {

    @FieldWhiteMatcher(condition = "#root.judge")
    private Integer age;

    private Boolean judge;
}
```

```java
@Data
@Accessors(chain = true)
public class ConditionEntity3 {

    @FieldWhiteMatcher(condition = "min(#current, #root.num2) > #root.num3")
    private Integer num1;
    private Integer num2;
    private Integer num3;
}
```


