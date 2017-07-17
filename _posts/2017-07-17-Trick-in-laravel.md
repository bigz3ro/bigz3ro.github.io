---
layout: post
title: Laravel - những điều hay ho
date:   2017-04-24 23:36:39
categories: laravel
---

# Giới thiệu
..Những note hay ho trong khi làm việc với laravel

## Note 1. Debug
_______________________________________________
```php
    //enable debug laravel
    \DB::connection()->enableQueryLog();
    //cau lenh query voi laravel
    $list = $query->where('id', '!=', $params['jobId'])->orderBy('created_at')->limit($limit)->get();
    //get query
    $queries = \DB::getQueryLog();
    //debug here
    dd($queries);
```