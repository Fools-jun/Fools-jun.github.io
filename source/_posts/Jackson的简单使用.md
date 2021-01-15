---
title: Jackson的简单使用
copyright: true
mathjax: true
categories:
  - 笔记
  - 后端
tags:
  - Jackson
abbrlink: 4168
date: 2021-01-11 11:35:56
---

Jackson可以轻松的将Java对象转换成json对象和xml文档，同样也可以将json、xml转换成Java对象ObjectMapper类是Jackson库的主要类。它称ObjectMapper的原因是因为它将JSON映射到Java对象（反序列化），或将Java对象映射到JSON（序列化）。它使用JsonParser和JsonGenerator的实例实现JSON实际的读/写。

<!-- less -->

博客转载至[谁在烽烟彼岸](https://www.jianshu.com/p/67b6da565f81)

## 简介

Jackson可以轻松的将Java对象转换成json对象和xml文档，同样也可以将json、xml转换成Java对象ObjectMapper类是Jackson库的主要类。它称ObjectMapper的原因是因为它将JSON映射到Java对象（反序列化），或将Java对象映射到JSON（序列化）。它使用JsonParser和JsonGenerator的实例实现JSON实际的读/写。



## Maven依赖

```
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <version>2.9.6</version>
</dependency>

<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-annotations</artifactId>
  <version>2.9.6</version>
</dependency>

<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.9.6</version>
</dependency>
```



## Jackson如何将JSON字段与Java字段匹配

- 方式一: Jackson通过将JSON字段的名称与Java对象中的getter和setter方法相匹配，将JSON对象的字段映射到Java对象中的字段。Jackson删除了getter和setter方法名称的“get”和“set”部分，并将剩余名称的第一个字符转换为小写。
- 方式二: Jackson还可以通过java反射进行匹配
- 方式三: 通过注解或者其它方式进行自定义的序列化和反序列化程序。



## 通过ObjectMapper进行转换

### 转Java对象

1. 将json串转换成对象

   ```java
   ObjectMapper objectMapper = new ObjectMapper();
   String carJson = "{ \"brand\" : \"Mercedes\", \"doors\" : 5 }";
   Car car = objectMapper.readValue(carJson, Car.class);
   ```

2. 通过reader对象将json串转换成对象

   ```java
   ObjectMapper objectMapper = new ObjectMapper();
   String carJson =  "{ \"brand\" : \"Mercedes\", \"doors\" : 4 }";
   Reader reader = new StringReader(carJson);
   Car car = objectMapper.readValue(reader, Car.class);
   ```

3. 将某个文件中的内容转换成对象

   ```java
   ObjectMapper objectMapper = new ObjectMapper();
   File file = new File("data/car.json");
   Car car = objectMapper.readValue(file, Car.class);
   ```

4. 将某个url中的文件的内容转换成对象

   ```java
   ObjectMapper objectMapper = new ObjectMapper();
   URL url = new URL("file:data/car.json");
   Car car = objectMapper.readValue(url, Car.class);
   ```

5. 将InputStream中的内容转换成对象

   ```java
   ObjectMapper objectMapper = new ObjectMapper();
   InputStream input = new FileInputStream("data/car.json");
   Car car = objectMapper.readValue(input, Car.class);
   ```

6. 将byte[]中内容转换成对象

   ```java
   ObjectMapper objectMapper = new ObjectMapper();
   String carJson =  "{ \"brand\" : \"Mercedes\", \"doors\" : 5 }";
   byte[] bytes = carJson.getBytes("UTF-8");
   Car car = objectMapper.readValue(bytes, Car.class);
   ```

7. 将json串转换成对象数组

   ```java
   String jsonArray = "[{\"brand\":\"ford\"}, {\"brand\":\"Fiat\"}]";
   ObjectMapper objectMapper = new ObjectMapper();
   Car[] cars2 = objectMapper.readValue(jsonArray, Car[].class);
   ```

8. 将json串转换成具体类型的List

   ```java
   String jsonArray = "[{\"brand\":\"ford\"}, {\"brand\":\"Fiat\"}]";
   ObjectMapper objectMapper = new ObjectMapper();
   List<Car> cars1 = objectMapper.readValue(jsonArray, new TypeReference<List<Car>>(){});
   ```

9. 将json串转换成具体类型的Map

   ```java
   String jsonObject = "{\"brand\":\"ford\", \"doors\":5}";
   ObjectMapper objectMapper = new ObjectMapper();
   Map<String, Object> jsonMap = objectMapper.readValue(jsonObject,
       new TypeReference<Map<String,Object>>(){});
   ```

   

### 转Json

ObjectMapper write有三个方法:

> - writeValue()
> - writeValueAsString()
> - writeValueAsBytes()

```java
ObjectMapper objectMapper = new ObjectMapper();
Car car = new Car();
car.brand = "BMW";
car.doors = 4;
//写到文件中
objectMapper.writeValue( new FileOutputStream("data/output-2.json"), car);
//写到字符串中
String json = objectMapper.writeValueAsString(car);
```





## 读取和写入其他数据格式

使用Jackson可以读取和写入除JSON之外的其他数据格式：

> - CBOR
> - MessagePack
> - YAML

其中这些数据格式比JSON更紧凑，因此在存储时占用的空间更少，并且读取和写入速度比JSON更快。

### 读写CBOR

CBOR是一种二进制数据格式，它与JSON兼容，但比JSON更紧凑，因此读写速度更快。Jackson ObjectMapper可以像读写JSON一样读写CBOR。为了使用Jackson读取和写入CBOR，您需要为项目添加额外的Maven依赖项。介绍了添加Jackson CBOR Maven依赖关系：

```java
import com.fasterxml.jackson.core.JsonProcessingException; 
import com.fasterxml.jackson.databind.ObjectMapper; 
import com.fasterxml.jackson.dataformat.cbor.CBORFactory; 

public class CborJacksonExample {
    public static void main(String[] args) {
        ObjectMapper objectMapper = new ObjectMapper(new CBORFactory());
        Employee employee = new Employee("John Doe", "john@doe.com");
        byte[] cborBytes = null;
        try {
            cborBytes = objectMapper.writeValueAsBytes(employee);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
            // normally, rethrow exception here - or don't catch it at all.
        }
        try {
            Employee employee2 = objectMapper.readValue(cborBytes, Employee.class);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



### 读写MessagePack

MessagePack是一种文本数据格式，与JSON兼容，但更紧凑，因此读写速度更快。Jackson ObjectMapper可以像读写JSON一样读写MessagePack。为了使用Jackson读写MessagePack，您需要为项目添加额外的Maven依赖项：

```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.msgpack.jackson.dataformat.MessagePackFactory;

import java.io.IOException;

public class MessagePackJacksonExample {
    public static void main(String[] args) {
        ObjectMapper objectMapper = new ObjectMapper(new MessagePackFactory());

        Employee employee = new Employee("John Doe", "john@doe.com");

        byte[] messagePackBytes = null;
        try {
            messagePackBytes = objectMapper.writeValueAsBytes(employee);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
            // normally, rethrow exception here - or don't catch it at all.
        }

        try {
            Employee employee2 = objectMapper.readValue(messagePackBytes, Employee.class);
            System.out.println("messagePackBytes = " + messagePackBytes);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```



### 读写YAML

YAML是一种文本数据格式，类似于JSON，但使用不同的语法。Jackson ObjectMapper可以像读写JSON一样读写YAML。为了使用Jackson读取和写入YAML，您需要为项目添加额外的Maven依赖项：

```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.dataformat.yaml.YAMLFactory;

import java.io.IOException;

public class YamlJacksonExample {

    public static void main(String[] args) {
        ObjectMapper objectMapper = new ObjectMapper(new YAMLFactory());

        Employee employee = new Employee("John Doe", "john@doe.com");

        String yamlString = null;
        try {
            yamlString = objectMapper.writeValueAsString(employee);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
            // normally, rethrow exception here - or don't catch it at all.
        }

        try {
            Employee employee2 = objectMapper.readValue(yamlString, Employee.class);

            System.out.println("Done");
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```



## ObjectMapper的设置

```java
ObjectMapper objectMapper = new ObjectMapper();
//去掉默认的时间戳格式     
objectMapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
//设置为东八区
objectMapper.setTimeZone(TimeZone.getTimeZone("GMT+8"));
// 设置输入:禁止把POJO中值为null的字段映射到json字符串中
objectMapper.configure(SerializationFeature.WRITE_NULL_MAP_VALUES, false);
 //空值不序列化
objectMapper.setSerializationInclusion(JsonInclude.Include.NON_NULL);
//反序列化时，属性不存在的兼容处理
objectMapper.getDeserializationConfig().withoutFeatures(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
//序列化时，日期的统一格式
objectMapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));
//序列化日期时以timestamps输出，默认true
objectMapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
//序列化枚举是以toString()来输出，默认false，即默认以name()来输出
objectMapper.configure(SerializationFeature.WRITE_ENUMS_USING_TO_STRING,true);
//序列化枚举是以ordinal()来输出，默认false
objectMapper.configure(SerializationFeature.WRITE_ENUMS_USING_INDEX,false);
//类为空时，不要抛异常
objectMapper.configure(SerializationFeature.FAIL_ON_EMPTY_BEANS, false);
//反序列化时,遇到未知属性时是否引起结果失败
objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
 //单引号处理
objectMapper.configure(JsonParser.Feature.ALLOW_SINGLE_QUOTES, true);
//解析器支持解析结束符
objectMapper.configure(JsonParser.Feature.ALLOW_UNQUOTED_CONTROL_CHARS, true);
```



## 自定义解析器

ObjectMapper 可以通过自定义解析器来定义解析方法
以下是自定义的反序列化的方法

```java
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.DeserializationContext;
import com.fasterxml.jackson.databind.JsonDeserializer;
import com.fasterxml.jackson.databind.JsonNode;
import org.springframework.data.geo.Point;

import java.io.IOException;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class Point_Fastjson_Deserialize extends JsonDeserializer<Point> {

    @Override
    public Point deserialize(JsonParser jsonParser, DeserializationContext deserializationContext) throws IOException, JsonProcessingException {
        JsonNode node = jsonParser.getCodec().readTree(jsonParser);
        Iterator<JsonNode> iterator = node.get("coordinates").elements();
        List<Double> list = new ArrayList<>();
        while (iterator.hasNext()) {
            list.add(iterator.next().asDouble());
        }
        return new Point(list.get(0), list.get(1));
    }
}
```





## 工具类

```java
package com.pzx.utils;

import com.fasterxml.jackson.annotation.JsonInclude.Include;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.core.JsonParser.Feature;
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.DeserializationFeature;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import com.fasterxml.jackson.databind.node.ArrayNode;
import com.fasterxml.jackson.databind.node.JsonNodeFactory;
import com.fasterxml.jackson.databind.node.ObjectNode;
import com.fasterxml.jackson.databind.type.CollectionType;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.net.URL;
import java.text.SimpleDateFormat;
import java.util.*;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

/**
 * @ClassName MapperUtils
 * @Description TODO jackSon转换工具类
 * @Author JunPzx
 * @CreateTime 2021/01/11 10:04
 * @Version 1.0
 */
public class MapperUtils {

    static final Logger LOG = LoggerFactory.getLogger(MapperUtils.class);

    private final static ObjectMapper MAPPER = new ObjectMapper();

    static {
        //设置时区
        MAPPER.setTimeZone(TimeZone.getTimeZone("GMT+8"));
        // 设置是否将日期写入时间戳
        MAPPER.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
        // 序列化的时候序列对象的不为Null的属性
        MAPPER.setSerializationInclusion(Include.NON_NULL);
        // 如果是空对象的时候,不抛异常
        MAPPER.configure(SerializationFeature.FAIL_ON_EMPTY_BEANS, false);
        // 美化输出，转换为格式化的json(默认为false,不美化)
        MAPPER.configure(SerializationFeature.INDENT_OUTPUT, false);
        // 取消时间的转化格式,默认是时间戳,可以取消,同时需要设置要表现的时间格式
        MAPPER.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);
        MAPPER.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));
        // 反序列化的时候如果多了其他属性,不抛出异常
        MAPPER.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        // 允许单引号（非标准）
        MAPPER.configure(Feature.ALLOW_SINGLE_QUOTES, true);
    }

    private MapperUtils() {

    }

    /**
     * 将obj转换成json
     *
     * @param entity 实体对象
     * @param <T>    泛型
     * @return json
     */
    public static <T> String obj2json(T entity) {
        String json = null;
        try {
            json = MAPPER.writeValueAsString(entity);
        } catch (JsonProcessingException var3) {
            var3.printStackTrace();
        }
        return json;
    }

    /**
     * 将obj转换成str
     *
     * @param entity 实体对象
     * @param <T>    泛型
     * @return str
     */
    public static <T> String obj2str(T entity) {
        return obj2json(entity);
    }

    /**
     * 将obj转换成byte[]
     *
     * @param entity 对象
     * @param <T>    泛型
     * @return byte[]
     * @throws Exception 异常
     */
    public static <T> byte[] obj2jsonBytes(T entity) throws Exception {
        return MAPPER.writeValueAsBytes(entity);
    }

    /**
     * 将obj转换成node
     *
     * @param entity 对象
     * @param <T>    泛型
     * @return JsonNode
     */
    public static <T> JsonNode obj2node(T entity) {
        return MAPPER.valueToTree(entity);
    }

    /**
     * 将obj写入file中
     *
     * @param filepath 写入的文件路径
     * @param entity   需要写入的实体
     * @param <T>      泛型,实体类型
     * @return boolean(true : Success, false : fail)
     * @throws Exception 异常
     */
    @SuppressWarnings("all")
    public static <T> boolean write2jsonFile(String filepath, T entity) throws Exception {
        File file = new File(filepath);
        if (!file.exists()) {
            try {
                file.createNewFile();
            } catch (IOException var4) {
                var4.printStackTrace();
                return false;
            }
        }
        return write2jsonFile(new File(filepath), entity);
    }

    /**
     * 将obj写入file中
     *
     * @param file   写入的文件
     * @param entity 需要写入的实体
     * @param <T>    泛型,实体类型
     * @return boolean(true : Success, false : fail)
     * @throws Exception 异常
     */
    public static <T> boolean write2jsonFile(File file, T entity) throws Exception {
        try {
            MAPPER.writeValue(file, entity);
            return true;
        } catch (FileNotFoundException var3) {
            LOG.error("File not exists");
            var3.printStackTrace();
            return false;
        }
    }

    /**
     * 将json转换成obj
     *
     * @param json json字符串
     * @param type 需要转换的对象类型
     * @param <T>  泛型
     * @return 转换后的对象
     * @throws Exception 异常
     */
    public static <T> T json2obj(String json, Class<T> type) throws Exception {
        return MAPPER.readValue(json, type);
    }

    /**
     * 将str转换成obj
     *
     * @param json 字符串
     * @param type 需要转换的对象类型
     * @param <T>  泛型
     * @return 转换后的对象
     * @throws Exception 异常
     */
    public static <T> T str2obj(String json, Class<T> type) throws Exception {
        return json2obj(json, type);
    }

    /**
     * 将json字符串转换成map
     *
     * @param json 需要转换的json字符串
     * @return map
     * @throws Exception 异常
     */
    @SuppressWarnings("unchecked")
    public static Map<String, Object> json2map(String json) throws Exception {
        return MAPPER.readValue(json, Map.class);
    }

    /**
     * 将json字符串转换成map<String,T>
     *
     * @param json 需要转换的json字符串
     * @param <T>  泛型
     * @param type value的类型
     * @return map
     * @throws Exception 异常
     */
    public static <T> Map<String, T> json2map(String json, Class<T> type) throws Exception {
        return MAPPER.readValue(json, new TypeReference<Map<String, T>>() {
        });
    }

    /**
     * 将map转换成obj
     *
     * @param map  需要转换的map
     * @param type 转换成的类型
     * @param <T>  泛型
     * @return 转换后的T类型的对象
     */
    @SuppressWarnings("rawtypes")
    public static <T> T map2obj(Map map, Class<T> type) {
        return MAPPER.convertValue(map, type);
    }

    /**
     * 将指定文件中的内容转换成指定的类型
     *
     * @param file 文件
     * @param type 需要转换成的类型
     * @param <T>  泛型
     * @return 转换后的对象
     * @throws Exception 异常
     */
    public static <T> T parseJSON(File file, Class<T> type) throws Exception {
        return MAPPER.readValue(file, type);
    }

    /**
     * 将指定url的内容转换成指定的类型
     *
     * @param url  链接
     * @param type 需要转换成的类型
     * @param <T>  泛型
     * @return 转换后的对象
     * @throws Exception 异常
     */
    public static <T> T parseJSON(URL url, Class<T> type) throws Exception {
        return MAPPER.readValue(url, type);
    }

    /**
     * 将json串转换成指定元素类型的List
     *
     * @param json json串
     * @param T    List的元素类型
     * @param <T>  泛型
     * @return 转换后的List
     * @throws Exception 异常
     */
    public static <T> List<T> json2list(String json, Class<T> T) throws Exception {
        CollectionType type = MAPPER.getTypeFactory().constructCollectionType(List.class, T);
        return MAPPER.readValue(json, type);
    }

    /**
     * 将str转换成指定元素类型的List
     *
     * @param str 字符串
     * @param T   List的元素类型
     * @param <T> 泛型
     * @return 转换后的List
     * @throws Exception 异常
     */
    public static <T> List<T> str2list(String str, Class<T> T) throws Exception {
        return json2list(str, T);
    }

    /**
     * 将json串转换成JsonNode
     *
     * @param json json
     * @return JsonNode
     * @throws Exception 异常
     */
    public static JsonNode json2node(String json) throws Exception {
        return MAPPER.readTree(json);
    }

    /**
     * 将str转换成JsonNode
     *
     * @param str str
     * @return JsonNode
     * @throws Exception 异常
     */
    public static JsonNode str2node(String str) throws Exception {
        return json2node(str);
    }

    /**
     * 判断是否格式正确的json串
     *
     * @param json 字符串
     * @return true: 是  false: 否
     */
    public static boolean isJsonString(String json) {
        try {
            MAPPER.readTree(json);
            return true;
        } catch (Exception var2) {
            if (LOG.isDebugEnabled()) {
                LOG.debug("check input string is json format;json : " + json + " ; exception;" + var2.getMessage());
            }
            return false;
        }
    }

    /**
     * 返回一个数组节点对象
     *
     * @return ArrayNode
     */
    public static ArrayNode arrayNode() {
        return JsonNodeFactory.instance.arrayNode();
    }

    /**
     * 返回一个Object节点对象
     *
     * @return ObjectNode
     */
    public static ObjectNode objectNode() {
        return JsonNodeFactory.instance.objectNode();
    }

}
```

