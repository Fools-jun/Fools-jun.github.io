---
title: ECMAScript事件
date: 2020-09-28 17:02:46
categories:
- 笔记
- 前端
tags:
- ECMAScript
copyright: true
---

ECMAScript的常用事件

<!-- less -->

# 常见类别

## 资源事件

| 事件名称       | 何时触发                              |
| :------------- | :------------------------------------ |
| `error`        | 资源加载失败时。                      |
| `abort`        | 正在加载资源已经被中止时。            |
| `load`         | 资源及其相关资源已完成加载。          |
| `beforeunload` | window，document 及其资源即将被卸载。 |
| `unload`       | 文档或一个依赖资源正在被卸载。        |



## 网络事件

| 事件名称  | 何时触发               |
| :-------- | :--------------------- |
| `online`  | 浏览器已获得网络访问。 |
| `offline` | 浏览器已失去网络访问。 |



## 焦点事件

| 事件名称 | 何时触发                   |
| :------- | :------------------------- |
| `focus`  | 元素获得焦点（不会冒泡）。 |
| `blur`   | 元素失去焦点（不会冒泡）。 |



## WebSocket 事件

| 事件名称  | 何时触发                                           |
| :-------- | :------------------------------------------------- |
| `open`    | WebSocket 连接已建立。                             |
| `message` | 通过 WebSocket 接收到一条消息。                    |
| `error`   | WebSocket 连接异常被关闭（比如有些数据无法发送）。 |
| `close`   | WebSocket 连接已关闭。                             |



## 会话历史事件

| 事件名称   | 何时触发                                                     |
| :--------- | :----------------------------------------------------------- |
| `pagehide` | A session history entry is being traversed from.             |
| `pageshow` | A session history entry is being traversed to.               |
| `popstate` | A session history entry is being navigated to (in certain cases). |



## CSS动画事件

| 事件名称             | 何时触发                            |
| :------------------- | :---------------------------------- |
| `animationstart`     | 某个 CSS 动画开始时触发。           |
| `animationend`       | 某个 CSS 动画完成时触发。           |
| `animationiteration` | 某个 CSS 动画完成后重新开始时触发。 |



## CSS过渡事件

| 事件名称           | 何时触发                                                     |
| :----------------- | :----------------------------------------------------------- |
| `transitionstart`  | A [CSS transition](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transitions) has actually started (fired after any delay). |
| `transitioncancel` | CSS过渡被取消                                                |
| `transitionend`    | CSS过渡已经完成                                              |
| `transitionrun`    | A [CSS transition](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Transitions) has begun running (fired before any delay starts). |



## 表单事件

| 事件名称 | 何时触发       |
| :------- | :------------- |
| `reset`  | 点击重置按钮时 |
| `submit` | 点击提交按钮   |



## 打印事件

| 时间名称      | 何时触发             |
| :------------ | :------------------- |
| `beforeprint` | 打印机已经就绪时触发 |
| `afterprint`  | 打印机关闭时触发     |



## 文本写作事件

| Event Name          | Fired When                                                   |
| :------------------ | :----------------------------------------------------------- |
| `compositionstart`  | The composition of a passage of text is prepared (similar to keydown for a keyboard input, but works with other inputs such as speech recognition). |
| `compositionupdate` | A character is added to a passage of text being composed.    |
| `compositionend`    | The composition of a passage of text has been completed or canceled. |



## 视图事件

| Event Name         | Fired When                                                   |
| :----------------- | :----------------------------------------------------------- |
| `fullscreenchange` | An element was turned to fullscreen mode or back to normal mode. |
| `fullscreenerror`  | It was impossible to switch to fullscreen mode for technical reasons or because the permission was denied. |
| `resize`           | The document view has been resized.                          |
| `scroll`           | The document view or an element has been scrolled.           |



## 剪贴板事件

| Event Name | Fired When                                 |
| :--------- | :----------------------------------------- |
| `cut`      | 已经剪贴选中的文本内容并且复制到了剪贴板。 |
| `copy`     | 已经把选中的文本内容复制到了剪贴板。       |
| `paste`    | 从剪贴板复制的文本内容被粘贴。             |



## 键盘事件

| Event Name | Fired When                                              |
| :--------- | :------------------------------------------------------ |
| `keydown`  | 按下任意按键。                                          |
| `keypress` | 除 Shift、Fn、CapsLock 外的任意键被按住。（连续触发。） |
| `keyup`    | 释放任意按键。                                          |



## 鼠标事件

