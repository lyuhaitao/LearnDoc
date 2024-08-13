# ECMA

European Computer Manufacturers Association

- ECMAScript is a kind of script language

- ECMA 262 standard

- compatibility problem

# Variable declaration

## let

1. 变量不能重复声明

2. 块级作用域-  （全局，函数，eval）
   
   1. ```js
      {
          let name="peter"
      }
      ```

 3.  不存在变量提 升

- 不影响作用域链

## const

- 常量一定要赋初始值

- 潜规则大写

- 不能修改

- 块级作用域

- 对数组和对象的元素修改，不算做堆常量的修改，不会报错

- ```js
  const TE = [1,2,3]
  TE.push(4)//不会报错
  ```

- 解构赋值 - 有写像Python

## 模板字符串

- 内容可以直接用换行符号

- 变量拼接 $$·{}·

## 对象简化写法

允许在打括号里，直接雪茹变量和函数

方法声明更简洁，不需要function关键字

## 箭头函数

```js
let fn = (a,b)=>{return 100;}
```

- this 是静态的，永远指向函数声明时候所在作用域下的this的值 

- call方法调用

- ```js
  let getName2 = ()=>{
      console.log(this.name)
  }
  const school = {
      name:"hello world"
  }
  getName2.call(school)
  ```

- 不能作为构造函数实例化对象

- 不能使用arguments变量

- 两种简写方式
  
  - 省略小括号，当形参有且只有一个
  
  - 省略花括号，当代码提只有一条语句，此时return必须省略，语句执行结果就是函数返回值

- 箭头函数适合域this有关的回调，计时器，数组的方法回调

- 它不适合域this有关的回调。 事件回调，对象的方法

## 函数参数的默认值

## Rest参数代替arguments用户获取函数实参

```js
function date(...args){
    console.log(args)
}
```

- rest参数必须要放在最后

## ES扩展运算符 `...`

- 将数组参数转换为逗号分隔的效果

- 数组克隆

- 将伪数组转成真正的数组

## symbol数据类型，表示独一无二

- Symbol()创建

- Symbol.for 可以创建连个相同的Symbol对象

- 不能与其他数据进行运输

## 七种数据类型

USONB

- undefined

- string 

- symbol

- null

- number

- boolean

## Symbol应用场景

- 扩展某个对象的方法

## 11 个Symbol内置值

## 迭代器

原生具备的数据

- Array

- Arguments

- STring

- Set

- Map

## 生成器-异步编程的解决方案

- next可以传入实参 

## Promise - ES6提供的异步编程的解决方案

- 实列化Promise对象

- 调用对象的then方法 
  
  - value : 成功
  
  - reason:失败

- then方法的返回对象也是Promise,脆响的状态由回调函数的结果决定

- 链式调用可以解决回调地狱 

## Map数据结构
