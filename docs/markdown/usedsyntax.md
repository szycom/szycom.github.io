## 超链接

> MarkDown支持两种形式的[链接语法][7]：行内式和参考式。

### 行内式

```markdown
[行内式](http://szycom.github.io)
```

**效果如下**

[行内式](http://szycom.github.io)

### 参考式

**语法**

> 参考式链接分为两部分
> 正文中的写法**[链接文字][链接标记]**
> 参考链接写法**在文本任意位置添加[链接标记]:链接地址 “链接标题”，链接地址与链接标题前有一个空格**
> 如果链接文字本身可以作为链接标记，**也可以写成[链接文字][]**

```markdown
简书里面有 [简书早报][1]、[简书晚报][2]以及 [简黛玉][3]
[简黛玉 美人][3] 是一个[才女][]

[1]:http://szycom.github.io "早报"
[2]:http://szycom.github.io "晚报"
[3]:http://szycom.github.io
[才女]:http://szycom.github.io
```

**效果如下**

简书里面有 [简书早报][1]、[简书晚报][2]以及 [简黛玉][3]
[简黛玉 美人][3] 是一个[才女][]

## 文章内跳转-锚点

**语法**

> * MarkDown Extra只支持在标题后插入锚点，其他地方无效。
> * 锚点中的标题如果有空格，则锚点无效。

```markdown
锚点连接页内标题

[标题1-超链接](#超链接)

[标题2-文章内跳转](#文章内跳转-锚点)

[标题3-支持复选框](#支持复选框)
```

**效果如下**

锚点连接页内标题

[标题1-超链接](#超链接)

[标题2-文章内跳转](#文章内跳转)

[标题3-支持复选框](#支持复选框)

## 支持复选框

Github[支持复选框][5]

```markdown 
But I have to admit, tasks lists are my favorite:

- [x] This is a complete item
- [ ] This is an incomplete item
```

**效果如下**

But I have to admit, tasks lists are my favorite:

- [x] This is a complete item
- [ ] This is an incomplete item

## table

**[语法][5]**

```markdown
First Header | Second Header
------------ | -------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column
```

**效果如下**

First Header | Second Header
:- | :-
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

### 对齐方式

**[语法][6]**

> * `:-`左对齐
> * `-:`右对齐
> * `:-:`居中对齐

**举例**
```markdown
左对齐

First Header | Second Header
:- | :-
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

右对齐

First Header | Second Header
-: | -:
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

居中对齐

First Header | Second Header
:-: | :-:
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column
```

**左对齐**

First Header | Second Header
:- | :-
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

**右对齐**

First Header | Second Header
-: | -:
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

**居中对齐**

First Header | Second Header
:-: | :-:
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column

## 换行

Markdown语法直接打一个回车是不会显示换行的。

* 行尾打两个或两个以上的空格之后回车

行尾打两个空格换行测试1  
行尾打两个空格换行测试2  
行尾打两个空格换行测试3  

* 打两个回车

行尾打两个回车换行测试1


行尾打两个回车换行测试2


行尾打两个回车换行测试3


> 他们的区别是第一种打出来的效果行间距近，而第二种更像是段落之间的分隔，行间距大。

[1]:http://szycom.github.io "早报"
[2]:http://szycom.github.io "晚报"
[3]:http://szycom.github.io
[才女]:http://szycom.github.io
[5]:https://guides.github.com/features/mastering-markdown/ "表格对齐方式 复选框"
[6]:https://www.cnblogs.com/anliux/p/10805103.html
[7]:https://www.zhang21.cn/2017/09/01/Markdown/ "超链接"
