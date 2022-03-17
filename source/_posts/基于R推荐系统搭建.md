---
title: 基于R推荐系统搭建
date: 2021-12-15 16:12:39
categories:
    - R语言数据分析
tags:
    - R语言
    - 机器学习
    - 推荐系统
cover: /img/基于R推荐系统搭建.jpg
---
## 前言

首先，R语言牛皮之处就不过多阐述，总之很方便很方便。R语言就是为了数据处理而生的，可以轻松链接数据库，可以轻松对数据进行处理和分析。

这篇博客主要归纳一下如何使用R语言搭建一个简单的推荐系统。首先导入包：
```
#install.packages("RJDBC") 如果第一次使用，要下载所需要的包，然后使用。
library(RJDBC)
#以下是一些画图使用的包，还有一些机器学习算法。
library(rvest)
library(ggplot2)
library(dplyr)
library(scales)
library(maps)
library(mapproj)
library(plotly)
library(rpart)
library(rpart.plot)
library(C50)
library(tree)
library(ROCR)
library(randomForest)
library(e1071)
library(naivebayes)
library(nnet)
library(kknn)
```
## 链接Oracle数据库
如果有小伙伴想复现这个项目，可以直接去我的github下载csv数据文件，用R导入csv数据也是一样的效果(Imm..表只给出了前几百条数据，因为数据文件太大，无法上传仓库)：

[项目仓库](https://github.com/1250681923/DataAnalyseMBDS.git)

虽然有些公司可能节约成本选择使用Mysql，但是学习Oracle还是非常有必要的。在顶端DBA大师眼中，能在PDB中进行协作的Oracle有着无可取代的天然优势。更别提我们可以通过Hive工具创建Oracle内外表后使用Oracle sql, Oracle Nosql, Hadoop HDFS, MongDB搭建数据湖。这对企业的大数据架构的整合有着巨大的价值。

在此之前需要下载jdbc的jar包：[点击链接进行下载drives文件](https://github.com/1250681923/picture.git)

链接配置规则： 
```
conn<-dbConnect(drv,“jdbc:oracle:thin:@主机IP:1521:数据库名称”,“用户名称”,“密码”)
```
远程链接：
```
drv <- RJDBC::JDBC(driverClass = "oracle.jdbc.OracleDriver", classPath =  Sys.glob("C:/Users/12506/OneDrive/Desktop/ESTIA3A/R/Oracle/drivers/*"))

conn <- dbConnect(drv, "jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=144.21.67.201)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=pdbest21.631174089.oraclecloud.internal)))", "ZHANG2B20", "ZHANG2B2001")

allTables <- dbGetQuery(conn, "SELECT owner, table_name FROM all_tables where owner = 'ZHANG2B20'")
```
本地链接(我没有试过，你们可以参考如下例子)：
```
drv<-JDBC("oracle.jdbc.driver.OracleDriver","ojdbc6_g.jar", identifier.quote="\"")  ##java中JDBC的套路
conn<-dbConnect(drv,"jdbc:oracle:thin:@10.0.0.214:1521:zlhis","zlhis1234","his123") ##建立一个连接
EMP<-dbReadTable(conn,'EMP') ##根据连接和表名获取Oracle中的表
table1<-dbGetQuery(conn,"select * from user_tables")  ##根据sql记录获取Oracle中表的数据
————————————————
版权声明：本文为CSDN博主「dltan」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/tandelin/article/details/99300248
```
如果数据有乱码：
```
names(table1)=iconv(names(table1),"UTF-8","GBK") ##若是表中列名为中文，读取时出现乱码，可用这句来搞定乱码情况
```

所以按着我的例子来吧~ 

接下来介绍下数据：
```
tableCatalogue <- dbGetQuery(conn, "select * from Catalogue")
tableClients <- dbGetQuery(conn, "select * from Clients")
tableIm <- dbGetQuery(conn, "select * from IMMATRICULATION")
tableMar <- dbGetQuery(conn, "select * from MARKETING")
View(tableCatalogue)
View(tableClients)
View(tableIm)
View(tableMar)
```
一共有四个表:
- Catalogue 记录着一共270种汽车型号，不同的颜色，品牌，马力等等。
- Client 记录着以往的购买记录，客户的信息对应所购买的车牌号。
- Immatriculation 记录着车牌号对应的汽车信息，这些汽车的种类包含于上述270种。
- Marketing 9个新客户，为他们提供一个车型的推荐，最终交付成果是给他们
![10](https://user-images.githubusercontent.com/59725125/146154033-65c3fcb0-6f36-4354-b64c-dbccde7a8a58.png)

## 确定标签

以下举一个例子，就好比我们要探究客户的特征和购买车辆的价格的关系。

因为Client表只记录了客户的信息对应所购买的车牌号（车管所数据）。我们需要融合Client和Catalogue两个表，就能把标签-价格和客户的特征放在同一个表中：
```
tableIm_f <- merge(tableIm, tableCatalogue, by = c("MARQUE","NOM", "PUISSANCE", "LONGUEUR", "NBPORTES","COULEUR","OCCASION","PRIX"))
summary(tableIm_f)
tableImPrix <- tableIm_f[c(8, 9)]
tablePrixFinal <- merge(tableClients, tableImPrix, by = "IMMATRICULATION", incomparables = NA)
```
(未完待续。。。)

