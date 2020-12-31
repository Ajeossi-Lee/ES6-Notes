# Proxy

------

Proxy 可以理解成在目标对象前架设一个"拦截层"，提供了一种机制可以对外界的访问进行改写和过滤

```javascript
let obj = new Proxy({}, {
    get: function (target, key, receiver) { // receiver是一个可选对象
        console.log(`getting ${key}`);
        return Reflect.get(target, key, receiver)
    },
    set: function (target, key, receiver) {
        console.log(`setting ${key}`);
        return Reflect.set(target, key, receiver)
    }
})

obj.name = 'fizz'
console.log(obj.name);

/* 
>>> setting name
>>> getting name
>>> fizz
*/
```

构造函数Proxy接受两个参数：

- 第一个参数：所有代理的目标对象
- 第二个参数：一个配置对象

对于每一个被代理的操作，需要提供一个对应的处理函数，该函数将拦截对应的操作

Proxy实例的方法:

-   get
-   set
-   has
-   deleteProperty
-   ownKeys
-   getOwnPropertyDescriptor
-   defineProperty
-   preventExtensions
-   getPrototypeOf
-   isExtensible
-   setPrototypeOf
-   apply
-   construct

注意：虽然Proxy可以代理针对目标对象的访问，但它不是目标对象的透明代理，即不做任何拦截的情况下也无法保证与目标对象的行为一致。主要原因就是在Proxy代理的情况下，目标内部的this指向Proxy代理