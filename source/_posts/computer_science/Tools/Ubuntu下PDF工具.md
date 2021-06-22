---
title: Ubuntu中的PDF工具的使用
date: 2021-06-21 15:00
# updated: 0000-00-00 10:00
mathjax: true
meta:
    date: true
categories: 
    - Computer Science
    - Tools
tags:
    - PDF
---

Ubuntu系统有两款是十分强大的PDF编辑工具，简单记录其用途及使用方法

---

<!-- more -->

## 1. pdfcrop

pdfcrop是随LaTeX安装的一款**专用于去除PDF白边**的工具，其输入输出都为PDF文件。
```shell
pdfcrop input.pdf output.pdf
```

## 2. ImageMagick

ImageMagick属于Ubuntu20自带的图像编辑工具，可以轻松将图像转换成各种格式。
当需要将PDF转换为png等像素图时，可以使用如下命令

```shell
convert input.pdf output.png # 生成普通像素png图片
convert -verbose -colorspace RGB -resize 1800 -interlace none -density 300 -quality 100 input.pdf output.png # 生成高清png
```

转换PDF时如遇到类似：
```shell
convert-im6.q16: attempt to perform an operation not allowed by the security policy `PDF' @ error/constitute.c/IsCoderAuthorized/408.
```
则需要修改其配置文件`/etc/ImageMagick-6/policy.xml`，修改PDF配置一行(在最后几行内)，将
```shell
<policy domain="coder" rights="none" pattern="PDF" />
```
改为
```shell
<policy domain="coder" rights="read|write" pattern="PDF" />
<policy domain="coder" rights="read|write" pattern="LABEL" />
```