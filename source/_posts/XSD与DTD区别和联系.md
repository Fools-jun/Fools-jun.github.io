---
title: XSD与DTD区别和联系
copyright: true
mathjax: true
categories:
  - 笔记
  - 基础
  - 其它
tags:
  - DTD
  - XSD
abbrlink: 40227
date: 2021-02-02 09:36:41
---

今天在看Spring源码的时候，发现其中存在一个spring.schemas文件，发现其中有很多关于xsd文件的路径，所以记录一下xsd文件到底有什么作用，以及它和其他XML文档描述文件的区别和联系。

<!-- less -->

# DTD

## 简介

以下内容来自[维基百科](https://zh.wikipedia.org/wiki/%E6%96%87%E6%A1%A3%E7%B1%BB%E5%9E%8B%E5%AE%9A%E4%B9%89)：

>[XML](https://zh.wikipedia.org/wiki/XML)文件的**文档类型定义**（Document Type Definition）可以看成一个或者多个XML文件的模板，在这里可以定义XML文件中的元素、元素的属性、元素的排列方式、元素包含的内容等等。
>
>DTD（Document Type Definition）概念缘于SGML，每一份SGML文件，均应有相对应的DTD。对XML文件而言，DTD并非特别需要，well-formed XML就不需要有DTD。DTD有四个组成如下：
>
>- 元素（Elements）
>- 属性（Attribute）
>- 实体（Entities）
>- 注释（Comments）
>
>由于DTD限制较多，使用时较不方便，近来已渐被[XML Schema](https://zh.wikipedia.org/wiki/XML_Schema)所取代。

文档类型定义（DTD）可定义合法的XML文档构建模块。它使用一系列合法的元素来定义文档的结构。

DTD 可被成行地声明于 XML 文档中，也可作为一个外部引用。



## 声明语法

- 元素声明语法如下：

```
<!ELEMENT 元素名称　元素內容>
```

- 属性声明语法如下：

```
<!ATTLIST 元素名称、属性名称、属性值类型、属性的内定值>
```

- 实体声明语法如下：

```
<!ENTITY 实体名称　实体内容>
```

- 注释语法如下：

```
<!-- 注解內容 -->
```



# XSD

## 简介

以下内容来自[维基百科](https://zh.wikipedia.org/wiki/XML_Schema)：

>**XSD (XML Schema Definition)**是[W3C](https://zh.wikipedia.org/wiki/World_Wide_Web_Consortium)于2001年5月发布的推荐标准，指出如何形式描述XML文档的元素。XSD是许多[XML Schema 语言](https://zh.wikipedia.org/wiki/XML_Schema_语言)中的一支。XSD是首先分离于XML本身的schema语言，故获取W3C的推荐地位。
>
>像所有[XML Schema 语言](https://zh.wikipedia.org/wiki/XML_Schema_语言)一样，XSD用来描述一组规则──一个XML文件必须遵守这些规则，才能根据该schema‘合法（Valid）’。
>
>然而，与其他[XML Schema 语言](https://zh.wikipedia.org/wiki/XML_Schema_语言)不同，XSD意图设计为在确认一个文档的有效性时，将会产生满足特定[数据类型](https://zh.wikipedia.org/wiki/數據類型)的一个信息集合。这种后验证的[XML信息集](https://zh.wikipedia.org/wiki/XML信息集)可用来开发XML文件处理软件。



## XSD名称的来源

因为有其他XML schema 语言存在，故在引用这W3C建议的语言时，使用XML Schema或W3C XML Schema，Schema永远前缀大写。

“XML Schema”在2001年5月成为W3C推荐标准。由于“XML Schema”作为一种W3C的推荐标准的名字与广义的[XML Schema 语言](https://zh.wikipedia.org/wiki/XML_Schema_语言)存在名称上的混淆，用户社区的一部分人采用了“WXS”来称呼它， 用户社区的另一部分人采用“**XSD**”（**X**ML **S**chema **D**efinition[首字母缩略字](https://zh.wikipedia.org/wiki/首字母縮略字)）来称呼它。W3C发布的1.1标准采用了“**XSD**”作为官方称呼。



## Schema与schema文档

技术上说**schema**是元数据的一个抽象集合，包含一套**schema component**: 主要是元素与属性的声明、复杂与简单数据类型的定义。这些schema component通常是在处理一批**schema document**时被创建。schema文档包含着schema component的源语言定义。在日常使用中，一个schema文档常被称作一个schema。

Schema文档通过名字空间组织起来：所有的被命名的schema component属于一个目标名字空间；这个目标名字空间是schema文档作为整体的一个属性。schema文档可以包含进来（include）使用同一名字空间的其它schema文档，也可以导入（import）使用不同名字空间的schema文档。

当一个实例文档针对一个schema来验证有效性时（这一过程称为*assessment*），用来验证有效性的schema可以作为参数提供给验证器，也可以在实例文档中作为两种特殊属性之一直接提供：

- `xsi:schemaLocation`
- `xsi:noNamespaceSchemaLocation`.这种机制要求客户启动验证以充分相信这个文档，知道文档对正确的schema是有效的。

"xsi"是名字空间"http://www.w3.org/2001/XMLSchema-instance"的传统前缀。

XML Schema Documents通常有[文件扩展名](https://zh.wikipedia.org/wiki/文件扩展名)".xsd". XSD还没有专门的[互联网媒体类型](https://zh.wikipedia.org/wiki/互联网媒体类型)，因此按照 RFC 3023使用"application/xml"或"text/xml" .



## Schema component

主要的schema component有:

- **元素声明**（Element declaration）, 定义了元素的性质。包括：元素名字、目标名字空间；一个非常重要的性质是元素的类型，它限制了元素包含哪些属性与子元素。在XSD 1.1标准中，可以根据属性的值来有条件定义元素类型。一个元素可以属于一个替换群（substitution group），如果元素E在元素H的替换群中，那么schema许可H出现的地方E都可以出现。元素可以有完整性（integrity）约束：唯一性（uniqueness）约束确定特定值在该元素为根的子树中是独一无二的；引用（referential）约束确定值必须匹配一些其它元素的标识符。元素声明可以是全局的或局部的，允许同一个名字被用于一个实例文档的不同部分的不相关的元素。
- **属性声明**（Attribute declaration）,定义了属性的性质。包括：属性名字、目标名字空间，属性类型限制了属性可以取哪些值，也可以指出属性的缺省值或固定值（fixed value，即属性只能取这个值）。
- **简单与复杂数据类型**（Simple and complex type）.详见下节
- **模型群**（model group）与**属性群**（attribute group）定义。这实际上是宏（macro）：被命名的元素的群与属性的群，可在许多数据类型定义中被重用。
- **属性使用**（attribute use）表示复杂数据类型与属性声明的关系，指出属性是必需的还是可选的，在什么时候使用这种数据类型。
- **元素粒子**（element particle）类似于表示复杂类型与元素的关系，指出元素在上下文中出现的最大与最小次数。类似于元素粒子，内容模型可以包括**模型群粒子**，在语法上相当于非终结符：定义了允许的元素序列的选择与重复的单位。此外，**通配符粒子**表示了一套元素或元素序列。

其它更专门的schema component包括annotations, assertions, notations, 以及包含了schema整体信息的**schema component**.



## 数据类型

简单数据类型（simple type）包含了可以出现在元素或属性的文本值。这是XSD与DTD的最大区别。

XSD提供了一套19个基本数据类型：

- `anyURI`
- `base64Binary`
- `boolean`
- `date`
- `dateTime`
- `decimal`
- `double`
- `duration`
- `float`
- `hexBinary`
- `gDay`
- `gMonth`
- `gMonthDay`
- `gYear`
- `gYearMonth`
- `NOTATION`
- `QName`
- `string`
- `time`).

可以从这些基本数据类型通过三种机制构建三种数据类型：

- restriction (减少值集的范围),
- list (允许一个值的序列),
- union (允许从几个数据类型中选择值).

XSD规范定义了25个导出数据类型。用户可以在schema中进一步定义自己的导出类型。

Restriction机制包括指出最大最小值、[正则表达式](https://zh.wikipedia.org/wiki/正则表达式)、限制字符串的长度、限制十进制数的位数等。XSD 1.1又增加了assertions, 即通过一个[XPath 2.0]]表达式给出任意约束的能力。

复杂数据类型描述了一个元素的许可内容。包括这个元素、属性、子元素的许可内容。复杂类型定义由一套属性使用与一个内容模型组成。内容模型可以是：

- 只有元素的内容（element-only content）, 不允许有文本（但可以有空白符或者子元素可以有文本）;
- 简单内容（simple content）, 允许有文本，不允许有子元素;
- 空内容（empty content）, 文本与子元素都不被允许;
- 混合内容（mixed content）, 文本与子元素都可以有.

复杂数据类型可以从别的复杂类型导出：

- restriction方法，不允许基类型允许的一些元素、属性或者值
- extension方法，允许额外的属性或元素出现。

XSD 1.1又增加了assertion方法来约束复杂类型, 即通过一个[XPath 2.0]]表达式必须求值为真



## Schema 既验信息集（Post-Schema-Validation Infoset）

基于 Schema 的验证完成后，可以按照 Schema 所隐含的数据模型来表达文档的结构与内容。XML Schema 数据模型包括：

- 词汇（元素与属性名称集）
- 内容模型（关联与结构）
- 数据类型

这些信息的集合即为 Schema 既验信息集（Post-Schema-Validation Infoset （PSVI））。对于有效的 XML，PSVI 给它赋以特定的“类型”，从而便于以对象方式来处理整个文档，并应用面向对象程序设计（OOP）范式。



## XML Schema的次要用途

XML Schema的主要用途是形式描述XML文档，然而最终的schema除了简单验证文档外还有许多其他用途。

### 代码生成

Schema可用于生成代码，这称作{[tsl|en|XML Data Binding}}。这些代码允许XML文档的内容作为编程环境中的对象。

### XML文件结构文档的生成

Schema可用于产生人可读的文档来描述一个XML文件的结构。这在作者利用了标记元素（annotation element）时非常有用。



## 示范

一个Schema的简易示例，描述某个指定的国家，是这样的：

```xml
<xs:schema
 xmlns:xs="http://www.w3.org/2001/XMLSchema">
   <xs:element name="country" type="Country">
     <xs:complexType name="Country">
      <xs:sequence>
       <xs:element name="name" type="xs:string"/>
       <xs:element name="population" type="xs:decimal"/>
      </xs:sequence>
     </xs:complexType>
   </xs:element>
</xs:schema>
```

一份遵从这个视图的XML文件：

```xml
<country
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:noNamespaceSchemaLocation="country.xsd">
  <name>France</name>
  <population>59.7</population>
</country>
```



# DTD和XSD的区别和联系

**以下内容转载至知乎-[wuxinliulei](https://www.zhihu.com/question/38843167/answer/78782017)**

DTD(Document Type Definition) 是一套关于标记符的语法规则。
它是XML1.0版规格的一部分,是XML文件的验证机制,属于XML文件组成的一部分。DTD 是一种保证XML文档格式正确的有效方法，可通过比较XML文档和DTD文件来看文档是否符合规范，元素和标签使用是否正确。XML文件提供应用程序一个 数据交换的格式,DTD正是让XML文件能成为数据交换标准,因为不同的公司只需定义好标准DTD,各公司都能依DTD建立XML文件,并且进行验证,如 此就可以轻易的建立标准和交换数据，这样满足了网络共享和数据交互。DTD文件是一个ASCII文本文件，后缀名为.dtd。

## 1.为什么需要dtd，xsd 这种xml文档定义描述？

对于一个格式良好的XML文档，我们只能保证这个文档的格式符合XML规范，但是元素与元素之间的关系、元素与属性的关系，属性的取值是否正确，我们就无法得知了。对于一个格式良好的文档，如果仅仅是在有限的应用中使用，或者作为数据的存储传输，那么也能很好的满足我们的应用。但是如果要让其他用户理解你的XML文档，或者和其他的应用进行数据交换，那么就有必要提供一种机制，来保证我们所写的XML文档和别人所写的XML文档其结构是相同的，元素与元素之间的关系是正确的，属性的取值也是符合要求的。



## **2.在XML当中引入DTD有哪些方式？**

    **我们可以直接在XML文档中定义DTD，也可以通过URI引用外部的DTD文件，或者同时采用这两种方式。**
     ### ①XML文档中内部定义DTD
      内部的 DOCTYPE 声明，通过下面的语法包装在一个 DOCTYPE 声明中： 

```text
   <!DOCTYPE 根元素 [元素声明]> 　
```

　　带有 DTD 的 XML 文档实例 　

```xml-dtd
<?xml version="1.0"?> 　
<!DOCTYPE note [ 　
　<!ELEMENT note (to,from,heading,body)> 　
　<!ELEMENT to (#PCDATA)> 　
　<!ELEMENT from (#PCDATA)> 　
　<!ELEMENT heading (#PCDATA)> 　
　<!ELEMENT body (#PCDATA)> 　　
]> 　

<note>
     <to>Tove</to> 　　
     <from>Jani</from> 　
   　<heading>Reminder</heading> 　
   　<body>Don't forget me this weekend</body> 　
</note>
```

**以上 DTD 解释如下：**

!DOCTYPE note (第二行)定义此文档是 note 类型的文档。 　
!ELEMENT note (第三行)定义 note 元素有四个元素："to、from、heading,、body" 　
!ELEMENT to (第四行)定义 to 元素为 "#PCDATA" 类型 　　
!ELEMENT from (第五行)定义 from 元素为 "#PCDATA" 类型 　
!ELEMENT heading (第六行)定义 heading 元素为 "#PCDATA" 类型 　
!ELEMENT body (第七行)定义 body 元素为 "#PCDATA" 类型

   再举一个例子：

```xml
<?xml version="1.0" encoding="gb2312" standalone="yes">
<!DOCTYPE greeting [
   <!ELEMENT greeting (#PCDATA)>
 ]>
```

  文档类型声明由<! 开始，后面紧跟一个关键字DOCTYPE，然后是文档根元素的名称，接下来是标记生命块，标记声明块是放在左中括号 [ 和右中括号 ] 之间的，由一个或者多个标记声明构成，最后由> 结束。
   在DTD当中，所有关键字都是大写的，就像在这里看到的ELEMENT、#PCDATA一样，在后面我们还会看到其他的关键字。不过在DTD中定义的元素和属性的大小写是可以任意指定的，但是要注意，XML文档是大小写相关的，所以一旦给一个元素命名吗，那么整个文档中都要使用相同的大小写。例如：greeting 和Greeting是两个不同的元素名。

     在XML文档中定义DTD，比较直观，修改也比较方便，而且不用担心XML处理器找不到DTD，但是它也有一些缺点：
    1.在文档中定义DTD会导致文档本身的长度增加，在传输数据市，即使不需要验证文档的有效性，这些声明也会随文档一起传输。
    2.如果多个XML文档需要共用一个DTD，我们就需要在每个文档中加入DTD，这是相当繁琐的。
要解决上面两个问题，我们将dtd放到一个单独的文件中去定义，在XML文档中，通过URI外部引用
（有没有发现写程序的时候的#include 和import也有同样的功效重用代码）

### ② 外部文档声明
     假如 DTD 位于 XML 源文件的外部，那么它应通过下面的语法被封装在一个 DOCTYPE 定义中： 
`<!DOCTYPE 根元素 SYSTEM "外部DTD文件的URI"> `　　
**SYSTEM关键字表示文档使用的是私有的DTD文件**，“外部DTD文件的URI”可以是相对URI或者绝对URI，相对URI是相对文档类型声明所在文档的位置。
比如

```text
<!DOCTYPE greeting SYSTEM "hello.dtd">
```

我们将DTD的定义放到了hello.dtd文件中，注意要将hello.dtd放在和XML文档同一目录下，这样XML处理器才能找到这个文件。在给DTD文件取名字的时候，文件名可以随便取，但扩展名一般为.dtd
如果位于不同位置的多个XML文档要使用同一个DTD，我们可以使用绝对URI来指明DTD文件的地址。假定hello.dtd位于

```text
 http://www.guowuxin.org/xml/hello.dtd
```

 我们可以在文档类型声明中使用此URI

```text
<!DOCTYPE greeting SYSTEM "http://www.guowuxin.org/xml/hello.dtd">
```

如果是一种企业或者行业领域标准，则不建议使用SYSTEM，而是使用public修饰
`<!DOCTYPE 根元素的名字 PUBLIC “DTD的名称" “外部DTD文件的URI">`
PUBLIC关键字用于声明公共的DTD，并且这个DTD还有一个名称，“DTD的名称” 也称为公共标识符

```text
<!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd">
```

比如上面是servlet2.3中web.xml的DTD
根元素web-app

```text
PUBILC "-//所有者//DTD文档描述的类型//ISO639语言标识符" “外部DTD文件URI”
```



下面这个 XML 文档和上面的第一个 XML 文档相同，但是拥有一个外部的 DTD: 　

```xml-dtd
<?xml version="1.0"?> 　
<!DOCTYPE note SYSTEM "note.dtd">
<note> 　
 　<to>Tove</to> 　
 　<from>Jani</from> 　
 　<heading>Reminder</heading> 　
 　<body>Don't forget me this weekend!</body> 　
</note> 　
```


　这是包含 DTD 的 "note.dtd" 文件： 　

```xml-dtd
<!ELEMENT note (to,from,heading,body)> 　
<!ELEMENT to (#PCDATA)> 　　
<!ELEMENT from (#PCDATA)> 　
<!ELEMENT heading (#PCDATA)> 　
<!ELEMENT body (#PCDATA)> 
```



通过 DTD，每一个 XML 文件均可携带一个有关其自身格式的描述，独立的团体可一致地使用某个标准的 DTD 来交换数据。应用程序也可使用某个标准的 DTD 来验证从外部接收到的数据。 　
   还可以使用 DTD 来验证自身的数据。



## 3.DTD的优势

每一个XML文档都可携带一个DTD，用来对该文档格式进行描述，测试该文档是否为有效的XML文档。
既然DTD有外部和内部之分，当然就可以为某个独 立的团体定义一个公用的外部DTD，那么多个XML文档就都可以共享使用该DTD，使得数据交换更为有效。甚至在某些文档中还可以使内部DTD和外部 DTD相结合。
在应用程序中也可以用某个DTD来检测接收到的数据是否符合某个标准。 
对于XML文档而言，虽然DTD不是必须的，但它为文档的编制带来了方便。加强了文档标记内参数的一致性，使XML语法分析器能够确认文档。

如果不使用DTD来对XML文档进行定义，那么XML语法分析器将无法对该文档进行确认。 　　

每个XML文档都只有一个根元素，其它的子元素都包含在该根元素中。
因此在DTD中对根元素的声明是必不可少的。

元素声明的一般形式如下： 　

```text
<!DOCTYPE root[ 　　<!-- 子元素 --> ]> 　
```

DOCTYPE是“document type”（文档类型）的简写，DOCTYPE声明必须放在文档最顶部，在所有代码和标识之上，DOCTYPE声明是必不可少的关键组成部分。DTD语法 要求DOCTYPE必须要大写，而且DOCTYPE和元素之间必须要有空格隔开，**如在以上代码中DOCTYPE和根元素root之间要有空格隔开。**



## 4.DTD的缺陷

利用DTD验证有效性的解析器，就能够立即对文档的完整性进行可靠的检查。DTD虽然比较实用，但DTD也有不少的缺陷。 　　

1. DTD有自己的特殊语法，其本身不是XML文档； 　
2. DTD只提供了有限的数据类型，用户无法自定义类型； 　
3. DTD不支持域名机制。

**servlet标准在2.5开始就放弃使用dtd，改用了xsd**