
<h3 align="center" style="color: #99CCFF">3行代码 为网站添加萌萌哒看板娘</h3>

**看板娘**是一种职业和习惯称呼，也是[ACGN](https://baike.baidu.com/item/ACGN?fromModule=lemma_inlink)次文化中的[萌属性](https://baike.baidu.com/item/萌属性/9305130?fromModule=lemma_inlink)之一。简而言之就是小店的女服务生，也有“吸引顾客，招揽生意，提高人气”等作用类似品牌形象代言人的含义。看板(kanban)是日语特有的管理名词，其英文可以是“signboard”或者“visible record”。通常，看板是一张标签或卡片，放在透明颜料袋装内，或贴在零件上，或贴在盛装制品的容器上，还可以是流水线上各种颜色的小球或信号灯、电视图象等。

- 以上都是百度说的

在逛一些大佬们的博客的时候，时不时会看到他们的博客里会有一些萌萌可爱的小人，比如下面这些

![show01](https://s1.ax1x.com/2022/08/31/v5Fwuj.png)

![show02](https://s1.ax1x.com/2022/08/31/v5F0Ds.png)

![show03](https://s1.ax1x.com/2022/08/31/v5FavQ.png)

试问谁能拒绝将一只萌萌可爱的看板娘养在自己的博客里呢？

- 皮一下很开心 (｡>∀<｡)

于是我就开始了漫长的寻找资料的旅程，其中走了很多很多的弯路，终于把这些可爱的小姑娘加进来

其中的问题包括但不仅限于：看板娘无法正常显示、看板娘无法切换模型、看板娘无法与网站交互......今天我们一个个解决！

- 看板娘拥有的功能如下

![power](https://s1.ax1x.com/2022/08/31/v5A0kq.png)

- 看板娘默认只在PC端才会显示，手机屏幕太小，看板娘容易遮挡内容哦

#### 如何安装看板娘

1. 首先在github上把项目下载下来：[live2d_demo](https://github.com/Kindnit/live2d_demo.git)

2. 然后将其放进自己的项目目录下：

   ![directory](https://s1.ax1x.com/2022/08/31/v5Aa0s.png)

3. 最后在网页中引入这三行代码即可

```html
<link rel="stylesheet" href="https://fastly.jsdelivr.net/npm/@fortawesome/fontawesome-free@6/css/all.min.css">
<script src="../live2d-widget/autoload.js"></script>
<script src="https://cdn.jsdelivr.net/npm/jquery@3.2/dist/jquery.min.js"></script>
```

- 这里需要特别注意的就是路径问题，大多数模型无法正常显示的原因都是因为路径不对，这里大家在浏览器里用F12或者其他方式使用开发者模式检查错误。当然也有可能是我们无法访问外网访问超时啦，这里大家就需要自己各展绝技，我这里就不细说了。

这里我故意将路径改错，看板娘便无法显示，并且浏览器会报404错误

![error](https://s1.ax1x.com/2022/08/31/v5Ad7n.png)

![404](https://s1.ax1x.com/2022/08/31/v5ADhV.png)

大家可以注意到我们的引用里只有`<script src="../live2d-widget/autoload.js"></script>`这个是使用了本地的项目文件，那么显而易见的`autoload.js`就是我们的主要配置(调教)入口。

- 首先就是刚刚看到的`autoload.js`里对各个文件的路径参数`live2d_path = "/live2d-widget/"`

```js
// 注意：live2d_path 参数应使用绝对路径 看看有没有改成功
// const live2d_path = "https://fastly.jsdelivr.net/gh/kindnit/live2d-widget@latest/";
const live2d_path = "/live2d-widget/";

// 加载 waifu.css live2d.min.js waifu-tips.js
if (screen.width >= 768) {
	Promise.all([
		loadExternalResource(live2d_path + "waifu.css", "css"),
		loadExternalResource(live2d_path + "live2d.min.js", "js"),
		loadExternalResource(live2d_path + "waifu-tips.js", "js")
	]).then(() => {
		initWidget({
			waifuPath: live2d_path + "waifu-tips.json",
			//apiPath: "https://live2d.fghrsh.net/api/",
			// cdnPath: "https://fastly.jsdelivr.net/gh/fghrsh/live2d_api/"
			apiPath: "https://api.zsq.im/live2d",
		});
	});
}
// initWidget 第一个参数为 waifu-tips.json 的路径，第二个参数为 API 地址
// API 后端可自行搭建，参考 https://github.com/fghrsh/live2d_api
// 初始化看板娘会自动加载指定目录下的 waifu-tips.json
```

这里大家自己针对浏览器里报错的信息，来检查自己的哪个文件找不到，如果有说`autoload.js`找不到，那么可能就是大家在网页里的引用地址写错。如果是`waifu.css`、`live2d.min.js`、`waifu-tips.js`这几个找不到的话，那么估计就是`autoload.js`里的路径参数`live2d_path`写错了，大家多尝试改改就行，我这里就不多赘述。

#### 如何定制看板娘语录

1. 从github中下载项目: [live2d_demo](https://github.com/Kindnit/live2d_demo.git)

2. 更改waifu-tip.json里面的内容

   这里面的内容包括预设的语录和自定义语录，这里我们分别举例说明

   ```json
   {
   		"selector": "#waifu-tool .fa-comment",
   		"text": ["猜猜我要说些什么呢?", "我从青蛙王子那里听到了不少人生经验。"]
   }
   
   {
   		"selector": ".menu-item-archive",
   		"text": ["翻页比较麻烦吗，那就来看看文章归档吧。", "文章目录都整理在这里啦！"]
   }
   ```

   这里看到`"selector": "#waifu-tool .fa-comment"`这个选择器选择的是预设的功能键的id

   而`"selector": ".menu-item-archive"`这里则是选择的是我们自定义的id，效果如下：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Live2d</title>
       <link rel="stylesheet" href="https://fastly.jsdelivr.net/npm/@fortawesome/fontawesome-free@6/css/all.min.css">
       <script src="../live2d-widget/autoload.js"></script>
       <script src="https://cdn.jsdelivr.net/npm/jquery@3.2/dist/jquery.min.js"></script>
       <style>
           body{
               background: url("../image/background.png") !important;;
           }
       </style>
   </head>
   <body>
   <h3 align="center"  style="color: #99CCFF">3行代码 为网站添加萌萌哒看板娘</h3>
   <p style="color:#6698cb">展示</p>
   <a class="menu-item-archive">归档</a>
   </body>
   </html>
   ```

   ![show04](https://s1.ax1x.com/2022/08/31/v5AspT.png)

   

3. 更改autoload.js的引入位置，将其改成自己的路径，这里在上一部分已经说啦

#### 小结

看板娘的很多语言都是通过配置文件定制的, 所以看板娘的秉性全靠主人的本事, 我改了一下配置文件, 感觉很欢乐, 甚至玩出了人工智能的感觉~

如果大家还是不会玩可以参考我的个人博客代码：[Kindnit_blog](https://github.com/Kindnit/Kindnit_blog.git) 有问题也可以发到评论区集思广益一下

也可以参考刚刚下载下来的小demo，对照学习，多尝试一下。这里贴上原项目的开源地址[live2d-widget](https://github.com/stevenjoezhang/live2d-widget)

最后希望大家可以都可以成功将萌萌的看板娘放到自己的网站~
