---
layout: post
title: Học javascript
date:   2017-04-24 23:36:39
categories: javascript
---

# Giới thiệu
..* Những chia sẻ này không phải mục đích dạy bạn Javascrip . Đây chỉ là
	những nội dụng quan trọng trong javascrip mà đòi hỏi bạn phải hiểu rõ nó.

## 1. Object
_______________________________________________
Sử dụng object và các properties
Mọi thứ trong javascript đều là object , ngoại trừ null và undefined

```javascript
	false.toString();//false
	[1,2,3].toString();//'1,2,3'

	function Foo(){}
	Foo.bar = 1;
	Foo.bar;//1

```
Một sự khó hiểu đó là không thể sử dụng số như 1 đối tượng .

```javascript
	2.toString(); //raises SystaxError
```
Vì parse của javascript parse phần phía sau dấu chấm như là 1 số kiểu float.
Nhưng nếu bạn viết như sau thì sẽ được

```javascript
	2..toString();
	2 .toString();
	(2).toString();
```
Object trong javascript cũng được sử dụng như Hashmaps, chúng bao gồm tên thuộc tính
được map với value

Sử dụng kí hiệu {} - nó có thể tạo 1 đối tượng . Nó là đối tượng mới kế thừa từ 
Object.prototype và chưa có thuộc tính nào.

```javascript
	var foo = {};//a new empty object

	//a new object with a 'test' property with value 12
	var bar = { test: 12 };
```
