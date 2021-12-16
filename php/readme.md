### 房智汇 PHP 开发工程师面试问卷：
- 在5天内尽量完成，如果时间安排有冲突，请告知完成时间
- 可以上网查资料，问问题，问人，但必须自己动手完成
- 测试题比实际项目代码容易很多
- 对问题有不确定的地方及时发邮件 yuerengui（at）housesigma.com，tanwei（at）housesigma.com，reed（at）housesigma.com
- 远程面试本身就是对远程工作能力的测试，请当成正常工作交流
- 答案请发回： 
```
To: reed（at）housesigma.com
cc: yuerengui（at）housesigma.com, tanwei（at）housesigma.com
Subject: PHP开发面试问卷回复 （姓名，城市）
```

---
#### 公司业务背景：
HouseSigma（房智汇）是面向加拿大-多伦多购房者的投资买房利器。
- 提供过去15年的房屋成交历史和价格。
- 多种数据图表
- 提供买卖房经纪服务
活动用户量，月pv 同行业排名前3（多伦多地区）。
用户集中在加拿大英文用户，中文版流量占不到4%

桌面版网站：https://housesigma.com/web/
移动版网站：https://housesigma.com/app/    
iOS App：https://itunes.apple.com/ca/app/housesigma-toronto-sold-price/id1255490256?mt=8    
Android App: https://play.google.com/store/apps/details?id=com.housesigma.android&hl=en_CA    
Android APK：https://housesigma.com/static/housesigma-2.13.0-prd.apk    

---
### 工作时间安排
多伦多和中国有12小时时差（冬季13小时）我们的工作时间
- 每周 1，4 团队语音会议
  - 夏季: 北京时间早上9：00，相当于多伦多时间晚上9：00
  - 冬季: 北京时间早上9：00，相当于多伦多时间晚上8：00
- 其余时间灵活安排自己的工作进度，工作时间，休息时间。
- 工作内容在 issue tracker + 钉钉。有急事打电话。

我们平时的工作氛围比较轻松
- 极少出现加班的情况。（过去一年里大概有几次，紧急bug）
- 安静的在家钻研技术打磨产品。不搞那些有用没用的 work hard play hard
- 请假，调休提前说。急事就直接去, 钉钉给我留言。

期待每周5x8小时工作是有效率的工作。
不想看到的：
- 无故连续不能提交进度
- 工作时有紧急业务不响应
- 工作时不查看协作工具

---
### 写在最前面的问题
> 我们是个小团队，花费时间招人不易，能提供的岗位也有限。希望尽量多了解申请人，力争每个成员都可以安心的在这里工作。    
> 有孩子，不能加班 在我们这里不是减分项。正常情况也不要加班。    

- 目前是否在职，为什么会想来房智汇做远程职位？这个工作对你来说有哪些好处和不便的地方？
- 未来2年有打算常住哪个城市？
- 如果将来的5年都在我们公司工作，对职位，收入，工作内容有什么期待？（最理想的状况）
- 团队会议时间外，平时的工作日打算怎么安排时间？
- 有什么想问的问题？


---
### 1. 搭建开发环境 Linux + NGINX + PHP
> 公司服务器统一使用的是centos8 + PHP7 + phalcon3，个人开发环境需要和服务器一致。
开发人员windows/osx电脑安装虚拟机运行linux

#### 任务：在本机上搭建开发环境。
1. 安装virtual box + centos 8，如果用其它类似的虚拟机也可以。如vmware。
2. Centos下安装软件 
    - NGINX
    - PHP7.3
    - PHP-PHALCON
1. 用git导出项目
    - https://github.com/housesigma/hr-interview  (git@github.com:housesigma/hr-interview.git)
1. 用NGINX架设本地测试网站 `localhost` 
2. 浏览器在本地环境运行git项目


##### 统一开发环境，php使用remi源安装：
```
# 安装源
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
dnf install yum-utils -y
dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm

# 安装PHP7.3
dnf module reset php
dnf module install php:remi-7.3

dnf install php php-fpm php-pdo php-mysqlnd php-phalcon3

dnf install -y php-fpm php-opcache php-bcmath php-gd php-mbstring php-mcrypt php-pdo php-process php-intl php-json php-xml php-mysqlnd
dnf install -y php-pecl-json-post php-pecl-mysql php-pecl-apcu php-pecl-http php-pecl-imagick php-pecl-memcache php-pecl-memcached php-pecl-xdebug  php-pecl-mongodb php-pecl-yaml

# 启动php-fpm
systemctl status php-fpm
systemctl enable php-fpm
systemctl restart php-fpm
```
#### 答案格式
- 虚拟机命令行结果
  - 切换到git项目目录，运行`git branch -a`
- 浏览器输出结果保存为文件


