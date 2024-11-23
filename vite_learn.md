# 依赖预构建

vite首先找到对应的依赖，然后调用esbuild对js语法进行统一的规范，将common.js规范格式导数的包变成esmodule规范，转换后的文件放入当前目录`node_module`下的`.vite/deps`目录，同时对esmodule规范的各个模块进行统一集成。

解决了三个问题：

1. 不同的第三方包的不同的导出格式

2. 对路径的处理，直接使用.vite/depth,方便路径重写

3. 解决网络多包传输的性能问题：客户端向服务器发送request请求的方式加载.js文件

# vite 环境变量

dotenv读取.env文件，解析环境变量，并注入到process对象中，但是vite考虑与其他变量冲突的可能，vite不会直接把变量注入到process对象

这里牵涉到一些关键的vite配置

- root

- envDir: 当前环境变量的地址

为了避免冲突和覆盖vite 提供loadEnv函数

.env: 所有环境都需要的环境变量

.env.development:默认开发环境需要的变量， vite默认命名development

.env.production:生产环境需要的环境变量

```bash
pnpm dev --mode development
```

调用loadEnv,做如下事：

- 解析.env里的环境变量

- 根据传近来的mode拼接对应的配置文件，并解析并放进一个对象，

- 新解析的值对覆盖从.env里解析的变量

在客户端vite将环境变量注入到`import.meta.env`

默认vite环境变量的前缀是`VITE`， 如果想修，同过属性`envPrefix`配置

# 路径字符串处理

`path.resolve` 用来拼接字符串

`__dirname` 返回文件所在位置的路径

# vite加载静态资源

除动态API以为，都是静态资源

# SVG: scalable vector graphics

尺寸小，不失真

没法很好标识层次丰富的图片信息

# vite插件

在不同的生命周期中的不同阶段，去调用不同的插件已达到不同的目的

```typescript
// req, 请求对象：用户发过来的请求，请求头，请求体
// res: 响应对象：res.header
// next: 是否交给下一个中间件，调用next方法，将处理结果交给下一个中间件
server.middlewares.use(
    (req,res,next)=>{
    
    }
)
```





# mockjs:模拟海量数据

# Vite and TS

```typescript
"build": "tsc --noEmit && vite build"
```

## vite-env.d.ts


