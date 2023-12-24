
[参考视频](https://www.bilibili.com/video/BV1pu4y1j7kA/?spm_id_from=333.999.0.0)



## 1. 介绍

[Markdown CommonMark 标准文档](https://spec.commonmark.org/) 

**什么是Markdown？**

- 是一种轻量级文本标记语言（markup language）
- 可以通过纯文本来表示带有格式的文档，同时保证易读性
- 语法简单，易于学习，易于使用
- 可以轻松转换为 HTML（映射到 HTML 的子集）

> Markdown 本质是一种标记语言，是对HTML的一种简化
>
> - 所有的最终视觉上的效果都由HTML + CSS 决定的

![image-20231223190035181](assets/image-20231223190035181.png)

## 2. 语法概述

### 标题
!!! note

    `#` + 空格(开头)，后跟内容
    Ps:  # 和标题间至少一个空格

* 使用 # 的称为 ATX 样式
* 只有 1～6 级标题，7级以上不会变成标题格式
* 可以跨过某一级，但不推荐
    + 明确好层级关系
### 段落
!!! note

    - 直接编写文本即为普通段落
    - 段落间通过空行来分割（有空行就有新的段落）
    - 段落内换行需要在行尾加两个空格（`<br>`）i

__关于空格:__

- 多个连续的空格会被解析为一个空格
- 但是在代码块中，空格会被保留
- 使用多个空格可以使用`&nbsp; &emsp;`等 HTML语法
### 引言
!!! note 

    一个 `>` 加一个空格后面跟内容

- 内部可任意嵌套并使用MD语法
- 连续的`>`属于同一引言， 使用Enter来进行退出
### 列表
#### 无序列表
!!! note 

    `- + * ` 后加空格再跟内容

```markdown
- node 1
- node 2

  content in node 2
- node 3
* 第一层
    + 第二层
        * 第三层
    + 第二层
* 第一层
```
#### 有序列表
!!! note 

    数字加点(.) 后接空格 再接内容
### 分割线 & 代码块
!!! note 
    
    使用`* - _` 中任意一个字符重复至少三次

会被转换为html 中的`<hr>`  
PS : 分割线上下最好都加空行

!!! note 

    使用三个或以上 ` 或 ~ 围起来构成代码块
    
    - \ ' 或~ 后面可以加语言名称，可进行高亮
### 行内标记
```markdown
*斜体*         _也是斜体_  
**粗体**      __也是粗体__
***粗斜体***   ___也是粗斜体___
`行内代码`
~~删除线~~
<u>下划线</u>
```
若要显示原本用于格式化 Markdown 文档的字符，在字符前面添加反斜杠字符 \\ 。
### 插入图片和链接
!!!note 

    感叹号! - 方括号() - 圆括号结合的形式[]   =>  \!(图片描述)[图片位置/URL]  

常规 MD 语法插入图片无法调大小，使用 html 中img 的 style 可以调节

`<img src="图片位置" alt="图片描述" style="..."/>`

!!! note 
    
    插入链接: 方括号[] 和 圆括号()组合
    =>  \[文字描述](链接URL)

PS: markdown 中一般可以直接使用 html 语法和 css 样式

## 3.  扩展语法
### 表格
```markdown
| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |
```
输出如下:

| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |

在标题行中的连字符左侧，右侧或两侧添加冒号`:`，将列中的文本对齐到左，右或居中

仅可处理简单的表格，[关于复杂的表格](https://www.tablesgenerator.com/)
### 脚注
!!! note

    使用[^脚注名]插入脚注

在文中任意位置添加`[^脚注名]:脚注内容` 来定义脚注内容

- 脚注名只是标记、匹配使用的，可以是任何字符串
- 最终的编号一般由在**文中出现的顺序**决定

比如:
```markdown
[^note]: note content

footnote[^1] and note[^note]

[^1]: footnote content
```
生成如下:

[^note]: note content

footnote[^1] and note[^note]

[^1]: footnote content
### 任务列表
!!! note 

    使用`- [  ]` 插入未完成任务 ; 使用`- [x]`  插入未完成任务
    
    - 可以和其它列表混合使用
如:
```markdown
- [ ] to do list
- [x] finished
```
生成如下: 

- [ ] to do list
- [x] finished

### 公式
!!! note 

    一般使用一对\$作为行内公式标记，一对$$作为块级公式标记

关于公式处理的一切都不在 markdown->HTML 的过程中,如图所示:

![](assets/Snipaste_2023-12-24_14-33-53.jpg)

- HTML 保留公式文本，交给[MathJax](https://www.mathjax.org/)或[KaTex](https://katex.org/)等 js库来处理
    * 内部均使用[LaTeX](https://www.latex-project.org/) 公式语法

Ps: 关于流程图 / 时序图 / 甘特图.. 
-> 参考[mermaid.js](https://mermaid.js.org/intro/)