# 测试

## Test Spy

**什么是spy**

测试spy是一个函数，他可以记录它调用时的参数，返回值，`this`值和异常值。spy可以是一个匿名函数，也可以是一个现有函数的**wrap**.

**何时用spy**

通常用spy来测试callback，也用来测试某个方法是如何在测试中被调用的。 如下例程演示了一个callback的相关测试

```js
"test should call subscribers on publish": function () {
    var callback = sinon.spy();
    PubSub.subscribe("message", callback);
    PubSub.publishSync("message");
    assertTrue(callback.called);
}
```


## Test Stubs

**什么是stubs**

Test Stubs是有着预编程行为的spy函数，它们支持所有的spy api。

**何时用stubs**

* 在测试中控制一个方法的行为向特定发现发展。比如强制抛出异常
* 假装一个特定的方法被直接调用。

## Mocks

**什么是Mock*

mock是一个具有预编程行为和结果的假函数。
