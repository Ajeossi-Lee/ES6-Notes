# Reflect

**Reflect**是ES6为了操作对象而提供的新的API

**设计的目的**：

1. 将Object对象的一些明显属于语言内部的方法放到Reflect对象上
2. 修改某些Object方法的返回结果，让其变得合理
3. 让Object操作都变成函数行为
4. Reflect对象的方法与Proxy对象的方法一一对应

**静态方法**：

-   Reflect.get
-   Reflect.set
-   Reflect.has
-   Reflect.deleteProperty
-   Reflect.ownKeys
-   Reflect.getOwnPropertyDescriptor
-   Reflect.defineProperty
-   Reflect.preventExtensions
-   Reflect.getPrototypeOf
-   Reflect.isExtensible
-   Reflect.setPrototypeOf
-   Reflect.apply
-   Reflect.construct

```javascript
obj = {
    name: "fizz",
    age: 27
}

console.log(Reflect.get(obj, 'name'));
```

使用Proxy编写一个**观察者模式**的简单实现（实现observable和observe两个函数）

```javascript
// 定义一个观察者的集合
const queuedObservers = new Set()

// 定义观察者
const observe = fn => {
    queuedObservers.add(fn)
}

// 给对象代理的方法
function set(target, key, val, receiver) {
    result = Reflect.set(target, key, val, receiver)
    queuedObservers.forEach(element => {
        element()
    });
    return result
}

// 定义被观察者 返回一个Proxy对象
const observable = obj => new Proxy(obj, {
    set
})

// 测试
const obj = observable({
    name: "fizz",
    age: 27
})

function print() {
    console.log(`${obj.name}-${obj.age}`);
}

observe(print)

obj.name = "ajeossi"
```

