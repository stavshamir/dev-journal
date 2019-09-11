## Implementing python-like decorators in Kotlin

### Decorators in python
Python [decorators][decorators-link] are a really nice feature. They decleratively extend the functionality of a function without explicitly modifying its body, allowing for higher readablity and code reuse.

A classic example for the use of a decorator is wrapping the output of a function with html tags:
```python
@italic
def hello():
    return "hello"

hello()
```

As you can probably guess, the output of this function without the decorator is "hello", and with it - "<i>hello</i>". We could replace the decorator with a direct call:
```python
italic(hello())
```
However, without the decorator feature, we will need to call the function like this everytime, or explicitly call it inside the `hello` function. In this very simple example it does not seem much, but in more complex examples decorators can really may your code better.

### Decorators and annotations in Kotlin
Java, and Kotlin, don't have this feature. Annotations may somewhat resemble decorators, but while decorators actually run code, annotations simply provide metadata and do nothing on their own - we must seperately write and run a class to process the annotations. Annotations are very powerfull, but require more work to get going than python decorators.

#### Decorating a function with zero parameters
I decided to play around with Kotlin to try and implement a python-like decorators. The requirements are that the result should be declerative, it should not interrupt the flow of the decorated function, and should be declared at function decleration and not at function use. Using higher-order functions seems appropriate and looks like this:
```kotlin
fun italic(function: () -> String) = "<i>${function()}</i>"

fun hello() = italic {
    "hello"
}

assert("<i>hello</i>" == hello())
``` 

The function `italic` receives a function that returns a `String`, and returns a `String`. Then `hello` uses the lambda as last parameter syntax, giving IMO the looks and feel ofa python-like decorator.

#### Decorating a function with parameters
We can easily add parameters to `hello` without changing the signature of `italic`, since the parameters can be used in the lambda's scope:
```kotlin
fun italic(function: () -> String) = "<i>${function()}</i>"

fun hello(name: String) = italic {
    "hello $name"
}
``` 

#### Decorator with parameters 
Decorators in python can be parametriezed - let's try parameterizing a Kotlin "decorator":
```kotlin
fun htmlTag(tag: String, function: () -> String) = "<$tag>${function()}</$tag>"

fun hello(name: String) = htmlTag("i") {
    "hello $name"
}
``` 
It's important to leave the `function` parameter last, to allow the special lambda syntax.

#### Multiple Decorators
Let's try making it even more interesting - decorators in python may be stacked:
```python
@bold
@italic
def hello():
    return "hello"
```

Can we acheive the same in Kotlin? The trivial solution is this:
```kotlin
fun hello() = bold {
    italic {
        "hello"
    }
}
```
The output of `hello` will be "<b><i>hello</i></b>", as expected, but the syntax is not that clean anymore. We can do better, but it's more challenging - what we need is a function that takes two decorators and compose them into a single decorator:
```kotlin
fun compose(first: (() -> String) -> String, second: (() -> String) -> String, function: () -> String) = second { first(function) }

fun hello() = compose(::italic, ::bold) {
    "hello"
}
```
Let's see what's going on here - we define a `compose` function with two parmeters of type `(() -> String) -> String`, which means a function that takes a function returing a `String` and returns a `String` itself - basically a decorator of a `String` producing function. We can use the `typealias` feature to make this less confusing:
```kotlin
typealias StringDecorator = (() -> String) -> String

fun compose(first: StringDecorator, second: StringDecorator, function: () -> String) = second { first(function) }
```
In the body of our `compose` function, we apply the first decorator to the function. We use parens because we refernce a parametr and not a lambda. Since the type resulting from applying a `StringDecorator` to a `String`-returning function is also a `String`-returning function, it qualifies as an argument to the second decorator. Since applying the first decorator to the function parameter can be described as a lambda, we use the curly brace syntax. Now `compose(::italic, ::bold)` returns a composite decorator.

What about composing more than two decorators? We can use the `varargs` keyword:
```kotlin
fun compose(vararg decorators: StringDecorator, function: () -> String): String {
    var result = function()

    for (decorator in decorators) {
        result = decorator { result }
    }
    
    return result
}

fun hello() = compose(::italic, ::bold, ::emphasized) {
    "hello"
}
```
Nice! The only problem with this approach is that the decorators themselves can't have parameters.

Now let's use generics so we can decorate any function with any return type!
I will also change the name `compose` to `decorateWith` to make it more self-explanatory.
```kotlin
typealias Decorator<T> = (() -> T) -> T

fun <T> decorateWith(vararg decorators: Decorator<T>, function: () -> T): T {
    var result = function()

    for (decorator in decorators) {
        result = decorator { result }
    }

    return result
}

fun <T> printResult(function: () -> T): T {
    val result = function()
    println("The result is: $result")
    return result
}

fun add(x: Int, y: Int) = decorateWith(::printResult) {
    x + y
}

fun hello(name: String) = decorateWith(::bold, ::printResult) {
    "hello $name"
}
```
And now we have python-like decorators in Kotlin (and they are type safe!).
It sort of feels like it could also be accomplished with Java, but I'm sure it will be much more verbose.

[decorators-link]: https://www.thecodeship.com/patterns/guide-to-python-function-decorators/

---

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
