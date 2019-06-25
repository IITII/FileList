## FileList
> [https://github.com/ToyoDAdoubi/DirectoryLister](https://github.com/ToyoDAdoubi/DirectoryLister) 的自定义版本，毕竟原来的不符合个人的要求  
> 现在改好以后开箱即用  
### 文件结构
假设你的虚拟主机是 `/home/wwwroot/xxx.xx`
``` bash
/home/wwwroot/xxx.xx/
├─ resources/
│   ├ themes/
│   │ └ bootstrap/
│   │    ├ css/
│   │    ├ fonts/
│   │    ├ img/
│   │    ├ js/
│   │    ├ default_footer.php # 底部公共文件 #
│   │    ├ default_header.php # 顶部公共文件 (可以放网站流量统计代码)
│   │    └ index.php # 网页主文件，其中可以修改顶部公告栏内容 #
│   │
│   ├ DirectoryLister.php # 可修改顶部标题 (FileList)
│   ├ config.php # 配置文件
│   └ fileTypes.php # 区分文件类型的php文件
│
├ README.html # 该文件夹页面内的 说明简介文件 #
├ index.php
│
├─ 其他文件夹/
│   ├ 其他文件.txt
│   └ README.html # 该文件夹页面内的 说明简介文件 #
│
└ 其他文件.txt
```
### 注意事项：

#### 文件修改说明

1. 修改网站中头部导航标题，去`/resources/DirectoryLister.php `这个文件里搜索 `FileList` 然后全部替换为自己要改的。  

2. 修改网站标签栏的标题，去`/resources/themes/bootstrap/index.php `这个文件里把开头 `<title>` 标签中的`FileList`替换为自己要改的。  

3. 修改网站顶部公告栏内容，去`/resources/themes/bootstrap/index.php `这个文件里搜索 `FileList`。  

4. 网站头部公共文件：`/resources/themes/bootstrap/default_header.php `

5. 网站底部公共文件：`/resources/themes/bootstrap/default_footer.php `

6. 如果想要插入流量统计代码，那只需要把代码写到 `default_header.php` 文件内即可。

#### 不显示文件和目录

如果安装 lnmp一键包上传Directory Lister后，Directory Lister不显示文件和目录，那么可能是 PHP函数` scandir `被禁用了，取消禁用即可。
``` bash
sed -i 's/,scandir//g' /usr/local/php/etc/php.ini
# 取消scandir函数禁用
/etc/init.d/php-fpm restart
# 重启 PHP生效
```
#### 程序放在网站子目录不显示 README.html 的解决方法

因为程序有个判断 `README.html` 路径的代码，而如果是正常使用域名或IP(即使加上)，都是可以自适应的。

但是如果把程序放在子目录下，就会无法获取正确 `README.html` 路径，需要你手动修改下程序里的一句代码。

假设你将程序放在了子目录 `zimulu` 中（也就是 `http://xxx.xx/zimulu` 才能访问到程序网页）。

首先打开该文件： `/resources/themes/bootstrap/index.php`  

找到第5行的： `$suffix_array = explode('.', $_SERVER['HTTP_HOST']);`  

将其修改为： `$suffix_array = explode('.', $_SERVER['HTTP_HOST']."/zimulu");`
