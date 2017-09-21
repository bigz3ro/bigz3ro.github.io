---
layout: post
title: Arguments in javscript
date:   2017-05-03 22:04
categories: javscript
---

# Arguments javascript
_______________________________________________
Trong javscript thì mỗi functions đều có 1 tham số mặc định đó là array "arguments".
Đây là 1 pattern lỗi vì arguments không thực sự là 1 array. Nó giống như 1 object giống array.
# Return
- 1 function thì luôn trả về 1 value. Nếu value không được xác định thì undefined sẽ được trả về.
- Nếu 1 function được invoked bằng từ khóa new và return value không phải 1 object thì "this" sẽ trả về thay
thế.

# Exceptions
Try - catch là để bắt lỗi . Khi try catch thì dừng thực thi function.