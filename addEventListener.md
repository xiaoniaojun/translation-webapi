# EventTarget.addEventListener()
*EvenetTarget.addEventListener()*方法为[EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)注册指定的监听器。事件目标可以是document中的一个元素，可以是[document](https://developer.mozilla.org/en-US/docs/Web/API/Document)本身，可以是[Window](https://developer.mozilla.org/en-US/docs/Web/API/Window)，还可以是其他支持事件的对象（比如[XMLHttpRequest](https://developer.mozilla.org/en-US/docs/DOM/XMLHttpRequest)）。
## 语法
```
target.addEventListener(type,listener,[options]);
target.addEventListener(type,listener,[useCapture]);
target.addEventListener(type,listener,[useCapture, wantsUntrusted]);//只待见Gecko/Mozilla
```
### 参数
#### type
一个表示需要被监听的事件类型(event type)的字符串。
#### listener
当指定事件类型被触发时，一个listener对象会收到一个通知(也是个对象，它实现了Event类的借口)。这个对象必须是一个实现了EventListener类接口的对象，或者也可以简单地提供一个js函数。
#### 选项[可选]
一个选项对象，它指定event listener的特征。可选的选项有：
* capture:一个Boolean对象，它指定这种类型的事件在被分发给所有DOM树下的事件目标(EventTarget)之前，会先分发给已经注册的listener。
* once:一个Boolean对象，加上这个的话就是指定这个listener应当至多只被触发一次。如果为真，这个listen就会在被触发时被自动移除。
* passive:一个布尔值，指定listener将不会调用preventDefault()。这样一来，用户代理应该忽略它并生成一个控制台警告。
* ⚠️mozSystemGroup:只能在XBL或firefox`chrome中可用，它是一个布尔值，决定这个listener是否被添加进系统群(system group)。
#### useCapture[可选]
一个Boolean对象，它指定这种类型的事件在被分发给所有DOM树下的事件目标(EventTarget)之前，会先分发给已经注册的listener。本来事件会向上冒泡来走一遍整颗DOM树，但是指定了use capture之后就不会再触发listener。事件冒泡和捕捉是两种不同的传播事件的方式(当元素嵌套再另一个元素下，且它们都部署了事件处理时会发生这种情况)。事件传播模式决定了元素收到事件消息的顺序。浏览[DOM Level 3 Event](http://www.w3.org/TR/DOM-Level-3-Events/#event-flow)以及[JavaScript Event order](http://www.quirksmode.org/js/events_order.html#link4)获取更详细的介绍。如果不指定，useCapture默认为false.
