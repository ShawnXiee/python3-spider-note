### 1.说明

XPath，全称 XML Path Language，即 XML 路径语言，它是一门在XML文档中查找信息的语言。XPath 最初设计是用来搜寻XML文档的，但是它同样适用于 HTML 文档的搜索

### 2.相关链接

官方文档:[https://www.w3.org/TR/xpath/](https://www.w3.org/TR/xpath/)

### 3.XPath常用规则 {#2-xpath常用规则}

| 表达式 | 描述 |
| :--- | :--- |
| nodename | 选取此节点的所有子节点 |
| / | 从当前节点选取直接子节点 |
| // | 从当前节点选取子孙节点 |
| . | 选取当前节点 |
| .. | 选取当前节点的父节点 |
| @ | 选取属性 |

### 4. 实例引入 {#4-实例引入}

```
from lxml import etree

content = '''
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a>
     </ul>
 </div>
'''
# 调用etree模块的HTML类构造一个XPath解析对象
html = etree.HTML(content)
result = etree.tostring(html)
print(result.decode('utf-8'))
```

HTML 文本中的最后一个 li 节点是没有闭合的，但是 etree 模块可以对 HTML 文本进行自动修正

运行结果:

```
<html><body><div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a>
     </li></ul>
 </div>
</body></html>
```

可以直接读取文本进行解析

```
from lxml import etree

html = etree.parse('test.html',etree.HTMLParser())
result = etree.tostring(html)
print(result.decode('utf-8'))
```

test.html内容

```
<div>
    <ul>
         <li class="item-0"><a href="link1.html">first item</a></li>
         <li class="item-1"><a href="link2.html">second item</a></li>
         <li class="item-inactive"><a href="link3.html">third item</a></li>
         <li class="item-1"><a href="link4.html">fourth item</a></li>
         <li class="item-0"><a href="link5.html">fifth item</a>
     </ul>
 </div>
```

解析后结果:

```
E:\Python36\python.exe E:/JetBrains/code_project/xpath_demo/test2.py
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN" "http://www.w3.org/TR/REC-html40/loose.dtd">
<html><body><div>&#13;
    <ul>&#13;
         <li class="item-0"><a href="link1.html">first item</a></li>&#13;
         <li class="item-1"><a href="link2.html">second item</a></li>&#13;
         <li class="item-inactive"><a href="link3.html">third item</a></li>&#13;
         <li class="item-1"><a href="link4.html">fourth item</a></li>&#13;
         <li class="item-0"><a href="link5.html">fifth item</a>&#13;
     </li></ul>&#13;
 </div></body></html>
```


