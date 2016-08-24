# Element.classList

**Element.classList**是一个只读属性，返回一个live DOMToKenList，它是元素的类属性的一个集合。

使用*classList*来访问元素的类列表，也可以选择通过*element.classNmae*，用空格分割的字符串来做这件事。

##语法

```
var elementClasses = elementNodeReference.classList;
```
*elementClasses*是一个DOMTokenList，表示*elementNodeReference*的类属性。如果类属性还没有被设置，或者为空，则*elementClasses.length*返回0。*element.classList*本身是只读的，然而你还是可以通过*add()*或*remove()*方法来修改它。
##方法
add(String[,String])
添加一个指定的类。如果这些类已经存在于element中，那么它们就会被忽略。

remove(String[,String])
移除指定的类。

item(Number)
返