| Event Name          | Fired When                                                   |
| :------------------ | :----------------------------------------------------------- |
| `auxclick`          | A pointing device button (ANY non-primary button) has been pressed and released on an element. |
| `click`             | 在元素上按下并释放任意鼠标按键。                             |
| `contextmenu`       | 右键点击（在右键菜单显示前触发）。                           |
| `dblclick`          | 在元素上双击鼠标按钮。                                       |
| `mousedown`         | 在元素上按下任意鼠标按钮。                                   |
| `mouseenter`        | 指针移到有事件监听的元素内。                                 |
| `mouseleave`        | 指针移出元素范围外（不冒泡）。                               |
| `mousemove`         | 指针在元素内移动时持续触发。                                 |
| `mouseover`         | 指针移到有事件监听的元素或者它的子元素内。                   |
| `mouseout`          | 指针移出元素，或者移到它的子元素上。                         |
| `mouseup`           | 在元素上释放任意鼠标按键。                                   |
| `pointerlockchange` | 鼠标被锁定或者解除锁定发生时。                               |
| `pointerlockerror`  | 可能因为一些技术的原因鼠标锁定被禁止时。                     |
| `select`            | 有文本被选中。                                               |
| `wheel`             | 滚轮向任意方向滚动。                                         |



## 拖放事件

| Event Name  | Fired When                                                   |
| :---------- | :----------------------------------------------------------- |
| `drag`      | 正在拖动元素或文本选区（在此过程中持续触发，每 350ms 触发一次） |
| `dragend`   | 拖放操作结束。（松开鼠标按钮或按下 Esc 键）                  |
| `dragenter` | 被拖动的元素或文本选区移入有效释放目标区                     |
| `dragstart` | 用户开始拖动HTML元素或选中的文本                             |
| `dragleave` | 被拖动的元素或文本选区移出有效释放目标区                     |
| `dragover`  | 被拖动的元素或文本选区正在有效释放目标上被拖动 （在此过程中持续触发，每350ms触发一次） |
| `drop`      | 元素在有效释放目标区上释放                                   |



## 媒体事件

| Event Name       | Fired When                                                   |
| :--------------- | :----------------------------------------------------------- |
| `audioprocess`   | The input buffer of a [`ScriptProcessorNode`](https://developer.mozilla.org/zh-CN/docs/Web/API/ScriptProcessorNode) is ready to be processed. |
| `canplay`        | The browser can play the media, but estimates that not enough data has been loaded to play the media up to its end without having to stop for further buffering of content. |
| `canplaythrough` | The browser estimates it can play the media up to its end without stopping for content buffering. |
| `complete`       | The rendering of an [`OfflineAudioContext`](https://developer.mozilla.org/zh-CN/docs/Web/API/OfflineAudioContext) is terminated. |
| `durationchange` | The `duration` attribute has been updated.                   |
| `emptied`        | The media has become empty; for example, this event is sent if the media has already been loaded (or partially loaded), and the [`load()`](https://developer.mozilla.org/zh-CN/docs/XPCOM_Interface_Reference/NsIDOMHTMLMediaElement) method is called to reload it. |
| `ended`          | Playback has stopped because the end of the media was reached. |
| `loadeddata`     | The first frame of the media has finished loading.           |
| `loadedmetadata` | The metadata has been loaded.                                |
| `pause`          | Playback has been paused.                                    |
| `play`           | Playback has begun.                                          |
| `playing`        | Playback is ready to start after having been paused or delayed due to lack of data. |
| `ratechange`     | The playback rate has changed.                               |
| `seeked`         | A *seek* operation completed.                                |
| `seeking`        | A *seek* operation began.                                    |
| `stalled`        | The user agent is trying to fetch media data, but data is unexpectedly not forthcoming. |
| `suspend`        | Media data loading has been suspended.                       |
| `timeupdate`     | The time indicated by the `currentTime` attribute has been updated. |
| `volumechange`   | The volume has changed.                                      |
| `waiting`        | Playback has stopped because of a temporary lack of data.    |



## 进度事件

| Event Name  | Fired When                                                   |
| :---------- | :----------------------------------------------------------- |
| `abort`     | Progression has been terminated (not due to an error).       |
| `error`     | Progression has failed.                                      |
| `load`      | Progression has been successful.                             |
| `loadend`   | Progress has stopped (after "error", "abort" or "load" have been dispatched). |
| `loadstart` | Progress has begun.                                          |
| `progress`  | In progress.                                                 |
| `timeout`   | Progression is terminated due to preset time expiring.       |



## 存储事件

`change`
`storage`



## 更新事件

`checking`
`downloading`
`error`
`noupdate`
`obsolete`
`updateready`



## 值变化事件

`broadcast`
`CheckboxStateChange`
`hashchange`
`input`
`RadioStateChange`
`readystatechange`
`ValueChange`



## 未分类的事件

`invalid`
`message`
`message`
`open`
`show`



[更多事件](https://developer.mozilla.org/zh-CN/docs/Web/Events)