---
title: 数据仓库ETL工具Kettle
date: 2022-01-29 19:34:29
categories:
    - BI商业大数据分析平台搭建
tags:
    - 大数据分析
    - BI平台
cover: /img/Kettle1.jpg
---
## 数据仓库与ETL

### 1、数据仓库

- 本质：专门针对于数据存储模型
- 实现：MySQL、Oracle、Hive……
- 应用：专门用于实现将各种各样数据进行统一化规范化的数据存储，为所有数据应用提供数据
  - 数据分析
  - 数据挖掘
  - 用户画像
  - 推荐系统
  - 风控系统
- 特点
  - 本身不产生数据
  - 本身也不使用数据
  - 用于实现复杂数据的存储
- 与数据库区别
  - 数据库：一般用于支撑业务数据的存储
    - 网站后台：用户数据、商品数据、订单数据
  - 数据仓库：专门为数据数据处理提供数据的
    - 业务数据
    - 用户行为
    - 爬虫数据
    - 第三方数据
    - 日志数据
- 问题
  - 数据种类非常的多，每一种数据的内容或者格式都不一样
    - 有结构化、有非结构化
    - 有合法的，有非法的
    - 有需要的，有不需要的
  - MySQL是一个专门用于存储结构化数据的数据存储工具
    - ·结构化
    - 需要
    - 合法
  - 如何将各种各样的数据存储在MYSQL中？
- 解决
  - 数据产生以后，不能直接放入数据仓库【MySQL】中存储
  - 对原始数据进行一步预处理，将需要的、合法的数据放入数据仓库中
  - 这一步预处理：ETL【数据清洗】

### 2、ETL

- 功能：实现数据的预处理，数据清洗过程，将原始数据经过ETL处理变成想要的数据，进行下一步的应用
- 实现
  - 抽取：读取需要处理的原始数据
  - 转换：将原始数据转换为目标数据
    - 过滤：将不需要的数据过滤掉
      - 原始数据中有100列
      - 实际需要30列
      - 过滤掉70列
    - 补全：将需要用到的数据补全
      - 每一个访问网站或者APP时，会有一个IP地址
      - 后台通过IP能获取到我们当前所在的国家、省份、城市
    - 转换：原始数据的格式不是我们想要的格式，转换为想要的格式
      - 原始数据：22/Aug/2020:12:20:35
      - |
      - |  转换
      - |
      - 目标格式：2020-08-22  12:20:35
  - 加载：将处理好的目标数据放入数据仓库中

### 3、Kettle

- 功能：实现可视化ETL
  - 可视化：不用写复杂的代码程序，可以通过图形化的界面来实现数据的处理
- 特点
  - 学习以及使用的成本比较低
  - 功能非常强大



## Windows版本部署Kettle

### 1、JDK安装配置

- 安装JDK

- 配置JDK环境变量

  - 添加bin目录的位置。我安装的是jdk 1.8，这个版本没什么要求，尽量选取稳定的，不要那么新的版本。

- 验证安装的结果

  - 在windows命令行执行

    ```
    java -version
    ```
    我的结果就是：
    ```cmd
    C:\Users\12506>java -version
    java version "1.8.0_281"
    Java(TM) SE Runtime Environment (build 1.8.0_281-b09)
    Java HotSpot(TM) 64-Bit Server VM (build 25.281-b09, mixed mode)
    ```



### 2、Kettle安装启动

- 解压安装

解压到一个不包含中文的路径中即可

- 启动

