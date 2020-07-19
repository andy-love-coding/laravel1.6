## 3 构建页面
### 3.2 创建应用
  - 1.做好准备
    ```
    > cd ~/Homestead && vagrant up
    > vagrant ssh
    ```
  - 2.创建应用 
    ```
    $ cd ~/Code
    $ composer create-project laravel/laravel laravel1.6 --prefer-dist "6.*"
    ```
  - 3.修改 hosts  
    Mac 下打开 Hosts 文件：
    ```
    subl /etc/hosts
    ```
    在 hosts 文件最后面新增下面一行以完成设置
    ```
    192.168.10.10   laravel1.6.test
    ```
  - 4.新增站点  
    打开 Homestead.yaml 文件
    ```
    subl ~/Homestead/Homestead.yaml
    ```
    ```
    ---
    ip: "192.168.10.10"
    memory: 2048
    cpus: 1
    provider: virtualbox

    authorize: ~/.ssh/id_rsa.pub

    keys:
        - ~/.ssh/id_rsa
        - ~/.ssh/id_rsa.pub

    folders:
        - map: ~/Code
          to: /home/vagrant/Code

    sites:
        - map: note.test
          to: /home/vagrant/Code/note/public
        - map: laravel1.6.test
          to: /home/vagrant/Code/laravel1.6/public
        - map: laravel2.2.test
          to: /home/vagrant/Code/laravel3.0/public

    databases:
        - note
        - laravel1.6
        - laravel2.2
    ```
  - 5.重启虚拟器
    ```
    > cd ~/Homestead && vagrant provision && vagrant reload
    ```
  - 6.修改 `.env` 文件
    ```
    APP_NAME=Weibo
    APP_ENV=local
    APP_KEY=base64:dIAK2B7exLbY2S54RFQdI3c6vg9/qIWWjYy5M4ESa4A=
    APP_DEBUG=true
    APP_URL=http://laravel1.6.test

    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel1.6
    DB_USERNAME=homestead
    DB_PASSWORD=secret
    ```
  - 7.Git代码版本控制
    ```
    $ cd ~/Code/laravel1.6
    $ git init
    $ git add -A
    $ git commit -m "Initial commit"

    $ git remote add origin git@github.com:andy-love-coding/laravel1.6.git
    $ git push -u origin master
    ```
  - 8.统一代码风格
    .editorconfig
    ```
    root = true

    [*]
    charset = utf-8
    end_of_line = lf
    insert_final_newline = true
    indent_style = space
    indent_size = 4
    trim_trailing_whitespace = true

    [*.md]
    trim_trailing_whitespace = false

    [*.yml]
    indent_size = 2

    [*.{js,html,blade.php,css,scss}]
    indent_style = space
    indent_size = 2
    ```
### 3.3 静态页面
  - 1.新建分支
    ```
    $ git checkout master
    $ git checkout -b static-pages
    ```
    合并分支示例：
    ```
    $ git merge fake-branch
    ```
    删除分支示例：
    ```
    $ git branch -d fake-branch
    ```
  - 2.移除无用视图
    ```
    rm resources/views/welcome.blade.php
    ```
  - 3.配置路由 routes/web.php
    ```
    <?php

    Route::get('/', 'StaticPagesController@home');
    Route::get('/help', 'StaticPagesController@help');
    Route::get('/about', 'StaticPagesController@about');
    ```
  - 4.生成静态页面控制器
    ```
    $ php artisan make:controller StaticPagesController
    ```
    app/Http/Controllers/StaticPagesController.php
    ```
    public function home()
    {
        return view('static_pages/home');
    }

    public function help()
    {
        return view('static_pages/help');
    }

    public function about()
    {
        return view('static_pages/about');
    }
    ```
  - 5.使用通用视图 resources/views/layouts/default.blade.php
    ```
    <!DOCTYPE html>
    <html>
      <head>
        <title>@yield('title', 'Weibo App') - Laravel 新手入门教程</title>
      </head>
      <body>
        @yield('content')
      </body>
    </html>
    ```
  - 6.添加静态页面视图  
    resources/views/static_pages/home.blade.php
    ```
    @extends('layouts.default')
    @section('content')
      <h1>主页</h1>
    @stop
    ```
    resources/views/static_pages/help.blade.php
    ```
    @extends('layouts.default')
    @section('title', '帮助')

    @section('content')
      <h1>帮助页</h1>
    @stop
    ```
    resources/views/static_pages/about.blade.php
    ```
    @extends('layouts.default')
    @section('title', '关于')

    @section('content')
      <h1>关于页</h1>
    @stop
    ```
  - 7.Git代码版本控制
    ```
    $ git add -A
    $ git commit -m "3.3 基础页面 用用视图页面"
    ```
  - 8.提交代码（合并分支）
    ```
    $ git checkout master
    $ git merge static-pages
    ```
### 3.4 Artisan 命令
  - Artisan 是 Laravel 提供的 CLI（命令行接口）
    | 命令                         | 说明             |
    | ---------------------------- | ---------------- |
    | php artisan key:generate     | 生成 App Key     |
    | php artisan make:controller  | 生成控制器       |
    | php artisan make:model       | 生成模型         |
    | php artisan make:policy      | 生成授权策略     |
    | php artisan make:seeder      | 生成 Seeder 文件 |
    | php artisan migrate          | 执行迁移         |
    | php artisan migrate:rollback | 回滚迁移         |
    | php artisan migrate:refresh  | 重置数据库       |
    | php artisan db:seed          | 填充数据库       |
    | php artisan tinker           | 进入 tinker 环境 |
    | php artisan route:list       | 查看路由列表     |
  - 查看 Artisan 命令帮助信息
    ```
    $ php artisan help migrate    
    ```
    或者
    ```
    $ php artisan help migrate
    ```

