# pointer-events
##概述
CSS属性*point-events*可以让创建者控制在什么情况下一个图像元素可以触发鼠标事件,或者说成为一个可以触发鼠标事件的[目标(target)](https://developer.mozilla.org/en-US/docs/Web/API/event.target).如果不指定这个属性,the same characteristics of the visiblePainted value apply to SVG content.
此外,指定元素为非鼠标事件目标,那么将属性设置为none则命令鼠标"穿透"该元素,这种情况下,在此元素"下面"的元素将成为触发对象.

| 初始值 | auto |
| --- | --- |
| 适用于 | 所有元素  |
| 可继承 | 是  |
| 媒体 | 视觉的  |
| 计算值 | 指定的 |
| 可执行动画 | 不可 |
| 规范化顺序 |  |
##语法
```
/* 关键字值 */
pointer-events: auto;
pointer-events: none;
pointer-events: visiblePainted;
pointer-events: visibleFill;
pointer-events: visibleStroke;
pointer-events: visible;
pointer-events: painted;
pointer-events: fill;
pointer-events: stroke;
pointer-events: all;

/* 全局值 */
pointer-events: inherit;
pointer-events: initial;
pointer-events: unset;
```
###属性值
**auto**
元素的行为将会和没有设置这个属性时一样.在SVG content,这个值以及visiblePainted值的效果一样.

**none**
元素将永远不会[target](https://developer.mozilla.org/en-US/docs/Web/API/event.target)(触发)鼠标事件;然而,鼠标事件会target它的descendant(子,下层)元素,如果这些元素设置了除none以外的某些值.在这种情况下,如果子元素存在冒泡或者捕捉过程,那么鼠标事件将会视情况触发这个父元素的事件listeners(监听器).

**还有一大堆SVG适用的属性**
并不想翻译!
## 说明
注意使用pointer-event让一个元素不成为鼠标事件的target并不意味着它必然不能或不会被触发.如果一个元素的子元素明确地设置了pointer-events值为允许子元素成为鼠标事件触发目标,则任何触发了子元素的事件将会传递经过父元素(因为事件会沿着继承链传递),并且视情况触发父元素的事件listener.当然,在屏幕上由父元素覆盖而没有子元素在下层的部分上的鼠标事件将不会触发任何东西(它会穿过父元素到下方的任何元素上去).

我们有时候会希望在Html里实现更好的粒度控制,比如在什么时候什么地方的元素会捕捉到事件.To help us in deciding how pointer-events should be further extended for HTML, if you have any particular things that you would like to be able to do with this property, then please add them to the Use Cases section of this wiki page (don't worry about keeping it tidy).


