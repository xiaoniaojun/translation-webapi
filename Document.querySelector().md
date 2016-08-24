# Document.querySelector()
找到在所有指定群组的selector中匹配到的第一个元素，并返回它。（使用深度优先算法遍历decument的节点|从document建立的第一个元素开始，迭代完其所有的子节点，再继续下一个节点）。
## Syntax
```
element = document.querySelector(selectors);
```
其中

*元素是一个element对象。
*selectors是一个包含一个或多个CSS selectors的字符串，用逗号分隔。
##举个例子

举个例子，返回document中的第一个具有“myclass”类的元素。

```
var el = document.querySelector(".myclass");
```
## 举个更强大的例子
看下面的演示！selectors可以写得很强势！这里，返回的是document里在`<div class="user-panel main">`中的第一个元素`<input name="login"/>`:

```
var el = document.querySelector("div.user-panel.main input[name=login]");
```
## 说明
如果没有找到元素，则返回`null`，否则返回第一个匹配到的元素。

如果selector匹配到了一个ID，并且这个ID在document中被多次错误地使用，返回的是第一个匹配到的元素。

如果指定的selectors是无效的，浏览器会不想理你并且抛出一个`SYNTAX_ERR`异常给你。

传递给querySelector的字符串参数必须遵循CSS语法。

要想匹配ID或者不符合CSS语法的selectors，你必须用反斜杠`\`来转码。反斜杠在javascript中是一个转义字符，因此如果你输入一个literal string，那就必须用双斜杠来`\\`表示它（一个是对针对js，一个是针对querySelector）:

```
<div id="foo\bar"></div>
<div id="foo:bar"></div>

<script>
    <div id="foo\bar"></div>
<div id="foo:bar"></div>

<script>
  console.log('#foo\bar')               // "#fooar"
  document.querySelector('#foo\bar')    // Does not match anything

  console.log('#foo\\bar')              // "#foo\bar"
  console.log('#foo\\\\bar')            // "#foo\\bar"
  document.querySelector('#foo\\\\bar') // Match the first div

  document.querySelector('#foo:bar')    // Does not match anything
  document.querySelector('#foo\\:bar')  // Match the second div
</script>
```
