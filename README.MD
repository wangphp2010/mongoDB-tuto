 
git remote add origin https://github.com/wangphp2010/mongoDB-tuto.git

git push -u origin master


### mongoimport中有几个参数

-h,–host ：代表远程连接的数据库地址，默认连接本地Mongo数据库；
–port：代表远程连接的数据库的端口，默认连接的远程端口27017；
-u,–username：代表连接远程数据库的账号，如果设置数据库的认证，需要指定用户账号；
-p,–password：代表连接数据库的账号对应的密码；
-d,–db：代表连接的数据库；
-c,–collection：代表连接数据库中的集合；
-f, –fields：代表集合中的字段，可以根据设置选择导出的字段；
–type：代表导出输出的文件类型，包括csv和json文件；
-o, –out：代表导出的文件名；
-q, –query：代表查询条件；
–skip：跳过指定数量的数据；
–limit：读取指定数量的数据记录；
–sort：对数据进行排序，可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而-1是用于降序排列,如sort({KEY:1})
 


## 创建数据目录

MongoDB将数据目录存储在 db 目录下。但是这个数据目录不会主动创建，我们在安装完成后需要创建它。请注意，数据目录应该放在根目录下（(如： C:\ 或者 D:\ 等 )。

## 命令行下运行 MongoDB 服务器

```bash
cd C:\Program Files\MongoDB\Server\4.0\bin\

mongod --dbpath c:\data\db
 

```

## 连接MongoDB

```bash
cd C:\Program Files\MongoDB\Server\4.0\bin\

mongo

```

## 操作集合

```bash
$ mongo
> show dbs; // show all tables
> use myblog // switched to  db myblog or create the database myblog
> show collections ; // show all table at database myblog
> db.createCollection("users"); // create table named users
> show collections;
> db.users.renameCollection('staff');
> show collections;
> db.staff.drop(); // delete table staff
> show  collections;
> db.dropDatabase(); // delete all datas that we created
> show dbs;

```

## 操作文档

    就是相当于mysql数据的操作数据
```bash
$ mongo
> use  myblog;
> show collections
> db.createCollection('posts');
> db.posts.insert(
    {
        title:" my first blog",
        content:"so exciting starting to write my blog "
    }
);
> show collections;
> db.posts.find();
> db.posts.insert({
     title:"my seconde blog",
     content:"write something",
     tag:["not classed"]
});
> db.posts.find();
> for(var i=3;i<=10;i++){
    db.posts.insert(  
       {title:"my " + i +" blog"}
    )
};
> db.posts.find();
> db.posts.count();
> db.posts.remove({}); // remove all datas
> db.posts.count();
> db.posts.find();

```

## 带条件的文档 db.find

* db.[collectionName].find({'':''})
* $gte,$gt,$lte,$lt // 分别是大于等于 , 大于, 小于等于, 小于
* $eq,$ne //分别是 等于, 不等于
* 正则表达式:/k/ , /^k/
* db.[collectionName].distinct('fieldName') 取唯一值 去重


```bash
$ mongo
> use  blog;
> db.posts.remove({});
> db.posts.insert({title:"test the world of monster hunter","rank":2,'tag':"game"});
> db.posts.insert({title:"card mario Trial experience","rank":1,'tag':"game"});
> db.posts.insert({title:"installation of Utunbu16LTS","rank":3,'tag':"it"});
> db.posts.insert({title:"xinzhanyewangdazhi sold over 10000","rank":4,'tag':"game"});
> db.posts.insert({title:"is the development efficiency really hight of Ruby?","rank":7,'tag':"it"});
> db.posts.insert({title:"the tale of Saban release DLC","rank":4,'tag':"game"});
> db.posts.find({'tag':'game'});
> db.posts.find({'rank':{$gte:4}});
> db.posts.find({'rank':{$gt:4}});
> db.posts.find({'rank':{$lte:4}});
> db.posts.find({'rank':{$lt:4});
> db.posts.find({'title':/u/});
> db.posts.find({'title':/^R/});
> db.posts.find({'title':/^U/});
> db.posts.distinct('tag');

```

## 复杂条件抽文档
 

```bash
$ mongo 
use blog;
db.posts.find();
db.posts.find({'title':/u/ , 'rank':{$gte:5}  })   // AND  条件
db.posts.find({ $or:[{'title':/u/} ,{ 'rank':{$gte:4}  }   ]  })  // OR 条件
db.posts.find({ 'rank':{$in:[3,4]}}) // IN 条件
db.posts.insert({ 'title':"Oops! the chevalier has a big dill " , 'istop':true })
db.posts.find({'istop':{$exists:true }})  // 布尔值条件 如果这个字段存在着取出 ,不是判定这个字段的值是否为真
db.posts.insert({ 'title':"Oops! isbot" , 'isbot': false })
db.posts.find({'isbot':{$exists:true }}) 

```

