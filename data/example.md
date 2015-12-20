###本页面代码示例
本页面的功能
####路由功能
####使用Ajax获取MD数据，并渲染出来
####绑定左侧的点击事件


可以查看本页面的代码，你会发现，本页面连事件绑定、ajax都没有，并且很好的完成hash路由的功能

我们来逐一分析一下代码

下面是完整的代码
<pre class="brush:js">
var tutorialList = [
  {title: "首页", link: "home"},
  {title: "快速了解", link: "glance"},
  {title: "三层架构体系", link: "thoery"},
  {title: "渲染页面", link: "renderpage"},
  {title: "模块的二进制状态化", link: "modelrelationship"},
  {title: "模块的关系模型", link: "relation"},
  {title: "通讯模式", link: "commucation"},
  {title: "模型继承", link: "extendModel"},
  {title: "Abstract事件机制", link: "event"},
  {title: "使用延时加载策略", link: "load"},
  {title: "结束模型生命", link: "stop"},
  {title: "cgiModel", link: "cgiModel"},
  {title: "linkModel", link: "linkModel"},
  {title: "模型配置与注入", link: "config"},
  {title: "扩展模型", link: "extend"},
  {title: "Soda模板概述", link: "sodaRender"},
  {title: "Abstract 2.0通用规范", link: "codeguide"},
  {title: "本页面代码示例", link: "example"},
  {title: "示例代码", link: "example2"},
  {title: "API文档", link: "API"},
];

var renderList = new RenderModel({
  data: {
    list: tutorialList
  },

  tmpl: '\
    <ul>\
    <li class="link" soda-repeat="item in list" id="{{item.link}}">{{item.title}}</li>\
    </ul>\
      <div class="wrapper copyright">\
        ©  AlloyTeam·dorsywang\
      </div>\
  ',
  el: ".index"
});

var content = new RenderModel({
  el: ".content",
  myData: {
  },
  processData: function(data){
    var md = marked(data, {sanitize: false});;

    return {
      content: md
    };

  },

  method: "GET",

  tmpl: "<div soda-bind-html='content'></div>",

  complete: function(data, cgiCount){
        SyntaxHighlighter.highlight();

        console.log(this.myData);

        window.location.hash = this.myData.selector;
  }
});

var tabs = new MultitabModel();
for(var i = 0; i < tutorialList.length; i ++){
  var item = tutorialList[i];

  var link = item.link;

  var tab;
  if(link === "home"){
     tab = new LinkModel({
         url: "index.html"
     });

  }else{
    tab = content.extend({
        url: "data/" + link + ".md",
        myData: {
          selector: link
        }
    });
  }

  tabs.add("#" + link, tab);
}

if(window.location.hash){
  tabs.init(window.location.hash);
}else{
  tabs.init("#" + tutorialList[1].link);
}

var pageModel = new PageModel();
pageModel.add(renderList);
pageModel.add(tabs);

pageModel.rock();
</pre>

####第一块内容： 配置项

下面是完整的代码
<pre class="brush:js">
var tutorialList = [
  {title: "首页", link: "home"},
  {title: "快速了解", link: "glance"},
  {title: "三层架构体系", link: "thoery"},
  {title: "渲染页面", link: "renderpage"},
  {title: "模块的二进制状态化", link: "modelrelationship"},
  {title: "模块的关系模型", link: "relation"},
  {title: "通讯模式", link: "commucation"},
  {title: "模型继承", link: "extendModel"},
  {title: "Abstract事件机制", link: "event"},
  {title: "使用延时加载策略", link: "load"},
  {title: "结束模型生命", link: "stop"},
  {title: "cgiModel", link: "cgiModel"},
  {title: "linkModel", link: "linkModel"},
  {title: "模型配置与注入", link: "config"},
  {title: "扩展模型", link: "extend"},
  {title: "Soda模板概述", link: "sodaRender"},
  {title: "Abstract 2.0通用规范", link: "codeguide"},
  {title: "本页面代码示例", link: "example"},
  {title: "示例代码", link: "example2"},
  {title: "API文档", link: "API"},
];
</pre>

上面的代码主要是配置了左侧的菜单和对应的md文件名和路由名


####第二块内容： 配制模型 渲染左侧菜单

<pre class="brush:js">

var renderList = new RenderModel({
  data: {
    list: tutorialList
  },

  tmpl: '\
    <ul>\
    <li class="link" soda-repeat="item in list" id="{{item.link}}">{{item.title}}</li>\
    </ul>\
      <div class="wrapper copyright">\
        ©  AlloyTeam·dorsywang\
      </div>\
  ',
  el: ".index"
});
</pre>


左侧菜单将会渲染到className为index的dom元素中，上面的代码也很简单

####第三块内容： 配制模型 定义右侧内容的渲染模型

<pre class="brush:js">

var content = new RenderModel({
  el: ".content",
  myData: {
  },
  processData: function(data){
    var md = marked(data, {sanitize: false});;

    return {
      content: md
    };

  },

  method: "GET",

  tmpl: "<div soda-bind-html='content'></div>",

  complete: function(data, cgiCount){
        SyntaxHighlighter.highlight();

        console.log(this.myData);

        window.location.hash = this.myData.selector;
  }
});
</pre>

可以看到，每个左侧的菜单对应一个右侧的渲染页面

右侧的页面的行为都是一样的，基本模式也一样，并且他们的关系都是互斥关系

下面我们正好使用一个MultitabModel来约束他们

每个模型之间可以通过继承获得

####第四块内容： 配制关系 
