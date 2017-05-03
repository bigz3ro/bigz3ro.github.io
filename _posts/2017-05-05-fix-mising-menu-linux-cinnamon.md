---
layout: post
title: Fix lỗi giao diện trên linux mint cinnamon 18.1
date:   2017-05-03 22:04
categories: linux
---

# Giới thiệu
..* Khắc phục lỗi giao diện trên VMware workstation 12

## 1. Cắch khắc phục
_______________________________________________
Mở file bootstrap :
sudo vim /etc/vmware/bootstrap
- Và thêm dòng sau vào:
export VMWARE_USE_SHIPPED_GTK="yes"

