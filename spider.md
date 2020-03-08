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

---
### ip代理

本地客户端与服务器不直接连接，代理服务器连接,客户端向代理服务器发送http报文，代理服务器再向服务器发送http报文
代理分为:透明代理，匿名代理，高匿代理。使用透明代理时，服务器知道客户端ip，而匿名代理，服务器不知道客户端ip，但知道客户端使用了代理，而高匿代理，服务器既不知道客户端ip，也不知道其使用了代理。
代理类型由remote_addr,x-forward-for(xff),via决定
* REMOTE_ADDR
  REMOTE_ADDR表示客户端的IP，但是它的值不是由客户端提供的，而是服务器根据客户端的IP指定的。 
  如果使用浏览器直接访问某个网站，那么网站的web服务器（Nginx、Apache等）就会把REMOTE_ADDR设为客户端的IP地址。
  如果我们给浏览器设置代理，我们访问目标网站的请求会先经过代理服务器，然后由代理服务器将请求转化到目标网站。那么网站的web代理服务器就会把REMOTE_ADDR设为代理服务器的IP。
* X-Forwarded-For（XFF）
  X-Forwarded-For是一个HTTP扩展头部，用来表示HTTP请求端真实IP。当客户端使用了代理时，web代理服务器就不知道客户端的真实IP地址。为了避免这个情况，代理服务器通常会增加一个X-Forwarded-For的头信息，把客户端的IP添加到头信息里面。
  X-Forwarded-For请求头格式如下：
  X-Forwarded-For:client,proxy1,proxy2
  client表示客户端的IP地址；proxy1是离服务端最远的设备IP;proxy2是次级代理设备的IP；从格式中，可以看出从client到server是可以有多层代理的。
  如果一个HTTP请求到达服务器之前，经过了三个代理Proxy1、Proxy2、Proxy3，IP分别为IP1、IP2、IP3，用户真实IP为IP0，那么按照XFF标准，服务端最终会收到以下信息：
  X-Forwarded-For:IP0,IP1,IP2
  Proxy3直连服务器，它会给XFF追加IP2，表示它是在帮Proxy2转发请求。列表中并没有IP3，IP3可以在服务端通过RemoteAddress字段获得。我们知道HTTP连接基于TCP连接，HTTP协议中没有IP的概念，RemoteAddress来自TCP连接，表示与服务端建立TCP连接的设备IP，在这个例子里就是IP3。
* HTTP_VIA
  via是HTTP协议里面的一个header,记录了一次HTTP请求所经过的代理和网关，经过1个代理服务器，就添加一个代理服务器的信息，经过2个就添加2个。

---
### handle
`urllib.request.urlopen(url)`底层是先构造一个处理器handle，再构造一个访问器opener，通过opener打开url
```python
#!/usr/bin/python3
import urllib.request

url="https://blog.csdn.net/cpongo1/article/details/89533131"
handler=urllib.request.HTTPHandler()
opener=urllib.request.build_opener(handler)
response=opener.open(url)
data=response.read().decode()
with open("4.html","x",encoding="utf-8") as f:
        f.write(data)
```
添加代理ip
```python
#!/usr/bin/python3
import urllib.request

url="https://blog.csdn.net/cpongo1/article/details/89533131"
proxy={"http":"https://117.88.177.52:3000"}
handler=urllib.request.ProxyHandler(proxy)
opener=urllib.request.build_opener(handler)
response=opener.open(url)
data=response.read().decode()
with open("5.html","w",encoding="utf-8") as f:
        f.write(data)
```
