# 数组类型

有两种写法

```js
let numbers:number[] = [1,2,3]
# 2 泛型
let names: Array<string> = ['a','b','c']
```

# 联合类型

通过联合将不同类型的变量放在一个数组, 不同类型用`|`隔开 

```js
let arr:(number|string)[] = [1,2,3,'abc']
#
let timerId:number | null = null
timerId = setTimeout(()=>{},1000)
```

只能提示共有的方法和属性

# 类型别名 - type

```js
type ArrType = (number | string)[]
#
let arr1: ArrType = [1,2,3,'hello']
```

```js
type ItemType = number | string
let arr1: ItemType[] = [1,2,3,'aa']
```

# 函数类型

## 函数表达式

```js
const fn = function(a:number,b:number):sting{
    return a+b+''
}
```

## 箭头函数

```js
const sub = (a:number, b:number):number =>{
    return a+b
}
```

## 单独指定参数和返回类型

```js
function add(a:number, b:number):number{
    return a+b
}
```

## 指定函数类型

```js
# 函数类型
type FnType = (a:number, b:number)=> number
```

```js
# 函数类型
type FnType = (a:number, b:number)=> number
# 用于限制箭头函数 和函数表达式
const sub:FnType = (a,b)=>{
    return a-b
}
#
const fn:FnType = function(a,b){
    return a+b
}
```

## void类型

函数没有return，将返回undefined,但是ts推断为void

```js
const sayHello = (content:string):void=>{
    console.log(content)
}
```

## 可选择的参数

```js
const slice = (start?:number|undefined, end?:number|undefined):string=>{
    ...
    return 'hello'
}
```

# 对象类型

```js
let obj:{name:string, age:number, 
gender:string, sayHi:(c:string)=>void} = {
    name:'peter',age:15, gender:'male', 
sayHi(content){console.log(content)}
}
```

- 定义一个类的type

```js
type Person = {
name:string,
age:number,
gender:string,
girlfriend?:string,
sayHi:(c:string)=>void
}
#
let obj:Person = {
    name:'peter',
age:15
gender:'male',
sayHi(c){console.log(c)}
}
```

```js
obj.girlFriend && obj.girlFriend.condat('hhh')
```

接口类型基本使用

```js
interface IPerson{
    name:string
age:number
gender:string
girlFriend?:string
sayHi: (c:string)=>void
}
```

接口可以继承

```js
interface Istudent extends IPerson{
    score:number
}
```

type如何和interface 实现一样的继承效果

```js
type Student = {...} & Person
```

# 元组限制固定类型和长度

# 字面量类型

```ts
type Direction = 'up' | 'down'| 'left' | 'right'
```

# 枚举类型

# 类型断言 - as

我们知道是a标签，但是ts不知道,

强行指定获取到的结果类型

```js
const a = document.getElementById('link') as HTMLAnchorElement
```

# 泛型

由外部决定参数或函数返回值的类型

```js
function getID<T>(val:T){
    return val
}
//
console.log(getID<string>('hello')
```

- 指定明确的类型

- 类型收缩+.

## 泛型约束 - 添加约束

给泛型给找一个爸爸，通过关键字`extends`

```ts
interface ILength{
    length:number
}
// add constraint
function getID<T extends ILength>(val:T){
    console.log(val.length)
}
```

## 定义多个泛型 - keyof

定义一个函数，传入一个对象，再传入一个字符串属性名，返回属性值

```ts
// error function declare
function getProp(obj:object, key:string){
    return obj[key]
}
```

上述代码在TS下，会报错。

```ts
function getProp<O,K extends keyof O>(obj:O,key:K){
    return obj[key]
}
```

## 泛型接口

```ts
interface Student{
    id:number
    name:string
    hobby:string[]
}
// create a student object
let s1:Student = {
    id:123,
    name:'Peter'
    hobby:['smoke','hair']
}
```

```ts
interface Student <T>{
    id:number
    name:T
    hobby:string[]
}
// create a student object
let s1:Student<string> = {
    id:123,
    name:'Peter'
    hobby:['smoke','hair']
}
//
let s2:Student<number> = {
    id:123,
    name:3000
    hobby:['smoke','hair']
}
```

# # Vue and Typescript

## defineProps 与Typescript

```ts
// there is data in the fathre component
const house = 'vila'
// 在子组件中接收这个值
type PropsType = {house:string}
defineProps<PropsType>()

// 在子组件中接收这个值,但是可选
type PropsType = {house?:string}
defineProps<PropsType>()
//设置默认值，在解构时候设置
const {house = 'small'}  = defineProps<PropsType>()
```

## defineEmits 约Typescript 子传父亲

```ts
// 父组件定义一个事件接受儿子的数据
@getGift = "getGift"
//
const emit = defineEmits<
    {
    (e:'getGift',gift:string):void
}
>()

const hClick = ()=>{
    emit('getGift','hello')
}
```

## ref与Typescript

```ts
const msg = ref<string>('Hello world')
//
const ss = ref <{
    id:number,name:string
}[]>(
    {id:1,name:'John'},
    {id:2,name:'pet  er'}
)
```

## reactive, compted与Typescript

### 不建议使用reactive

### computed 与 Typescript

1. 通过泛型可以指定computed计算属性的类型，通常可省略

```ts
const leftCount = computed<number>(()=>{
    return list.value.filter(item=>item.done).length
})
```

## 事件处理与Typescript

```ts
const move= (e:MouseEvent)=>{
    mouse.value.x=e.pageX
}
```

- console.log(e)去查看对象的类型

- `hclick($event)`鼠标放在上面，可以显示出类型

## Template Ref与Typescript

这里的`ref`是获取页面DoM元素的

```html
<img ref='automan' src='1.jpg'>

//
```

# 可选链操作符

# 非空断言 - ! 避免TS检查 -慎用

```ts
// 断言automan.value含有src属性
console.log(automan.value!.src)
```

# Typescript类型声明文件

- 平时写ts文件

- 上线打包变成js文件

- 如果提供功能为外部需要将类型放在`.d.ts`文件

## 内置类型声明文件
