### 1.说明

BeautifulSoup 就是 Python 的一个 HTML 或 XML 的解析库，我们可以用它来方便地从网页中提取数据

### 2. 解析器 {#3-解析器}

BeautifulSoup 支持的解析器及优缺点

| 解析器 | 使用方法 | 优势 | 劣势 |
| :--- | :--- | :--- | :--- |
| Python标准库 | BeautifulSoup\(markup, "html.parser"\) | Python的内置标准库、执行速度适中 、文档容错能力强 | Python 2.7.3 or 3.2.2\)前的版本中文容错能力差 |
| LXML HTML 解析器 | BeautifulSoup\(markup, "lxml"\) | 速度快、文档容错能力强 | 需要安装C语言库 |
| LXML XML 解析器 | BeautifulSoup\(markup, "xml"\) | 速度快、唯一支持XML的解析器 | 需要安装C语言库 |
| html5lib | BeautifulSoup\(markup, "html5lib"\) | 最好的容错性、以浏览器的方式解析文档、生成 HTML5 格式的文档 | 速度慢、不依赖外部扩展 |

### 3.基本使用

实例:

```
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.prettify())
print(soup.title.string)
```

运行结果:

```
<html>
 <head>
  <title>
   The Dormouse's story
  </title>
 </head>
 <body>
  <p class="title" name="dromouse">
   <b>
    The Dormouse's story
   </b>
  </p>
  <p class="story">
   Once upon a time there were three little sisters; and their names were
   <a class="sister" href="http://example.com/elsie" id="link1">
    <!-- Elsie -->
   </a>
   ,
   <a class="sister" href="http://example.com/lacie" id="link2">
    Lacie
   </a>
   and
   <a class="sister" href="http://example.com/tillie" id="link3">
    Tillie
   </a>
   ;
and they lived at the bottom of a well.
  </p>
  <p class="story">
   ...
  </p>
 </body>
</html>
The Dormouse's story
```

* prettify\(\):把要解析的字符串以标准的缩进格式输出
* soup.title.string:选择HTML中的title节点，再调用string属性得到里面的文本

### 4. 节点选择器 {#5-节点选择器}

#### 选择元素

例子:

```
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.title)
print(type(soup.title))
print(soup.title.string)
print(soup.head)
# 只能选择第一个p节点
print(soup.p)
```

运行结果：

```
<title>The Dormouse's story</title>
<class 'bs4.element.Tag'>
The Dormouse's story
<head><title>The Dormouse's story</title></head>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
```

#### 提取信息 {#提取信息}

##### 获取名称 {#获取名称}

可以利用 name 属性来获取节点的名称

实例:选择title，调用name属性得到节点的名称

```
print(soup.title.name)
```

运行结果:

```
title
```

##### 获取属性 {#获取属性}

调用 attrs 获取所有属性

```
# 返回字典
print(soup.p.attrs)
print(soup.p.attrs['name'])
```

运行结果:

```
{'class': ['title'], 'name': 'dromouse'}
dromouse
```

##### 获取内容 {#获取内容}

利用 string 属性获取节点元素包含的文本内容

```
print(soup.p.string)
```

运行结果:

```
The Dormouse's story
```

#### 嵌套选择 {#嵌套选择}

实例：获取head节点内部的title节点

```
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.head.title)
print(type(soup.head.title))
print(soup.head.title.string)
```

运行结果:

```
<title>The Dormouse's story</title>
<class 'bs4.element.Tag'>
The Dormouse's story
```

#### 关联选择 {#关联选择}

##### 子节点和子孙节点 {#子节点和子孙节点}

获取直接子节点可以调用 contents 属性

实例:获取body节点下子节点p

```
html = """
<html>
    <head>
        <title>The Dormouse's story</title>
    </head>
    <body>
        <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a href="http://example.com/elsie" class="sister" id="link1">
                <span>Elsie</span>
            </a>
            <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> 
            and
            <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
        <p class="story">...</p>
"""

from bs4 import  BeautifulSoup

soup = BeautifulSoup(html,'lxml')
print(soup.body.contents)
```

