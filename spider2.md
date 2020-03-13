[TOC]

### Regular Expression

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