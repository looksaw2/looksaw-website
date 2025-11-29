---
title: "函数式编程"  # 文章标题（必填）
date: 2025-10-16T10:00:00+08:00   # 发布日期（必填，格式：YYYY-MM-DDTHH:MM:SS±HH:00）
description: "这是一些关于函数式的内容"  # 文章描述（用于 SEO 和摘要）
tags: ["Hugo", "Blowfish", "Functional","博客"]  # 标签（可选，数组格式）
categories: ["技术"]  # 分类（可选）
cover: "feature.svg"    # 封面图（可选，若用文件夹格式，填文件名；否则填完整路径）
draft: false          # 是否为草稿（true 则不发布，默认 false）
---

1. 函数式编程是什么
函数式编程是一个不同于OOP的一种编程范式，任何语言都可以使用，他不是一种类似于特定的库之类的方法，相反的任何语言都可以使用他。相比之于OOP那种类似与命令式编程的方式而言，函数式编程更趋向于声明式编程。下面举一段具体的例子，如果使用命令式编程:
```csharp
print(ComposeMessage(1, 2));

string ComposeMessage(int a, int b)
{
    return $"{a} + {b} = {a + b}";
}

void print(string msg)
{
    Console.WriteLine(msg);
}
```

但是如果你使用函数式编程情况就会大不相同
```csharp
Func<int, int, string> ComposeMessage = (a, b) => $"{a} + {b} = {a + b}";
Action<string>print = (s) => Console.WriteLine(s);
print(ComposeMessage(2, 3));
```
通过函数式编程的方法，你可以想搭积木一样的使用小的函数来构成更大的函数来构建应用，他提供和OOP完全不一样的构建应用的技巧。
2. 如何达到函数式编程呢？
+ 首要的是使用表达式而非语句想象一下你有一个int a你需要让a + 10一个常规的想法是
```csharp
int addTen(int a){
    return a + 10;
}
```
如果从函数式的视角来看呢？函数式可以更加简洁的来做到这件事，例如:
```csharp
int addTen(int x) => x + 10;
```
如果这个例子不够直观，看不出来使用函数式编程有什么好处，那么不妨来看下面这个问题:输入一个月份,输出这个月份的
天数，咋一看好像是一个十分简单的问题，但是如果你使用命令式编程，你很有可能得到如下的解决方案:
```csharp
int NumberOfDays(int month,bool isLeapYear)
{
    if(month == 2)
    {
        if (isLeapYear)
        {
            return 29;
        }
        else
        {
            return 28;
        }
    }
    if(month == 1 || month == 3 || month == 5 ||
       month == 7 || month == 8 || month == 10 ||
       month == 10
        )
    {
        return 31;
    }
    else
    {
        return 30;
    }
}
```
看起来可能十分的冗长，但是如果你切换成函数式编程，那么一切便可以大有改观
```csharp
int NumberOfDays(int month, bool isLeapYear) =>
    (month, isLeapYear) switch
    {
        { month: 2, isLeapYear: true } => 29,
        { month: 2 } => 28,
        { month: 1 or 3 or 5 or 7 or 8 or 10 or 12 } => 31,
        _ => 30
    };
```
下面使用一个更加具体的例子来说明函数式编程和OOP的不同之处,假设有一个电影类，其包含了name和genres两个Field，如果想从其中取出对于的genres的电影，如果使用命令式，一个显而易见的编写出的代码如下:
```csharp
var fakeData = new List<Film>
{
    new Film("The Great Chase","action"),
    new Film("A Quiet Reflection","drama"),
    new Film("Desert Storm","action"),
    new Film("The Family Secret","drama")
}; 
record File(string name , string genres);
```

```csharp
IEnumerable<Film> GetFilmsByGenre(string genre)
{
    var chosenFilm = new List<Film>();
    
    foreach (var f in fakeData)
    {
        if(f.genre == genre)
        {
            chosenFilm.Add(f);
        }
    }
    return chosenFilm;
}
```
显然这个函数不够优雅并且还一次性的写出了所有选的类型的电影，不必要的占用了大量的内存，所以你想到了第一种的改进方法
```csharp
IEnumerable<Film> GetFilmsByGenre(string genre)
{
    
    foreach (var f in fakeData)
    {
        if(f.genre == genre)
        {
            yield return f;
        }
    }
}
```
显然，这下不需要全部获取对应的，使用yield可以一个一个的得到Film，但是使用函数式编程，这个可以更加的优雅
```csharp
IEnumerable<Film> GetFilmsByGenre(string genre) => fakeData.Where(x => x.genre == genre);
```