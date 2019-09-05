## Spring Boot validation annotations not working
The following annotation does not work because it is applied to the constructor parameter, while it needs to be applied to the field:
```kotlin
data class Foo(
    @NotEmpty
    val foo: String
)
```

Use this instead:
```kotlin
data class Foo(
    @field:NotEmpty
    val foo: String
)
```

* [reference](https://stackoverflow.com/a/36521309/6469095)

---
