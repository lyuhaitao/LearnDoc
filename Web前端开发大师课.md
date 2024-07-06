# VS Code前端开发插件配置

-    live server

- live server preview

- HTML Boilerplate

- prettier code fomatter

- css peak

- javascript ES6 code snippet

- Bracket Pair colorize DLW

- vetur  

- Es7 React

- Angular language service

- Angular2-switch

# 事件循环 Event Cycle

## 浏览器的进程模型

### What is Process? 进程

### What is Thread? 线程

### 浏览器有哪些进程和线程？

# 渲染主线程是如何工作的？

# 重要的概念和解释

## 何为异步

单线程是异步产生的原因

事件循环是异步实现方式

### JavaScript为何会阻碍渲染？

- 如何理解JS的异步

JS是一门单线程的编程语言，这是因为它运行在浏览器的渲染主线程中，而渲染主线程只有一个。而且渲染主线程承担着诸多的工作，渲染页面。执行JS都在其中运行。如果使用同步的方式，极有可能导致主线程产生阻塞，从而导致消息队列中很多其它的任务无法得到执行。这会导致繁忙的主线程白白消耗时间，另一方面导致页面无法及时更新，给用户造成卡死现象。所以浏览器采用异步的方式避免上述的问题。具体做法是某些任务发生时，比如计时器，网络连接、时间监听，主线程将任务交给其它线程处理，自身立即结束任务的执行，转而执行后续代码。当其它线程完成时，将事先传递的回调函数包装成任务对象，加入到消息队列的末尾进行排队，等待主线程的再次调用执行。在异步模式下，浏览器永不阻塞，从而保证单线程的流畅运行。

`JavaScript is a single-threaded programming language because it runs in the browser's main rendering thread, of which there is only one. Moreover, the main rendering thread is responsible for many tasks, including rendering the page and executing JavaScript, all running within it. If a synchronous approach is used, it is very likely to cause the main thread to block, preventing many other tasks in the message queue from being executed. This would lead to the main thread wasting time while busy, and on the other hand, cause the page not to update in time, creating a freezing phenomenon for the user. Therefore, browsers adopt an asynchronous method to avoid these problems. Specifically, when certain tasks occur, such as timers, network connections, or event listening, the main thread hands over the task to other threads to handle, and it immediately ends the task execution to move on to subsequent code. When other threads complete the task, they package the previously passed callback function into a task object and add it to the end of the message queue to queue up, waiting for the main thread to call it again for execution. In asynchronous mode, the browser never blocks, thus ensuring the smooth running of the single thread.`

- 微队列：用户存放需要最快执行的任务
  
  1. Promise
  
  2. MutationObserver

```js
Promise.resolve().then(Function)
```

# 浏览器渲染原理

渲染 render

- html string -> 像素信息

# 渲染的时间点

- 网络： fetch Html

- 渲染：parse Html

# 渲染流程

- HTML 解析 - Parse HTML
  
  1. DOM Tree (Document Object Model)
  
  2. CSSOM Tree (CSS Object Model)
     
     - 内部样式表 <style>
     
     - 外部样式表 <link>
     
     - 行内样式表 <div style=''>
     
     - 浏览器默认样式表
  
  3- 预解析器进程 - 提高整体解析效率，在不同的线程上面，负责下载相关的CSS文件，所以CSS不会阻塞渲染主线程
  
  4- 渲染主线程遇到JS时候，必须暂停一切行为，等待下载执行完才能继续，因为JS可能修改当前的DOM tree.

- 样式计算 - Recalculate Style
  
  - CSS 属性值的计算过程
    
    - 层叠
    
    - 继承
  
  - 视觉格式化模型
    
    - 盒模型
    
    - 包含块：标签的活动区域
  
  - 计算后的样式： 所有的CSS属性值都有值；很多预设值会变成绝对值；带有样式的DoM树

- 布局
  
  - 得到每个节点的位置信息，宽高
  
  - 隐藏节点没有位置信息，没有几何信息，所以不会生成到布局树
  
  - 内容必须在行盒中
  
  - 行盒和块盒不能相邻。 <p>块盒，文字属于行盒，->造成DoM树和Layout树不一定一一对应

- 分层
  
  - 提高网页渲染的效率
  
  - 堆叠上下文，例如z-index
  
  - `will-change` 可以让div标签单独分层
  
  - 主线程使用一套复杂的策略进行分层，某一层改变后，单独对该层进行绘制

- 绘制 - paint
  
  - 为每一层生成绘图指令集合，主线程到此结束工作
  
  - 完成绘制后，主线程将每个图层绘制信息提交给合成线程，剩余工作由合成线程完成
  
  - 合成线程首先对每个区域进行分块，将其划分为更多小区域
  
  - 从线程池拿取多个线程完成分块工作

