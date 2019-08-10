# 复杂类型黑白名单匹配
更复杂结构

```java
@Data
@Accessors(chain = true)
public class WhiteCEntity {

    @Check
    private List<CEntity> cEntities;
    @Check
    private BEntity bEntity;
}
```

```java
@Data
@Accessors(chain = true)
public class
CEntity {

    @FieldWhiteMatcher({"a", "b"})
    private String name;
    @Check
    private List<BEntity> bEntities;
}
```

```java
@Data
@Accessors(chain = true)
public class BEntity {
    @FieldCheck(includes = {"a","b"})
    private String name;
    @FieldCheck
    private AEntity aEntity;
}
```

```java
@Data
@Accessors(chain = true)
public class AEntity {
    @FieldCheck(includes = {"a","b","c","null"})
    private String name;
    @FieldCheck(excludes = {"null"})
    private Integer age;
    private String address;
}
```

```groovy
def "复杂类型白名单集合复杂结构"() {
    given:
    WhiteCEntity entity = new WhiteCEntity();
    entity.setCEntities(Arrays.asList(new CEntity().setName(ccName)
            .setBEntities(Arrays.asList(new BEntity().setName(cb1Name), new BEntity().setName(cb2Name)))))
            .setBEntity(new BEntity().setName(cName).setAEntity(new AEntity().setName(cbaName).setAge(12)))
    Assert.assertEquals(result, Checks.check(entity))
    if (!Checks.check(entity)) {
        println Checks.getErrMsg()
    }
    expect:
    where:
    ccName | cb1Name | cb2Name | cName | cbaName || result
    "a"    | "a"     | "a"     | "a"   | "a"     || true
    "a"    | "a"     | "a"     | "a"   | "b"     || true
    "a"    | "a"     | "a"     | "a"   | "c"     || true
    "a"    | "a"     | "b"     | "a"   | "a"     || true
    "b"    | "a"     | "b"     | "a"   | "a"     || true
    "b"    | "c"     | "b"     | "a"   | "a"     || false
    "b"    | "a"     | "b"     | "a"   | null    || true
}
```
输出

```text
数据校验失败-->属性[name]的值[c]不在白名单[a, b]中-->类型[BEntity]核查失败-->类型[CEntity]的属性[bEntities]核查失败-->类型[CEntity]核查失败-->类型[WhiteCEntity]的属性[cEntities]核查失败-->类型[WhiteCEntity]核查失败
```

<h2 id="指定参数匹配">指定参数匹配</h2>

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

