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



# 使用文档

## 1.Maven中添加依赖

```
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



## 2.注入配置

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