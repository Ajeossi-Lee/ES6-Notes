# Iterator

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

