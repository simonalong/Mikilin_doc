# regex
自定义的正则表达式

#### 注意：
这里的正则表达式是Java的正则表达式

```java
/**
 * @author zhouzhenyong
 * @since 2019/3/10 下午10:13
 */
@Data
@Accessors(chain = true)
public class RegexEntity {

    @WhiteMatcher(regex = "^\\d+$")
    private String regexValid;

    @BlackMatcher(regex = "^\\d+$")
    private String regexInValid;
}
```


