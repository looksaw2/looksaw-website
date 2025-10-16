---
title: "设计模式"  # 文章标题（必填）
date: 2025-10-16T10:00:00+08:00   # 发布日期（必填，格式：YYYY-MM-DDTHH:MM:SS±HH:00）
description: "这是一些关于设计模式的内容"  # 文章描述（用于 SEO 和摘要）
tags: ["Hugo", "Blowfish", "Design_patterns","博客"]  # 标签（可选，数组格式）
categories: ["技术"]  # 分类（可选）
cover: "feature.svg"    # 封面图（可选，若用文件夹格式，填文件名；否则填完整路径）
draft: false          # 是否为草稿（true 则不发布，默认 false）
---

### 关于开闭原则
在面向对象编程领域中，开闭原则 (The Open/Closed Principle, OCP) 规定“软件中的对象（类，模块，函数等等）应该对于扩展是开放的，但是对于修改是封闭的”，这意味着一个实体是允许在不改变它的源代码的前提下变更它的行为。

下面我们来看具体的代码,可以简单的写出下面的代码:
```go
//Name的定义
type Name string

//Color的定义
type Color int 
const (
	Red Color = iota
	GREEN
	BLUE
)
//Size的定义
type Size int 
const (
	SMALL Size = iota
	MEDIUM 
	LARGE
	YUGA
)
//产品
type Product struct {
	Name Name
	Color Color
	Size Size
}

func NewProduct(name Name , color Color ,size Size) *Product {
	return &Product{
		Name: name,
		Color: color,
		Size: size,
	}
}
```