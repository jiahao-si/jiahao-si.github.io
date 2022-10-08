---
layout: post
title: Content-Type
subtitle: 标记网络文件类型，方便浏览器解析
date: 2019-10-30
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - FullStack
---

### 作用

- 定义网络文件的类型和网页编码
- 决定浏览器以什么格式、什么编码读取这个文件（比如打开一个 php 网页，结果却是下载一个文件）

### 格式

```
// mime + 编码
Content-Type：text/html;charset=utf-8
Content-type:multipart/form-data;boundary=something
```

### 常见的 MIME 类型（媒体格式类型）

- text 开头
  - text/html html 格式
  - text/css css 文件
  - text/plain 纯文本格式
- image 开头
  - image/gif
  - image/jpeg
  - image/jpg
- application 开头
  - application/x-www-form-urlencoded **表单默认的提交数据的格式**
  - application/json JSON 数据格式
  - application/javascript js 文件
  - application/ecmascript js 文件
  - application/pdf pdf 文件
  - application/octet-stream 二进制流数据（如常见的**文件下载**）
- multipart/form-data **上传文件**时使用
