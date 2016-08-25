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
一个Boolean对象，它指定这种类型的事件在被分发给所有DOM树下的事件目标(EventTarget)之前，会先分发给已经注册的listener。本来事件会向上冒泡来走一遍整颗DOM树，但是指定了use capture之后就不会再触发listener。事件冒泡和捕捉是两种不同的传播事件的方式(当元素嵌套再另一个元素下，且它们都部署了事件handler时会发生这种情况)。事件传播模式决定了元素收到事件消息的顺序。浏览[DOM Level 3 Event](http://www.w3.org/TR/DOM-Level-3-Events/#event-flow)以及[JavaScript Event order](http://www.quirksmode.org/js/events_order.html#link4)获取更详细的介绍。如果不指定，useCapture默认为false.

`Note:针对所有附加在事件目标上的listener;事件在目标阶段，而不是捕获或冒泡阶段;事件在目标阶段会触发element上的所有listeners,无论_useCapture_参数如何。
Note:useCapture成为可选参数只是在最新的一些主流浏览器中，例如，在火狐6之前的版本中是不支持的。你应该提供这个参数来让你的函数调用更加安全。`

####wantsUntrusted

##Example
###Add a simple listener

HTML Content

```
<table id="outside">
  <tr><td id="t1">one</td></tr>
  <tr><td id="t1">one</td></tr>
</table>
```

JavaScrpt Content

```
//改变t2的内容
Function modifyText() {
    var t2 = document.getElementById("t2");
    if (t2.firstChild.nodeValue == "three"){
        t2.firstChild.nodeValue = "two";
    } else {
        t2.firstChild.nodeValue = "three";
    }
}

//

```

one
two

-------

这个例子中，modifyText()是由addEventListener()注册的点击事件listener.当你点击table的任何位置,点击事件都会冒泡给handler(handler),然后运行modifyText().

如果你想传递参数给listener函数,你可以使用一个匿名函数.

###事件Listener和匿名函数
HTML内容

```
<table id="outside">
    <tr><td id="t1">one</td></tr>
    <tr><td id="t2">two</td></tr>
</table>
```
JavaScript内容

```
// 改变t2的内容
function modifyText(new_text){
    var t2 = document.getElementById("t2");
    t2.firstChild.nodeValue = new_text;
}

// 添加事件Listener给表格
var el = document.getElementById("outside");
el.addEventListener("click", function(){modifyText("four")},false);
```

##说明
###为什么要用addEventListener?
*addEventListener*是一种注册W3C DOM事件的listener的方法.使用它有这些好处:

* 它允许为同一事件添加多个handler.这在DHTML库或者Mozilla extensions这些需要和其他库或者扩展一起配合使用的情形下非常有用.
* 它提供给你一种细粒度(finer-grained)控制方法,让你在listener被触发时掌控一切.
* 它可以用于任何DOM元素,而不仅仅是HTML元素.

接下来介绍一种可选的,旧事件listener注册方法[older way to register event listener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Older_way_to_register_event_listeners).

###在事件分发时添加listener
如果一个EventListener被添加给一个正处于处理过程中的EventTarget,那么这个事件将不会被触发.然而,同样的listener可能会在一些更迟的事件流中被触发,例如冒泡过程.

###多个完全相同的事件listener
如果多个参数完全相同的EventListener被注册给相同的事件目标,那么多余的实例就会被丢弃.它们并不会被多次调用,所以不必手动调用removeEventListener.

###事件handler中的this值是什么
它一般是触发了事件的元素的引用,比如使用通常handler来处理一些相同元素集.

当使用addEventListener()给元素附加handler时,handler内的this值则是该元素的引用.它和传递给handler的参数中的event对象的currentTarget属性值相同.

如果在Html源中指定了一个事件属性(比如onclick)给一个元素,属性值中的javaScript代码会被有效的包裹在一个handler函数中,这个handler函数会利用addEventListener()以一种习惯的方式绑定this值;an occurrence of this within the code represents a reference to the element. Note that the value of this inside a function called by the code in the attribute value behaves as per standard rules. Therefore, given the following example:

```
<table id="t" onclick="modifyText();">
...
```

像这种通过onclick事件调用的函数modifyText()里的值为全局对象(window)或者在严格模式下为undefined.

> Note:JavaScript 1.8.5介绍了使用*Function.prototype.bind()*方法来在一个函数的所有调用中使用一个指定的this值.当你不清楚函数调用时上下文将会如何影响this值时,利用这种方法可以简单地绕过这个问题.但是请注意,你需要维护一个listener的引用在稍后移除这个给this的bind.

这是一个没有使用bind的例子:

```
var Something = function(element) {
    // |this|是一个新创建的object
    this.name = 'Something Good';
    this.onclick1 = function(event) {
        console.log(this.name); //未定义,因为this是这个元素
    };
    this.onclick2 = function(event){
        console.log(this.name);//'Something Good',因为this绑定给了心创建的对象
    element.addEventListener('click',this.onclick1,false);
    element.addEventListener('click',this.onclick2.bind(this),false);
}
var s = new Something(document.body);
```

上面的例子有一个问题,就是你不能移除使用了bind的listener.另一个解决方案是使用一个叫做handleEvent的指定函数来捕获所有的事件:

```
var Something = function(element) {
// |this|是一个新创建的对象
this.name = 'Something Good';
this.handleEvent = function(event) {
    console.log(this.name);//'Something Good',因为this绑定为新创建的对象
        switch(event.type){
        case 'click':
        // 一些代码
        break;
        case 'dblclick':
        // 一些代码
        break;
    }
};
    
// 注意在这种情况下,this是|this|,而不是this.handleEvent
element.addEventListener('click',this,false);
element.addEventListener('dblclick',this,false);

// 你可以正确地移除listeners
element.removeEventListener('click',this,false);
element.removeEventListener('dblclick',this,false);
}
var s = new Something(document.body);
```
### 老古董IE和attachEvent
在IE9之前的IE版本中,你必须使用[attachEvent](http://msdn.microsoft.com/en-us/library/ms536343(VS.85).aspx)来替代addEventListener.修改一下之前的例子:

```
if(el.addEventListener){
    el.addEventListener('click',modifyText,false);
}else if{el.attachEvent){
    el.attachEvent('onclick',modifyText);
}
```

使用attachEvent有个缺点,就是this的值是window对象的引用,而不是被触发的元素.

###兼容性
You can work around the addEventListener, removeEventListener, Event.preventDefault and Event.stopPropagation not being supported by IE 8 using the following code at the beginning of your script. The code supports the use of handleEvent and also the DOMContentLoaded event.
> Note: useCapture is not supported, as IE 8 does not have any alternative method of it. Please also note that the following code only adds support to IE 8.

> Also note that this IE8 polyfill ONLY works in standards mode: a doctype declaration is required.

```
(function() {
  if (!Event.prototype.preventDefault) {
    Event.prototype.preventDefault=function() {
      this.returnValue=false;
    };
  }
  if (!Event.prototype.stopPropagation) {
    Event.prototype.stopPropagation=function() {
      this.cancelBubble=true;
    };
  }
  if (!Element.prototype.addEventListener) {
    var eventListeners=[];
    
    var addEventListener=function(type,listener /*, useCapture (will be ignored) */) {
      var self=this;
      var wrapper=function(e) {
        e.target=e.srcElement;
        e.currentTarget=self;
        if (typeof listener.handleEvent != 'undefined') {
          listener.handleEvent(e);
        } else {
          listener.call(self,e);
        }
      };
      if (type=="DOMContentLoaded") {
        var wrapper2=function(e) {
          if (document.readyState=="complete") {
            wrapper(e);
          }
        };
        document.attachEvent("onreadystatechange",wrapper2);
        eventListeners.push({object:this,type:type,listener:listener,wrapper:wrapper2});
        
        if (document.readyState=="complete") {
          var e=new Event();
          e.srcElement=window;
          wrapper2(e);
        }
      } else {
        this.attachEvent("on"+type,wrapper);
        eventListeners.push({object:this,type:type,listener:listener,wrapper:wrapper});
      }
    };
    var removeEventListener=function(type,listener /*, useCapture (will be ignored) */) {
      var counter=0;
      while (counter<eventListeners.length) {
        var eventListener=eventListeners[counter];
        if (eventListener.object==this && eventListener.type==type && eventListener.listener==listener) {
          if (type=="DOMContentLoaded") {
            this.detachEvent("onreadystatechange",eventListener.wrapper);
          } else {
            this.detachEvent("on"+type,eventListener.wrapper);
          }
          eventListeners.splice(counter, 1);
          break;
        }
        ++counter;
      }
    };
    Element.prototype.addEventListener=addEventListener;
    Element.prototype.removeEventListener=removeEventListener;
    if (HTMLDocument) {
      HTMLDocument.prototype.addEventListener=addEventListener;
      HTMLDocument.prototype.removeEventListener=removeEventListener;
    }
    if (Window) {
      Window.prototype.addEventListener=addEventListener;
      Window.prototype.removeEventListener=removeEventListener;
    }
  }
})();
```
###Older way to register event listeners
addEventListener() was introduced with the DOM 2 Events specification. Before then, event listeners were registered as follows:
```
// Pass a function reference — do not add '()' after it, which would call the function!
el.onclick = modifyText;

// Using a function expression
element.onclick = function() {
  // ... function logic ...
};
```
This method replaces the existing click event listener(s) on the element if there are any. Similarly for other events and associated event handlers such as blur (onblur), keypress (onkeypress), and so on.

Because it was essentially part of DOM 0, this method is very widely supported and requires no special cross–browser code; hence it is normally used to register event listeners dynamically unless the extra features of addEventListener() are needed.
###内存问题
```
var i;
var els = document.getElementsByTagName('*')

//Case 1
for(i = 0; i < els.length; i++){
    els[i].addEventListener("click",function(e){/*do something*/},false);
    }
//Case 2
function processEvent(e){
    /*do something*/
}

for(i=0l; i<els.length;i++){
    els[i].addEventListener("click",processEvent,false);
}    
```
在case 1中,每个循环都创建了一个新的(匿名)函数.在case 2中,所有事件处理都是用的一个事先定义好的函数.结果是两者耗费的内存资源相同.此外,在第一个例子中,因为没有保留匿名函数的引用,所以不能调用element.removeEventListener.然而第二个例子的中,你就可以写上myElement.removeEventListener('click',processEvent,false).

###使用被动listeners来提升滚动性能
```
var elem = document.getElementById('elem');
elem.addEventListener('touchmove',function listener(){
    /*do something */
},{passive: true});
```

这样一来touchmove listener就不会在用户滑动页面(同样适用鼠标滚轮事件)时被阻塞.[一个Demo](https://developers.google.com/web/updates/2016/06/passive-event-listeners)(Google开发者页面).


