## 安装MongoDB

下载地址：https://www.mongodb.com/download-center#community

下载完安装包，并解压 `tgz`（以下演示的是 64 位 Linux上的安装） 。
```bash
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz    # 下载
tar -zxvf mongodb-linux-x86_64-3.0.6.tgz                                   # 解压

mv  mongodb-linux-x86_64-3.0.6/ /usr/local/mongodb                         # 将解压包拷贝到指定目录
```

MongoDB 的可执行文件位于 bin 目录下，所以可以将其添加到 PATH 路径中：
```bash
export PATH=<mongodb-install-directory>/bin:$PATH
```
__<mongodb-install-directory>__  为你 MongoDB 的安装路径。如本文的 /usr/local/mongodb 。


#### 创建数据库目录

* `MongoDB`的数据存储在`data`目录的`db`目录下，但是这个目录在安装过程不会自动创建，所以你需要手动创建`data`目录，并在`data`目录中创建`db`目录。
* 以下实例中我们将data目录创建于根目录下(/)。

_注意：`/data/db` 是 `MongoDB` 默认的启动的数据库路径`(--dbpath)`。_

```bash
mkdir -p /data/db
```

#### 命令行中运行 MongoDB 服务

可以再命令行中执行`mongo`安装目录中的`bin`目录执行`mongod`命令来启动`mongdb`服务。

_注意：如果你的数据库目录不是/data/db，可以通过 --dbpath 来指定。_

```mongodb
$ ./mongod
2015-09-25T16:39:50.549+0800 I JOURNAL  [initandlisten] journal dir=/data/db/journal
2015-09-25T16:39:50.550+0800 I JOURNAL  [initandlisten] recover : no journal files present, no recovery needed
2015-09-25T16:39:50.869+0800 I JOURNAL  [initandlisten] preallocateIsFaster=true 3.16
2015-09-25T16:39:51.206+0800 I JOURNAL  [initandlisten] preallocateIsFaster=true 3.52
2015-09-25T16:39:52.775+0800 I JOURNAL  [initandlisten] preallocateIsFaster=true 7.7
```

#### MongoDB后台管理 Shell

如果你需要进入MongoDB后台管理，你需要先打开mongodb装目录的下的bin目录，然后执行mongo命令文件。

MongoDB Shell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境。

当你进入mongoDB后台后，它默认会链接到 test 文档（数据库）：

```bash
$ cd /usr/local/mongodb/bin
$ ./mongo
MongoDB shell version: 3.0.6
connecting to: test
Welcome to the MongoDB shell.
……
```
由于它是一个`JavaScript shell`，您可以运行一些简单的算术运算:
```bash
> 2+2
4
> 3+6
9
```
现在让我们插入一些简单的数据，并对插入的数据进行检索：
```mongodb
> db.test.insert({x:10})
WriteResult({ "nInserted" : 1 })
> db.test.find()
{ "_id" : ObjectId("5604ff74a274a611b0c990aa"), "x" : 10 }
>
```
第一个命令将数字 10 插入到 test 集合的 x 字段中。

#### MongoDb web 用户界面

MongoDB 提供了简单的 HTTP 用户界面。 如果你想启用该功能，需要在启动的时候指定参数 `--rest` 。

_注意：该功能只适用于 MongoDB 3.2 及之前的早期版本。_

```bash
$ ./mongod --dbpath=/data/db --rest
```

`MongoDB` 的 `Web` 界面访问端口比服务的端口多`1000`。

如果你的`MongoDB`运行端口使用默认的`27017`，你可以在端口号为`28017`访问`web`用户界面，即地址为：[http://localhost:28017](http://localhost:28017)。