网上有很多教程，这里放一个我成功后的截图（运行 Spoon.bat 启动得很慢，是正常的~）：
![1643531212(1)](https://user-images.githubusercontent.com/59725125/151692395-b7f30098-f738-4b9d-a9f6-2feb37eadb7d.png)


## 三、Kettle使用

### 1、转换

- 功能：实现一个转换的程序
  - 输入：要读取什么数据进行转换
  - 转换：要对数据怎么进行处理【过滤、补全、转换】
  - 输出：要将处理好的数据保存到什么地方


### 2、作业

- 功能：将多个转换根据需求构建任务流
  - 任务流：很多个任务【每一个转换程序】根据自动运行的条件来运行就是任务流
  - 实际工作中，一次要执行很多个转换任务，如何实现这些任务的自动化执行
  - 自动运行
    - 第一种：定时运行
      - 每天的00:01分开始自动运行
    - 第二种：依赖关系
      - A先运行，A运行成功，B就自动运行

- 举例
  - 转换1：实现对数据的过滤
  - 转换2：实现对数据的补全
  - 转换3：实现对数据的转换
  - 作业：一个任务流
    - 转换1：每天00:10分自动运行
    - 转换2：转换1运行成功，转换2就开始运行
    - 转换3：转换2运行成功，转换3就开始运行

## txt与Excel交互

### 1、需求

- 将txt文件中的数据写入Excel表格中
    ```hql
    id,name,age,gender,province,city,region,phone,birthday,hobby,register_date
    392456197008193000,张三,20,0,北京市,昌平区,回龙观,18589407692,1970-8-19,美食;篮球;足球,2018-8-6 9:44
    267456198006210000,李四,25,1,河南省,郑州市,郑东新区,18681109672,1980-6-21,音乐;阅读;旅游,2017-4-7 9:14
    892456199007203000,王五,24,1,湖北省,武汉市,汉阳区,18798009102,1990-7-20,写代码;读代码;算法,2016-6-8 7:34
    492456198712198000,赵六,26,2,陕西省,西安市,莲湖区,18189189195,1987-12-19,购物;旅游,2016-1-9 19:15
    392456197008193000,张三,20,0,北京市,昌平区,回龙观,18589407692,1970-8-19,美食;篮球;足球,2018-8-6 9:44
    392456197008193000,张三,20,0,北京市,昌平区,回龙观,18589407692,1970-8-19,美食;篮球;足球,2018-8-6 9:44
    ```

### 2、分析

- 任务：一个转换程序
  - 输入：读取txt文件中内容
  - 转换：不需要
  - 输出：将内容加载到一个Excel文件中

### 3、实现

- step1：构建转换流程图

  - 新建一个转换任务
  
    - 有两种创建方式，要么双击转换，要么点文件-新建-转换。出来的页面：
    
    <img width="865" alt="6bc73de1d621c38c31fab1407f9b256" src="https://user-images.githubusercontent.com/59725125/154639532-1ffe897e-d90a-4712-aa2d-00459dd088be.png">

  - 将输入和输出拖入流程图的面板中。直接拖拽输入栏下的文本文件输入（因为是txt文件）。Excel 作为输出同理。
  
    <img width="517" alt="1645170844(1)" src="https://user-images.githubusercontent.com/59725125/154641131-8ef5e5b4-8e02-4814-b67f-7d800a54cb5d.png">

  - 连线： 选中文本文件输入，按住shift和鼠标左键，连接到Excel输出。

    <img width="193" alt="1645170935(1)" src="https://user-images.githubusercontent.com/59725125/154641354-ccf8025c-0e41-403c-b892-adcee8a5e495.png">

- step2：配置输入

  - 关联文件：双击文本文件输入输出。改名字，浏览选中文件，然后点增加
  
    <img width="485" alt="1645171597(1)" src="https://user-images.githubusercontent.com/59725125/154642918-2d1f3f77-fcac-442d-bfb9-216873fccbd1.png">
  
  - 配置文件的格式：刚才的界面点击内容。根据分隔符的不同进行选择，如果有中文，编码格式改成UTF-8。
  
    <img width="485" alt="1645171818(1)" src="https://user-images.githubusercontent.com/59725125/154643459-b3ef8de8-3816-4357-85fe-646a12228525.png">
  
  - 选择输出到下一步的数据： 刚才界面选择字段，点击获取字段，确定就好。默认把第一行的名称作为列名。
  
    <img width="485" alt="1645171891(1)" src="https://user-images.githubusercontent.com/59725125/154643640-539f62c8-bf18-4318-b987-3fa2f16293d8.png">

    :::warning
    右键可以选择删除，上移下移
    :::
    也可以点击预览。没问题最终点确定。
    <img width="466" alt="1645172083(1)" src="https://user-images.githubusercontent.com/59725125/154644062-96399400-7771-4193-a19f-3d4242ec29b3.png">

- step3：配置输出
    
    - 基本就改个地址就好。改个名字。

- step4：测试运行
    
    - 点击运行就好。

## Excel与Mysql交互

### 1、需求

- 读取刚刚输出的Excel文件中的数据，存储MySQL中

:::warning
因为一个excel文件有多个表，不要忘记选择工作表和字段。看好后缀，2007以后的excel后缀和之前的不一样。
:::

### 2、分析

- 这是一个转换程序

- 输入：读取这个Excel文件

- 转换：不需要实现转换

- 输出：存储到MySQL中的表中

  - 数据库：kettle_demo

    ```sql
    create database kettle_demo;
    ```

  - 表：t_user

    - 让Kettle根据上游的数据自行创建

### 3、实现

- step1：构建转换流程图

    <img width="217" alt="1645187135(1)" src="https://user-images.githubusercontent.com/59725125/154682687-dfd9cc3b-a940-49b2-aac7-4d2fe7dc6dd5.png">

- step2：配置输入

    读取刚刚输出的Excel文件中的数据

- step3：配置输出

    - 将连接MySQL的工具放入Kettle的lib目录中
    
        Kettle要想连接到MySQL，必须要安装一个MySQL的驱动，就好比我们装完操作系统要安装显卡驱动一样。
        将 MySQL jdbc 驱动包mysql-connector-java-5.1.47.jar和mysql-connector-java-8.0.13.jar导入到 data-integration/lib 中
        
        <img width="193" alt="1645188571(1)" src="https://user-images.githubusercontent.com/59725125/154685931-33963be0-fd1d-4d01-a185-277d48b01ba2.png">
        
        :::warning
        重新启动Kettle
        :::

    - 构建MySQL连接：地址、用户名、密码、数据库
    
        - 双击表输出，如果是第一次使用sql连接就需要点创建
        
        查看mysql的ip地址（大多数玩家是localhost）：
        ```sql
        select SUBSTRING_INDEX(host,':',1) as ip , count(*) from information_schema.processlist group by ip;
        ```
        
        <img width="359" alt="1645188237(1)" src="https://user-images.githubusercontent.com/59725125/154685169-91510057-02a7-418c-a147-581bb8c130c4.png">

        ![1645188816(1)](https://user-images.githubusercontent.com/59725125/154686486-dccc63b5-500a-4904-a85e-45cb89179170.jpg)
        
        手动输入目标表的名字，点击SQL，然后就能预览kettle会自动生成这个表的语句。当然手动可以更改。
        <img width="591" alt="1645189812" src="https://user-images.githubusercontent.com/59725125/154688783-85e451eb-b43b-4da6-99e5-5c4bc8bab245.png">

- step4：测试运行
    
    - 然后刷新Dategrid就能看见这张表了。返回Kettle运行就好了~
    - 所以，最快乐的事莫过于此，完美运行，即使有bug也能动动聪明的小脑瓜解决。
![](可视化ETL工具Kettle1_images/9ace3ee8.png)

## 六、常用组件

### 1、共享数据库连接


- 新建的数据库连接都只属于某一个转换程序
- 如果你想让所有的转换程序都能使用这个连接，需要开启共享

![](可视化ETL工具Kettle1_images/a6f37e77.png)

![](可视化ETL工具Kettle1_images/c76a0588.png)

然后在其他的转换任务中就能发现可以使用这个数据库连接了
![](可视化ETL工具Kettle1_images/b2147330.png)
### 2、表输入组件

- 需求：将t_user中的数据，同步到t_user1这张表中
![](可视化ETL工具Kettle1_images/968f3ee9.png)
1. 双击表输入组件，在弹出对话框中选择「获取SQL查询语句」。
![](可视化ETL工具Kettle1_images/69bbb85b.png)
2. 选择 t_user 表，点击确定。
![](可视化ETL工具Kettle1_images/302b1022.png)
![](可视化ETL工具Kettle1_images/23af42e4.png)
3. 点击预览
![](可视化ETL工具Kettle1_images/2e04c571.png)
4. 双击输出组件
![](可视化ETL工具Kettle1_images/6da5a7a9.png)
5. 点击SQL，可以看到自动生成的语句
![](可视化ETL工具Kettle1_images/a0587c8d.png)
6. 点击执行，确定，来到数据库就能看到目标表了~
![](可视化ETL工具Kettle1_images/6f6b689d.png)



### 3、插入更新组件
插入/更新组件能够将Kettle抽取的数据，与某个表的数据进行对比，如果数据存在就更新，不存在就插入。
测试之前开发的t_user_to_t_user1转换。直接执行转换，我们发现，Kettle又将t_user表中的数据新增到了t_user1表中
![](可视化ETL工具Kettle1_images/2c7dde14.png)

然后我们恢复数据
![](可视化ETL工具Kettle1_images/642e9dbb.png)
清空后执行一次后恢复原样。

这个时候我们用到插入更新组件！

![](可视化ETL工具Kettle1_images/d4877fb0.png)

1. 双击表输入组件，点击获取SQL查询语句，选择 t_user1 表。

![](可视化ETL工具Kettle1_images/7e1b0cc3.png)

2. 点击 预览按钮，查看Kettle是否能够从MySQL中读取数据。

![](可视化ETL工具Kettle1_images/b332ef09.png)

3. 双击 插入/更新 组件，点击浏览按钮，找到 t_user1 表。

![](可视化ETL工具Kettle1_images/ad7aa634.png)

4. 添加用来查询的关键字，设置表字段为：id，比较符为：=，流里的字段为：id。

![](可视化ETL工具Kettle1_images/cce01fcc.png)

5. 点击「获取和更新字段」，这样Kettle将会自动更新、或插入所有的字段。

![](可视化ETL工具Kettle1_images/f86a8f7c.png)



### 4、 switch/case组件
假设有三种数据，我们要分开。switch/case提供了一种条件判断的实现。
![](可视化ETL工具Kettle1_images/40e1a51a.png)

1.搭建流程图

![](可视化ETL工具Kettle1_images/36aa54fe.png)
![](可视化ETL工具Kettle1_images/bdb41341.png)

2.将组件按照下图方式连接起来
![](可视化ETL工具Kettle1_images/c81c2b60.png)

3. 配置输入很简单。配置switch/case组件如下：
![](可视化ETL工具Kettle1_images/0a83e6e7.png)

4. 配置输出也很简单。运行后得：
![](可视化ETL工具Kettle1_images/f03f31ec.png)



## JOB(作业)开发

每5秒钟执行一次Kettle转换，也就是每5秒钟将Excel中的数据抽取并装载到MySQL中

![](可视化ETL工具Kettle1_images/7a0f3427.png)

配置转换这里选择作业中要执行的转换，此处选择之前开发好的excel_to_mysql.ktr即可

![](可视化ETL工具Kettle1_images/e98ef9cf.png)

**注意：此处要先保存作业，然后再配置转换。**

在Start组件中，配置定时执行
![](可视化ETL工具Kettle1_images/29151190.png)