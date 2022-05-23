# MongoDB

## 1. Window

### 1.1 启动 MongoDB mongod.exe

> 在解压目录中，手动建立一个目录用于存放数据文件，如 data/db

#### 方式1：命令行参数方式启动服务

在 bin 目录中打开命令行提示符，输入如下命令：

```
mongod --dbpath=..\data\db
.\mongod.exe --dbpath=..\data\db
```

我们在启动信息中可以看到，mongoDB的默认端口是27017，如果我们想改变默认的启动端口，可以通过--port来指定端口。 为了方便我们每次启动，可以将安装目录的bin目录设置到环境变量的path中， bin 目录下是一些常用命令，比如 mongod 启动服务用的， mongo 客户端连接服务用的

#### 方式2：配置文件方式启动服务

在解压目录中新建 config 文件夹，该文件夹中新建配置文件 mongod.conf ，内如参考如下：

详细配置项内容可以参考[官方文档](https://gitee.com/link?target=https%3A%2F%2Fdocs.mongodb.com%2Fmanual%2Freference%2Fconfiguration-options%2F)

```
storage: 
    #The directory where the mongod instance stores its data.Default Value is \data\db on Windows. 
    dbPath: E:/mongodb-4.0.12/data/db
```

常用命令

```
mongod -f ../config/mongod.conf 
.\mongod.exe -f ../config/mongod.conf 

mongod --config ../config/mongod.conf
.\mongod.exe --config ../config/mongod.conf

# 更多配置
systemLog:
    destination: file
    #The path of the log file to which mongod or mongos should send all diagnostic logging information 
    path: E:/mongodb-4.0.12/log/mongod.log
    logAppend: true
storage:
    journal:
        enabled: true
    #The directory where the mongod instance stores its data.Default Value is /data/db. 
    dbPath: E:/mongodb-4.0.12/data/db
net:
    #bindIp: 127.0.0.1 
    port: 27017 
setParameter: 
    enableLocalhostAuthBypass: false
```

### 1.2 连接数据库

#### 1.2.1 shell 连接 MongoDB mongo.exe

```
.\mongo.exe
.\mongo.exe --host=127.0.0.1 --port=27017
```

#### 1.2.2 Compass 图形化界面客户端

> [下载地址](https://gitee.com/link?target=https%3A%2F%2Fwww.mongodb.com%2Fdownload-center%2Fv2%2Fcompass%3Finitial%3Dtrue)

如果是下载安装版，则按照步骤安装；如果是下载加压缩版，直接解压，执行里面的 MongoDBCompassCommunity.exe 文件即可。

在打开的界面中，输入主机地址、端口等相关信息，点击连接

![img.png](https://gitee.com/jcsn/mongo-db/raw/master/assets/img151515151.png)

## 2. Linux

### 2.1 下载安装

- 先到官网下载压缩包 mongod-linux-x86_64-4.0.10.tgz

- 上传压缩包到Linux中，解压到当前目录

  > tar -xvf mongodb-linux-x86_64-4.0.10.tgz

- 移动解压后的文件夹到指定的目录中

  > mv mongodb-linux-x86_64-4.0.10 /usr/local/mongodb

- 新建几个目录，分别用来存储数据和日志

```
#数据存储目录
mkdir -p /mongodb/single/data/db
#日志存储目录
mkdir -p /mongodb/single/log
```

- 新建并修改配置文件

```
    vim /mongodb/single/mongod.conf
systemLog:
    # MongoDB发送所有日志输出的目标指定为文件
    # The path of the log file to which mongod or mongos should send all diagnostic logging information
    destination: file
    # mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
    path: /mongodb/single/log/mongod.log
    # 当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
    logAppend: true
storage:
    # mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
    # The directory where the mongod instance stores its data.Default Value is /data/db.
    dbPath: /mongodb/single/data/db
    journal:
        # 启用或禁用持久性日志以确保数据文件保持有效和可恢复。
        enabled: true
processManagement:
    # 启用在后台运行mongos或mongod进程的守护进程模式。
    fork: true
net:
    # 服务实例绑定的IP，默认是localhost，192.168.142.196为虚拟机局域网 IP
    bindIp: localhost,192.168.142.196
    port: 27017
```

### 2.2 启动MongoDB服务

```
/usr/local/mongodb/bin/mongod -f /mongodb/single/mongod.conf

about to fork child process, waiting until server is ready for connections.
forked process: 1427
child process started successfully, parent exiting

# 如果启动后不是 successfully ，则是启动失败了
# 原因基本上就是配置文件有问题
# 查看服务是否启动
ps -ef |grep mongod
```

### 2.3 分别使用mongo命令和compass工具来连接测试

```
# 如果远程连接不上，需要配置防火墙放行，或直接关闭linux防火墙

# Centos
# 查看防火墙状态 
systemctl status firewalld 
# 临时关闭防火墙 
systemctl stop firewalld 
# 开机禁止启动防火墙 
systemctl disable firewalld

#  Ubuntu
# 查看防火墙状态 
ufw status

# 开启防火墙 
ufw enable
# 默认拒绝外部访问主机
ufw default deny
# 运行以上两条命令后，开启了防火墙，并在系统启动时自动开启
# 关闭所有外部对本机的访问，但本机访问外部正常

# 关闭防火墙 
ufw disable
# 默认允许外部访问本机
ufw default allow
# 允许外部访问53端口
ufw allow 53
# 拒绝外部访问53端口
ufw deny 53
# 允许某个IP地址访问本机所有端口
ufw allow from 192.168.0.1
```

### 2.4 停止关闭服务

```
* 快速关闭方法（快速，简单，数据可能会出错）

    * 通过系统的kill命令直接杀死进程 杀完要检查一下，避免有的没有杀掉
    * 如果一旦是因为数据损坏，则需要进行如下操作
    * 删除lock文件 ```rm -f /mongodb/single/data/db/*.lock```
    * 修复数据 ```/usr/local/mongdb/bin/mongod --repair --dbpath=/mongodb/single/data/db```

* 标准的关闭方法

    ```shell
        cd /usr/local/mongodb/bin
        # 客户端登录服务
        # 如果需要远程登录，必须先登录认证才行。 
        mongo --port 27017 
        # 切换到admin库 
        use admin 
        # 关闭服务 
        db.shutdownServer()
        # 开启 MongoDB服务
        /usr/local/mongodb/bin/mongod -f /mongodb/single/mongod.conf
    ```
```

## 3. 基本常用命令

### 3.1 数据库操作

#### 3.1.1 创建数据库

```
# 数据库不存在则自动创建
use 数据库名称
```

> 注意: 在 MongoDB 中，集合只有在内容插入后才会创建! 就是说，创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建。

#### 3.1.2 查看当前正在使用的数据库命令

```
# MongoDB 中默认的数据库为 test，如果你没有选择数据库，集合将存放在 test 数据库中
db
```

#### 3.1.3 特殊作用的数据库

admin： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特 定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。

local: 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合

config: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

#### 3.1.4 数据库的删除

```
# 主要用来删除已经持久化的数据库
db.dropDatabase()
```

### 3.2 集合操作 [ 表 ]

```
# 集合的显式创建
db.createCollection("dbname")
# 当前库中的表
show tables
show collections

# 集合的删除 返回 true false
db.dbname.drop()
```

集合的隐式创建 当向一个集合中插入一个文档的时候，如果集合不存在，则会自动创建集合

### 3.3 文档的插入

#### 3.3.1 单个文档插入，使用insert() 或 save() 方法向集合中插入文档，语法如下：

```
db.collection.insert(
    < document or array of documents > ,
    {
        writeConcern: < document > ,
        ordered: < boolean >
    }
)

# ordered
  # 如果为真，则按顺序插入数组中的文档，如果其中一个文档出现错误 MongoDB将返回而不处理数组中的其余文档
  # 如果为假，则执行无序插入，如果其中一个文档出现错误，则继续处理数组中的主文档
```

示例

```
db.comment.insert({
    "articleid": "100000",
    "content": "今天天气真好，阳光明 媚",
    "userid": "1001",
    "nickname": "Rose",
    "createdatetime": new Date(),
    "likenum": NumberInt(10),
    "state": null
})
```

1）comment集合如果不存在，则会隐式创建

2）mongo中的数字，默认情况下是double类型，如果要存整型，必须使用函数NumberInt(整型数字)，否则取出来就有问题了。

3）插入当前日期使用 new Date()

4）插入的数据没有指定 _id ，会自动生成主键值

5）如果某字段没值，可以赋值为null，或不写该字段。

注意：

1. 文档中的键/值对是有序的。
2. 文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)。 3. MongoDB区分类型和大小写。
3. MongoDB的文档不能有重复的键。
4. 文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符。

文档键命名规范：

- 键不能含有\0 (空字符)。这个字符用来表示键的结尾。
- .和$有特别的意义，只有在特定环境下才能使用。
- 以下划线"_"开头的键是保留的(不是严格要求的)。

#### 3.3.2 批量插入

```
try {
    db.comment.insertMany([{
        "_id": "1",
        "articleid": "100001",
        "content": "我们不应该把清晨浪费在手机上，健康很重要，一杯温水幸福你我 他。",
        "userid": "1002",
        "nickname": "相忘于江湖",
        "createdatetime": new Date("2019-08- 05T22:08:15.522Z"),
        "likenum": NumberInt(1000),
        "state": "1"
    }, {
        "_id": "2",
        "articleid": "100001",
        "content": "我夏天空腹喝凉开水，冬天喝温开水",
        "userid": "1005",
        "nickname": "伊人憔 悴",
        "createdatetime": new Date("2019-08-05T23:58:51.485Z"),
        "likenum": NumberInt(888),
        "state": "1"
    }, {
        "_id": "3",
        "articleid": "100001",
        "content": "我一直喝凉开水，冬天夏天都喝。",
        "userid": "1004",
        "nickname": "杰克船 长",
        "createdatetime": new Date("2019-08-06T01:05:06.321Z"),
        "likenum": NumberInt(666),
        "state": "1"
    }, {
        "_id": "4",
        "articleid": "100001",
        "content": "专家说不能空腹吃饭，影响健康。",
        "userid": "1003",
        "nickname": "凯 撒",
        "createdatetime": new Date("2019-08-06T08:18:35.288Z"),
        "likenum": NumberInt(2000),
        "state": "1"
    }, {
        "_id": "5",
        "articleid": "100001",
        "content": "研究表明，刚烧开的水千万不能喝，因为烫 嘴。",
        "userid": "1003",
        "nickname": "凯撒",
        "createdatetime": new Date("2019-08- 06T11:01:02.521Z"),
        "likenum": NumberInt(3000),
        "state": "1"
    }]);
} catch (e) { print (e); }
```

#### 3.3.3 查询

```
# 查询所有
db.comment.find()
db.comment.find({})

# 查询条件 所有
db.comment.find({userid:'1003'})

# 返回指定条数的记录
db.comment.find().limit(3)

# 投影查询 只显示 userid、nickname
db.comment.find({userid:"1003"},{userid:1,nickname:1})

# 跳过前三条
db.comment.find().skip(3)

# 查询条件 单个
db.comment.findOne({userid:'1003'})

# 分页查询：需求：每页2个，第二页开始：跳过前两条数据，接着值显示3和4条数据
db.comment.find().skip(0).limit(2)    #第一页
db.comment.find().skip(2).limit(2)    #第二页
db.comment.find().skip(4).limit(2)    #第三页

# sort() 1->为升序排列 -1->降序排列
# 对userid降序排列，并对访问量进行升序排列
db.comment.find().sort({userid:-1,likenum:1})

# AND 查询
db.comment.find({
    $and: [{
        likenum: {
            $gte: NumberInt(700)
        }
    }, {
        likenum: {
            $lt: NumberInt(2000)
        }
    }]
})

# OR 查询
db.comment.find({
    $or: [{
        userid: "1003"
    }, {
        likenum: {
            $lt: 1000
        }
    }]
})
```

skip(), limilt(), sort()三个放在一起执行的时候，执行的顺序是先 sort(), 然后是 skip()，最后是显示的 limit()，和命令编写顺序无关。

#### 3.3.4 文档的更新

```
# 覆盖的修改
db.comment.update({_id:"1"},{likenum:NumberInt(1001)})

# 局部修改 默认只修改第一条数据
db.comment.update({_id:"2"},{$set:{likenum:NumberInt(889)}})

# 修改所有符合条件的数据
db.comment.update({userid:"1003"},{$set:{nickname:"凯撒大帝"}},{multi:true})

# 列值增长的修改
db.comment.update({_id:"3"},{$inc:{likenum:NumberInt(1)}})

# 列值减少的修改
db.comment.update({_id:"3"},{$inc:{likenum:NumberInt(-10)}})
```

#### 3.3.5 删除文档

```
# 删除全部
db.comment.remove({})

# 删除_id=1的记录
db.comment.remove({_id:"1"})
```

#### 3.3.6 统计查询

```
# 统计所有记录数
db.comment.count()

# 统计userid为1003的记录条数
db.comment.count({userid:"1003"})
```

#### 3.3.7 正则的复杂条件查询 [ js语法 ]

```
# 查询评论内容包含“开水”的所有文档
db.comment.find({content:/开水/})

# 查询评论的内容中以“专家”开头
db.comment.find({content:/^专家/})
```

#### 3.3.8 比较查询

```
# 大于: field > value
db.集合名称.find({ "field" : { $gt: value }}) 

# 小于: field < value
db.集合名称.find({ "field" : { $lt: value }}) 

# 大于等于: field >= value
db.集合名称.find({ "field" : { $gte: value }}) 

# 小于等于: field <= value
db.集合名称.find({ "field" : { $lte: value }}) 

# 不等于: field != value
db.集合名称.find({ "field" : { $ne: value }}) 

# 查询评论点赞数量大于700的记录
db.comment.find({likenum:{$gt:NumberInt(700)}})
```

#### 3.3.9 包含查询

```
# 查询评论的集合中userid字段包含1003或1004的文档  
db.comment.find({userid:{$in:["1003","1004"]}})
# 取反
db.comment.find({userid:{$nin:["1003","1004"]}})
```

## 4. 索引-Index

### 4.1 索引的查看

```
# comment集合中所有的索引情况
db.comment.getIndexes()

[
        {
                "v" : 2,              # 索引版本
                "key" : {
                        "_id" : 1     # 1-升序 -1 降序
                },
                "name" : "_id_",      # 索引名
                "ns" : "test.comment" # 所属表
        }
]
```

### 4.2 索引创建与删除

#### 4.2.1 语法

```
db.collection.createIndex(keys, options)
```

#### 4.2.2 参数

| Parameter | Type     | Description                                                  |
| --------- | -------- | ------------------------------------------------------------ |
| keys      | document | 包含字段和值对的文档，其中字段是索引键，值描述该字段的索引类型。1-升序 -1 降序。比如：{字段:1或-1}。另外，MongoDB支持几种不同的索引类型，包括文本、地理空间和哈希索引。 |
| options   | document | 可选。包含一组控制索引创建的选项的文档。有关详细信息，请参见选项详情列表。 |

options（更多选项）列表：

| Parameter          | Type          | Description                                                  |
| ------------------ | ------------- | ------------------------------------------------------------ |
| background         | Boolean       | 建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，默认值为false |
| unique             | Boolean       | 建立的索引是否唯一，默认值为false.                           |
| name               | string        | 索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称 |
| expireAfterSeconds | integer       | 指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。 |
| v                  | index version | 索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本 |
| weights            | document      | 索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重 |
| default_language   | string        | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语 |
| language_override  | string        | 对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为language |

#### 4.2.3 示例

```
# 单字段升序创建索引
db.comment.createIndex({userid:1})

# 复合索引：对 userid 和 nickname 同时建立复合（Compound）索引
db.comment.createIndex({userid:1,nickname:-1})

# 指定索引的移除
db.comment.dropIndex({userid:1})

# 所有索引的移除 _id 的字段的索引是无法删除的，只能删除非 _id 字段的索引
db.comment.dropIndexes()
```

### 4.3 索引的使用

### 4.3.1 执行计划

```
# 查看根据userid查询数据的情况
db.comment.find({userid:"1003"}).explain()
{
        "queryPlanner" : {
                "plannerVersion" : 1,
                "namespace" : "test.comment",
                "indexFilterSet" : false,
                "parsedQuery" : {
                        "userid" : {
                                "$eq" : "1003"
                        }
                },
                "winningPlan" : {
                        "stage" : "COLLSCAN",   # 表示全集合扫描
                        "filter" : {
                                "userid" : {
                                        "$eq" : "1003"
                                }
                        },
                        "direction" : "forward"
                },
                "rejectedPlans" : [ ]
        },
        "serverInfo" : {
                "host" : "wenjiabao",
                "port" : 27017,
                "version" : "4.0.10",
                "gitVersion" : "c389e7f69f637f7a1ac3cc9fae843b635f20b766"
        },
        "ok" : 1
}

# 创建索引
db.comment.createIndex({userid:1})

# 查看根据userid查询数据的情况
db.comment.find({
    userid: "1003"
}).explain() {
    "queryPlanner": {
        "plannerVersion": 1,
        "namespace": "articledb.comment",
        "indexFilterSet": false,
        "parsedQuery": {
            "userid": {
                "$eq": "1013"
            }
        },
        "winningPlan": {
            "stage": "FETCH",
            "inputStage": {
                "stage": "IXSCAN",    # 基于索引的扫描
                "keyPattern": {
                    "userid": 1
                },
                "indexName": "userid_1",
                "isMultiKey": false,
                "multiKeyPaths": {
                    "userid": []
                },
                "isUnique": false,
                "isSparse": false,
                "isPartial": false,
                "indexVersion": 2,
                "direction": "forward",
                "indexBounds": {
                    "userid": ["[\"1013\", \"1013\"]"]
                }
            }
        },
        "rejectedPlans": []
    },
    "serverInfo": {
        "host": "9ef3740277ad",
        "port": 27017,
        "version": "4.0.10",
        "gitVersion": "c389e7f69f637f7a1ac3cc9fae843b635f20b766"
    },
    "ok": 1
}
```

### 4.3.2 涵盖的查询

```
# 当查询条件和查询的投影仅包含索引字段时，MongoDB直接从索引返回结果，而不扫描任何文档或将文档带入内存
db.comment.find({userid:"1003"},{userid:1,_id:0})
```

## 5. 案例

文章评论的增删改查

![img.png](https://gitee.com/jcsn/mongo-db/raw/master/assets/img11111.png)

代码已实现