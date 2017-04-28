---
layout: post
title: textContent VS innerText
category: tech
tags: 技术
keywords: textContent,innerText
---

## javascript中innerText和textContent异同分析

[TOC]

> 由于innerText并非W3C标准属性，因此我们无法在FireFox中使用它，一般情况下我们可以使用textContent来代替

#### textContent与innerText的区别 
- 1.textContent 会获取所有元素的 content,包括 `<script>` 和 `<style>`元素；
- 2.innerText 会获取hidden元素的 content,而textContent不会；
- 3.innerText 会触发reflow(回流), 而textContent不会；
- 4.innerText 返回值会被格式化，而textContent 不会。

#### innerHTML
由于innerText和textContent均为对innerHTML内容作不同的处理而成，因此我们需要先明确innerHTML属性的特点。
 赋值操作：先对值内容进行模式匹配，然后把处理后的值赋予给innerHTML属性。

模式匹配结果将导致 保留 和 将字符转换为HTML实体 两个操作。
a. 以下情况将被保留
``` python
1. HTML实体（ASCII实体、符号实体和字符实体）的实体名或实体编号；
2. 符号实体和字符实体对应的字符；
3. 没有HTML实体与之对应的字符。
``` 
b. 以下情况将会执行字符转换为HTML实体
``` python
1. ASCII实体对应的字符（<、>、&、'和"）。
也就是说除了 <、>、&、'和" 会被转换为实体名外，将原封不动地将值赋予给innerHTML属性。
``` 

**取值操作**：直接获取innerHTML属性值。

``` python
<style type="text/css">
 .line3, .line4{
   float: left;
 }
 .line5::after{
   content: "test"
 }
</style>
<div id="view">
  <div>line1</div>
  <div>line2</div><br/>
  <div class="line3">line3</div>
  <div class="line4">line4</div>
  <div style="clear:both;"></div>
  <div class="line5">line5</div>
</div>
<script type="text/javascript">
  var view = document.getElementById('view')
</script>
``` 

#### innerText　
浏览器支持： IE、Chrome
赋值操作：先将ASCII实体对应的字符（<、>、&、'和"）转换为实体名，然后把处理后的值赋予给innerHTML属性。
取值操作：innerText的取值实际上就是对innerHTML的属性值进行一系列处理，然后返回，具体步骤如下:

* 1. 对HTML标签进行解析；
* 2. 对CSS样式进行带限制的解析和渲染；
* 3. 将ASCII实体转换为对应的字符；
* 4. 剔除格式信息（如\t、\r和\n等），将多个连续的空格合并为一个。

#### textContent　
浏览器支持：IE9~11、FireForx、Chrome

赋值操作：先将ASCII实体对应的字符（<、>、&、'和"）转换为实体名，然后把处理后的值赋予给innerHTML属性。

取值操作：textContent的取值实际上就是对innerHTML的属性值进行一系列处理，然后返回，具体步骤如下:

* 1. 对HTML标签进行剔除；
* 2. 将ASCII实体转换为相应的字符。

注意：
* a). 对HTML标签是剔除不是解析，也不会出现CSS解析和渲染的处理，因此<br/>等元素是不生效的。
* b). 不会剔除格式信息和合并连续的空格，因此\t、\r、\n和连续的空格将生效。


#### 表单元素中的innerHTML、innerText、textContent和value