- 分块- Tiling
  
  - 分块会将每一层分为很多小的区域
  
  - 优先画眼睛感兴趣的区域

- 光栅化 raster
  
  - 分块完成进入光栅化阶段
  
  - 合成线程会将块信息交给GPU进程，以极高的速度完成光栅化
  
  - GPU进程会开启多个线程完成光栅化，并且优先处理眼睛感兴趣区域
  
  - 光栅化的结果是一块块的位图

- 绘画
  
  - 合成线程拿到每个层、每个块的位图后，生成一个个`指引 (quad)`信息
  
  - 指引会标识每个位图该画到屏幕的位置信息，同时会考虑到旋转，缩放，变形等
  
  - 变形发生在合成线程，与渲染主线程无关，这是transform效率高的原因
  
  - 合成线程会把quad提交给GPU进程，由GPU进程产生系统调用，提交给GPU硬件，完成最终的屏幕成像

上一个阶段是下一个阶段的输入

- 渲染进程运行在沙盒中，优势是安全
  
  - 渲染主线程
  
  - 合成线程

- 显卡调用属于系统调用

# 什么是reflow?

重新排版，尽量少做影响几何信息的操作

他的本质是重新计算layout树

# JavaScript实战-永远不要率先考虑优化

- backticks can be used to create the strings with formats

- the object construct function of JavaScript
  
  - ```js
    function = UIGoods(g){
        this.choose=0;
        this.data = g;
    }
    UIGoods.prototype.getTotalPrice = function(){
        return this.data.price * this.choose;
    }
    UIGoods.prototype.isChoose = function(){
        return this.choose > 0;
    }
    ```
  
  - JS - es6 version: 
    
    ```js
    class UIGoods{
        constructor(g){
            this.data = g;
            this.choose = 0;
        }
        getTotalPrice(){
            return this.data.price * this.choose;
        }
        isChoose(){
            return this.choose > 0;
        }
        // add one
        increase(){
            this.choose++;
        }
        decrease(){
            if (this.choose ===0){return;}
            this.choose--;
        }
    }
    //整个界面
    class UIData{
        constructor(){
            var uiGoods = [];
            for (var i=0; i<goods.length; i++){
                var uig = new UIGoods(goods[i]);
                uiGoods.push(uig)
        }
            this.uiGoods = uiGoods;
            this.deliveryThreshold = 30;
            this.deliveryPrice = 5;
        }
        getTotalPrice(){
            var sum = 0;
            for(var i=0; i<this.uiGoods.length; i++){
                var g = this.uiGoods[i];
                sum += g.getTotalPrice();
            }
            return sum;
        }
        increase(index){
            this.uiGoods[index].increase();
        }
        decrease(index){
            this.uiGoods[index].decrease();
        }
        getTotalChooseNumber(){
            var sum = 0;
            for(var i=0; i< this.uiGoods.length; i++){
                sum += this.uiGoods[i].choose;
            }
            return sum;
        }
        hasGoodsInCar(){
            return this.getTotalChooseNumber()>0;
        }
        isCrossDeliveryThreshold(){
            return this.getTotalPrice()>this.deliveryThrshold;
        }
        isChoose(index){
            return this.uiGoods[index].isChoose();
        }
    }
    var ui = new UIData()
    ```

数据逻辑写完后，开始写界面

