---
layout: post
title: writing-mode实践
category: tech
tags: 技术
keywords: writing-mode
---

## writing-mode实践

关于writing-mode，大家的了解大概都来源于张鑫旭的博文 [改变CSS世界纵横规则的writing-mode属性](http://www.zhangxinxu.com/wordpress/2016/04/css-writing-mode)
文章提到了很多属性值，其中大部分是IE专有的，由于所涉项目的局限性我只在chrome下使用，常用下面三个属性值
``` python
writing-mode: horizontal-tb;    /* 默认值 */
writing-mode: lr-tb;    /* 从左向右，从上往下 */
writing-mode: tb-rl;    /* 从上往下，从右向左 */
```
在移动端浏览器中，以上属性表现一直如常，数字和英文也完美地“躺着”显示。
![Alt text](/picture/2018-02-24/1.png)



但是使用**html2canvas**实现浏览器截图，使用了**writing-mode**非默认属性的时候，数字和英文无法由上而下排布，还是横版的排版。如下：![Alt text](/picture/2018-02-24/2.png)




这可怎么办？给文本内容逐个加上br标签的话，数字还是不能躺着。
思来想去反正是移动端浏览器用，肯定支持css3，用个**span**标签把数字部分包起来，加个**writing-mode: horizontal-tb**，
然后再加一个旋转**transform: rotate(90deg)**，完美。



完美？Are you sure?
![Alt text](/picture/2018-02-24/3.png)


看起来是挺正常的，紧接着**html2canvas**一渲染。

![Alt text](/picture/2018-02-24/4.png)

怎么回事？长得不一样啊，渲染出来的dom会发生偏移。



截图看了看渲染前后的变化，不是span的位置变化了，是文本部分的“**你好，**”，因为没有被标签包裹着，所以发生了偏移，向左边移动了几个像素。

![Alt text](/picture/2018-02-24/5.png)

都是**html2canvas**的锅，对**writing-mode**解析有偏差。我还能怎么样，当然是选择原谅它。（不原谅的话据说可以选用 **phantomjs** 代替它，当然我还没用过）

最后没有办法，只能把数字用标签一个个包起来：

``` python
<b>你</b><b>好</b><b>，</b>
<span>
    <b>2</b>
    <b>0</b>
    <b>1</b>
    <b>8</b>
</span>
```

![Alt text](/picture/2018-02-24/6.png)


这时候的显示比较正常了，再控制一下标点符号的** ine-height**就好了。

比较low的办法，但是能解决问题。



另，在react中，动态传入html我是这样操作的

``` python
render() {
	const txt = '你好，2018';
	const txtHtml = txt.split('').map(function(e){return '<b>' + e + '</b>';}).join('');
	return (
		<span dangerouslySetInnerHTML={{__html: txtHtml}}></span>
	)
}
``` 
end
