+++
date = '2024-11-18T21:35:35+08:00'
draft = true
title = 'Kafka'
+++

### 1.下载kafka安装包

前往[kafka官网](https://kafka.apache.org/downloads)下载，

本文选择的是 kafka_2.13-3.9.0.tgz，只需要下载3.9.0

> Binary downloads:
>
> - Scala 2.12  - [kafka_2.12-3.9.0.tgz](https://downloads.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz) ([asc](https://downloads.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz.asc), [sha512](https://downloads.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz.sha512))
> - Scala 2.13  - [kafka_2.13-3.9.0.tgz](https://downloads.apache.org/kafka/3.9.0/kafka_2.13-3.9.0.tgz) ([asc](https://downloads.apache.org/kafka/3.9.0/kafka_2.13-3.9.0.tgz.asc), [sha512](https://downloads.apache.org/kafka/3.9.0/kafka_2.13-3.9.0.tgz.sha512))

选择下面那个就可以了。

这里简单说明一下

> ## Supported releases 支持版本
>
> 
>
> ### 3.9.0
>
> - Released November 6, 2024
>   发布于 2024 年 11 月 6 日
>
> - [Release Notes 发布说明](https://downloads.apache.org/kafka/3.9.0/RELEASE_NOTES.html)
>
> - Docker image: [apache/kafka:3.9.0](https://hub.docker.com/layers/apache/kafka/3.9.0/images/sha256-be37a49a4466f6620e2b67614dc2185e35d0e02e5cdc04ad5735ddd368a76b3e).
>   Docker 镜像：apache/kafka:3.9.0.
>
> - Docker Native image: [apache/kafka-native:3.9.0](https://hub.docker.com/layers/apache/kafka-native/3.9.0/images/sha256-ecf23c833391b00803876332b8642e261e675237e133cba8bc6a59ab8292a4a6).
>   Docker 原生镜像：apache/kafka-native:3.9.0.
>
> - Source download: [kafka-3.9.0-src.tgz](https://downloads.apache.org/kafka/3.9.0/kafka-3.9.0-src.tgz) ([asc](https://downloads.apache.org/kafka/3.9.0/kafka-3.9.0-src.tgz.asc), [sha512](https://downloads.apache.org/kafka/3.9.0/kafka-3.9.0-src.tgz.sha512))
>   源代码下载：kafka-3.9.0-src.tgz (asc, sha512)
>
> - Binary downloads:
>
>    二进制下载：
>
>   - Scala 2.12  - [kafka_2.12-3.9.0.tgz](https://downloads.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz) ([asc](https://downloads.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz.asc), [sha512](https://downloads.apache.org/kafka/3.9.0/kafka_2.12-3.9.0.tgz.sha512))
>     Scala 2.12 - kafka_2.12-3.9.0.tgz (asc, sha512)
>   - Scala 2.13  - [kafka_2.13-3.9.0.tgz](https://downloads.apache.org/kafka/3.9.0/kafka_2.13-3.9.0.tgz) ([asc](https://downloads.apache.org/kafka/3.9.0/kafka_2.13-3.9.0.tgz.asc), [sha512](https://downloads.apache.org/kafka/3.9.0/kafka_2.13-3.9.0.tgz.sha512))
>     Scala 2.13 - kafka_2.13-3.9.0.tgz (asc, sha512)
>
>   We build for multiple versions of Scala. This only matters if you are using Scala and you want a version built for the same Scala version you use. Otherwise, any version should work (2.13 is recommended).
>
>   我们为多个版本的 Scala 进行构建。这只有在您使用 Scala 并且希望使用与您使用的相同 Scala 版本构建的版本时才重要。否则，任何版本都应该可以工作（推荐使用 2.13）。

kafka 3.9.0在官网上是的这样的，

额，下面的中文官网是没有的，是已经翻译之后的。

kafka提供了三种下载方式，通过docker镜像可以一键安装部署。

通过源代码下载，你将获得kafka的源代码，你可以自行编译。

下面的二进制下载则是打包后能直接运行的应用程序，我们这次就是选择的这个方式。

但是你会看到二进制下载有两种，什么2.12 2.13之类的。

这是scala的版本，kafka是基于scala构建的所以这里有不同版本的scala编译的程序。

虽然kafka是基于scala构建的，但scala和kotlin类似也是一门jvm语言，所以你运行的话只需要准备java环境就可以了不需要下载scala。

### 2.解压

额，就是字面意思，解压压缩包就可以了。

Windows不用教了吧，下个什么7-zip，bandizip之类的解压就可以了。

linux命令也不难，

```bash
tar -zxvf 压缩包目录 -C 解压目录
```

解压完我个人的习惯是把解压目录改名成kafka，这个随意不是必须做的。

### 3.进行配置

我这次选择在Windows上部署kafka，主流方式是部署在linux上啦，其实都一样的。

但是部署在linux上一般不需要进行下面的配置：

先建一个文件夹用来存放kafka的日志。

然后进入kafka\config用vim或记事本打开里面的server.properties。

找到62行

> log.dirs=/tmp/kafka-logs

这是日志文件的地址，很显然这不是Windows文件系统应该有的地址。

现在换成你刚刚建的文件夹的位置。

但是注意，要把路径的单斜杠\重复一遍进行转义，即把\换成 \\\

接下来的步骤，不管部署在Windows还是linux上都要进行处理。

找到34行

>  listeners=PLAINTEXT://:9092

这是是kafka的地址，改成

```properties
listeners=PLAINTEXT://localhost:9092
```

这样kafka就会监听本地9092端口了。

