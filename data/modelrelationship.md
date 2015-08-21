###二进制表述所有状态
在Abstract中，模块间的关系抽象为两态。其实事物的状态有多种，但计算机可以使用二进制的状态描述事物的, Abstract也是如此，模块的状态也只有两种，激活态与非激活态。

###激活态
模型处于激活态时，一般会有一些预定的行为。这就好比一个睡着了，他处理静止的状态，如果醒了，就会进行一些行为。

对于传统的RenderModel，处于激活态会进行，拉取cgi、渲染页面的操作。

对于多次渲染的ScrollModel，处于激活态意味着监听到底部的事件被激活了，用户下拉到底部的时候就会进行下一次的渲染。

通常来讲，调用rock方法即是激活当前模型。就好比一个小丑的一个部分，调用rock方法即是给其点击，就会让这部分活起来一样。

调用rock方法会调用该模块的active方法，同时该模块会抛出一个actived的事件出去并进行冒泡，用户可以通过该模块或上层模块进行捕获事件或者取消传播

####修改模块的默认激活态行为
如下的事例代码会展示如何进行修改默认的激活态行为
<pre class="brush:js">
var common = new RenderModel({
	el: ".container"
});

// 覆盖onactive默认方法
// RenderModel的渲染将不会继续进行
common.active = function(){
	alert("common被激活了");
};

Model.$("#rock").on("click", function(e){
	common.rock();
});
</pre>

<a target="_blank" href="http://runjs.cn/code/9t7qhcm0">点击这里看代码运行</a>

####监听模块的激活态事件

使用addEventListener方法可以监听模块的事件，冒泡的事件可以在上层进行捕获

模块激活时会抛出一个actived事件进行传播
<pre class="brush:js">
var common = new RenderModel({
	el: ".container",
  tmpl: "{{title}}",
  data: {
    title: "Abstract"
  }
});

// 监听actived事件
common.addEventListener("actived", function(e){
	alert("common被激活了");
});

Model.$("#rock").on("click", function(e){
	common.rock();
});

</pre>

<a target="_blank" href="http://runjs.cn/code/bmvhrq2w">点击这里看代码运行</a>

####在上层捕获事件
如下代码展示如何在上层进行捕获事件

<pre class="brush:js">
var common = new RenderModel({
	el: ".container",
  tmpl: "{{title}}",
  comment: 'common子元素',
  data: {
    title: "Abstract"
  }
});

var parent = new PageModel();
parent.add(common);

// 在parent层监听事件
parent.addEventListener("actived", function(e){
	alert(e.target.comment + "被激活了");
});

Model.$("#rock").on("click", function(e){
	common.rock();
});

</pre>

<a target="_blank" href="http://runjs.cn/code/wusvy5ec">点击这里看代码运行</a>

###非激活态
模型处于非激活态时，一般也会有一些预定的行为。

模型处于非激活态时会抛出事件unatived

对于传统的RenderModel，处于非激活态时会进行, 基本不会进行相关的操作，只是将当前的状态参数置为非激活态，并抛出事件unactived

对于多次渲染的ScrollModel，处于非激活态意味着监听的滚动下拉事件会禁止。

可以通过修改unactive方法来覆盖默认的非激活态行为

手动调用stop方法将进行抛出非激活态事件



<pre class="brush:js">
var common = new RenderModel({
	el: ".container",
  tmpl: "{{title}}",
  comment: 'common子元素',
  data: {
    title: "Abstract"
  }
});

var parent = new PageModel();
parent.add(common);

// 在parent层监听事件
parent.addEventListener("unactived", function(e){
	alert(e.target.comment + "处于了非激活态,且没有渲染");
});

Model.$("#rock").on("click", function(e){
	common.stop();
});

</pre>

<a target="_blank" href="http://runjs.cn/code/jzs5hdkd">点击这里看代码运行</a>
