---
layout: post
title: 使用 FileReader 解析 txt 和 excel文件
subtitle: FileReader、xlsl 解析库
date: 2020-02-24
author: nolan
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
  - js
---

### FileReader

FileReader 对象可以一步读取文件内容，使用 File 对象或 Blob 对象指定要读取的文件或数据。

File 对象：可以是 input 元素上选择文件后返回的 FileList 对象，也可以是来自拖放操作生成的 DataTransfer 对象， 还可以是来自在一个 HTMLCanvasElement 上执行 mozGetAsFile 方法后返回的结果。

Blob 对象：表示一个不可变、原始数据的类文件对象，Blob 表示的不一定是 JavaScript 原生格式的数据。File 接口继承自 Blob 对象，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。Blob.size 表示包含数据大小（字节），Blob.type 表示该 Blob 对象的 Mime 类型。

#### 属性

- FileReader.error
  - 只读
  - 读取文件错误时
- FileReader.readyState
  - 常量名为 EMPTY，值为 0， 还没有加载任何数据；
  - 常量名为 LOADING，值为 1，数据正在加载；
  - 常量名为 DONE，值为 2，已完成全部的读取请求；
- FileReader.result
  - 结果
  - 只读

#### 方法

- readAsText
  - 常用来**读取 txt 文件**，一般会指定 utf-8 编码；
- readAsDataURL
  - result 属性中将包含一个 data：**URL 格式的 Base64 字符串**；
  - 常用来**读取本地图片**并展示图片；
- readAsBinaryString
  - result 属性中将包含所读取文件的**原始二进制数据**；
  - 如读取 excel 文件，并配合 xlsx 解析库解析；
- readAsArrayBuffer
  - result 属性中保存的将是被读取文件**的 ArrayBuffer 数据对象**；
- abort
  - 结束读取

#### 事件处理

- onabort
- onerror
- onload
  - 在读取操作完成时触发；
- onloadstart
- onloadend - 处理 loadend 事件。该事件在读取操作结束时（要么成功，要么失败）触发；
- onprogress

### 实战：基于 fileReader 和 xlsx 解析包解析 txt 和 excel 文件

```
function ParseDataFromFile({ onUpload: Function }) {
    const fileInstance = useRef() as any;


    // 从解析出的 excel 里提取 domain
    // ["A1='xls.com ", "B1=1231231312", "A2='tse.xls"]
    const parseDomain = (raw: string[]) => {

    }

    const parseExcel = (e) => {
        //当读取完成后回调这个函数,然后此时文件的内容存储到了result中,直接操作即可
        try {
            try {
                let data = e.target.result;
                var workbook = xlsx.read(data, {
                    type: 'binary'
                }), // 以二进制流方式读取得到整份excel表格对象
                    content = []; // 存储获取到的数据
            } catch (e) {
                console.log('文件类型不正确');
                return;
            }

            // 表格的表格范围，可用于判断表头是否数量是否正确
            let fromTo = '';
            // 遍历每张表读取
            for (let sheet in workbook.Sheets) {
                if (workbook.Sheets.hasOwnProperty(sheet)) {
                    content = content.concat(xlsx.utils.sheet_to_formulae(workbook.Sheets[sheet]));
                    break; // 如果只取第一张表，就取消注释这行
                }
            }

            console.log(content);
        } catch (e) {
            console.log('文件类型不正确', e);
            return;
        }
    }

    const parseTxt = (e) => {
        console.log(6666, e.target.result)
    }

    const uploadHandler = () => {
        const txtReader = new FileReader(), excelReader = new FileReader();
        const uploadFile = fileInstance.current.files[0],
            isTxt = uploadFile.name.indexOf('txt') !== -1;

        if (isTxt) {

            txtReader.onloadend = parseTxt;
            txtReader.readAsText(uploadFile, 'utf-8')
        } else {

            excelReader.onloadend = parseExcel;
            excelReader.readAsBinaryString(uploadFile)
        }
    }

    return <>
        <input ref={fileInstance} type="file" id="upload_file" onChange={uploadHandler.bind(this)} />
    </>
}
```
