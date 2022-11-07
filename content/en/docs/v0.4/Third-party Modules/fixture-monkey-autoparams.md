---
title: "fixture-monkey-autoparams"
weight: 6
---
{{< alert color="warning" title="Warning">}}
This module does not support 0.4 LabMonkey
{{< /alert >}}

## Featuers
Extending [AutoParams](https://github.com/AutoParams/AutoParams) ParameterizedTest

Using `@FixtureMonkeyAutoSource` instead of `@AutoSource` in AutoParams

## How-to
### 1. Adding denependcy
```groovy
testImplementation("com.navercorp.fixturemonkey:fixture-monkey-autoparams:0.4.2")
```

```xml
<dependency>
  <groupId>com.navercorp.fixturemonkey</groupId>
  <artifactId>fixture-monkey-autoparams</artifactId>
  <version>0.4.2</version>
  <scope>test</scope>
</dependency>
```

## Usage
```java
@Data   // lombok getter, setter
public class Order {
    @NotNull
    private Long id;

    private String productName;

    private int quantity;

    @Nullable
    private String sample;
}

@ParameterizedTest
@FixtureMonkeyAutoSource
void test(Order order, ArbitraryBuilder<Order> orderBuilder) {
    then(order).isNotNull();

    Order actual = orderBuilder
        .generator(JacksonArbitraryGenerator.INSTANCE)
        .set("name", "factory")
        .set("quantity", Arbitraries.integers().between(5, 10))
        .sample();

    // then
    then(actual.getId()).isNotNull();    // @NotNull
    then(actual.getProductName()).isEqualTo("factory");
    then(actual.getQuantity()).isBetween(5, 10);
}
```