运行结果:返回结果是列表形式

```
['\n', <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
            and
            <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>, '\n', <p class="story">...</p>, '\n']
```

可以调用 children 属性，得到相应的结果:

```
from bs4 import BeautifulSoup

soup = BeautifulSoup(html,'lxml')
print(soup.body.children)
for i,child in enumerate(soup.body.children):
    print(i,child)
```

运行结果:

```
<list_iterator object at 0x00000217D33CD048>
0 

1 <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
            and
            <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
2 

3 <p class="story">...</p>
4
```

要得到所有的子孙节点的话可以调用 descendants 属性

```
from bs4 import BeautifulSoup

soup = BeautifulSoup(html,'lxml')
print(soup.body.descendants)
for i,child in enumerate(soup.body.descendants):
    print(i,child)
```

运行结果:

```
<generator object descendants at 0x0000014D106353B8>
0 

1 <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
            and
            <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
2 
            Once upon a time there were three little sisters; and their names were

3 <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
4 

5 <span>Elsie</span>
6 Elsie
7 

8 

9 <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
10 Lacie
11  
            and

12 <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
13 Tillie
14 
            and they lived at the bottom of a well.

15 

16 <p class="story">...</p>
17 ...
18
```

##### 父节点和祖先节点 {#父节点和祖先节点}

要获取某个节点元素的父节点，可以调用 parent 属性：

实例:获取节点a的父节点p下的内容

```
html = """
<html>
    <head>
        <title>The Dormouse's story</title>
    </head>
    <body>
        <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a href="http://example.com/elsie" class="sister" id="link1">
                <span>Elsie</span>
            </a>
        </p>
        <p class="story">...</p>
"""

from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(soup.a.parent)
```

运行结果:

```
<p class="story">
            Once upon a time there were three little sisters; and their names were
            <a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
</p>
```

要想获取所有的祖先节点，可以调用 parents 属性

```
html = """
<html>
    <body>
        <p class="story">
            <a href="http://example.com/elsie" class="sister" id="link1">
                <span>Elsie</span>
            </a>
        </p>
"""

from bs4 import BeautifulSoup
soup = BeautifulSoup(html,'lxml')
print(type(soup.a.parents))
print(list(enumerate(soup.a.parents)))
```

运行结果:

```
<class 'generator'>
[(0, <p class="story">
<a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
</p>), (1, <body>
<p class="story">
<a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
</p>
</body>), (2, <html>
<body>
<p class="story">
<a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
</p>
</body></html>), (3, <html>
<body>
<p class="story">
<a class="sister" href="http://example.com/elsie" id="link1">
<span>Elsie</span>
</a>
</p>
</body></html>)]
```

##### 兄弟节点 {#兄弟节点}

* next\_sibling :获取节点的下一个兄弟节点
*  previous\_sibling:获取节点上一个兄弟节点
* next\_siblings :返回所有前面兄弟节点的生成器
* previous\_siblings :返回所有后面的兄弟节点的生成器

实例:

```
html = """
<html>
    <body>
        <p class="story">
            Once upon a time there were three little sisters; and their names were
            <a href="http://example.com/elsie" class="sister" id="link1">
                <span>Elsie</span>
            </a>
            Hello
            <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> 
            and
            <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>
            and they lived at the bottom of a well.
        </p>
"""

from bs4 import BeautifulSoup

soup = BeautifulSoup(html,'lxml')
print('next sibling:',soup.a.next_sibling)
print('previous sibling:',soup.a.previous_sibling)
print("next siblings:",list(soup.a.next_siblings))
print("previouos siblings:",list(soup.a.previous_siblings))

```

运行结果:

```
next sibling: 
            Hello
            
previous sibling: 
            Once upon a time there were three little sisters; and their names were
            
next siblings: ['\n            Hello\n            ', <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, ' \n            and\n            ', <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>, '\n            and they lived at the bottom of a well.\n        ']
previouos siblings: ['\n            Once upon a time there were three little sisters; and their names were\n            ']

```


