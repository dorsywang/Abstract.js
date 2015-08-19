#快速上手
##概览
###理论
通常我们只关心怎么用数据与渲染页面，而且我们花了很大的时间去写一陀代码就是为了渲染数据。Abstract.js从中提供了相应的抽象的模型，比如RenderModel

下面我们将学习如何用Abstract去写我们的页面

###分割模块
在写代码之前，我们需要思想怎么将页面划分成每个渲染模块

如下是兴趣部落的页面，我们将它划分好每个的渲染模块

<img src="image/blocks.png" alt="" />

###编辑html框架
为每个模块写一个div框架，用于渲染

<pre class="brush: xml">
&lt;div class="header">&lt;/div>
&lt;div class="nav">&lt;/div>
&lt;div class="recommend">&lt;/div>
&lt;div class="list">&lt;/div>
</pre>

###配置
header,nav,recomment模块将会渲染一次，我们就把它们认为是传统的渲染模块。而list模块会在我们拉到底部的时候，继续渲染，所以它应该是一个ScrollModel。我们上面的考虑进行如下的配置。
<pre class="brush:js">
var header = new RenderModel({
   el: ".header",
   url: "cgi/get_header",
   // 数据回来的时候，加工数据
   processData: function(data){
   },

   tmpl: "{{content}}...",

   // 渲染完成，做一些事情
   complete: function(){
   }
});

// 同样配置nav模块
var nav = new RenderModel({
        // ... some code here
});

var recommend = new RenderModel({
        // ... some code here
});

// ScrollModelt和RenderModel有同样的配置项
var list = new ScrollModel({
        // ... some code here
});
</pre>

### 设置每个模型之前的关系
Abstract中只有两种关系去表述模型之前的关系。而且我们可以证明，它足够表述。

其中的一个是PageModel，用来表述这种关系：当一个模型被激活了，其兄弟模型都会激活

另外一个是MutexModel，用来表述这种关系：当一个模型激活了，其兄弟模型全部处于非激活态

更多的是，还有一个MultitabModel，这个并不是一个新的模型，是MutexModel的增加模型。后续会详细介绍MultitabModel

所以我们可以发现对于这个页面的这些模块它们之前是什么关系. 这四个模块都会同时激活，所以它们具有PageModel这种关系,具体的配置代码如下

<pre class="brush: javascript">
var page = new PageModel();
page.add(header);
page.add(nav);
page.add(recommend);
page.add(list);
</pre>

### 页面开始动起来
现在，页面不会执行出任何东西。因为我们没有让它们活起来。这就像是我们组装了小丑的每一块，我们需要用电击来让它活起来。

这里我们使用Rock方法，页面将会活起来

<pre class="brush: javascript">
page.rock();
</pre>

这时候你就看到页面就渲染出来了
