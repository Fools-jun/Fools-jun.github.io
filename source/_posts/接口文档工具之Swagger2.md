---
title: 接口文档工具之Swagger2
copyright: true
mathjax: true
abbrlink: 4810
date: 2021-01-18 10:30:09
categories:
- 使用说明
tags:
- Swagger2
---

Swagger 是一套围绕 OpenAPI 规范构建的开源工具，可以帮助您设计，构建，记录和使用 REST API。



Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。Swagger 的目标是对 REST API **定义一个标准的和语言无关的接口**，可让人和计算机无需访问源码、文档或网络流量监测就可以发现和理解服务的能力。当通过 Swagger 进行正确定义，用户可以理解远程服务并使用最少实现逻辑与远程服务进行交互。

![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118102344.png)

<!-- less -->

# 一、简介

相信无论是前端还是后端开发，都或多或少地被接口文档折磨过。前端经常抱怨后端给的接口文档与实际情况不一致。后端又觉得编写及维护接口文档会耗费不少精力，经常来不及更新。其实无论是前端调用后端，还是后端调用后端，都期望有一个好的接口文档。但是这个接口文档对于程序员来说，就跟注释一样，经常会抱怨别人写的代码没有写注释，然而自己写起代码起来，最讨厌的，也是写注释。所以仅仅只通过强制来规范大家是不够的，随着时间推移，版本迭代，接口文档往往很容易就跟不上代码了。

![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118141339.png)

发现了痛点就要去找解决方案。解决方案用的人多了，就成了标准的规范，这就是Swagger的由来。通过这套规范，你只需要按照它的规范去定义接口及接口相关的信息。再通过Swagger衍生出来的一系列项目和工具，就可以做到生成各种格式的接口文档，生成多种语言的客户端和服务端的代码，以及在线接口调试页面等等。这样，如果按照新的开发模式，在开发新版本或者迭代版本的时候，只需要更新Swagger描述文件，就可以自动生成接口文档和客户端服务端代码，做到调用端代码、服务端代码以及接口文档的一致性。

但即便如此，对于许多开发来说，编写这个yml或json格式的描述文件，本身也是有一定负担的工作，特别是在后面持续迭代开发的时候，往往会忽略更新这个描述文件，直接更改代码。久而久之，这个描述文件也和实际项目渐行渐远，基于该描述文件生成的接口文档也失去了参考意义。所以作为Java届服务端的大一统框架Spring，迅速将Swagger规范纳入自身的标准，建立了Spring-swagger项目，后面改成了现在的Springfox。通过在项目中引入Springfox，可以扫描相关的代码，生成该描述文件，进而生成与代码一致的接口文档和客户端代码。这种通过代码生成接口文档的形式，在后面需求持续迭代的项目中，显得尤为重要和高效。



# 二、使用文档

## 1.Maven中添加依赖

```xml
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
```



## 2注入配置

```java
package com.pzx.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.ParameterBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.schema.ModelRef;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Parameter;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.ArrayList;
import java.util.List;

/**
 * @ClassName Swagger2Config
 * @Description TODO Swagger2的配置文件
 * @Author JunPzx
 * @CreateTime 2021/01/18 10:43
 * @Version 1.0
 */ 
@Configuration
@EnableSwagger2
public class Swagger2Config {
    @Bean
    public Docket createOpenApi() {
        ParameterBuilder ticketPar = new ParameterBuilder();
        List<Parameter> pars = new ArrayList<>();
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .globalOperationParameters(pars)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.pzx.sys"))
                .paths(PathSelectors.any())
                .build().groupName("系统管理");
    }

    @Bean
    public Docket createUusApi() {
        ParameterBuilder ticketPar = new ParameterBuilder();
        List<Parameter> pars = new ArrayList<>();
        ticketPar.name("Authorization").description("Authorization")
                .modelRef(new ModelRef("string")).parameterType("header")
                .required(false).build();
        pars.add(ticketPar.build());
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .globalOperationParameters(pars)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.pzx.user"))
                .paths(PathSelectors.any())
                .build().groupName("统一用户资源管理");
    }


    protected ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("统一用户资源管理平台API接口文档")
                .version("1.0.0")
                .build();
    }
}
```



### 说明

- ApiInfo:api文档的基本信息
- Docket:配置框架的工作方式
  - apiInfo():配置基本说明信息
  - apis(): 配置api选择方式(包,类注解,方法注解)
  - paths():配置扫描路径(所有,正则匹配)





## 3.定义实例

```java
@RestController
@RequestMapping(value = "/test")
@Api(tags = "这个类的标签")
public class TestController {

    @ApiOperation(value = "描述接口作用", notes = "对接口的额外说明", response = String.class)
    @ApiImplicitParam(name = "te", value = "解析该参数作用..", required = true, dataType = "String", paramType = "path")
    @RequestMapping(value = "/ok",method = RequestMethod.GET)
    public String ok (String te) {
        return "ok";
    }

}
```





## 4.效果图

![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118113334.png)



## 5.注解说明

> @ApiOperation：用在请求的方法上，说明方法的用途、作用
> value=“说明方法的用途、作用”
> notes=“方法的备注说明”

> @ApiImplicitParam ：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面
> name：参数名
> value：参数的汉字说明、解释
> required：参数是否必须传
> paramType：参数放在哪个地方
>
> - header --> 请求参数的获取：@RequestHeader
> - query --> 请求参数的获取：@RequestParam
> - path（用于restful接口）–> 请求参数的获取：@PathVariable
> - body（不常用）
> - form（不常用）
>
> dataType：参数类型，默认String，其它值dataType=“Integer”
>
> defaultValue：参数的默认值

[Swagger2注解文档](https://www.junpzx.cn/posts/63365.html#more)





# 三、创建注释模板

1. 打开setting --> Editor --> Live Templates

   ![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118135946.png)

2. 点击右侧的加号,创建`userDefine分组`

3. 选中新建的分组,点击加号,选中创建模板

4. 编辑模板

   ```java
   @ApiOperation(value = "", notes = "",response=$resultType$.class,httpMethod="Get")
   @ApiImplicitParams(value = {$params$})
   ```

   ![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118135940.png)

   ```groovy
   groovyScript("def result=''; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+=' @ApiImplicitParam(name=\"' + params[i] + ((i < params.size() - 1) ? '\",value=\"\",required = true, dataType = \"参数类型\", paramType = \"header or body or form or path \"),\\n':'\",value=\"\",required = true, dataType = \"参数类型\", paramType = \"header or body or form or path \")')}; return result", methodParameters()) 
   ```

   ![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118135934.png)

5. 使用,在方法内部输入刚才设定的快捷代码,然后将生成的copy到方法上,修改对应的参数

   ![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118140903.png)

   ![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118140821.png)



