# XSS

## 实验要求

编写漏洞网页，进行 XSS 攻击

## 实验环境

MacOS

## 实验过程

### 搭建PHP开发环境

#### 开启Apache服务

PHP文件需要在Apache下运行，Apache服务在Mac种默认关闭的，因此需要一下修改配置文件：

1. Apache服务默认安装路径在 **/private/etc/apache2** ,属于系统私有目录。在该目录下找到并打开 **httpd.conf** 文件

   ```shell
   vim http.conf
   ```

2. 搜索 **#LoadModule php7_module libexec/apache2/libphp7.so** ,将前方的 **#** 删除

3. 启动Apache服务

   ```shell
   sudo apachectl start
   ```

4. 在浏览器中输入`http://localhost`或` [http://127.0.0.1`网址来检查Apache服务是否启动成功。如果成功,显示**It works!**

#### Apache项目部署目录

Apache服务默认部署路径在 **/Library/WebServer/Documents/** 因此我们的项目需要放置在该路径下

1. 我们也可以在 **/Library/WebServer/Documents/** 下新建一个 **info.php** 测试程序

   ```
   <?php phpinfo(); ?>
   ```

2. 在浏览器中输入如下网址即可查看到PHP相关信息
   http://localhost/info.php

当然我们也可以修改部署路径,可以在 **/private/etc/apache2** 目录下找到并打开 **httpd.conf** 文件,搜索 **Documentroot** 并修改部署路径，重启apache生效

### 网页代码

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Weak Website</title>
</head>
<body>
    <form method="post">
        <input type="text" name="xss">
        <input type="submit" value="提交">
    </form>
    <?php
        if(isset($_POST["xss"]))
        {
            echo $_POST["xss"];
        }
    ?>
</body>
</html>
```