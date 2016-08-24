# Element.classList

**Element.classList**是一个只读属性，返回一个live DOMToKenList，它是元素的类属性的一个集合。

使用*classList*来访问元素的类列表，也可以选择通过*element.classNmae*，用空格分割的字符串来做这件事。

## 语法

```
`var elementClasses = elementNodeReference.classList;
```
`*elementClasses*是一个DOMTokenList，表示*elementNodeReference*的类属性。如果类属性还没有被设置，或者为空，则*elementClasses.length*返回0。*element.classList*本身是只读的，然而你还是可以通过*add()*或*remove()*方法来修改它。
## 方法
add(String[,String]())
添加一个指定的类。如果这些类已经存在于element中，那么它们就会被忽略。

remove(String[,String]())
移除指定的类。

item(Number)
通过索引值从集合中检出一个类。

toggle(String[,force]())
当只有一个参数时：切换这个类；举个栗子，如果类存在则移除它并且返回false，反之，添加它并返回true。
如果有两个参数时：如果第二个参数为true，则添加指定的类，否则移除。

contains(String)
检查指定类是否存在于元素的类属性当中。

# 栗子
```
`// div是一个\<div\>元素的引用，并且带有class="foo bar"
div.classList.remove("foo");
div.classList.remove("anotherclass");

// 如果visible存在则移除，否则添加
div.classList.toggle("visible");

// 添加/移除visible,这取决于测试条件，i小于10
div.classList.toggle("visible",i \< 0);

alert(div.classList.contains("foo"));

div classList.add("foo","bar"); //一次添加多个类
````

