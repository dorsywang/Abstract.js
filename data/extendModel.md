###模型继承
继承方法是复用性的很好例子，很多时候，有许多相似的模块，他们无需再实例化，而是直接继承即可

###继承方式
####extend方法

<pre class="brush:js">
var common = new RenderModel({
	el: ".container",
  tmpl: "{{title}}",
  comment: 'common子元素',
  data: {
    title: "Abstract"
  }
});

var common2 = common.extend({
    el: ".container2'
});

var page = new PageModel();
page.add(common);
page.add(common2);

page.rock();
 
</pre>

###继承的本质
继承实际上是让相同的模块有相似的属性

###继承的注意点
注意：为了防止events中事件的多次触发，events是不会继承，如果events被覆盖，则events会重新执行
