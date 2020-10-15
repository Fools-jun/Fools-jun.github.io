---
title: Java报错整理
date: 2020-10-14 11:21:10
categories:
- 笔记
- 后端
tags:
- Java
copyright: true
---

整理在Java中遇到过的问题，可能该问题的场景有多种，此处只记录我遇到过的问题的正确解决方案，仅供大家参考

<!-- less -->

# 资源包报错

## 使用Junit4.12时报错

**报错信息：**

```java
java.lang.NoClassDefFoundError: org/hamcrest/SelfDescribing
```

**解决方案：**

1. 更换版本为4.10,因为从4.11版本开始，包中不在包含hamcrest，如果必须使用4.11及其之后的版本，请使用方案2
2. Junit4.12 + hamcrest-core-1.3

**下载链接：**

[junit4.10](https://pan.baidu.com/s/1Ki365BCzJE4YRdmdxFTboQ)

[junit4.11](https://pan.baidu.com/s/1P5kIyxILtKrs0ZB-b_RX_g)

[junit4.12](https://pan.baidu.com/s/1wTsBM-7QAe6BpY7VVAM1Kw)

[hamcrest-core-1.3](https://pan.baidu.com/s/1YXlqpT8zB6Ly0wF8XdX-xQ)

提取码为：0000



