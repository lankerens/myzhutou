### 前端

-----

?> 就直接按照 https://docs.v2board.com/deploy/aapanel.html 的搭建就好了

#### 使用 aapanel 部署

> aaPanel是宝塔(bt.cn)-删库塔的国际版本
> <br>
> 
> 地址：https://forum.aapanel.com/d/9-aapanel-linux-panel-6-5-4-installation-tutorial
> 


Centos 拉取最新 aapanel :
```bash
yum install -y wget && wget -O install.sh http://www.aapanel.com/script/install_6.0_en.sh && bash install.sh
```


安装完成后我们登陆 aaPanel 进行环境的安装。

选择使用LNMP的环境安装方式勾选如下信息

☑️ Nginx 1.17

☑️ MySQL 5.6

☑️ PHP 7.4


选择 Fast 快速编译后进行安装。


#### 安装 redis

aaPanel 面板 > App Store > 找到PHP 7.4点击Setting > Install extentions > redis,fileinfo 进行安装。


#### 解除被禁止的函数

aaPanel 面板 > App Store > 找到PHP 7.3点击Setting > Disabled functions 

将 `putenv` 、 `proc_open` 、`pcntl_alarm`、 `pcntl_signal` 从列表中删除。



#### 添加站点

aaPanel 面板 > Website > Add site。

> 在 Domain 填入你指向服务器的域名   <br>
> 在 Database 选择MySQL              <br>
> 在 PHP Verison 选择PHP-74          <br>



#### 安装 v2b

通过SSH登录到服务器后访问站点路径如：/www/wwwroot/你的站点域名。

以下命令都需要在站点目录进行执行。


```bash
# 删除目录下文件
chattr -i .user.ini
rm -rf .htaccess 404.html index.html .user.ini
```

执行命令从 Github 克隆到当前目录。

```bash
git clone https://github.com/v2board/v2board.git ./
```

执行命令安装依赖包以及V2b

```bash
sh init.sh
```


根据提示完成安装




#### 配置站点目录及伪静态


1. 添加完成后编辑添加的站点 > Site directory > Running directory 选择 /public 保存。

2. 添加完成后编辑添加的站点 > URL rewrite 填入伪静态信息。


```php
location /downloads {
}

location / {  
    try_files $uri $uri/ /index.php$is_args$query_string;  
}

location ~ .*\.(js|css)?$
{
    expires      1h;
    error_log off;
    access_log /dev/null; 
}
```


#### 配置定时任务

aaPanel 面板 > Cron。

> 在 Type of Task 选择 Shell Script  <br>
> 在 Name of Task 填写 v2board   <br>
> 在 Period 选择 N Minutes 1 Minute  <br> 
> 在 Script content 填写 php /www/wwwroot/路径/artisan schedule:run

根据上述信息添加每1分钟执行一次的定时任务。



#### 启动队列服务


V2board的邮件系统强依赖队列服务，你想要使用邮件验证及群发邮件必须启动队列服务。下面以aaPanel中nodejs的PM2服务来守护队列服务作为演示。

aaPanel 面板 > App Store > Tools

找到Supervisor进行安装，安装完成后点击设置 > Add Daemon按照如下填写

在 Name 填写 V2board
在 Run User 选择 www
在 Run Dir 选择 站点目录 在 Start Command 填写 php artisan horizon 在 Processes 填写 1

填写后点击Confirm添加即可运行。




#### 常见问题

?> 问题：500错误
 <br>
 <br>
解决方案：检查站点根目录权限，递归755，保证目录有可写文件的权限，也有可能是Redis扩展没有安装或者Redis没有按照造成的。你可以通过查看storage/logs下的日志来排查错误或者开启debug模式。







<br>


<br>


<br>

-------

<p align="right">最近更新： {docsify-updated}</p>
