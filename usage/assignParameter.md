# 指定参数匹配

修饰实体

```java
@Data
@Accessors(chain = true)
public class TestEntity {

    @FieldBlackMatcher({"nihao", "ok"})
    private String name;
    @FieldWhiteMatcher(range = "[12, 32]")
    private Integer age;
    @FieldWhiteMatcher({"beijing", "shanghai"})
    private String address;
}
```

核查代码，只核查指定的参数，参数可指定多个

```groovy
def "测试指定的属性age"() {
    given:
    TestEntity entity = new TestEntity().setName(name).setAge(age)

    expect:
    def act = Checks.check(entity, "age");
    Assert.assertEquals(result, act)
    if (!act) {
        println Checks.errMsg
    }

    where:
    name     | age | result
    "nihao"  | 12  | true
    "ok"     | 32  | true
    "hehe"   | 20  | true
    "haohao" | 40  | false
}
```