```js
class UI{
    constructor(){
        this.uiData = new UIData();
        this.doms = {
            goodsContainer:document.querySelector('.goods-list'),
            deliveryPrice: document.querySelector('.footer-car-tip'),
            footerPay:document.querySelector('.footer-pay'),
            footerPayInnerSpan:document.querySelector('.footer-pay span'),
            totalPrice:document.querySelector('.footer-car-total'),
            car:document.querySelector('.footer-car'),
            badge:document.querySelector('.footer-car-badge'),
        };
        // jump position
        var carRect = this.doms.car.getBoundingClientRect();
        var jumpTarget={
            x:carRect.left + carRect.width/2,
            y:carRect.top + carRect.height /5,
        };
        this.jumpTarget = jumpTarget;
        this.createHTML();
        this.updateFooter();
        this.listenEvent();
    }
    // begin listening at the beginning
    listenEvent(){
        // registe a event
        this.doms.car.addEventListener('animationend',
                function(){
                    this.classList.remove('animate');
                }
            );    
    }
    // create product list based on data
    createHTML(){
        var html = ''
        for(var i=0; i<this.uiData.uiGoods.length; i++){
            var g = this.uiData.uiGoods[i];
            html += `从网页拷贝div相关元素代码- 为后面考虑，同时为加号和减号
                创建自定义index属性：
                    `
        }
        this.doms.goosContainer.innerHTML = html
    }
    increase(index){
        this.uiData.increase(index);
        this.updateGoodsItem(index);
        this.updateFooter();
        this.jump(index);
    }
    decrease(index){
        this.uiData.decrease(index);
        this.updateGoodsItem(index);
        this.updateFooter();
    }
    updateGoodsItem(index){
        var goodsDOM = this.doms.goodsContainer.children[index];
        if(this.uiData.isChoose(index)){
            goodsDom.classList.add('active');
        } else{
            goodsDom.classList.remove('active');
        }
        // update product num
        var span = goodsDom.querySelector('.goods-btns span');
        span.textContent = this.uiData.uiGoods[index].choose;
    }
    updateFooter(){
        var total=this.uiData.getTotalPrice();
        this.doms.deliveryPrice.textContent = `Delivery Fee${this.uiData.deliveryPrice}`
        if (this.uiData.isCrossDeliveryThreshold()){
            this.doms.footerPay.classList.add('active');
        } else{
            this.doms.footerPay.classList.remove('active');
            //更新差错少money
            var dis = this.uiData.deliveryThreshold - total;
            dis = Math.round(dis);
            this.doms.footerPayInnerSpan.textContent = `还差$ ${dis}起送`；
        }
        // set total price
        this.doms.totalPrice.textContent = total.toFixed(2);
        // set cart status
        if(this.uiData.hasGoodsInCar()) {
            this.doms.car.classList.add('active');
        } else{
            this.doms.car.classList.remove('active');    
        }
        // set car badge
        this.doms.badge.textContent = this.uiData.getTotalChooseNumber();
    }
    // add car animate
    carAnimate(){
        this.doms.car.classList.add('animate');
    }
    // jump animation
    jump(index){
        var btnAdd = this.doms.goodsContainer.children[index].querySelector('.加号class');
        var rect = btnAdd.getBoundingClientRect();
        var start = {
            x: rect.left,
            y: rect.top,
        };
        // jump - 横向匀速，
        var div = document.createElement('div');
        div.className = 'add-to-car';
        //div.innerHTML = 'copy from web page'
        var i = document.createElement('i');
        i.className = 'iconfont 加号class';
        // set initial position
        div.style.transform=`translate(${start.x}px)`;
        i.style.transform=`translate(${start.y}px)`;
        div.appendChild(i);
        document.body.appendChild(div)
        //强行让浏览器渲染，读取任何属性等同与调用reflow
        // best way: requestAnimationFrame
        div.clientWidth;
        // set end postion
        var x = this.jumpTarget.x;
        var y = this.jumpTarget.y;
        div.style.transform = `translate(${x}px)`
        i.style.transform =`translate(${y}px)`;
        // 去CSS文件的加号元素和i元素加上过度效果：transition: 2s
        //添加过度结束的事件
        // store the pointer this
        var that = this;
        div.addEventListener('transitionend',
            function(){
                div.remove();
                that.carAnimate()
            },
            {once:true,//事件仅仅触发一次}
        )
    }
}
// event
var ui = new UI();
ui.doms.goodsContainer.addEventListener('click',
    function(e){
        var flagAdd = e.target.classList.contains(加号的样式);
        var flagMin = e.target.classList.contains(-号的样式);
        if (flag){                
             var index = +e.target.getAttribute('index');//+转数字
             ui.increase(index);
        } else if(flagMin){
             var index = +e.target.getAttribute('index');//+转数字
              ui.decrease(index);
        }
    }
);
```

可以响应键盘事件

```javascript
window.addEventListener('Keypress', 
    function(e){
        if (e.code==='Equal'){ui.increase(0);}
        else if(e.code === 'Minus'){ui.decrease(0);}
    }
)
```

# 05 属性描述符

- getOwnPropertyDescriptor

- ```javascript
  var obj = {a:1,b:2}
  Object.getOwnPropertyDescriptor(obj,'a')
  ```

- Object.defineProperty()

- ```js
  Object.defineProperty(obj,'a',{
      value:10,
      writable:false,
  })
  ```

- ```js
  g = {...g};//拷贝对象
  //类的原型
  UIGoods.prototype
  ```

# Vue Frame

# 走进VUE作者的内心世界

# 前段知识图谱

`TypeScript`, `ThreeJs`, WebGL, Echarts, NodeJS, Egg, Redis, Mongodb, Sequelize, Express, Koa,