---
### 2. PHP，mySQL
- 可以使用任意MVC框架编写. 例如：phalcon, ci, kohana, laravel, slim, silex...
- 使用PDO连接数据库
- 正确使用PHP，MYSQL内置的json function

#### 任务2.1：导入数据到 Mysql 并转换数据后导出
- 下载：[sample.json](https://drive.google.com/file/d/1hXSxrxc342WtqPRAuzpLpTmi-wKprb_J/view?usp=sharing)
- 准备一台 mysql 服务器（8.0+版本或者差不多的mariadb，perconadb），创建一个 `sample` 表，添加 `string`, `string_json` 字段
```
CREATE TABLE `sample` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`string` TEXT NULL,
	`string_json` JSON NULL DEFAULT NULL,
	PRIMARY KEY (`id`)
)
```
- 将 sample.json 导入到 sample 表, `string` 字段中,
- 把 ` string` 字段里面用`^` 断字的文字转换为json格式，`^Food^^Health^^Arts^^Education^` ==> `["Food","Health","Arts","Education"]`
整张表每一行都要转换
- 转换结果写入到 'sample.string_json` 字段
- 转换成功后将 sample 表导出为 sample_output.json
- 问题：怎样用PHP长时间运行后台程序？
- 问题：处理大量数据记录时防止内存溢出

转换示例：
```
// 输入数据源
sample.json
[{"string": "^Food^^Health^^Arts^^Education^"}...]

// 导入到 sample 表

// 转换结果写入到 'sample.string_json` 字段

// 导出 sample 表
sample_output.json
[{"string": "^Food^^Health^^Arts^^Education^", "string_json": ["Food","Health","Arts","Education"]}...]
```

#### 任务2.2：在刚才创建的数据库上，写一条mySQL查询语句
- 参考 mySQL json function 
    - https://dev.mysql.com/doc/refman/8.0/en/json-search-functions.html
- 找出`sample` 表所有符合条件的：
  - `string_json` 数组包含>=3个元素
  - AND `string_json` 里面，文字`directional` 排在数组第三个

#### 答案格式
- 简单介绍解决方法
- 完整代码打包
- sample_output.json 结果打包，如果数据太大，使用网盘分享的方式贴上分享链接。


---
### 3. 写两条 ElasticSearch 查询语句
- ES 服务器：https://interview-test.es.asia-northeast1.gcp.cloud.es.io:9243
- 用户：interview  密码：n=H#b#e373
- index：listing_map
```
# 测试连接
curl --location --request GET 'https://interview-test.es.asia-northeast1.gcp.cloud.es.io:9243/listing_map' --header 'Authorization: Basic aW50ZXJ2aWV3Om49SCNiI2UzNzM='
```

- 问题 1 : 请查询 `(status.live === 1 && type_own_srch === 'D.') || ( status.sold === 1 && type_own_srch === 'V.')`  的其中 100 条结果
- 问题 2 : 请查询在限定的 Bounding Box 范围内（`-80.112304,43.239200,-78.134765,43.903829`）, `date_search` 距离当前时间小于三个月的所有结果


#### 答案格式
- 简单介绍解决方法
- 完整代码打包

---
### 4. 使用 Mongodb 验证某个点是否在多边形中

- 准备一台 Mongodb 服务器
- 将下面数据源中的多边形数据插入到 mongodb 记录中
- [查看数据源](http://geojson.io/#id=gist:yuerengui/287051fc8e767732744d3f91a8808df0&map=16/47.5401/-52.7550)
- 使用 mongodb 语句查询下面的点是否在多边形数据中
`-52.7570942, 47.5408525`，`-52.753193,47.5423525`

#### 答案格式
- 简单介绍解决方法
- 所用查询语句

---
### 5.  PHP 性能调试

服务器使用 TICK 全家桶做性能监控。有一天发现 php-fpm 进程忽然阻塞了大约1分钟（如图）。往回看监控发现类似的状况不止一次，不定时，似乎与服务器压力没有直接关联。
请问如何进行分析找到问题根源
- 非繁忙时期
- 3台 php-fpm 在同一时间进程耗尽阻塞
- nginx，php-fpm，mysql，mongodb，elasticsearch 分别运行在不同的服务器上
- 没有用容器，都是直接在 VPS 上安装的服务
- 同时在 TICK 上没有观察到突发IO，CPU 
- 其它的服务器没有观察到同一时间有突发IO，CPU

![snapshot_1611203014188zuvrav.png](https://image.tracup.com/snapshot_1611203014188zuvrav.png)

#### 答案格式
- 请尽量完整的介绍解决思路, 有可能的排查方向|手段, 如果使用到某些工具也请一并列出

---
### 6. 分享一个你难忘的开发经历，可以是修复某一个BUG、开发一个功能、一场技术分享或者其它你觉得可以说道的事情。
