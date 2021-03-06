---
title: C++ 元组介绍
categories: [
  "C++"
]
tags: [
  "C++"
]
date: 2015-07-19
image: "stuck.jpg"
---

## 介绍说明

对于熟悉 Python 语言者而言，如下这段代码相信你是熟悉的：

    >>> t = (1, 'hello', 3.14, 1, 3.14)
	>>> type(t)
	<type 'tuple'>
	>>> t.count(3.14)
	2
	>>> t.index('hello')
	1

对，没错，这就是 Python 提供的 tuple（元组）：一个元组可以定义至少一个元素，或者多个元素同时多个元素并不要求类型一致。函数 count 返回 tuple 中有多少个给定值的元素（注：当元素不在 tuple 中时返回 0），而函数 index 返回 tuple 中给定值的元素的下标（注：当元素不在 tuple 中时抛出异常）。

对于 C++ 而言，也有类似的数据结构：**pair** 和 **tuple**，相信你对于 pair 一定是非常熟悉的，而 tuple 是 c++ 11 新添加的特性，后文将主要介绍这两种数据结构。

## pair 介绍

#### 1. pair 基本概念

类型 pair 定义在头文件 **utility** 中。

一个 pair 类型保存**两个**数据成员，它是用来生成特定类型的**模板**。当创建一个 pair 时，必须提供两个类型名，pair 的数据成员将具有对应的类型，**两个数据类型并不要求一致**。形如：

	std::pair<std::string, int> test1; // 保存一个 string 和一个 int
	std::pair<std::string, std::string> test2; // 保存两个 string
	std::pair<std::string, std::vector<int>> test3; // 保存一个 string 和一个 vector

pair 的默认构造函数对数据成员进行**值初始化**。故：test1 中的 string 被初始化为空，int 被初始化为 0；test2 是包含两个空 string 的pair；test3 则保存一个空 string 和一个空 vector。

也可以使用**列表初始化**方式对 pair 进行初始化，形如：

	std::pair<std::string, int> student{"Tian", 90};

该语句创建了一个名为 student 的 pair， 两个成员被初始化为 "Tian" 和 90。

需要特别注意的是：pair 的数据成员都是 **public** 的，同时，两个成员分别命名为 **first** 和 **second**，使用**点运算符**来访问它们。

#### 2. pair 操作

标准库仅定义了有限的几个 pair 操作，如下：

	pair<T1, T2> p; // 值初始化
	pair<T1, T2> p(v1, v2); // 类型为 T1 和 T2，first 和 second 成员分别用 v1 和 v2 进行初始化
	pair<T1, T2> p{v1, v2}; // 列表初始化
	pair<T2, T2> p = {v1, v2};
	make_pair(v1, v2); // 返回用 v1 和 v2 初始化的 pair
	p.first; // 返回名为 first 的数据成员 
	p.second; // 返回名为 second 的数据成员
	p1 relop p2; // 关系运算符按字典序定义
	p1 == p2;
	p2 != p2;

#### 3. pair 实例

pair 类型使用比较多的场景是当做简单的数据类型来操作，不用动用 vector 或 map 这样的相对重型点的数据结构。

另一个使用场景是用在函数返回时，形如：

	std::pair<std::string, in> process(std::vector<std::string> &v)
	{
		// 处理v
		if(!v.empty())
		{
			return {v.back(), v.back().size}; // 列表初始化
		}
		else
		{
			return std::pair<std::string, int>(); // 隐式构造返回值
		}

若 v 不为空，则返回一个由 v 中最后一个 string 及其大小组成的 pair；否则隐式构造一个空 pair 并返回之。

注：在以前的 C++ 版本中，不允许用花括号包围的初始化器（即列表初始化）来返回 pair 类型的对象，必须显示构造返回值，形如：

	if(!v.empty())
	{
		return std::pair<std::string, int>(v.back(), v.back().size);
	}

或者，使用 make_pair 来生成 pair 对象， pair 的两个类型来自于 make_pair 的参数，形如：

	if(!v.empty())
	{
		return std::make_pair(v.back(), v.back().size);
	}

## tuple 介绍

#### 1. tuple 基本概念

类型 tuple 定义在头文件 **tuple** 中。

tuple 类型是类似 pair 的模板，一个 tuple 可以有**任意数量**的成员，且 tuple 类型的**成员类型不必相同**。

如果希望将一些数据组合成单一对象但又不想定义一个新数据结构来表示这些数据时， tuple 就可以派上用场了。

#### 2. tuple 定义和初始化

定义一个 tuple 时，需要指出每个成员的类型，形如：

	std::tuple<int, int, int> test1; // 三个成员都设置为 0
	std::tuple<std::string, std::vector<double>, int, std::list<int>>
		test2("hello", {3.14, 0.618}, 3, {1, 3, 5, 7});

创建一个 tuple 对象时，可以使用 tuple 的默认构造函数，它会对每个成员进行值初始化；也可以像本例中 test2 一样，为每个成员提供一个初始值。

tuple 的构造函数是 **explicit** 的，所以必须使用直接初始化语法，形如：

	std::tuple<int, int, int> test1 = {1, 2, 3}; // 错误
	std::tuple<int, int, int> test1 = {1, 2, 3}; // 正确

类似 make_pair 函数，标准库定义了 make_tuple 函数，可以用它来生成 tuple 对象，形如：

	auto item = std::make_tuple("Tian", 1234, 96);

和 make_pair 一样，make_tuple 函数使用初始值的类型来推断 tuple 的类型，item 是一个 tuple，类型为 tuple<const char*, int, int>。

#### 3. tuple 成员访问

一个 pair 总是仅有两个成员，标准库就可以给它们命名（first 和 second）。但 tuple 类型的成员数量是没有限制的，所以不能像 pair 那样来命名，故 tuple 的成员都是未命名的。

访问一个 tuple 的成员，就需要使用标准库函数模板 **get**，为了使用 get，必须指定一个显式模板实参，它指出想要访问第几个成员，传递给 get 一个 tuple 对象，它返回指定成员的引用，形如：

	auto name = get<0>(item); // 返回 item 的第一个成员
	auto number = get<1>(item); // 返回 item 的第二个成员
	auto score = get<2>(item); // 返回 item 的第三个成员

注：尖括号中的参数必须是**整型常量表达式**，计数从 0 开始。

若不知道 tupler 的准确类型细节信息，可以使用如下两个辅助模板类来查询 tuple 成员的**数量**和**类型**：

	typedef decltype(item) trans; // trans 是 item 的类型
	
	// 返回 trans 类型对象中成员的数量
	size_t sz = std::tuple_size<trans>::value; // 返回 3

	// cnt 的类型与 item 中第二个成员相同
	tuple_element<1, trans>::type cnt = get<1>(item); // cnt 是一个 int

为了使用 tuple_size 和 tuple_element，需要知道一个 tuple 对象的类型，而确定一个对象的类型的最简单方法就是使用 **decltype**。

tuple_size 模板有一个名为 value 的 public static 数据成员，表示给定 tuple 中成员的数量；tuple_element 模板除了一个 tuple 类型外，还接受一个索引值，它有一个名为 type 的public 类型成员，表示给定 tuple 类型中指定成员的类型。和 get 类似，tuple_element 所使用的索引也是从 0 开始。

#### 4. tuple 实例

和 pair 一样，tuple 的一个常见用途也是从一个函数返回多个值。

## 参考资料

> [pair reference](http://www.cplusplus.com/reference/utility/pair/)

> [tuple reference](http://www.cplusplus.com/reference/tuple/)
