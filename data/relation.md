###关系模型
关系模型通过add方法将渲染模型增加进来，用于表述渲染模型之前的关系

<img src="image/constructor.png" width="600" />

####互斥关系模型MutexModel
用于表述互斥的关系，即： 一个模型激活，其兄弟模型处于非激活态

比如常见的多tab模型

<img src="image/mutirelation.png" />

其他代码如下

<pre class="brush:js">
var todayHot = new RenderModel({
    el: ".tab1",
    tmpl: "{{tabname}}",
    data: {
        tabname: "今日最热"
    }
});

// 再次激活前把上次的渲染数据清掉 防止二次渲染
todayHot.addEventListener("beforeactived", function(){
	Model.$(this.el).html("");
});



var movie = new RenderModel({
    el: ".tab2",
    tmpl: "{{tabname}}",
    data: {
        tabname: "影视剧"
    }
});

// 再次激活前把上次的渲染数据清掉 防止二次渲染
movie.addEventListener("beforeactived", function(){
	Model.$(this.el).html("");
});

var showtab = new RenderModel({
    el: ".tab3",
    tmpl: "{{tabname}}",
    data: {
        tabname: "综艺"
    }
});

// 再次激活前把上次的渲染数据清掉 防止二次渲染
showtab.addEventListener("beforeactived", function(){
	Model.$(this.el).html("");
});

// 配置关系
var tab = new MutexModel();
tab.add(todayHot);
tab.add(movie);
tab.add(showtab);

tab.addEventListener("unactived", function(){
	this.hide();
});
tab.addEventListener("actived", function(e){
	this.show();
	
});


// tab激活
tab.rock();

Model.$("#rock").on("click", function(e){
	movie.rock();
});

Model.$("#rock2").on("click", function(e){
	showtab.rock();
});


</pre>


<a href="http://runjs.cn/code/egmopkvb" target="_blank">点击查看示例代码</a>


####互斥关系模型加强关系MultitabModel
MultitabModel是Mutex的加强关系，与MutexModel不同的是，其add方法可以有一个点击事件的selector，表示该元素被点击了之后，会将该模型激活，其他模型就处于非激活态了

上面的例子，使用MultitabModel会更加简单一些，如下代码
<pre class="brush:js">
var todayHot = new RenderModel({
    el: ".tab1",
    tmpl: "{{tabname}}",
    data: {
        tabname: "今日最热"
    }
});


var movie = new RenderModel({
    el: ".tab2",
    tmpl: "{{tabname}}",
    data: {
        tabname: "影视剧"
    }
});



var showtab = new RenderModel({
    el: ".tab3",
    tmpl: "{{tabname}}",
    data: {
        tabname: "综艺"
    }
});



// 配置关系
var tab = new MultitabModel();
tab.add("#rock0",todayHot);
tab.add("#rock", movie);
tab.add("#rock2", showtab);


// tab激活
tab.rock();
</pre>


<a href="http://runjs.cn/code/xueeysyh" target="_blank">点击查看示例代码</a>

####更抽象的例子-弹框
比如首页的实例弹窗

<img src="image/popup.gif" width="600" />

弹窗可以抽象成如下的结构体系

<img src="image/popup.png" />
