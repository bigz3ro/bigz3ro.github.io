---
layout: post
title: Invocation in javscript
date:   2017-05-03 22:04
categories: javscript
---

# Giới thiệu
..* Tìm hiểu về invocation trong javascript
## 1. Ví dụ
_______________________________________________
Invoking 1 function là hoãn thực thi function hiện tại, truyền quyền điều khiển và các parameter đến 1 function mới.
Ngoài ra những tham số đã được khai báo, mỗi function nhận vào 2 params: this và arguments. Tham số "this" là vô cùng quan trọng trong lập trình hướng đối tượng, và gía trị của nó được xác định bởi invocation pattern.
Có 4 pattern invoication trong JS :
+ Method invocation pattern .
+ Function invocation pattern.
+ Contructor invocation pattern.
+ Apply invocation pattern.
- Các pattern này khác nhau mà tham số "this" được init.

1.The Method Invocation Pattern
Khi 1 function được lưu trữ trong 1 object thì nó được coi như là 1 method. Khi mà method này được invoked, "this" sẽ
được bind cho object đó.
- Method có thể sử dụng "this" để truy cập đến object nhưng nó có thể lấy ra những gía trị từ object hoặc modify object. Việc binding "this" cho object xảy ra tại thời gian invocation.

2.The Function Invocation Pattern

```javascript
    var sum = add(3, 4);
```
Khi 1 function được invoked ở pattern thì this được bind cho global object. Nó là 1 mistake của ngôn ngữ. Với các ngôn ngữ design đúng thì khi inner function được invoked "this" sẽ được bind cho cho biến "this" của outer function. Hậu quả của error này là 1 method không thể sử dụng 1 inner function để làm công việc của nó vì inner function không chia sẻ cách truy cập đến object như vậy là "this" bị bind sai gía trị. Các khắc phục là khai báo biến gán cho "this" và inner function sẽ truy cập "this" qua biến đó.