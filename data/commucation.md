###通讯模式

Abstract中通讯模式是用于描述模块之间通讯的方法，与现实世界对应通讯方式，最直接的两种是Tell和Watch，一种是主动通知，另外一种是监听模块的状态变化

####tell
tell sb, sth, dowhat  

告诉某人，某事，要做什么

<pre class="brush:js">
model1.tell(model2, data, function(){
});
</pre>

由于实现和接收该方法的只能是Abstract模型，所以Model只能将自己的某个属性来告知另外一个模型.

但注意Abstract都是配置型代码，此处的Tell方法并不是主动通知，而是配置了这种主动通知的关系，即model1的data一但发生变化，model2就会获得此信息，如下策略代码
<pre class="brush:js">

model1.tell(model2, 'data', function(value){
    if(value){
        model2.data = value;

        model2.rock();
    }
});
</pre>

####watch
watch sb, attr, dowhat
坚听某人的某个属性变化，然后做一些事情


此处的watch可以监听一个object，也可以是一个Abstract Model,比如
<pre class="brush:js">
var watched = {
    pageNum: 1
};

// 如果页码发生变化，则模块要更新
model1.watch(watched, 'pageNum', function(value){
    if(value){
        model2.param.pageNum = value;

        model2.rock();
    }
});
</pre>
