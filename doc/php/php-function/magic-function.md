# 盘点下php的魔术方法有哪些

- 参考

  https://www.php.net/manual/zh/language.oop5.magic.php

  

## [__construct()](https://www.php.net/manual/zh/language.oop5.decon.php#object.construct)

> 构造函数:具有构造函数的类会在每次创建新对象时先调用此方法，所以非常适合在使用对象之前做一些初始化工作。

## [__destruct()](https://www.php.net/manual/zh/language.oop5.decon.php#object.construct)

>  析构函数: 会在到某个对象的所有引用都被删除或者当对象被显式销毁时执行

## [__clone()](https://www.php.net/manual/zh/language.oop5.cloning.php#object.clone)

> ​	深拷贝一个对象实例的副本

## [__call()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.call)  

>在对象中调用一个不可访问方法时，[__call()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.call) 会被调用。

## [__callStatic()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.call)

>在静态上下文中调用一个不可访问方法时，[__callStatic()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.callstatic) 会被调用。

## [__get()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.get)

>读取不可访问（protected 或 private）或不存在的属性的值时，[__get()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.get) 会被调用。

## __set()

>在给不可访问（protected 或 private）或不存在的属性赋值时，[__set()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.set) 会被调用。

## __isset()

> 当对不可访问（protected 或 private）或不存在的属性调用 [isset()](https://www.php.net/manual/zh/function.isset.php) 或 [empty()](https://www.php.net/manual/zh/function.empty.php) 时，[__isset()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.isset) 会被调用。

## __unset()

> 当对不可访问（protected 或 private）或不存在的属性调用 [unset()](https://www.php.net/manual/zh/function.unset.php) 时，[__unset()](https://www.php.net/manual/zh/language.oop5.overloading.php#object.unset) 会被调用。

## [__sleep()](https://www.php.net/manual/zh/language.oop5.magic.php#object.sleep)

> [__sleep()](https://www.php.net/manual/zh/language.oop5.magic.php#object.sleep) 方法常用于提交未提交的数据，或类似的清理操作。同时，如果有一些很大的对象，但不需要全部保存，这个功能就很好用。

## __wakeup()

> [__wakeup()](https://www.php.net/manual/zh/language.oop5.magic.php#object.wakeup) 经常用在反序列化操作中，例如重新建立数据库连接，或执行其它初始化操作。

## __serialize()

> [serialize()](https://www.php.net/manual/zh/function.serialize.php) 函数会检查类中是否存在一个魔术方法 [__serialize()](https://www.php.net/manual/zh/language.oop5.magic.php#object.serialize) 。如果存在，该方法将在任何序列化之前优先执行。它必须以一个代表对象序列化形式的 键/值 成对的关联数组形式来返回，如果没有返回数组，将会抛出一个 [TypeError](https://www.php.net/manual/zh/class.typeerror.php) 错误。

## __unserialize()

>  [unserialize()](https://www.php.net/manual/zh/function.unserialize.php) 检查是否存在具有名为 [__unserialize()](https://www.php.net/manual/zh/language.oop5.magic.php#object.unserialize) 的魔术方法。此函数将会传递从 [__serialize()](https://www.php.net/manual/zh/language.oop5.magic.php#object.serialize) 返回的恢复数组。然后它可以根据需要从该数组中恢复对象的属性。

## __toString()

> [__toString()](https://www.php.net/manual/zh/language.oop5.magic.php#object.tostring) 方法用于一个类被当成字符串时应怎样回应。例如 `echo $obj;` 应该显示些什么。

```php
<?php
// 声明一个简单的类
class TestClass
{
    public $foo;

    public function __construct($foo) 
    {
        $this->foo = $foo;
    }

    public function __toString() {
        return $this->foo;
    }
}

$class = new TestClass('Hello');
echo $class;
?>
```

以上例程会输出：

```
Hello
```

## __invoke()

> 当尝试以调用函数的方式调用一个对象时，[__invoke()](https://www.php.net/manual/zh/language.oop5.magic.php#object.invoke) 方法会被自动调用。

```php
<?php
class CallableClass 
{
    function __invoke($x) {
        var_dump($x);
    }
}
$obj = new CallableClass;
$obj(5);
var_dump(is_callable($obj));
?>
```

以上例程会输出：

```
int(5)
bool(true)
```

## __set_state()

## __debugInfo()