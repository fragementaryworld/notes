### 编码与解码

![encode and decode](https://img2018.cnblogs.com/blog/733013/201812/733013-20181222072124440-1244874607.png)  


编码与解码是字符与字节之间的转换，编码类型有unicode,utf-8,gbk,ascii等，而无论什么编码都包含ascii编码。
关于unicode和utf-X格式的编码关系，粗略地可以认为utf-X是unicode格式的一种特殊类型。实际上在存储utf数据时，内部会自动在Unicode和utf之间进行转换。
当文件出现乱码的原因在于编码与解码的过程用了不同的编码类型
```python
"我".encode("utf-8").decode("gbk")
```

---
### URL
url的编码方式有三类:escape,encodeURL,encodeURLComponent
可用urllib.parse.urlencode,urllib.parse.quote来编码不支持的字符，如汉字，空格等，其中urlencode对字典进行编码，quote直接对字符串进行编码
> 在`urllib.parse.quoto(url,safe="/")`中，有safe参数，其含义为不编码字符，默认不对/进行编码，可设置为`safe="/,:,=,?"`等，按需求设置，也可设置为`safe=string.printable`

```python
#!/usr/bin/python3
import urllib.request
import urllib.parse
import string

def get_para():
    url1="http://www.baidu.com/s?"
    dic={"wd":"中文","key":"china"}
    url2=urllib.parse.urlencode(dic)
    url=url1+url2
    response=urllib.request.urlopen(url)
    data=response.read().decode()
    with open("1.html","w",encoding="utf-8") as f:
        f.write(data)

get_para()
```
`url.request.urlopen(url)`函数返回HTTPResponse对象
`req=url.request.Request(url,headers=dict)`生成Request对象,请求头用`headers`参数指定，但必须是字典，也可用`req.add_head(key,value)`来添加，查看由`req.get_head(key)`,`req.headers`,`req.get_full_url()`,`req.full_url`
```python
#!/usr/bin/python3
  1 #!/usr/bin/python3
  2 import urllib.request
  3 import urllib.parse
  4 import string
  5 
  6 url1="https://cn.bing.com/search?"
  7 dic={"q":"中国"}
  8 url2=urllib.parse.urlencode(dic)
  9 url=url1+url2
 10 #header={"User-Agent":"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (K    HTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36"}
 11 #req=urllib.request.Request(url,headers=header)
 12 req=urllib.request.Request(url)
 13 req.add_header("User-Agent","Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537    .36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36")
 14 response=urllib.request.urlopen(req)
 15 data=response.read().decode()
 16 with open("2.html","w",encoding="utf-8") as f:
 17     f.write(data)
```
> post请求: `urllib.request.urlopen(url,data="...")`

### random
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

import random
import string

# 随机整数：
print random.randint(1,50)

# 随机选取0到100间的偶数：
print random.randrange(0, 101, 2)

# 随机浮点数：
print random.random()
print random.uniform(1, 10)

# 随机字符：
print random.choice('abcdefghijklmnopqrstuvwxyz!@#$%^&*()')

# 多个字符中生成指定数量的随机字符：
print random.sample('zyxwvutsrqponmlkjihgfedcba',5)

# 从a-zA-Z0-9生成指定数量的随机字符：
ran_str = ''.join(random.sample(string.ascii_letters + string.digits, 8))
print ran_str

# 多个字符中选取指定数量的字符组成新字符串：
print ''.join(random.sample(['z','y','x','w','v','u','t','s','r','q','p','o','n','m','l','k','j','i','h','g','f','e','d','c','b','a'], 5))

# 随机选取字符串：
print random.choice(['剪刀', '石头', '布'])

# 打乱排序
items = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
print random.shuffle(items)
```
=======
### http
1. http的请求方式
    * get 请求
        * 请求方便
        * 不安全，明文
        * 参数长度有限制
    * post 请求
        * 比较安全
        * 数据整体没有限制
        * 上传文件
    * put(不完全)
    * delete(删除)
    * head(请求头)
