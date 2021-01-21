# R-markdown-notes
学r markdown的笔记
---
title: "official guide of r markdown"
author: "Hailey"
date: "2021/1/15"
output: html_document
---

## R Markdown
Check more tutorials at  <http://rmarkdown.rstudio.com>.

### 1.Code Chunks

由chunk headers 和 codes 组成
插入代码块后，在r后面加逗号，可以看到可以进行设置的全部变量。以下介绍常用的几种。完整版介绍也可以在 https://yihui.org/knitr/options/ 找到。


#### ①chunk options 代码块设置

`include = FALSE`——不显示代码和结果,在后台运行，结果可被其他chunk所用  
`echo = FALSE`——不显示代码，但显示结果（内嵌图表好用）  
`message = FALSE`——不显示代码执行的message  
`warning = FALSE`——不显示warning  
`fig.cap = "..."`——给图表加标题  


`results = "markup"/"asis"/"hold"/"hide"`——除warning，message，error之外的text output的输出形式，markup：显示在fenced code blocks (```)z中；asis：as-is，即显示raw text results，没有markups；hold：不即时显示，保留到chunk末尾显示；hide：不显示
`strip.white = TRUE`——消除chunk前后的空行
`fig.width = "..."`——宽度
`fig.height = "..."`——长度


eval: (TRUE; logical or numeric)
eval = c(1, 3, 4)
eval = -(4:5)

#### ②global options 默认设置  
每个代码块单独设置很麻烦，可以设置代码块的默认属性。  
格式：`knitr::opts_chunk$set(...)`  default values

```{r,setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, message = FALSE, warning = FALSE)
#语言和后面具体的设置项之间可以逗号或者空格隔开
#global options的setup只能有一次，一般是页首第一个chunk
#{}内容(chunk headers）必须在同一行，.rmd文件用```{}, .Rnw(R + LaTeX)则是<< >>=
```


### 2.Inline Code
用``包裹行内代码。  
如果在r代码块内赋值，也可以多次引用。  
行内代码默认显示结果而不是过程。  
根据前后文自动识别格式，不能自主设置。  

```{r include=FALSE}
QQQ <- "MY PIC"
```

`my_name <- "Hailey"`

look at `r QQQ`


### 3.Code Languages

-Python  
-SQL  
-Bash  
-Rcpp  
-Stan  
-JavaScript  
-CSS  

### 4.Parameters  
参数设置方便改变输入变量，输出相同步骤的报告。

具体代码：在文首设置`params:...`  
如有意改变data，则`params:  
  data: "hawaii"`

渲染时可以在console中使用render()修改，如修改前例写成：  
`render("xxx.Rmd", params = list(data = "xxx"))`  
修改更多参数，则增加list中的项数。  

更直观的方法，Knit-Knit with parameters...也可以设置渲染参数。  

### 5.Tables  
Rmd中表格都在terminal里，如果想在正文里输出，可以使用`knitr::kable`功能。  

```{r,echo = FALSE,results='asis'}
library(knitr)
kable(mtcars[1:5,],caption = "A knitr kable.")

```

### 6.Output Formats  



## Pandoc Markdown  
Markdown语法的修订版  
### 1.Paragraphs  
换行符默认视作空格
换行需要2+空格，或者反斜线+空格  
（多行或网状表格中只能后者，因为末尾空格的会忽略）\ 

### 2.Headers--Setext and ATX  
#### ①Setext（不常用）
只有两级  
我是s双下划綫标题我比较大  
===  
我也是单下划线标题我稍小  
---  
也可以在标题内添加格式: 详见下面的[Inline formatting](#### Inline formatting)  

#### ②ATX（常用）  
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
####### Heading 7
>总结：Pandoc中，*#后面必须有空格*才能识别，#前必有后随意，heading最多六级，超过6个#则为正文格式（6级标题比正文还小）。Pandoc中标题前*需要空白行*来避免混淆（比如有时候列编号前面就会加个#），标准markdown中不用。   

两种标题类型都可以在标题内添加格式，如：  

### A level-three header with a [link](/url) and *emphasis*  
#### ③Header identifiers  

##### *a.header_attributes*
先设置identifier标识，可以设置class、class key、key三个attributes：  
`{#identifier .class .class key=value key=value}`  
下面举例，这三个header都被标记成`foo`了：  

`# My header {#foo}`

`## My header ##    {#foo}`

`My other header   {#foo}
---------------`  
一般用不着，因为markdown就是为了简约快捷，详见[PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/)（好复杂我没看）  
 
下面这俩效果一样，效果为不给编号（优先选用{-}，多语言友好）：  

# My header {-}  

# My header {.unnumbered}  

##### *b.auto_identifiers*  

好复杂，不想看了，估计用不上，详见https://rmarkdown.rstudio.com/authoring_pandoc_markdown.html#Pandoc_Markdown

##### *c.implicit_header_references*  
Pandoc里可以方便地链接到标题，如下： 标题是：   

# Header identifiers in HTML  
引用写成[Header identifiers in HTML]，或[Header identifiers in HTML][]，或[the section on header identifiers][header identifiers in
HTML]，而不是传统复杂地写[Header identifiers in HTML](#header-identifiers-in-html)  
几个标题内容一样的话，默认只链接到第一个，其他的只能用传统复杂办法。  

也可以自定义一下，用[header]:xxx，[header]在此出现就会链到xxx，而不是那个header本身。  

### 3.Block quotations引用  
前加>即可（*前面必须空行*），可嵌套（*下一层和上一层之间也必须空行，且该空行前有相应数量的>，以表明内容还在这个引用块内*）

> This is a block quote.  
Second line.
> 
> > A block quote within a block quote.  

五个空格隔开>与内容，识别为代码。  

>     print("HELLO WORLD")  

### 4.Verbatim (code) blocks 逐字记录（代码）块  
想要原原本本显示出来，却不想要引用格式？有三种方法实现  
#### ①Indented code blocks 缩进的代码块
前面加4个空格/一个tab

    if (a > 3) {
      moveShip(5 * gravity, DOWN);
    }

#### ②Fenced code blocks  
3+个~，上下围起来（上下数量要一致）    

~~~~~~~
if (a > 3) {
  moveShip(5 * gravity, DOWN);
}
~~~~~~~  
如果内容本身上下带~~~，就用更长的上下围起来  

~~~~~~~~~~~~~~~~
~~~~~~~~~~
code including tildes
~~~~~~~~~~
~~~~~~~~~~~~~~~~  

可以给这种代码块设置attributes，identifier、class、attributes之间，它们彼此之间都用`空格+.`隔开（注意上下还是要一样长，连带着设置attributesd 字符一起数）如下：  

~~~~ {#mycode .haskell .numberLines startFrom="100"}
qsort []     = []
qsort (x:xs) = qsort (filter (< x) xs) ++ [x] ++
               qsort (filter (>= x) xs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


前后```，得到可注明语言，但不运行的逐字代码块：  

```r
library(knitr)
kable(mtcars[1:5,],caption = "A knitr kable.")
```  
在可执行代码块的语言前面加个点，也能得到这种不运行的代码块：  
```{.r}
library(knitr)
kable(mtcars[1:5,],caption = "A knitr kable.")
```  

### 5.Line blocks词块

前面|+空格，可以使后面文字保持原格式，不受markdown回车当空格的影响：  

| The limerick packs laughs anatomical
| In space that is quite economical.
|    But the good ones I've seen
|    So seldom are clean
| And the clean ones so seldom are comical

### 6.Lists列表  

#### Bullet lists  
前加*/+/-即可,三种符号等效可组合：  

* one
* two
+ three
- four
* five

想各项各自成段宽松点，则用空行隔开：  

* one

* two

* three

#### The four-space rule



























#### Inline formatting  
