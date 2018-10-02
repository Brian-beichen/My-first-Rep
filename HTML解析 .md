# HTML解析

## Beautifulsoup

创建bs对象的两种方法

```Python
s = bs(html_str,'lxml',from_encoding = 'utf-8')#通过含有html文本的字符串创建bs对象
s = BeautifulSoup(open('index.html'))#通过html文件创建bs对象
```

bs对象的属性

bs中的tag对象

```python
soup.prettify()#将html文本格式化输出
s.title#html文本中的第一个title标签对象
s.title.name#该标签的名字
s.title.attrs#以键值对的形式返回该对象的所有属性和属性值
s.title.string#该标签内部的文本
#如果该标签下有多个字节点，那么string属性会返回none，此时需要用到strings
#strings方法会循环遍历某标签所有的子节点，取出其中的文本，而stripped_strings则会去掉输出字符串中的空格和空行
for string in s.strings:
    print(string)
for string in s.stripped_strings:
    print(string)
'''
如果该文本中的内容是注释，那么其type为bs4.element.Comment
'''

#tag对象的contents属性可以将tag子节点以列表的方式输出
s.head.contents
#tag对象的children属性返回一个生成器用以对tag的子节点进行循环
for child in s.head.children:
    print(child)

#tag对象的descendants属性可以以同样的方式通过生成器对其所有子孙节点进行递归循环
for child in s.head.descendants:
    print(child)
    
#.parent属性能获取该tag的父节点(整个节点的所有内容)
s.title.parent
#parents属性将元素所有父辈节点全都迭代地放入一个迭代器中，可以通过循环得到
for parent in s.title.parents:
    if parent is None:
        print(parent)
    else:
        print(parent.name)
        
        
#.next_sibling和.previous_sibling会返回该节点的下一个和前一个兄弟节点
s.a.next_sibling
s.a.previous_sibling
#.next_siblings也是一个迭代器，可以将当前节点的所有兄弟节点迭代输出
for sibling in s.a.next_siblings:
    print(repr(sibling))
    
next_element
previous_element
next_elemebts
previous_elements
#这四个属性返回的是该标签的前后节点，并非一定是兄弟节点，在当前节点后的任意节点都会被返回
```

bs中的搜索方法：find_all：用于搜索当前tag的所有tag子节点，并判断是否符合过滤器的条件

函数原型：find_all(name,attrs,recursive,text,**kwargs)

- name参数：

  查找所有标签名为name的标签，name取值可以是字符串，正则表达式，列表，True和方法

  如果找到多个，则会返回一个包含找到的所有对象的列表

  可以传入一个方法作为过滤器，该方法接受元素参数tag节点，如果返回True则表示当前元素匹配并且被找到，否则返回False。

  ```python
  #过滤包含class属性和id属性的元素
  def hasClass_Id(tag):
      return tag.has_attr('class') and tag.has_attr('id')

  print(soup.fina_all(hasClass_ID))
  ```

- kwargs参数

  可以在find_all中传入形如**属性名='要过滤的属性名'**作为参数，前提是该属性名非find_all函数的内置参数名

  ```python
  result = soup.find_all(id='abc')#寻找id属性为abc的节点
  result = soup.find_all(id=True)#寻找含有id属性的节点，不论id是什么
  result = soup.find_all(class_='abc')#次数要用class进行过滤，但是class是python的关键字，在后面加个下划线即可

  #也可以同时过滤多个属性
  result = soup.find_all(id='abc',class_='aaa')
  ```

  也可以给attr传入一个字典参数来搜索包含特殊属性的tag

  ```python
  result = soup.find_all(attrs = {'data-foo':'value'}))
  ```

- text参数

  通过text参数可以搜索tag中的字符串内容

  text参数也可以接受字符串，正则表达式，列表，True

  当仅有text参数时，返回的是满足该text参数的字符串

  ```python
  result = soup.find_all(text='你要搜索的字符串')#这种情况下，仅当某个标签内的文本和你要搜索的字符串完全匹配时，才会返回值，若想模糊匹配，则需要使用正则表达式
  result = soup.find_all(text=re.compile('正则表达式'))
  ```

  当text参数和其他方法混用时，则返回搜索到的标签

  ```python
  result = soup.find_all('a',text='abc')
  #此时会输出内容为abc的a标签:<a>abc</a>
  ```

- limit参数

  用于限制返回结果的数量

- recursive参数：recursive = Flase表示只搜索tag的直接子节点（默认搜索所有子孙节点）

bs中的CSS选择器

Web中的CSS选择器：标记名不加修饰，类名前加".",id名前加"#"

'>'查找某个节点下的标记，'~'查找兄弟标记，'+'查找紧随其后的标记

bs中有方法soup.select()，通过传入CSS选择器（字符串）来选择tag，返回类型是list

```python
#直接查找title标记
soup.select('title')

#逐层查找title标记
soup.select('html head title')

#查找head下的title标记
soup.select('head > title')

#查找p下的'id=link1'标记
soup.select('p > # link1')

#查找兄弟节点
#查找id='link1'之后class=sister的所有兄弟标记
soup.select('# link1 ~ .sister')

#查找紧跟着id = 'link1'之后class=sister的子标记
soup.select('# link1 + .sister')

#通过CSS类名查找
soup.seletc('.sister')
soup.select('[class_=sister]')

#通过tag的id查找
soup.select('# link1')
soup.select('a# link2')#查找id为link2的a标签

#通过是否存在某个属性来查找
soup.select('a[href]')#存在href属性的a标签

#通过属性值来查找
soup.select('a[href='xxx']')#属性值等于xxx的
soup.select('a[href^='xxx']')#属性值以xxx开头的
soup.select('a[href$='xxx']')#属性值以xxx结尾的
soup.select('a[href*='xxx']')#属性值包含xxx的
```







