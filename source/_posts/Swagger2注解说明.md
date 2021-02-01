---
title: Swagger2注解说明
copyright: true
mathjax: true
categories:
  - 使用说明
tags:
  - Swagger2
abbrlink: 63365
date: 2021-01-18 10:16:03
---

Swagger2中的注解及其参数的作用

![](https://gitee.com/junpzx/blog-img/raw/master//img/20210118102344.png)

<!-- less -->



|        注解        | 属性                                                         | 备注                                                         |
| :----------------: | :----------------------------------------------------------- | :----------------------------------------------------------- |
|       `@Api`       | 用于类上，说明该类的作用。可以标记一个Controller类做为swagger 文档资源示例：`@Api`(value = "xxx", description = "xxx") |                                                              |
|                    | value                                                        | url的路径值                                                  |
|                    | tags                                                         | 如果设置这个值、value的值会被覆盖                            |
|                    | description                                                  | 对api资源的描述                                              |
|                    | basePath                                                     | 基本路径可以不配置                                           |
|                    | position                                                     | 如果配置多个Api 想改变显示的顺序位置                         |
|                    | produces                                                     | For example, "application/json, application/xml"             |
|                    | consumes                                                     | For example, "application/json, application/xml"             |
|                    | protocols                                                    | Possible values: http, https, ws, wss.                       |
|                    | authorizations                                               | 高级特性认证时配置                                           |
|                    | hidden                                                       | 配置为true 将在文档中隐藏                                    |
|  `@ApiOperation`   | 用于方法上，说明方法的作用，每一个url资源的定义示例：`@ApiOperation`(value = "xxx",httpMethod="POST", notes= "xxx",response=String.class) |                                                              |
|                    | value                                                        | url的路径值                                                  |
|                    | tags                                                         | 如果设置这个值、value的值会被覆盖                            |
|                    | notes                                                        | 对api资源的描述                                              |
|                    | position                                                     | 如果配置多个Api 想改变显示的顺序位置                         |
|                    | produces                                                     | For example, "application/json, application/xml"             |
|                    | consumes                                                     | For example, "application/json, application/xml"             |
|                    | protocols                                                    | Possible values: http, https, ws, wss.                       |
|                    | authorizations                                               | 高级特性认证时配置                                           |
|                    | hidden                                                       | 配置为true 将在文档中隐藏                                    |
|                    | response                                                     | 返回的对象                                                   |
|                    | responseContainer                                            | 这些对象是有效的 "List", "Set" or "Map".，其他无效           |
|                    | httpMethod                                                   | "GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS" and "PATCH" |
|                    | code                                                         | http的状态码 默认 200                                        |
|                    | extensions                                                   | 扩展属性                                                     |
|     @ApiParam      | 用于方法、参数、字段上，请求属性示例： public ResponseEntity<User> createUser(@RequestBody @ApiParam(value = "Created user object", required = true)  User user) |                                                              |
|                    | name                                                         | 属性名称                                                     |
|                    | value                                                        | 属性值                                                       |
|                    | defaultValue                                                 | 默认属性值                                                   |
|                    | allowableValues                                              | 可以不配置                                                   |
|                    | required                                                     | 是否属性必填                                                 |
|                    | access                                                       |                                                              |
|                    | allowMultiple                                                | 默认为false                                                  |
|                    | hidden                                                       | 隐藏该属性                                                   |
|                    | example                                                      | 示例                                                         |
|    @ApiResponse    | 用于方法上，响应配置示例：@ApiResponse(code = 400, message = "Invalid user supplied") |                                                              |
|                    | code                                                         | http的状态码                                                 |
|                    | message                                                      | 描述                                                         |
|                    | response                                                     | 默认响应类 Void                                              |
|                    | reference                                                    | 参考ApiOperation中配置                                       |
|                    | responseHeaders                                              | 参考 ResponseHeader 属性配置说明                             |
|                    | responseContainer                                            | 参考ApiOperation中配置                                       |
|   @ApiResponses    | 用于方法上，响应集配置示例： @ApiResponses({ @ApiResponse(code = 400, message = "Invalid Order") }) |                                                              |
|                    | value                                                        | 多个ApiResponse配置                                          |
|  @ResponseHeader   | 用于方法上，响应头设置示例：@ResponseHeader(name="head1",description="response head conf") |                                                              |
|                    | name                                                         | 响应头名称                                                   |
|                    | description                                                  | 头描述                                                       |
|                    | response                                                     | 默认响应类 Void                                              |
|                    | responseContainer                                            | 参考ApiOperation中配置                                       |
| @ApiImplicitParams | 用于方法上，包含一组参数说明                                 |                                                              |
| @ApiImplicitParam  | 用于方法上，用在@ApiImplicitParams注解中，指定一个请求参数的各个方面 |                                                              |
|                    | paramType                                                    | 参数放在哪个地方:     - header 参数在request headers 里边提交（@RequestHeader）- query 直接跟参数完成自动映射赋值（@RequestParam）- path 用于restful接口，以地址的形式提交数(@PathVariable)      - body 以流的形式提交 仅支持POST(@RequestBody)- form 以form表单的形式提交 仅支持POST |
|                    | name                                                         | 参数名                                                       |
|                    | value                                                        | 参数的汉字说明、解释                                         |
|                    | dataType                                                     | 参数类型，默认String，其它值dataType="Integer"  ，无用       |
|                    | required                                                     | 是否必要                                                     |
|                    | defaultValue                                                 | 参数的默认值                                                 |
|     @ApiModel      | 用于类上，描述一个Model的信息（这种一般用在post创建的时候，使用@RequestBody这样的场景，请求参数无法使用@ApiImplicitParam注解进行描述的时候 |                                                              |
| @ApiModelProperty  | 用于方法、字段上，描述一个model的属性                        |                                                              |
|     @ApiIgnore     | 用于类，属性，方法上，忽略某项api,使用@ApiIgnore             |                                                              |