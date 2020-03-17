[TOC]

### Regular Expression

![re](https://img-blog.csdn.net/20140929200042391?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGlzb25nbGlzb25nbGlzb25n/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

1. re.S,re.M,re.I

```python
# re.M多行匹配
>>> s= '12 34/n56 78/n90'
 
>>> re.findall( r'^/d+' , s , re.M )          # 匹配位于行首的数字
 
['12', '56', '90']
 
>>> re.findall( r’/A/d+’, s , re.M )        # 匹配位于字符串开头的数字
 
['12']
 
>>> re.findall( r'/d+$' , s , re.M )          # 匹配位于行尾的数字
 
['34', '78', '90']
 
>>> re.findall( r’/d+/Z’ , s , re.M )        # 匹配位于字符串尾的数字
 
['90']

# re.I忽略大小写
>>> res = re.findall(r"A", "abc", re.I)
>>> print(res)
 
['a']

# re.S表示 “.” 的作用扩展到整个字符串，包括“\n”
import re
a = '''asdfhellopass:
    worldaf
    '''
b = re.findall('hello(.*?)world',a)
c = re.findall('hello(.*?)world',a,re.S)
print 'b is ' , b
print 'c is ' , c

运行结果：
b is  []
c is  ['pass:\n\t123\n\t']
```
> 正则表达时前面加r的原因是：一般字符会将`"ad\ncd"`中反斜杠转义为换行符，但若在其前面加上`r`，其不会转义，会将其看为普通字符，故正则表达时前面加上`r`，避免其被字符串转义，而让其被正则转义。

2. complile
re 模块提供了 re.compile() 函数将一个字符串编译成 pattern object，用于匹配或搜索。函数原型如下：`re.compile(pattern, flags=0)`,ex:`p = re.compile('ab*', re.I|re.M)`

3. re.match
`regex.match(string[, pos[, endpos]])：`
* 匹配从 pos 到 endpos 的字符子串的开头。匹配成功返回一个 match object，不匹配返回 None。
* pos 的默认值是0，endpos 的默认值是 len(string)，所以默认情况下是匹配整个字符串的开头。
```python
import re
pattern = re.compile("ar")
print(pattern.match("army"))     # "ar"在开头，匹配成功
print(pattern.match("mary"))     # "ar"不在开头，匹配失败
print(pattern.match("mary", 1))  # "ar"不在开头，但在子串的开头
 
# 输出结果：
# <_sre.SRE_Match object; span=(0, 2), match='ar'>
# None
# <_sre.SRE_Match object; span=(1, 3), match='ar'>
```
`match.group([group1, ...])：`

* 返回 match object 中的字符串。
* 每一个 ( ) 都是一个分组，分组编号从1开始，从左往右，每遇到一个左括号，分组编号+1。
* 组 0 总是存在的，它就是整个表达式 。
* 没有参数时，group1默认为0，这时返回整个匹配到的字符串。
* 指定一个参数（整数）时，返回该分组匹配到的字符串。
* 指定多个参数时，返回由那几个分组匹配到的字符串组成的 tuple。
```python
pattern = re.compile(r"(\w+) (\w+)")
m = pattern.match("Kobe Bryant, Lakers")
print(m)               # <_sre.SRE_Match object; span=(0, 11), match='Kobe Bryant'>
print(m.group())       # Kobe Bryant
print(m.group(1))      # Kobe
print(m.group(2))      # Bryant
print(m.group(1, 2))   # ('Kobe', 'Bryant')
```
`match.groups()`：返回由所有分组匹配到的字符串组成的 tuple。
```python
>>> m = re.match(r"(\d+)\.(\d+)", "24.1632")
>>> m.groups()
('24', '1632')
```

4. re.search
`regex.search(string[, pos[, endpos]])`：

* 扫描整个字符串，并返回它找到的第一个匹配（Match object）。
* 和 regex.match() 一样，可以通过 pos 和 endpos 指定范围。
```python
pattern = re.compile("ar{1}")
match = pattern.search("mary")   # search
print(match)
 
# 输出结果：
# <_sre.SRE_Match object; span=(1, 3), match='ar'>
```
> 返回匹配结果与match相同，都是用group()来返回

5. re.findall
`regex.findall(string[, pos[, endpos]])`：

* 找到所有匹配的子串，并返回一个 list 。
* 可选参数 pos 和 endpos 和上面一样。

```python
pattern = re.compile(r"\d+")
p = pattern.finditer("abc1def2rst3xyz") 
for i in p:
    print(i)
 
# 输出结果：
# <_sre.SRE_Match object; span=(3, 4), match='1'>
# <_sre.SRE_Match object; span=(7, 8), match='2'>
# <_sre.SRE_Match object; span=(11, 12), match='3'>
```

6. re.finditer

`regex.finditer(string[, pos[, endpos]])`：

* 找到所有匹配的子串，并返回由这些匹配结果（match object）组成的迭代器。
* 可选参数 pos 和 endpos 和上面一样。
```python
pattern = re.compile(r"\d+")
p = pattern.finditer("abc1def2rst3xyz") 
for i in p:
    print(i)
 
# 输出结果：
# <_sre.SRE_Match object; span=(3, 4), match='1'>
# <_sre.SRE_Match object; span=(7, 8), match='2'>
# <_sre.SRE_Match object; span=(11, 12), match='3'>
```

7. re.split
split()函数在匹配的地方将字符串分割，并返回一个 list。同样的，re 模块提供了两种 split 函数，一个是 pattern object 的方法，一个是模块级的函数。
regex.split(string, maxsplit=0)：

* maxsplit用于指定最大分割次数，不指定将全部分割。
`re.split(pattern, string, maxsplit=0, flags=0)`：
```python
pattern = re.compile(r"[A-Z]+")
m = pattern.split("abcDefgHijkLmnoPqrs")
print(m)

# 输出结果：
# ['abc', 'efg', 'ijk', 'mno', 'qrs']
```

* 模块级函数，功能与 regex.split() 相同。
* flags用于指定匹配模式。
```python
m = re.split(r"[A-Z]+","abcDefgHijkLmnoPqrs")
print(m)
 
# 输出结果：
# ['abc', 'efg', 'ijk', 'mno', 'qrs']
```

8. re.sub
`regex.sub(repl, string, count=0)`：

* 使用 repl 替换 string 中每一个匹配的子串，返回替换后的字符串。若找不到匹配，则返回原字符串。
* repl 可以是一个字符串，也可以是一个函数。
* 当repl是一个字符串时，任何在其中的反斜杠都会被处理。
* 当repl是一个函数时，这个函数应当只接受一个参数（Match对象），并返回一个字符串用于替换。
* count 用于指定最多替换次数，不指定时全部替换。
```python
def fun(m):
    return m.group().upper()
 
pattern = re.compile(r"like", re.I)
s1 = pattern.sub(r"love", "I like you, do you like me?")
s2 = pattern.sub(fun, "I like you, do you like me?")
print(s1)
print(s2)
 
# 输出结果：
# I love you, do you love me?
# I LIKE you, do you LIKE me?
```

---
### LXML

```python
 1 #!/usr/bin/python3
  2 import requests
  3 from lxml import etree
  4 url="https://news.baidu.com/"
  5 headers={"User-agent":"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36"}
  6 data=requests.get(url,headers=headers).content.decode()
  7 html=etree.HTML(data)
  8 result=html.xpath("/html/head/title/text()")
  9 result=html.xpath('//a[@mon="ct=1&a=1&c=top&pn=0"]/text()')
 10 result=html.xpath('//a[@mon="ct=1&a=1&c=top&pn=0"]/@href')
 11 print(result)

```

|路径表达式|结果|
|--------|----|
|bookstore	|选取 bookstore 元素的所有子节点。
|/bookstore	| 选取根元素 bookstore。注释：假如路径起始于正斜杠( / ),则此路径始终代表到某元素的绝对路径！|
|bookstore/book|选取属于 bookstore 的子元素的所有 book 元素。|
|//book|选取所有 book 子元素，而不管它们在文档中的位置。|
|bookstore//book	|选择属于 bookstore 元素的后代的所有 book 元素，而不管它们位于 bookstore 之下的什么位置。|
|//@lang	|选取名为 lang 的所有属性。|
|/bookstore/book[1]	|选取属于 bookstore 子元素的第一个 book 元素。|
|/bookstore/book[last()]|	选取属于 bookstore 子元素的最后一个 book 元素。|
|/bookstore/book[last()-1]	|选取属于 bookstore 子元素的倒数第二个 book 元素。|
|/bookstore/book[position() < 3]	|选取最前面的两个属于 bookstore 元素的子元素的 book 元素。|
|//title[@lang]	|选取所有拥有名为 lang 的属性的 title 元素。|
|//title[@lang='eng']	|选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性。|
|/bookstore/book[price>35.00]	|选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00。|
|/bookstore/book[price>35.00]/title|	选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。|


```python
  1 #!/usr/bin/python3
  2 from lxml import etree
  3 import requests
  4 import json
  5 
  6 class ForumSpider(object):
  7         def __init__ (self):
  8                 self.baseurl="https://daily.zhihu.com"
  9                 self.headers={"User-agent":"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36"}
 10         def request_data(self,url):
 11                 response=requests.get(url,headers=self.headers)
 12                 return response.content.decode()
 13         def parse_data(self,data):
 14                 html=etree.HTML(data)
 15                 title_list=html.xpath('//span[@class="title"]/text()')
 16                 url_list=html.xpath('//a[@class="link-button"]/@href')
 17                 data_list=[]
 18                 for index,item in enumerate(title_list):
 19                         dic={}
 20                         dic["name"]=item
 21                         dic["url"]=self.baseurl+url_list[index]
 22                         data_list.append(dic)
 23                 return data_list
 24         def save_data(self,data):
 25                 js_data=json.dumps(data)
 26                 with open("1.json","w",encoding="utf-8") as f:
 27                         f.write(js_data)
 28         def run(self):
 29                 url=self.baseurl
 30                 data=self.request_data(url)
 31                 data_list=self.parse_data(data)
 32                 self.save_data(data_list)
 33 
 34 case=ForumSpider()
 35 case.run()

```

> `//div[contains(@id,"normalthread")]`,xpath中的模糊查询，寻找id属性含有normalthread字段的div节点。

---
### bs4

bs4 能自动补全缺失的html

```python
#!/usr/bin/python3

import bs4 

html_doc = """ 
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""

soup=bs4.BeautifulSoup(html_doc,"lxml")

# 几个简单的浏览结构化数据的方法:
soup.title
# <title>The Dormouse's story</title>

soup.title.name
# u'title'

soup.title.string
# u'The Dormouse's story'

soup.title.parent.name
# u'head'

soup.p
# <p class="title"><b>The Dormouse's story</b></p>

soup.p['class']
# u'title'

soup.a
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

soup.find_all('a')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.find(id="link3")
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

# 从文档中找到所有<a>标签的链接:

for link in soup.find_all('a'):
    print(link.get('href'))
    # http://example.com/elsie
    # http://example.com/lacie
    # http://example.com/tillie

# 从文档中获取所有文字内容:

print(soup.get_text())
# The Dormouse's story
#
# The Dormouse's story
#
# Once upon a time there were three little sisters; and their names were
# Elsie,
# Lacie and
# Tillie;
# and they lived at the bottom of a well.
#
# ...
```

> `soup.p`为第一个p标签

bs4中的四个对象BeautifulSoup,Tag,NavigableString,Comment

```python
  1 #!/usr/bin/python3
  2 import bs4
  3 
  4 html_doc = """
  5 <html><head><title>The Dormouse's story</title></head>
  6 <body>
  7 <p class="story"><!--Hey, buddy. Want to buy a used parser?--></p>
  8 
  9 <p class="title"><b>The Dormouse's story</b></p>
 10 
 11 <p class="story">Once upon a time there were three little sisters; and their names were
 12 <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
 13 <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
 14 <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
 15 and they lived at the bottom of a well.</p>
 16 
 17 """
 18 soup=bs4.BeautifulSoup(html_doc,"lxml")
 19 print(type(soup))
 20 print(type(soup.a))
 21 print(type(soup.a.string))
 22 print(type(soup.p.string))
# <class 'bs4.BeautifulSoup'>
# <class 'bs4.element.Tag'>
# <class 'bs4.element.NavigableString'>
# <class 'bs4.element.Comment'>

```

常用函数find,find_all,select_one,select

```python
#!/usr/bin/python3
import bs4

html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="story"><!--Hey, buddy. Want to buy a used parser?--></p>

<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

"""
# find查找符合条件的第一个标签
soup = bs4.BeautifulSoup(html_doc)
soup.find(name="p",attrs={"class":"story"})
# <p class="story"><!--Hey, buddy. Want to buy a used parser?--></p>
# find_all 查找符合条件的所有标签，返回列表
soup.find_all(name="a",limit=2)
soup.find_all(attrs={"class":"sister"})
# select_one  css selector
soup.select_one(".sister")
# select
soup.select(".sister",limit=2)
```

**css3 选择器**

|选择器	|例子	|例子描述	|CSS|
|------|------|----------|---|
|.class|.intro|	选择 class="intro" 的所有元素|1|
|#id|#firstname选择 id="firstname" 的所有元素|1|
|* | * | 选择所有元素|2|
|element|p|选择所有 < p > 元素|1|
|element,element|div,p|选择所有 < div > 元素和所有 < p > 元素|1|
|element element|div p|	选择 < div > 元素内部的所有 < p > 元素|1|
|element>element|div>p|选择父元素为 < div > 元素的所有 < p > 元素|2|
|element+element|div+p|选择紧接在 < div > 元素之后的所有 < p > 元素|2|
|[attribute]|[target]|选择带有 target 属性所有元素|2|
|[attribute=value]|[target=_blank]|选择 target="_blank" 的所有元素|2|
|[attribute~=value]|[title~=flower]|选择 title 属性包含单词 "flower" 的所有元素|2|
|[attribute|=value]|[lang|=en]|选择 lang 属性值以 "en" 开头的所有元素|2|
|:link|a:link|选择所有未被访问的链接|1|
|:visited|a:visited|选择所有已被访问的链接|1|
|:active|a:active|选择活动链接|1|
|:hover|a:hover|选择鼠标指针位于其上的链接|1|
|:focus|input:focus|选择获得焦点的 input 元素|2|
|:first-letter|p:first-letter|选择每个 < p > 元素的首字母|1|
|:first-line|p:first-line|选择每个 < p > 元素的首行|1|
|:first-child|p:first-child|选择属于父元素的第一个子元素的每个 < p > 元素|2|
|:before|p:before|在每个 < p > 元素的内容之前插入内容|2|
|:after|p:after|在每个 < p > 元素的内容之后插入内容|2|
|:lang(language)|p:lang(it)|选择带有以 "it" 开头的 lang 属性值的每个 < p > 元素|2|
|element1~element2|p~ul|选择前面有 < p > 元素的每个 < ul > 元素|3|
|[attribute^=value]|a[src^="https"]|选择其 src 属性值以 "https" 开头的每个 < a > 元素|3|
|[attribute$=value]|a[src$=".pdf"]|选择其 src 属性以 ".pdf" 结尾的所有 < a > 元素|3|
|[attribute*=value]|a[src*="abc"]|选择其 src 属性中包含 "abc" 子串的每个 < a > 元素|3|
|:first-of-type|p:first-of-type|选择属于其父元素的首个 < p > 元素的每个 < p > 元素|3|
|:last-of-type|p:last-of-type|选择属于其父元素的最后 < p > 元素的每个 < p > 元素|3|
|:only-of-type|p:only-of-type|选择属于其父元素唯一的 < p > 元素的每个 < p > 元素|3|
|:only-child|p:only-child|选择属于其父元素的唯一子元素的每个 < p > 元素|3|
|:nth-child(n)|p:nth-child(2)|选择属于其父元素的第二个子元素的每个 < p > 元素|3|
|:nth-last-child(n)|p:nth-last-child(2)|同上，从最后一个子元素开始计数|3|
|:nth-of-type(n)|p:nth-of-type(2)|选择属于其父元素第二个 < p > 元素的每个 < p > 元素|3|
|:nth-last-of-type(n)|p:nth-last-of-type(2)|同上，但是从最后一个子元素开始计数|3|
|:last-child|p:last-child|选择属于其父元素最后一个子元素每个 < p > 元素|3|
|:root|:root|选择文档的根元素|3|
|:empty|p:empty|选择没有子元素的每个 < p > 元素（包括文本节点）|3|
|:target|#news:target|选择当前活动的 #news 元素|3|
|:enabled|input:enabled|选择每个启用的 < input > 元素|3|
|:disabled|input:disabled|选择每个禁用的 < input > 元素|3|
|:checked|input:checked|选择每个被选中的 < input > 元素|3|
|:not(selector)|:not(p)|选择非 < p > 元素的每个元素|3|
|::selection|::selection|选择被用户选取的元素部分|3|