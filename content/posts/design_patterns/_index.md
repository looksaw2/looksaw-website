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

现在有一简单需求，需要根据Product的Name或者Color,Size过滤用户，一个简单的想法是写出下面的代码

```go
type SimpleFilter struct {}


func(s SimpleFilter)FilterByName(products []Product,name Name)([]Product){
	var res []Product
	for _ , p := range products {
		if p.Name == name {
			res = append(res, p)
		}
	}
	return res
}

func(s SimpleFilter)FilterByColor(products []Product,color Color)([]Product){
	var res []Product
	for _ , p := range products {
		if p.Color == color {
			res = append(res, p)
		}
	}
	return res
}

func(s SimpleFilter)FilterBySize(products []Product,size Size)([]Product){
	var res []Product
	for _ , p := range products {
		if p.Size == size {
			res = append(res, p)
		}
	}
	return res
}

```
但是从这里可以看到每次更改代码就需要对于SimpleFilter这个类进行修改，也就是说每次改动都是侵入性质的，但是开闭原则提出了一个较好的实践，及不要直接修改对应的class,而是实现方法的添加，这样可以更好的修改，下面利用开闭原则的原理来对代码进重构:

```go
type ISpec[T any] interface{
	IsSatisfied(t T) bool
}

type IFilter[T any] interface {
	Filter([]T,ISpec[T])([]T)
}
//实现Color
type ColorSpec struct {
	TargetColor Color
}
func(cspec *ColorSpec)IsSatisfied(product Product) bool {
	if product.Color == cspec.TargetColor {
		return true
	}
	return false
}

//实现Size
type SizeSpec struct {
	TargetSize Size
}
func(sspec *SizeSpec)IsSatisfied(product Product) bool {
	if product.Size == sspec.TargetSize {
		return true
	}
	return false
}


type ProductBetterFilter struct {}
func(pb *ProductBetterFilter)Filter(products []Product , spec ISpec[Product])([]Product){
	var res []Product
	for _ , p := range products {
		if spec.IsSatisfied(p) {
			res = append(res, p)
		}
	}
	return res
}
```
可以发现使用开闭原则大大的减少了代码的侵入性，需要什么，实现对于的ISpec接口就可以了，这样的代码大大的减少了代码的侵入性。并且实现了对于接口的依赖

##### 其他语言版本
JAVA:
```java
public class BasicFilter {
    private List<Product> products;
    public BasicFilter(List<Product> products) {
        this.products = products;
    }
    //
    public Iterable<Product> FilterByName(String name){
        var result = new ArrayList<Product>();
        for (var product : products) {
            if (product.getName().equals(name)) {
                result.add(product);
            }
        }
        return result;
    }
    //
    public Iterable<Product> FilterByColor(Color color){
        var result = new ArrayList<Product>();
        for(var product : products){
            if (product.getColor().equals(color)) {
                result.add(product);
            }
        }
        return result;
    }
    //
    public Iterable<Product> FilterBySize(Size size){
        var result = new ArrayList<Product>();
        for(var product : products){
            if (product.getSize().equals(size)) {
                result.add(product);
            }
        }
        return result;
    }
}
```
普通的Filter
```java
public class BasicFilter {
    private List<Product> products;
    public BasicFilter(List<Product> products) {
        this.products = products;
    }
    //
    public Iterable<Product> FilterByName(String name){
        var result = new ArrayList<Product>();
        for (var product : products) {
            if (product.getName().equals(name)) {
                result.add(product);
            }
        }
        return result;
    }
    //
    public Iterable<Product> FilterByColor(Color color){
        var result = new ArrayList<Product>();
        for(var product : products){
            if (product.getColor().equals(color)) {
                result.add(product);
            }
        }
        return result;
    }
    //
    public Iterable<Product> FilterBySize(Size size){
        var result = new ArrayList<Product>();
        for(var product : products){
            if (product.getSize().equals(size)) {
                result.add(product);
            }
        }
        return result;
    }
}
```

其重构的版本
```java
public interface ISpec<T>{
    boolean IsSatisfied(T t);
}
public interface IFilter<T>{
    Iterable<T> Filter(Iterable<T> ts , ISpec<T> spec);
}
public class ColorSpec implements ISpec<Product>{
    private final Color color;
    public ColorSpec(Color color){
        this.color = color;
    }
    @Override
    public boolean IsSatisfied(Product product) {
        return color == product.getColor();
    }
}
public class SizeSpec implements ISpec<Product> {
    private final Size size;
    public SizeSpec(Size size) {
        this.size = size;
    }
    @Override
    public boolean IsSatisfied(Product product) {
        return this.size.equals(product.getSize());
    }
}
public class ProductFilter implements IFilter<Product> {

    @Override
    public Iterable<Product> Filter(Iterable<Product> products, ISpec<Product> spec) {
        var result = new ArrayList<Product>();
        for(var product : products) {
            if (spec.IsSatisfied(product)) {
                result.add(product);
            }
        }
        return result;
    }

    public static void main(String[] args){
        var products = new ArrayList<Product>();
        products.add(new Product("T恤", Color.RED, SMALL));
        products.add(new Product("牛仔裤", Color.GREEN, Size.MEDIUM));
        products.add(new Product("外套", Color.BLUE, Size.LARGE));
        products.add(new Product("帽子", Color.RED, Size.YUGA));

            // 重复颜色/尺寸但不同名称
        products.add(new Product("毛衣", Color.RED, Size.MEDIUM));    // 红色+中号（与T恤同色不同尺寸）
        products.add(new Product("短裤", Color.GREEN, SMALL));   // 绿色+小号（与牛仔裤同色不同尺寸）
        products.add(new Product("围巾", Color.BLUE, Size.YUGA));     // 蓝色+YUGA（与外套同色不同尺寸）

            // 边缘情况：名称包含特殊字符、尺寸为YUGA（非标准尺寸）
        products.add(new Product("冬季大衣_厚款", Color.RED, Size.LARGE));
        products.add(new Product("运动裤#123", Color.GREEN, Size.YUGA));
        products.add(new Product("休闲鞋", Color.BLUE, SMALL));

            // 同名不同属性（模拟同产品不同规格）
        products.add(new Product("帆布鞋", Color.RED, SMALL));
        products.add(new Product("帆布鞋", Color.BLUE, Size.MEDIUM));

        var filter = new ProductFilter();
        var r = filter.Filter(products,new ColorSpec(Color.RED));
        for(var product : r){
            System.out.println(product.getName());
            System.out.println(product.getColor());
            System.out.println(product.getSize());
        }

    }
}
```

C#的版本