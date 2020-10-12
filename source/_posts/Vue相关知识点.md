---
title: Vue相关知识点
date: 2020-09-29 14:59:24
categories:
- 笔记
- 前端
tags:
- Vue
---



学习Vue过程中，一些零碎的知识点:

- 数组中哪些方法是响应式的？



![https://res.cloudinary.com/junpzx/image/upload/v1600667958/Vue%E7%9B%B8%E5%85%B3/Vue%E5%85%A5%E9%97%A8/logo_j4jkbw.png](https://res.cloudinary.com/junpzx/image/upload/v1600667958/Vue相关/Vue入门/logo_j4jkbw.png)

<!-- less -->





# 数组中哪些方法是响应式的？

**注意：通过数组下标直接修改数组元素的值，界面不会发生改变，但是数组内的值已经修改，如果需要修改的时候是响应式的，那么可以通过以下方法：**

- **数组的splice方法**
- **vue.set(array,下标,值)**

 <table>
     <tr>
         <td>方法名</td>
         <td>作用</td>
         <td>语法</td>
         <td>是否是响应式</td>
     </tr>
     <tr>
         <td>push</td>
         <td>在数组最后追加元素</td>
         <td>array.push(元素1,元素2...)</td>
         <td>是</td>
     </tr>
     <tr>
         <td>pop</td>
         <td>移除数组中最后一位元素</td>
         <td>array.pop()</td>
         <td>是</td>
     </tr>
      <tr>
         <td>shift</td>
         <td>移除数组中第一位元素</td>
         <td>array.shift()</td>
         <td>是</td>
     </tr>
      <tr>
         <td>unshift</td>
         <td>在数组最前面添加元素</td>
         <td>array.unshift(元素1,元素2...)</td>
         <td>是</td>
     </tr>
      <tr>
         <td>splice</td>
         <td>删除元素/插入元素/替换元素</td>
         <td>array.splice(开始下标,需要删除的数量, 添加元素1,添加元素,...)</td>
         <td>是</td>
     </tr>
      <tr>
         <td>sort</td>
         <td>将数组进行排序</td>
         <td>array.sort(function)</td>
         <td>是</td>
     </tr>
      <tr>
         <td>reverse</td>
         <td>将数组内的元素进行反转</td>
         <td>array.reverse()</td>
         <td>是</td>
     </tr>
 </table>

