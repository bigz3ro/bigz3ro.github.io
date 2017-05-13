---
layout: post
title: Seed and factory in laravel
date:   2017-05-03 22:04
categories: php
---

# Introduction
Today, I speak How use seeder and factory in laravel
	
# 1. Usage
	- Factory is helper us create data in database auto.
	- Factory giống như 1 cái nhà máy giúp ta tạo ra các dữ liệu
	- Trong file ModelFactory trong thư mục database/factories
	ta định nghĩa các loại dữ liệu tương ứng với các trường trong database tương ứng .
	ở đây chúng ta sử dụng 1 thư viện tạo ra dữ liệu đúng với loại tương ứng với field trong đatabase.
	Ví dụ với field email ta sẽ sử dụng $fake->email để tạo dữ liệu 
	Có rất kiểu dữ liệu mà bạn sẽ tìm hiểu tại đây: (https://github.com/fzaninotto/Faker)

# 2. Các bước để tạo dữ liệu fake cho cơ sở dữ liệu
	B1: Bạn phải tạo Seeders = command sau:
	
	php artisan make:seeder PostTableSeeder
	
	Sẽ tạo ra file PostTableSeeder trong database/seeds
	File này bạn có thể hình dùng là người điều khiển cho cái máy factory làm việc
	
	B2: Tạo factory
	Trong file ModelFactory.php trong database/factories
```php
	$factory->define(App\Posts::class, function (Faker\Generator $faker) {
    static $password;

    return [
        'email' => $faker->unique()->email
    ];
	});
```
	Định ngĩa với trường email với $faker tạo ra các email
	B3: Gọi người Seed trong file DatabaseSeeder trong database/seeds
	trong method run();
```php
	public function run()
    {
     	$this->call(PostTableSeeder::class);
    }
```
	B4: Run artisan: 
	php artisan migrate:refresh --seed
	