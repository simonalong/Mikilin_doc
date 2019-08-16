# 三种注解
在该工具中只有三种注解：

- `@Check`：针对修饰的复杂属性，进行拆解
- `@FieldWhiteMatcher`和`@FieldBlackMatcher`：黑白名单核查
- `@FieldWhiteMatchers`和`@FieldBlackMatchers`：黑白名单分组核查，内部包含对应的黑白名单核查数组

<h3 id="@Check">@Check</h3>
该注解没有属性，修饰属性，用于表示该属性里面是有待核查的属性，如果不添加，则该属性里面的核查注解无法生效

<h3 id="@FieldWhiteMatcher">@FieldWhiteMatcher</h3>
该注解是白名单注解，修饰属性，表示修饰的属性只接收能匹配上该注解的值，用于对修饰的属性进行核查和筛选，该注解有如下的属性：

- group：分组，默认为\"\_default\_\"，该字段主要是在使用注解`@FieldWhiteMatchers`和`@FieldBlackMatchers`时候使用
- value：值列表
- model：既定的模式：身份证，手机号，固定电话，IP地址，邮箱
- enumType：枚举类型，可以设置对应的枚举，属性只有为String才识别
- range：范围类型，支持数值类型的范围：[a,b],[a,b),(a,b],(a,b),[a,null),(a,null],
- condition：java条件表达式，可以支持Java的所有运算符和所有返回值为`Boolean`的表达式，是一个小型表达式语言，两个占位符：`#root`（属性所在对象），`#current`（当前属性）。以及`java.lang.Math`中的所有函数，比如：`min(#root.num1, #root.num2) > #current`
- regex：正则表达式
- judge：调用的核查的类和函数对应的表达式，比如："com.xxx.AEntity#isValid"，其中#后面是方法，方法返回boolean或者包装类，第一个入参为当前Field对应的类型或者子类，第二个入参为属性对应的对象
- type：匹配属性为对应的类型，比如Integer.class，Long.class等等
- disable：是否启动该核查
以上的属性都是用于匹配的规则，只要满足以上任何一项规则，则称之为匹配成功，即通过核查，否则，如果没有命中任何一个规则，且设定了规则，那么认为匹配失败，即该值就是会被拒绝的。

##### 注意：

一旦修饰属性，则该属性的值就不能为null，否则就会命中失败，如果需要允许null，则需要在白名单中添加上允许为null的规则即可。

<h3 id="@FieldBlackMatcher">@FieldBlackMatcher</h3>
该注解是黑名单注解，修饰属性，表示修饰的属性不接受匹配上该注解的值，用于对修饰的属性进行核查和筛选，该注解的属性跟`@FieldWhiteMatcher`是完全一样的，只是逻辑判断不一样：只要满足属性中的任何一项匹配，则称之为匹配成功，即没有通过核查，调用`Checks.getErrMsg()`即可看到错误调用链。

<h3 id="@FieldWhiteMatchers">@FieldWhiteMatchers</h3>
该注解是用于在修饰的bean在不同的场景可能核查规则不同的情况下使用。

```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface FieldWhiteMatchers {

    FieldWhiteMatcher[] value();
}
```
其中对应的黑名单匹配器为对应的黑名单数组，使用如下，比如

```java
@Data
@Accessors(chain = true)
public class GroupEntity {

    @FieldBlackMatchers({
        @FieldBlackMatcher(range = "[50, 100]"),
        @FieldBlackMatcher(group = "test1", range = "[12, 23]"),
        @FieldBlackMatcher(group = "test2", range = "[1, 10]")
    })
    private Integer age;

    @FieldWhiteMatchers({
        @FieldWhiteMatcher(value = {"beijing", "shanghai", "guangzhou"}),
        @FieldWhiteMatcher(group = "test1", value = {"beijing", "shanghai"}),
        @FieldWhiteMatcher(group = "test2", value = {"shanghai", "hangzhou"})
    })
    private String name;
}
```
其中分组时候核查可以通过包含分组的函数`check`来进行核查，比如
```java
Checks.check("test1", entity);
```


