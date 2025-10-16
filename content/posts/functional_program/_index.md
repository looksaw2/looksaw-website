---
title: "函数式编程"  # 文章标题（必填）
date: 2025-10-16T10:00:00+08:00   # 发布日期（必填，格式：YYYY-MM-DDTHH:MM:SS±HH:00）
description: "这是一些关于函数式的内容"  # 文章描述（用于 SEO 和摘要）
tags: ["Hugo", "Blowfish", "Functional","博客"]  # 标签（可选，数组格式）
categories: ["技术"]  # 分类（可选）
cover: "feature.svg"    # 封面图（可选，若用文件夹格式，填文件名；否则填完整路径）
draft: false          # 是否为草稿（true 则不发布，默认 false）
---

一个计算中位数的函数
```charp
var arr = new[] { 6,5,4,3,2,1 };

var IsEvenNumber = (int x) => x % 2 == 0;

var sortedList = arr.OrderBy(x => x).ToList();
var sortedCount = sortedList.Count;

var median = IsEvenNumber(arr.Length) ?
    sortedList.Skip(sortedCount / 2 - 1).Take(2).Average()
    : sortedList.Skip(sortedCount / 2).Average();
Console.WriteLine(median.ToString());
```