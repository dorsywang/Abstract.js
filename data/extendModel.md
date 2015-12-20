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