## 指定抽出字段 

```bash
$ mongo
db.posts.find({} , {title:true,rank:1}) // 这里的true 和1 是一个意思 表示抽出字段title , rank
db.posts.find({} , {title:true,rank:1,_id:0}) //不抽id

```

## 文档的查找方法
* sort()
* limit() 
* skip(3) // 跳过多少条数据后,再取 相当于 offet 
* findOne()

```bash
$ mongo
use blog
db.posts.find({} , {_id:0}) . sort({rank:1}); // 从小到达 没有该字段的则按照默认排列
db.posts.find({} , {_id:0}) . sort({rank:-1}); // 从大到小
db.posts.find({} , {_id:0}) . limit(3);
db.posts.find({} , {_id:0}) . sort({rank:1}).limit(3);
db.posts.findOne({},{_id:0})
db.posts.find({} , {_id:0}).skip(3).limit(3);
```

## 文档更新
* update(<filter> , <update>, <option> )

官方文档 https://docs.mongodb.com/manual/reference/method/db.collection.update/

```bash

db.posts.findOne({'title':"test the world of monster hunter"});
db.posts.update({'title':"test the world of monster hunter"},{$set:{'rankd':10}})
db.posts.find();
db.posts.update({'tag':'it'} , {$set:{'rank':50} })// 默认是只更新第一条文档
db.posts.update({'tag':'it'} , {$set:{'rank':50},{multi:true} })//更新多条

db.posts.update({'title':"test the world of monster hunter"},{'rank':101}) //这条语句没有加$set 会把整条数据变成'rank':101 , 其他字段都会没有掉
 

```

## 几个特殊函数

* $inc 递加
* $mul 相乘
* $rename 字段改名
* $set 新增/修改 有则修改,无则新增
* $unset 字段删除

```bash
$ mongo
use blog 
db.posts.find({title:"card mario Trial experience"},{_id:0} ) ;
db.posts.update({title:"card mario Trial experience"},{$inc:{rank:3}}); // 让rank 增 3
db.posts.update({title:"card mario Trial experience"},{$mul:{rank:2}}); // 乘以2
db.posts.update({title:"card mario Trial experience"},{$rename:{'rank':'score'}});
db.posts.update({title:"card mario Trial experience"},{$set:{'istop':false }}); //把istop 设为 false

db.posts.update({title:"card mario Trial experience"},{$unset:{'istop':false }});  // 删除字段 不管后面的值是什么

```
## 文档的特殊更新

* upsert: 有则更新,无则追加 upsert = update + insert
* remove: 条件删除数据

```bash
mongo
use blog

db.posts.update({title:"this is a good game"  } , {title:"this is a good game" , rank:5,tag:"game"}) ; // 因为不存在这样的数据 , 所以没有原来数据不变
db.posts.find();
db.posts.update({title:"this is a good game"  } , {title:"this is a good game" , "rank":5,"tag":"game"},{upsert:true}) ;  // 有则更新,无则追加
db.posts.find();
db.posts.remove({title:"this is a good game" });
db.posts.find({},{_id:0});
```

## 使用索引 

* getIndexes()
* createIndex({},{})
* dropIndex({})

```bash

mongo
use blog
db.posts.getIndexes();
db.posts.createIndex({rank:-1}); // 建立rank 降序 索引
db.posts.getIndexes();
db.posts.dropIndex({rank:-1});
db.posts.getIndexes();
db.posts.createIndex({title:1},{unique:true}); // 建立title 升序索引 并且是唯一索引 
db.posts.getIndexes();
db.posts.find({},{_id:0});
db.posts.insert({title:"this is a bad game"})

```

## 备份和恢复

* mongodump
* mongorestore

```bash
$ mongo
> show dbs;
> use blog;
> db.posts.find({},{_id:0});
> exit
$ mkdir dbbak
$ cd dbbak
$ mongodump -d blog // 需要先增加环境变量
$ ls/dir // linux 用ls/wins用dir
$ mongo blog
> db.posts.find({},{_id:0});
> db.posts.remove({});
> db.posts.find({},{_id:0});
> exit;
$ mongorestore --drop // drop去掉当前数据,并用数据库文件进行恢复
$ mongo blog
> db.posts.find({},{_id:0});
> exit;
$ mongodump --help
 

```