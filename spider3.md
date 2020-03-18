[TOC]

### File

1. **read, readline,readlines**
`f.read([size])`: 一次以字符串的格式读取所有数据，遇到\n换行，一般字符型数据换行都有\n，而字节型数据换行字节以\n显示。
`f.readline([size])`: 逐行读取数据，只读取一行,字节型数据以\n分割。
`f.readlines()`: 逐行读取数据，每次读取一行，以列表的形式呈现，每行为列表的一个元素。

2. **write,writelines**
`f.write()` : 写入数据
`f.writelines()` : 将字符列表的数据以逐行的形式写入文件，不自动换行。
```python
f=open("a.txt","r")
g=open("b.txt","w")
g.writelines(f.readlines(f))
```
此会得到与a相同的文件，由于writelines不自动换行，但readlines会把换行符写入

3. **close**
打开文件写入后，若文件没有关闭，则内容不会写入文件，会在缓冲区内，只有当文件关闭后，才会写入内容，可用`f.shush()`将缓冲区的内容写入文件。

### json
```python
  1 #!/usr/bin/python3
  2 import json
  3 
  4 # json string -----> python dict/list
  5 json_str1='[{"name":"Jack","age":18},{"name":"Mary","age":20}]'
  6 list1=json.loads(json_str1)
  7 #print(list1)
  8 
  9 # python list/dict -----> json string
 10 list2=[{"name":"Jack","age":18},{"name":"Mary","age":20}]
 11 json_str2=json.dumps(list2)
 12 #print(type(json_str2))
 13 
 14 # 将python list/dict 以json string的形式写入文件与读取
 15 
 16 list3=[{"name":"Jack","age":18},{"name":"Mary","age":20}]
 17 json.dump(list3,open("1.json","w"))
 18 list4=json.load(open("1.json","r"))
 19 #print(type(list4))

```

### csv
```python
# 读取csv文件
import csv
with open("q.csv","r") as f:
    reader=csv.reader(f)
    for row in reader:
        print(row[0])
        ...
        # 对row[0],row[1]进行操作

# 写入csv文件
import csv
with open("2.csv","w") as f:
    writer=csv.writer(f)
    writer.writerow(list1)
    writer.writerow(list2)
    # writer.writerows([list1,list2])
```

> 使用reader,writer方法读写csv文件时，将整个文件看一个大列表，每行看作其中的元素，而每一行又可以看成一个小列表，其元素为以","分割的字符段

```python
#以字典的方式读写csv文件
class csv.DictReader(csvfile, fieldnames=None, restkey=None, restval=None, dialect='excel', *args, **kwds)

class csv.DictWriter(csvfile, fieldnames, restval='', extrasaction='raise', dialect='excel', *args, **kwds)
```
```python
# 读
import csv
with open("1.csv","r") as f:
    reader=csv.DictReader(f)
    for row in reader:
        print(row["first_name"],row["last_name"])

Baked Beans
Lovely Spam
Wonderful Spam

#写
import csv
with open("2.csv","w") as f:
    fieldnames = ["first_name","last_name"]
    writer=csv.DictWriter(f,fieldnames=fieldnames)
    writer.writerheader()
    writer.writerow({"first_name":"Baked","last_name":"Beans"})
    writer.writerow({'first_name': 'Lovely', 'last_name': 'Spam'})
    writer.writerow({'first_name': 'Wonderful', 'last_name': 'Spam'})
```
> 使用DictReader,DictWriter方法读写csv文件时，将整个文件看一个大列表，每行看作其中的元素，其中首行看为一个列表，为表头，其余行看为一个字典，字典的keys为首行，values为该行以“，”分割的字符段组成的列表。