### 编码与解码

![encode and decode](https://img2018.cnblogs.com/blog/733013/201812/733013-20181222072124440-1244874607.png)  


编码与解码是字符与字节之间的转换，编码类型有unicode,utf-8,gbk,ascii等，而无论什么编码都包含ascii编码。
关于unicode和utf-X格式的编码关系，粗略地可以认为utf-X是unicode格式的一种特殊类型。实际上在存储utf数据时，内部会自动在Unicode和utf之间进行转换。
当文件出现乱码的原因在于编码与解码的过程用了不同的编码类型
```python
"我".encode("utf-8").decode("gbk")
```

---
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
