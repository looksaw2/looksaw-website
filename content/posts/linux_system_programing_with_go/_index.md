---
title: "Linux系统编程使用GO  # 文章标题（必填）
date: 2025-10-16T10:00:00+08:00   # 发布日期（必填，格式：YYYY-MM-DDTHH:MM:SS±HH:00）
description: "这是一些关于Linux内存和进程的内容"  # 文章描述（用于 SEO 和摘要）
tags: ["Hugo", "Blowfish", "Functional","博客"]  # 标签（可选，数组格式）
categories: ["技术"]  # 分类（可选）
cover: "feature.svg"    # 封面图（可选，若用文件夹格式，填文件名；否则填完整路径）
draft: false          # 是否为草稿（true 则不发布，默认 false）
---


1. 什么是系统编程
众所周知,Linux操作系统分成用户态和内核态，通常我们写的函数都位于用户态里面，如果需要访问硬件就需要从用户态转移到内核态，这就需要发起系统调用