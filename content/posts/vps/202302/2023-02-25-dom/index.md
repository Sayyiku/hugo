---
title: "常用 DOM 方法比较"
date: "2023-02-25"
categories: 
  - "前端"
---

## [innerText 对比 textContent](https://www.zhangxinxu.com/wordpress/2019/09/js-dom-innertext-textcontent/)

总结如下：

- innerText 获取的文字的换行符依然保留；
- innerText 无法获取隐藏文字；
- innerText 性能要相对差一些；  
    

innerText 由于存在诸多特别的特性、以及兼容性差异，以及性能方面问题，以及实际开发的需求的考量，不推荐使用，**推荐使用 textContent 获取文本内容**。

```
var text = dom.textContent
```

如果你的项目还需要兼容 IE8 浏览器，则使用下面的代码：

```
var text = dom.textContent || dom.innerText
```

注：**vue 的 v-text 指令就是更新元素的 textContent**。

## getAttribute 对比 dataset 对象

例如，有如下 HTML：

```
<button id="button" data-author="zhangxinxu">作者是谁？</button>
```

使用 getAttribute 和 dataset 对象都能获取 data-author 属性值：

```
button.getAttribute('data-author') // zhangxinxu
button.dataset.author // zhangxinxu
```

差别在于大小写敏感的区别，**getAttribute 方法是无视大小写的，而 dataset 对象必需要使用小写**，例如：

```
<button id="button" data-AUTHOR="zhangxinxu">作者是谁？</button>
```

```
button.getAttribute('DATA-author') // zhangxinxu
button.dataset.AUTHOR // undefined
button.dataset.author // zhangxinxu
```

**如果自定义属性包含多个词组，则 dataset 对象属性值需要使用驼峰命名获取**：

```
<button id="button" data-article-author="zhangxinxu">感谢阅读！</button>
```

```
button.getAttribute('data-article-author') // zhangxinxu
button.dataset.articleAuthor // zhangxinxu
```

## getElementById 对比 querySelector

一般情况下这两个方法是等效的，但推荐使用 `getElementById()` 方法，因为这个 API 的容错性最强，不容易导致 JS 运行中断。

假设某个元素的 ID 是未知的，通过参数传过来的，但是这个字符串参数可能各种各样，假设这个字符串是'thanksForShare!'，此时分别运行下面的代码：

```
document.getElementById('thanksForShare!') // 正常运行
document.querySelector('#thanksForShare!') // 报错
```

结果 `getElementById()` 方法安全地返回了 null，但是 `querySelector()` 方法直接报错了：

> VM309:1 Uncaught DOMException: Failed to execute ‘querySelector’ on ‘Document’: ‘#thanksForShare!’ is not a valid selector.

也就是说，在使用 `querySelector()` 方法的时候，我们需要对里面的选择器进行合法性校验，或者 `try…catch` 处理，否则就会影响整个 JavaScript 代码的运行。**因此优先使用 `getElementById()` 方法去获取 DOM 元素**。
