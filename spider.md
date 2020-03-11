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

---
### proxy认证
```python
  1 #!/usr/bin/python3
  2 
  3 import urllib.request
  4 
  5 ##way 1
  6 #url="http://www.imcaviare.com/2018-12-18-1/"
  7 #proxy={"http":"usrname:passwd@19.168.1.1:8080"}
  8 #handle=urllib.request.ProxyHandler(proxy)
  9 #opener=urllib.request.build_opener(handle)
 10 #opener.open(url)
 11 
 12 #way 2
 13 url="http://www.baidu.com"
 14 uri="123.11.22.1:8080"
 15 name="username"
 16 passwd="passwd"
 17 manager=urllib.request.HTTPPasswordMgrWithDefaultRealm()
 18 manager.add_password(None,uri,name,passwd)
 19 handler=urllib.request.ProxyBasicAuthHandler(manager)
 20 opener=urllib.request.build_opener(handler)
 21 data=opener.open(url)
 22 print(data.read().decode())
```

---
### http认证

```python
  1 #!/usr/bin/python3
  2 
  3 import urllib.request
  4 
  5 ##way 1
  6 #url="http://www.imcaviare.com/2018-12-18-1/"
  7 #proxy={"http":"usrname:passwd@19.168.1.1:8080"}
  8 #handle=urllib.request.ProxyHandler(proxy)
  9 #opener=urllib.request.build_opener(handle)
 10 #opener.open(url)
 11 
 12 #way 2
 13 url="http://www.baidu.com"
 14 uri="123.11.22.1:8080"
 15 name="username"
 16 passwd="passwd"
 17 manager=urllib.request.HTTPPasswordMgrWithDefaultRealm()
 18 manager.add_password(None,uri,name,passwd)
 19 handler=urllib.request.ProxyBasicAuthHandler(manager)
 20 opener=urllib.request.build_opener(handler)
 21 data=opener.open(url)
 22 print(data.read().decode())
```

---
### Cookie

1. Cookie登陆
```python
  1 #!/usr/bin/python3
  2 import urllib.request
  3 url="https://www.yaozh.com/member"
  4 headers={"User-agent":"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (K    HTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36","Cookie":"think_langua    ge=en-US; _ga=GA1.2.1104644017.1583903706; _gid=GA1.2.860684517.1583903706;     acw_tc=2f624a5615839037133273241e7ee1af6db0be05482e943933f31475bd179e; PHPSE    SSID=qo9kn6dn09h86dk8da50rlun23; yaozh_logintime=1583904632; yaozh_user=8906    05%09wyname; yaozh_userId=890605; yaozh_jobstatus=kptta67UcJieW6zKnFSe2JyXno    aZcJVrlZmHnKZxanJT1qeSoMZYoNdzbZtaqdzTw87Jhpyqn26fhtHCpquUrJrOnlNu1HCWlHNZkm    1qlp64B1b841Eb104E05Ff309d8Ea6F37431DSlZqXk1mgqJ%2BYn4OnoKKdU5ysa2SUcIeVbm%2    BUcWKXm5WUlZ2TWaCy6bee4e68fdf17cd0aa9b721baf3dfff7; _gat=1; Hm_lpvt_65968db3    ac154c3089d7f9a4cbb98c94=1583904635; yaozh_uidhas=1; yaozh_mylogin=158390463    7; acw_tc=2f624a5615839037133273241e7ee1af6db0be05482e943933f31475bd179e; Hm    _lvt_65968db3ac154c3089d7f9a4cbb98c94=1583903706%2C1583903718%2C1583904447"}
  5 req=urllib.request.Request(url,headers=headers)
  6 response=urllib.request.urlopen(req)
  7 data=response.read().decode()
  8 with open("1.html","w",encoding="utf-8") as f:
  9         f.write(data)

```

2. 获取Cookie
* 通过浏览器获取，可能会有时效性
* 代码登录直接获取
> * 采用http.cookiejar库中的CookieJar类来自动保存cookie
> * 登录页面为get请求的url，可能会与post请求的url不同，故可向在浏览器检查选项中添加preserve log选项，登录后查看post请求url与其post数据，注意post数据为请求前（即填写时）的数据，需要在登录页面查找
> * 注意post请求的数据为需要转换为bytes形式，即与网络的数据交流均为bytes形式的数据，用`urllib.parse.urlencode(dict).encode()`
```python
  1 #!/usr/bin/python3
  2 import urllib.request
  3 import http.cookiejar
  4 import urllib.parse
  5 
  6 # get Cookie
  7 login_url="https://www.yaozh.com/login"
  8 headers={"User-agent":"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Ge    cko) Chrome/80.0.3987.132 Safari/537.36"}
  9 post_data={"username":"wyname","pwd":"ChEnGjUn20000204","formhash":"AD1A063EC2","backurl"    :"https%3A%2F%2Fwww.yaozh.com%2F"}
 10 login_data=urllib.parse.urlencode(post_data).encode()
 11 cookie=http.cookiejar.CookieJar()
 12 cookie_handler=urllib.request.HTTPCookieProcessor(cookie)
 13 opener=urllib.request.build_opener(cookie_handler)
 14 req=urllib.request.Request(login_url,headers=headers,data=login_data)
 15 opener.open(req)
 16 
 17 # spider
 18 center_url="https://www.yaozh.com/member"
 19 req2=urllib.request.Request(center_url,headers=headers)
 20 response=opener.open(req2)
 21 data=response.read().decode()
 22 with open("2.html","w",encoding="utf-8") as f:
 23         f.write(data)
~                               
```

### requests
```python
  1 #!/usr/bin/python3
  2 import requests
  3 url="http://www.baidu.com"
  4 headers={"User-agent":"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (K    HTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36"}
  5 response=requests.get(url,headers=headers)
  6 #print(response.content)  bytes格式的网页代码
  7 #print(response.text)     unicode格式的网页代码
  8 #print(response.request.headers)    请求头
  9 #print(response.headers)           响应头
 10 #print(response.status_code)       http状态马
 11 #print(response.request._cookies)   请求头Cookie
 12 #print(response.cookies)            响应头Cookie
 13 #print(response.url)               URL
```
> requests能直接编码url，无需将url用urllib.parse.urlencode或quote编码，同样post请求中，data可直接用字典传入，无需先urlencode再encode
