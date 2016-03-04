###Abstract事件机制

####事件冒泡
Model通过add方法形成了树形结构，那么就像DOM一样，Model的事件可以通过冒泡传递给上层Model

模型通过addEventListener进行监听事件，第一个参数为e，使用e.preventDefault()方法可以阻止事件的冒泡传递

<pre class="brush:js">
var top = new PageModel();
var banner = new RenderModel();

top.add(banner);

top.addEventListener('actived', function(e){
        alert('监听到了模型激活事件');
});

banner.rock();
</pre>

####通用事件列表

<table ceilspacing=0>
<tr>
<th>事件名</th>
<th>说明</th>
</tr>

<tr>
<td>beforeactived</td>
<td>在模型激活前的事件</td>
</tr>

<tr>
<td>actived</td>
<td>模型激活后的事件, 模型调用rock方法之后就会抛出该事件</td>
</tr>

<tr>
<td>unactived</td>
<td>模型处于失活态后的事件, 模型调用stop方法之后就会抛出该事件</td>
</tr>

<tr>
<td>reset</td>
<td>模型重置后的事件, 模型调用reset方法之后就会抛出该事件</td>
</tr>


<tr>
<td>refresh</td>
<td>模型刷新后的事件, 模型调用refresh方法之后就会抛出该事件</td>
</tr>

</table>

####通用事件列表
