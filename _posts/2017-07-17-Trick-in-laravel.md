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
    $list = $query->where('id', '!=', $params['jobId'])
                  ->orderBy('created_at')
                  ->limit($limit)
                  ->get();
    //get query
    $queries = \DB::getQueryLog();
    //debug here
    dd($queries);
```

## Note 2. Tối ưu thời gian thực thi code in php
_______________________________________________
```php
    //Trước khi thao tác với dữ liệu ta nên kiểm tra cho dữ liệu đó có tồn tại không
    //Code ví dụ
    if($request->hasFile('import_file')) {
        $path = $request->file('import_file')->getRealPath();

        $data = Excel::load($path, function($reader) {})->get();

        if(!empty($data) && $data->count()) {

            foreach ($data->toArray() as $key => $value) {
                if(!empty($value)) {
                    foreach ($value as $v) {
                      $insert[] = ['title' => $v['title'], 'description' => $v['description']];
                    }
                }
            }
            if(!empty($insert)) {
                Item::insert($insert);
                return back()->with('success','Insert Record successfully.');
            }

        }
    }
```