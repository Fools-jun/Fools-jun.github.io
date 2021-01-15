---
title: 接口文档工具之ShowDoc简单使用
copyright: true
mathjax: true
categories:
  - 使用说明
tags:
  - ShowDoc
abbrlink: 19954
date: 2021-01-07 15:18:17
---

一个非常适合IT团队的在线API文档、技术文档工具。

对于写API文档这件事，虽然说文本编辑软件或者接口管理软件能在某种程度上提高我们的效率，但我们依然可以试图借助技术的力量，更自动化地生成API文档，释放自己的生产力。
为此，showdoc官方提供了一种自动化解决方案。在代码里编写特定格式的程序注释<，然后showdoc通过读取这些注释来自动生成文档。由于这种方式不跟特定的语言耦合，因此它的使用范围相当广泛，可以支持c++、java、php、node等等常见的主流语言。
采用这种方式，尽管我们在第一次填写注释的时候可能会有些繁琐，但是它后期带来的可维护性是非常高的。代码变动后，不需要再额外登录showdoc，直接在代码里修改注释即可。同时自动化的脚本也可以加入持续集成或者某些自动化工程里，让“写API文档”这件事如”单元测试”般纳入工程工作流里面。

<!-- less -->



## 接口文档工具之ShowDoc简单使用

## 简介

对于写API文档这件事，虽然说文本编辑软件或者接口管理软件能在某种程度上提高我们的效率，但我们依然可以试图借助技术的力量，更自动化地生成API文档，释放自己的生产力。
为此，showdoc官方提供了一种自动化解决方案。在代码里编写特定格式的程序注释，然后showdoc通过读取这些注释来自动生成文档。由于这种方式不跟特定的语言耦合，因此它的使用范围相当广泛，可以支持c++、java、php、node等等常见的主流语言。
采用这种方式，尽管我们在第一次填写注释的时候可能会有些繁琐，但是它后期带来的可维护性是非常高的。代码变动后，不需要再额外登录showdoc，直接在代码里修改注释即可。同时自动化的脚本也可以加入持续集成或者某些自动化工程里，让“写API文档”这件事如”单元测试”般纳入工程工作流里面。



## 自动生成API文档

### windows下使用

0. 如果你的电脑无法运行.sh后缀的文件,那么请安装前置环境

   推荐下载git for windows：https://git-scm.com/download/win 下载后直接双击安装即可。
   如果从官网下载比较慢，可用考虑下载由第三方开发者维护的国内版(showdoc官方不保证其长期稳定)：
   https://npm.taobao.org/mirrors/git-for-windows/v2.17.0.windows.1/Git-2.17.0-64-bit.exe

1. 下载ShowDoc官方脚本https://www.showdoc.cc/script/showdoc_api.sh

2. 下载后，将showdoc_api.sh放在你的项目目录下。右击，选择编辑。

   - 修改文件开头的三个变量值
     - api_key : 需要登录showdoc进入某个项目的设置，点击开放API，便可以看到说明
     - api_token : 需要登录showdoc进入某个项目的设置，点击开放API，便可以看到说明
     - url : 如果是使用www.showdoc.cc ，则不需要修改。如果是使用开源版showdoc，则需要将地址改为http://xx.com/server/index.php?s=/api/open/fromComments ，其中，别忘记了url里含server目录。

3. 填写完毕，保存。然后直接双击运行，脚本会自动递归扫描本目录和子目录的所有文本代码文件，并生成API文档。



### Linux/Mac下使用

1. 先cd进入你的项目目录，命令行模式下输入：

   ```
   wget https://www.showdoc.cc/script/showdoc_api.sh
   ```

2. 编辑下载完成的文件

   ```\
   vi showdoc_api.sh
   ```

3. 修改文件开头的三个变量值

   - api_key : 需要登录showdoc进入某个项目的设置，点击开放API，便可以看到说明
   - api_token : 需要登录showdoc进入某个项目的设置，点击开放API，便可以看到说明
   - url : 如果是使用www.showdoc.cc ，则不需要修改。如果是使用开源版showdoc，则需要将地址改为http://xx.com/server/index.php?s=/api/open/fromComments ，其中，别忘记了url里含server目录。

4. 保存文件后。执行以下命令，脚本会自动递归扫描本目录和子目录的所有文本代码文件，并生成API文档。

   ```
    chmod +x showdoc_api.sh
   ./showdoc_api.sh
   ```



### 语法说明

```
/**
    * showdoc
    * @catalog 测试文档/用户相关
    * @title 用户登录
    * @description 用户登录的接口
    * @method get
    * @url https://www.showdoc.cc/home/user/login
    * @header token 可选 string 设备token 
    * @param username 必选 string 用户名 
    * @param password 必选 string 密码  
    * @param name 可选 string 用户昵称  
    * @return {"error_code":0,"data":{"uid":"1","username":"12154545","name":"吴系挂","groupid":2,"reg_time":"1436864169","last_login_time":"0"}}
    * @return_param groupid int 用户组id
    * @return_param name string 用户昵称
    * @remark 这里是备注信息
    * @number 99
    */
```

| 关键字          | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| `@catalog`      | 生成文档要放到哪个目录。如果只是二级目录，则直接写目录名字。如果是三级目录，而需要写二级目录/三级目录，即用`/`隔开。如”一层/二层/三层” |
| `@title`        | 表示生成的文档标题                                           |
| `@description`  | 是文档内容中对接口的描述信息                                 |
| `@method`       | 接口请求方式。一般是get或者post                              |
| `@url`          | 接口URL。不要在URL中使用&符号来传递参数。传递参数请写在参数表格中 |
| `@header`       | 可选。header说明。一行注释对应着表格的一行。用空格或者tab符号来隔开每一列信息。 |
| `@param`        | 参数表格说明。一行注释对应着表格的一行。用空格或者tab符号来隔开每一列信息。 |
| `@json_param`   | 可选。当请求参数是json的时候，可增加此标签。请把json内容压缩在同一行内。 |
| `@return`       | 返回内容。请把返回内容压缩在同一行内。如果是json，程序会自动进行格式化展示。 如果是非json内容，则原样展示。 |
| `@return_param` | 返回参数的表格说明。一行注释对应着表格的一行。用空格或者tab符号来隔开每一列信息。 |
| `@remark`       | 备注信息                                                     |
| `@number`       | 可选。文档的序号。                                           |



### 其它信息

官方文档:

> 请严格按照我们的语法来填写程序注释。如果格式不对，则可能引发未知的解析错误。
>
> 出于数据安全考虑，showdoc不允许直接通过代码删除文档。只能新增或者修改。所以，如果你要删除文档，请登录showdoc网页端完成。
>
> 本脚本只能通过特定的程序注释来生成文档，使用范围有限。如果你是想通过其他方式自由地生成文档，如通过word、通过博客软件等，请参考我们更自由的开放API：https://www.showdoc.cc/page/102098
>
> 如果你的项目下太多文件，则可能导致脚本扫描很慢。推荐把脚本放到需要生成注释的那个目录里。一般来讲，一个项目不会所有目录都需要生成文档的

