# Generator

### Generator基本概念

Generator函数是ES6提供的一种异步编程解决方案

- 从语法上，首先可以把它理解成一个状态机，封装了多个内部状态。执行Generator函数会返回一个遍历器对象。返回的遍历器对象可以依次遍历Generator函数内部的每一个状态。
- 形式上，Generator函数是一个普通函数，两个特征：
  - 一是function命令与函数名之间有一个星号
  - 二是函数体内内容使用yield语句定义不同的内部状态

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
    return 'ending';
}

// 函数有三个状态：hello、world、return语句（结束执行）
const hw = helloWorldGenerator();

hw.next() // { value: 'hello', done: false }
hw.next() // { value: 'world', done: false }
hw.next() // { value: 'ending', done: true }
hw.next() // { value: undefined, done: true }
```

### yield表达式

由于Generator函数返回的遍历器对象只有调用next方法才会遍历下一个内容状态，所以其实提供了一种可以暂停执行的函数。yield语句就是暂停标志，为JavaScript提供了手动的“惰性求值” lazy Evaluation语法功能

next方法的运行逻辑如下：

1. 遇到yield语句就会暂停执行后面的操作，并将紧跟在yield后面的表达式的值作为返回对象的value属性值
2. 下一次调用next方法时再继续往下执行，直到遇到下一条yield语句
3. 如果没有再遇到新的yield语句，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值作为对象的value属性值
4. 如果该函数没有return语句，则返回对象的value属性值为undefined

yield语句和return语句关系

- 相似：都能返回紧跟在语句后的表达式的值
- 区别：
  - 每次遇到yield函数暂停执行，下次会从该位置继续向后执行
  - 一个只能执行一条return语句，返回一个值；但yield可以执行多次，返回多个值

注意：

```JavaScript
function* demo() {
    // yield表达式如果在用另外一个表达式之中，必须放在圆括号里面
    console.log('Hello' + (yield));
    console.log('Hello' + (yield 123));
}
```

## next方法的参数





