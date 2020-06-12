![](https://chendongze.oss-cn-shanghai.aliyuncs.com/ipic/9gjn3.png)

---

>  **Apple 微信公众号**的排版效果总是让人眼前一亮，精致的细节、酷炫的动画让人不得不佩服 Apple 团队的用心与专业。
> 经过源码分析后发现，Apple 的微信公众号在排版上用了一些**黑科技**，其中包括了 SVG、Flex Layout、Chrome Inspect 等 Web 前端技术。
> **我今天就给大家分享下，Apple 微信公众号的效果是如何制作出来的。**

## 效果展示
我们来看下 iPhone SE 发布的那篇文章的展示效果
效果图：
![](https://chendongze.oss-cn-shanghai.aliyuncs.com/ipic/9i5r1.gif)

## 布局剖析

我们来分析下上面的布局效果：排版共有 5个模块组成。
我们分别用 A、B、C、D、E 来标记。如下图：
![](https://chendongze.oss-cn-shanghai.aliyuncs.com/ipic/86q1k.png)
排版剖析:

如果你稍懂 CSS，那么实现这样的布局是非常简单的。Apple 用的方式是使用 Flex Layout：

```html
<div style="display: flex">
    <div style="display: flex;width: 50%; flex-direction: column;">
         <div style="display: block;">
                 <!-- 内容 A -->
         </div>
        <div style="display: block;">
                <!-- 内容 B -->
         </div>
    </div>
    <div style="display: flex;width: 50%;flex-direction:column;">
<!-- 内容 C -->
    </div>
    <div style="display: flex;flex-direction:column;">
<!-- 内容 D -->
    </div>
    <div style="display: flex; flex-direction:column;">
<!-- 内容 E -->
    </div>
</div>
```
[完整源码](https://github.com/netpi/wechat-layout)

然而，微信官方的内容编辑器并不支持直接编辑 HTML，不过这难不倒我们，我们要使用一些简单的黑科技（后文会说明）就能把代码提交到微信后端，实现 Flex Layout 效果。

## 点击事件、动效

我们发现，Apple 的ABCDE每个模块都有点击事件，然而微信公众号并不支持 JS，那么点击事件是如何添加的的呢？
其实，Apple 团队采用 `SVG + JPEG/GIF 组合方案` 来给图片增加点击事件和动效的。** 我们来看下效果：

**1，SVG + JPEG + JPEG**
**完整效果，请用微信扫描文章顶部二维码：**
![](https://chendongze.oss-cn-shanghai.aliyuncs.com/ipic/1w8n1.gif)

**2，SVG + JPEG + GIF**
**完整效果，请用微信扫描文章顶部二维码：**
![](https://chendongze.oss-cn-shanghai.aliyuncs.com/ipic/3mxkj.gif)

**3，SVG + GIF + GIF**
**完整效果，请用微信扫描文章顶部二维码：**
![](https://chendongze.oss-cn-shanghai.aliyuncs.com/ipic/hg30g.gif)

之所以可以实现上述效果，是因为我们利用SVG给图片增加了点击事件。由于在 SVG 中 使用  `animate 标签` 可以添加事件，同时 animate 本身就有动画效果， 因此使用SVG，微信文章中的图片就拥有了交互能力。

使用这部分代码时，只需要将图片1、图片2 的URL替换成，你已经上传到微信图库中图片的 URL 即可实现上述效果。

当我们把 JPEG 用 GIF 来代替时，可以组合的效果选择就丰富多了。比如上面演示的 ``SVG + JPEG + GIF` 和 `SVG + JPEG + GIF`。

实现这个效果的主要代码如下：
```html
...
<div style="display: flex;width: 100%;flex-direction: column;">
    <svg xmlns="http://www.w3.org/2000/svg" 
          style="background-image: 
        url(图片2微信图库URL);">
        <animate attributeName="opacity"begin="click">
        </animate>
    </svg>
    <div
      background-image: 
url(&quot;图片1微信图库URL&quot;);">
    </div>
</div>
...
```
[完整源码](https://github.com/netpi/wechat-layout)
- - - - - 

## 用 Chrome Inspect 提交代码

我们知道，微信公众号的编辑器是不支持直接修改文章 HTML 的。那么我们该如何才能把编辑好的代码提交到微信后台呢？
这时候我们就要用到 `Chrome Inspect`，对于做前端的同学来说，Chrome Inspect 是调试过程中离不开的工具，它可以直接帮助我们修改前端 HTML 代码。因此提交代码的步骤是：
1. 用 Chrome 浏览器打开微信公众号的图文编辑器，在文章中随意输入一句话
2. 右键点击网页空白处，选择 `Inspect` 。
3. 打开 Inspect 后，用 Inspect 左上角的选择器选中最开始输入的内容，右键点击 <p> 标签，选择 `Edit as HTML`
4. 贴入代码（Flex 或 SVG），就能看到效果。
![](https://chendongze.oss-cn-shanghai.aliyuncs.com/ipic/u1u09.png)

## Flex Layout + SVG + Chrome Inspect 实现苹果动效

我们了解了 Apple  公众号的效果是如何实现的了，下面那么我们来实践一下。

**完整效果，请用微信扫描文章顶部二维码：**

![](https://chendongze.oss-cn-shanghai.aliyuncs.com/ipic/flm9g.gif)

- - - - - 
为了让大家方便使用，我已经把代码整理好提交到了 Github，点击 [完整源码](https://github.com/netpi/wechat-layout) 即可获得**。
