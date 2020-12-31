# Iterator

## Iterator（遍历器）的概念

**遍历器**(Iterator):一种接口，为各种不同的数据结构提供统一的访问机制

**作用**：

1.   为各种数据结构提供一个统一的、简便的访问接口
2.   使得数据结构的成员能够按某种次序排列
3.   ES6创造了一种新的遍历命令 for...of，Iterator接口主要提供for...of消费

Iterator的**遍历过程**如下：

1.   创建一个指针对象，指向当前数据结构的起始位置
2.   第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员
3.   第二次调用指针对象的next方法，指针就指向数据结构的第二个成员
4.   不断调用指针对象的next方法，直到它指向数据结构的结束位置

每次调用next方法，返回一个包含value和done两个属性的对象。

```javascript
// 遍历器生成函数，作用就是返回一个遍历器对象
function makeIterator(arr) {
    let nextIndex = 0
    return {
        next() {
            return nextIndex < arr.length ? {
                value: arr[nextIndex++],
                done: true
            } : {
                value: undefined,
                done: false
            }
        }
    }
}

let it = makeIterator(['a', 'b'])

console.log(it.next()) // {value: 'a', done: false}
console.log(it.next()) // {value: 'a', done: false}
console.log(it.next()) // {value: undefine, done: true}
```

## 默认Iterator接口

Iterator接口的目的是为所有数据结构提供一种统一的访问机制，即**for...of循环**。ES6规定，默认的Iterator接口部署在数据结构的**Symbol.iterator**属性（预定义好的、类型为Symbol的特殊值）

原生具备Iterator接口的数据结构如下：

Array、Map、Set、String、TypedArray、函数的arguments对象、NodeList对象

```javascript
let arr = ['a', 'b', 'c']
let iter = arr[Symbol.iterator]()

console.log(it.next()) // {value: 'a', done: false}
console.log(it.next()) // {value: 'b', done: false}
console.log(it.next()) // {value: 'c', done: false}
console.log(it.next()) // {value: undefine, done: true}
```

类似数组的对象调用数组的Symbol.iterator方法

```javascript
let obj = {
    0: 'a',
    1: 'b',
    2: 'c',
    length: 3,
    [Symbol.iterator]: Array.prototype[Symbol.iterator]
}

for (const iterator of obj) {
    console.log(iterator);
}

// 下面知识补充
const arr = [...obj]
console.log(arr);
```

## 调用Iterator接口的场合

- 解构赋值
  - 对数组和Set结构进行结构赋值时，会默认调用Symbol.iterator方法
- 扩展运算符
  - 只要某个数据结构部署了Iterator接口，就可以对它使用扩展运算符，将其转为数组
- yield*
  - yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口
- 其他场合
  - for...of
  - Array.from()
  - Map()、Set()、WeakMap()、WeakSet()
  - Promise.all()
  - Promise.race()

## Iterator接口与Generator函数

使用Generator函数实现Symbol.iterator方法

```javascript
let myIterable = {}

myIterable[Symbol.iterator] = function* () {
    yield 1
    yield 2
    yield 3
}

console.log([...myIterable]);

// 简介写法
let obj = {
    *[Symbol.iterator]() {
        yield 'hello'
        yield 'world'
    }
}

for (const iterator of obj) {
    console.log(iterator);
}
```

## 遍历器对象的return()、throw()

return方法的使用场合是，如果for...of循环提前退出（出错或者**break和continue语句**），就会调用return方法；如果一个对象在完成遍历前需要清理或释放资源，就可以部署return方法

## 数组遍历总结

- for循环

  - ```javascript
    for (let index = 0; index < array.length; index++) {
        const element = array[index];
    }
    ```

- for...in循环

  - ```javascript
    // for...in循环读取键名， 可遍历普通对象
    for (const key in object) {
        if (Object.hasOwnProperty.call(object, key)) {
            const element = object[key];
        }
    }
    ```

- for...of循环

  - ```JavaScript
    // for...of循环读取键值
    for (const iterator of object) {
        console.log(iterator);
    }
    ```

- forEach循环

  - ```javascript
    // forEach循环读取键值, 不能使用break和continue
    array.forEach(element => {
        console.log(element);
    });
    ```

注意：

​	ES6的数组、Set、Map都部署了以下三个方法：

- entries()返回一个遍历器对象
- keys()返回一个遍历器对象
- values()返回一个遍历器对象