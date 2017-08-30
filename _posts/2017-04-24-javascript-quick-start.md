---
layout: post
title: Javascript - những điều hay ho
date:   2017-04-24 23:36:39
categories: javascript
---

# Giới thiệu
..* Những chia sẻ này không phải mục đích dạy bạn Javascript . Đây chỉ là
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
###Object trong javascript cũng được sử dụng như Hashmaps, chúng bao gồm tên thuộc tính
được map với value

Sử dụng kí hiệu {} - nó có thể tạo 1 đối tượng . Nó là đối tượng mới kế thừa từ
Object.prototype và chưa có thuộc tính nào.

```javascript
	var foo = {};//a new empty object

	//a new object with a 'test' property with value 12
	var bar = { test: 12 };
```
###Cách truy cập tới properties của object
Có 2 cách để có thể truy cập vào property của object:
+ Sự dụng dấu chấm
+ Sử dụng ngoặc vuông

```javascript
	var foo = { name: 'kitchen'}
	foo.name; //kitchen
	var get = 'name';
	foo[get];//name

	foo.1234;//SyntaxError
	foo['1234'];//works
```

### Xóa properties
Để xóa properties của object bạn của thể sử dụng toán tử delete
Nếu thiết lập value là null hoặc undefined thì chỉ gía trị bị remove
còn key vẫn tồn tại
Example:

```javascript
var obj = {
	bar: 1,
	foo: 2,
	baz: 3
}
obj.bar = undefined;
obj.foo = null;
delete obj.baz;

for(var i in obj){
	if(obj.hasOwnProperty(i)){
		console.log(i, '', obj[i]);
	}
}
```
Output:

```
foo null
bar undefined
undefined
```

### JSON
A common use of JSON is to exchange data to/from a web server.

When sending data to a web server, the data has to be a string.

Convert a JavaScript object into a string with JSON.stringify().

