# 分组匹配

修饰的实体代码

```java
@Data
@Accessors(chain = true)
public class GroupMultiEntity {

    @BlackMatchers({
        @BlackMatcher(range = "[10, 20)"),
        @BlackMatcher(group = "test0", range = "[70, 80)"),
        @BlackMatcher(group = {"test1","test2"}, range = "[20, 30)"),
        @BlackMatcher(group = {MkConstant.DEFAULT_GROUP,"test3"}, range = "[30, 40)"),
    })
    private Integer age;

    @WhiteMatchers({
        @WhiteMatcher(value = {"hangzhou", "guangzhou"}),
        @WhiteMatcher(group = {"test2", "test3"}, value = {"beijing", "shanghai"}),
    })
    private String name;
}
```

核查代码
```groovy
def "测试默认分组"() {
    given:
    GroupEntity entity = new GroupEntity().setAge(age).setName(name)

    expect:
    def act = Checks.check(entity);
    Assert.assertEquals(result, act)
    if (!act) {
        println Checks.errMsg
    }

    where:
    age | name       | result
    12  | "shanghai" | true
    12  | "beijing"  | true
    49  | "beijing"  | true
    50  | "beijing"  | false
    100 | "beijing"  | false
    49  | "tianjin"  | false
}

def "测试指定分组"() {
    given:
    GroupEntity entity = new GroupEntity().setAge(age).setName(name)

    expect:
    def act = Checks.check("test1", entity);
    Assert.assertEquals(result, act)
    if (!act) {
        println Checks.errMsg
    }

    where:
    age | name        | result
    10  | "shanghai"  | true
    12  | "beijing"   | false
    23  | "beijing"   | false
    50  | "beijing"   | true
    100 | "guangzhou" | false
}    

def "分组多个组合_测试default"() {
    given:
    GroupMultiEntity entity = new GroupMultiEntity().setAge(age).setName(name)

    expect:
    def act = Checks.check(entity);
    Assert.assertEquals(result, act)
    if (!act) {
        println Checks.errMsg
    }

    where:
    age | name       | result
    20  | "shanghai" | false
    25  | "beijing"  | false
    30  | "beijing"  | false
    30  | "shanghai" | true
}
    
```

更全面的测试详见代码中的测试类
#### 注意
如果核查按照指定的分组，则如果没有配置该分组的，则不会核查到
