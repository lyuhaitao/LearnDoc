# Using a JavaScript library (without type declarations) in a TypeScript project.

link: https://medium.com/@steveruiz/using-a-javascript-library-without-type-declarations-in-a-typescript-project-3643490015f3

在实际开发中碰到如下情况, 在vite配置文件`vite.config.ts`

```js
import path from 'path'
```

但是包path没有对应的TypeScript所以总是飘红带错误警告：例如`implicity has an any type`. 根据上面网址的启示，我在根目录创建`model.d.ts`文件，在其内部声明该模块

```ts
declare module 'path'
```

然后再TypeScript配置文件`tsconfig.node.json`中的属性标签`include`添加该文件的路径。

```json
{
    ...
    "include":["module.d.ts","vite.config.ts"]
}
```

特别注意，因为`path`是在vite配置文件中导入使用的，所以在include中`module.d.ts`必须要放在vite 配置文件前。
