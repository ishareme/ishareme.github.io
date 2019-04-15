---
title: MongoDB与Mongoose 
date: 2018-10-08 22:49:01
comments: true
tags:
    - Mongoose
categories:
    - 渣技术
---
# MongoDB

## 何为MongoDB？
MongoDB是一个介于关系型数据库和非关系型数据库之间的开源产品，它是功能最为丰富的非关系型数据库，也是最像关系型数据库的非关系型数据库。

MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

## 下载安装
下载: 
```
sudo curl -O https://fastdl.mongodb.org/osx/mongodb-osx-ssl-x86_64-4.0.0.tgz
```
解压: 
```
sudo tar -zxvf mongodb-osx-ssl-x86_64-4.0.0.tgz
```
重命名为mongodb目录:
```
sudo mv mongodb-osx-ssl-x86_64-4.0.0 mongodb
```
## 概念

![image](http://oubl6fzsm.bkt.clouddn.com/1538979571423.jpg)

## 常见使用

- **查询所有结果**
```
select * from article

db.article.find()
```

- **指定返回哪些键**
```
select title, author from article

db.article.find({}, {"title": 1, "author": 1})
```

- **where条件**
```
select * from article where title = "深入浅出Node"

db.article.find({"title": "深入浅出Node"})
```

- **and条件**
```
select * from article where title = "深入浅出Node" and author = "朴灵"

db.article.find({"title": "深入浅出Node", "author": "朴灵"})
```

- **or条件**
```
select * from article where title = "深入浅出Node" or author = "朴灵"

db.article.find({"$or": [{"title": "深入浅出Node"}, {"author": "朴灵"}]})
```

- **比较条件**
```
select * from article where read >= 100

db.article.find({"like": {"$gt": 100}}) //$gt(>)、$gte(>=)、$lt(<)、$lte(<=) $ne(!=)


select * from article where read >= 100 and read <= 200

db.article.find({"like": {"$gte": 100, "lte": 200}})
```

- **in条件**
```
select * from article where author in ("a", "b", "c")

db.article.find({"author": {"$in": ["a", "b", "c"]}})
```

- **like条件**
```
select * from article where title like "%深入%"

db.article.find({"title": /深入/i})
```
- **取反**
```
db.article.find({"author": {"$not": /mongodb/i}})
```

- **排序**
```
//升序
select * from article where type = "mongodb" order by read desc
db.article.find({"type": "mongodb"}).sort({"like": -1})

//降序
select * from article where type = "mongodb" order by read asc
db.article.find({"type": "mongodb"}).sort({"like": 1})
```

- **更新特定字段（$set）**
```
update article set like = 10000 where _id = 123

db.article.update({"_id": 123}, { "$set": {"like": 10000}})
```

- **递增或递减（$inc）**
```
db.article.update({"_id": 123}, { "$inc": {"like": 10}}) // 每次count都加10
```

- **数组追加（$push）**
```
db.article.update({"_id": 123}, { "$push": {"comment": '真好'}})
```

- **删除指定文档**
```
delete from article where title = "mongodb"

db.article.remove({title: "mongodb"})
```

- **limit和skip**
```
select * from article limit 10, 20

db.article.find().skip(10).limit(20)  //结合limit()和skip()来达到分页效果
```

```
//数据库
use xxx          //创建或选择数据库

show dbs / collections

use xxx  db.dropDatabase()  //删除数据库


//集合
db.createCollection("mycol", { capped : true, autoIndexId : true, size : 6142800, max : 10000 } )   //创建集合
db.mycol2.insert({"name" : "黄明照"})  //创建集合并插入

db.mycol2.drop()   //删除集合

db.mycol2.save(...)   //无_id插入文档 有_id表示更新
               
db.mycol2.update({'age': 18},{$set:{'age': 20}}) //更新集合

db.mycol2.remove({'age': 20}) //删除文档

db.mycol2.find({key1:value1, key2:value2}).pretty()  //查询   并and
db.mycol2.find({$or: [{key1: value1}, {key2:value2}]}).pretty() //or 或
db.mycol2.find({"age": {$gt:18}, $or: [{"name": "hmz"},{"name": "黄明照"}]}).pretty()  //查询   并和或一起使用

//limit 和 skip 方法   做分页查询
db.COLLECTION_NAME.find().limit(NUMBER)
db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)

// 排序
db.col.find({},{"name":1, _id:0}).sort({"age":-1})
```

# Mongoose

## 何为Mongoose？
简单的说，Mongoose就是对node环境中MongoDB数据库操作的封装，一个对象模型工具，将数据库中的数据转换为JavaScript对象以供我们在应用中使用。

## Schema、Model、Entity

在使用Mongoose前，先了解一下 Mongoose 中的三个概念：Schema、Model、Entity

### Schema
`Schema`是一种以文件形式存储的数据库模型骨架，不具备数据库的操作能力，其实也可以看作是表结构的定义。

如何创建一个Schema？
```node
const UserSchema = new Schema({
    token: String,
    is_banned: {type: Boolean, default: false}, //是否禁言
    enable: { type: Boolean, default: true }, //用户是否有效
    is_actived: {type: Boolean, default: false}, //邮件激活
    username: String,
    password: String,
    email: String,  //email唯一性
    code: String,
    email_time: {type: Date},
    phone: {type: String},
    description: { type: String, default: "这个人很懒，什么都没有留下..." },
    avatar: { type: String, default: "http://p89inamdb.bkt.clouddn.com/default_avatar.png" },
    bg_url: { type: String, default: "http://p89inamdb.bkt.clouddn.com/FkagpurBWZjB98lDrpSrCL8zeaTU"},
    ip: String,
    ip_location: { type: Object },
    agent: { type: String }, // 用户ua
    last_login_time: { type: Date },
    openid: {
        WeiChat: String,
        WeiBo: String,
        QQ: String,
    },
    create_time: { type: Date },
    // retrieve_time: { type: Number }, // 用户发送激活请求的时间
    image_article: [{ type: mongoose.Schema.Types.ObjectId, ref: 'ImageArticle', }],
    collection_image_article: [{ type: mongoose.Schema.Types.ObjectId, ref: 'ImageArticle', }],
    collection_reading_article: [{ type: mongoose.Schema.Types.ObjectId, ref: 'ReadingArticle', }],
    collection_music_article: [{ type: mongoose.Schema.Types.ObjectId, ref: 'MusicArticle', }],
    collection_film_article: [{ type: mongoose.Schema.Types.ObjectId, ref: 'FilmArticle', }],
    collection_sound_article: [{ type: mongoose.Schema.Types.ObjectId, ref: 'SoundArticle', }],
    following_user: [{type: mongoose.Schema.Types.ObjectId, ref: 'User'}], //关注
    follower_user: [{type: mongoose.Schema.Types.ObjectId, ref: 'User'}] //粉丝
});

```

对了，MongoDB支持的数据类型很多，如下:
- String：字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。

- Integer：整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。

- Boolean：布尔值。用于存储布尔值（真/假）。

- Double：双精度浮点值。用于存储浮点值。

- Arrays：用于将数组或列表或多个值存储为一个键。

- Timestamp：时间戳。记录文档修改或添加的具体时间。

- Object：用于内嵌文档。

- Null：用于创建空值。

- Symbol：符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。

- Date：日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。

- Object ID：对象 ID。用于创建文档的 ID。

- Binary Data：二进制数据。用于存储二进制数据。

- Code：代码类型。用于在文档中存储 JavaScript 代码。

- Regular expression：正则表达式类型。用于存储正则表达式。

### Model
由Schema发布生成的模型，具有抽象属性和行为的数据库操作对象。正是Model的存在，让我们操作数据库更加方便快捷。
依赖`Schema`生成一个`Model`：

```node
const User = mongoose.model("User", UserSchema);
module.exports = User;
```

### Entity
由Model创建的实体，它的操作也会影响数据库。
依赖`Model`，创造一个`Entity`：
```node
const UserModel = require('../../models/user/user')
const newUser = new UserModel({
    username: '就这样子吧',
    password: sha1('xxx'),
    token: createToken('就这样子吧'),
    email: '651734877@qq.com',
    phone: '13003919397',
    agent: req.headers['user-agent'],
    ip: ip,
    ip_location: geoip.lookup(ip)
})
```

## document的CRUD操作

### 创建

```
const Articles = mongoose.model('Articles', articleSchema);

let article = new Articles({ title: '我不知道' });
//使用实例创建
article.save((err) => {
  if (err) return handleError(err);
  // saved!
})

//使用Model类创建
Articles.create({ title: '我不知道' },  (err, article) =>  {
  if (err) return handleError(err);
  // saved!
})
```

### 查询
Mongoose查找文档很容易，它支持MongoDB的丰富的查询语法。 可以使用每个models的find，findById，findOne等静态方法进行查找文档。
```
UserModel.findOne({ 'name': '黄' }, (err, person) => {
  if (err) return handleError(err);
 // get data
})

 let user = await UserModel.findOne({email: userObj.email, is_actived: false})
```

### 删除
有remove()、findOneAndRemove()、findByIdAndRemove()
```
ArticlesModel.remove({'_id': id}, (err) => {   
    if (err) {   
       return res.status(400).send({   
            message: '删除失败',  
            data: []   
        });   
    } else {   
    }   
})  
```

### 更新
有update()、updateMany()、find() + save()、updateOne()、findByIdAndUpdate()、fingOneAndUpdate()等等
```
const userObj = {
    email: req.body.email,  //email唯一性
    username: req.body.username,
    password: password,
    token: createToken(req.body.email),
    is_actived: true,
    ip: ip,
    ip_location: ip_location,
    agent: agent,
    create_time: Date.now(),
}
await UserModel.findOneAndUpdate({email: userObj.email, is_actived: false}, {
    $set: userObj
})
```

## 索引

索引可以加快查询速度，我们通过一个例子来看看效果。
在mongo Shell中，我们创建10000条数据：
```node
$ mongo

for (var i = 0; i < 10000; i++){
    db.users.insert({'name':'user' + i}); 
}
```
看看未加索引的情况下查询：
```
db.hello.find({'name': 'user1000'}).explain('executionStats')
```
![image](http://oubl6fzsm.bkt.clouddn.com/mongoose-explain.jpg)
图中可以看出`executionTimeMillis`字段为7。
现在我们来添加索引后执行查询：
```
db.hello.ensureIndex({name: 1});
db.hello.find({'name': 'user1000'}).explain('executionStats')
```
![image](http://oubl6fzsm.bkt.clouddn.com/mongoose-explain2.jpg)
图中可以看出`executionTimeMillis`字段为1。

从上面的实例可以看出，加了索引后，能快速的查询一条，这可以看到索引极大的提升了查询速度。

下面我们看看如何使用Mongoose创建：
```
const ArticlesSchema = new Schema({   
  title: {  
    ...
    index: true    / 1 / -1 
  }  
});
```
我们还可以创建唯一索引：
```
const ArticlesSchema = new Schema({   
  title: {   
    ...    
    index: true,   
    unique: true      
  }  
});
```
复合索引：
```
ArticlesSchema.index({name: 1, create_time: -1});  //可以指定不同字段的排序 1是正序，-1是倒序
```
**注**: 对于添加的每一条索引，每次写操作（插入、更新、删除）都将耗费更多的时间。这是因为，当数据发生变化时，不仅要更新文档，还要更新集合上的所有索引。通常，在一个特定的集合上，不应该拥有两个以上的索引。如果数据量很多且查询多于更新时，可以用索引提高查询的速度。

## 虚拟属性VirtualType
虚拟属性并不会存储到MongoDB中，利用它，我们可以格式化和自定义组合属性值。
```
const UsersSchema = new Schema({  
  ...   
  address: {   
    city: {type: String},   
    street: {type: String}   
  }  
});
const address = UsersSchema.virtual('address.full');   

address.get(function () {   
  return this.address.city + ' ' + this.address.street;  
});
```
通过定义set方法，我们可以给两个字段快速赋值：
```
address.set(function(v) {   
  const split = v.split(' ');   
  this.address.city = split[0];   
  this.address.street = split[1];  
});

const body = {   
  name: 'abcd',   
  phone: '13123123123',   
  age: 20,   
  sex: 'male'   
};   
const user = new UsersModel(body);   
user.address.full = 'xiamen meitu';   
user.save();
```

## 验证器Validate

### 内置验证器
Mongoose提供了几个内置验证器。

- 所有的SchemaType都有内置的require验证器。
- 数值（ Numbers ）有最大（max）和最小（min）的验证器。
- 字符串（String）有enum，maxLength和minLength验证器。

```
const UsersSchema = new Schema({  
  ...
  age: {   
    type: Number,   
    min: [18, "自定义错误提示"]   
    max: 30,   
    required: true   
  },   
  sex: {   
    type: String,   
    enum: {   
      values: ['male', 'female'],
      message: '`{PATH}` 是 `{VALUE}`， 您必须确认您的性别!'   
    },   
    required: true   
  }  
});  

mongoose.model('users', UsersSchema);
```

### 自定义验证器
```
const UsersSchema = new Schema({  
  ...   
  phone: {   
    type: String,   
    validate: {   
      validator: function (v) {
          return /1[3|5|8]\d{9}/.test(v);
      },
      message: '`{PATH}` 必须是有效的11位手机号码!'  
    },   
    required: true   
  }  
}); 

或者

UsersSchema.path('phone').validate(function (v) {
    return /1[3|5|8]\d{9}/.test(v);
}, '`{PATH}` 必须是有效的11位手机号码!');
```

### 错误提示
```
const user = new UsersModel(req.body);

user.save((err, result) => {
  if (err) {
    console.error(err.errors['name']['message']);
    return res.status(400)
      .send({
        message: err
       });
  } else {
    res.send(user);
  }
})
```

## statics和methods
都是schema的属性，statics和methods的区别:

区别就是一个给Model添加方法（statics），
一个给实例添加方法（methods）。
statics:
```
UserSchema.statics.find_by_username = function(username, cb) {
  return this.findOne({
    username: username
  }, cb);
};

UserModal.find_by_username('hmz', (err, user) => {
  console.log('find_by_username', user)
})
```
methods:
```
UserSchema.methods.is_exist = function(cb) {
  const query = {
    username: this.username,
    password: this.password
  };
  return this.model('UserModel').findOne(query, cb);
};

const user = new User({
  username: 'hmz',
  password: 'sadsaa';
});
user.is_exist(cb)
```

## 联表查询
如果你使用过MySql，肯定用过join，用来联表查询，但Mongoose中并没有join，不过它提供了一种更方便快捷的方法：`Population`。
用简短的话来概括Population的使用：在一个Collection(articles)中定义一个指向另一个Collection(comment)的_id字段的字段(comment)
```
//FilmComment
const filmCommentSchema = new Schema({
    ...
    article_id: {type: mongoose.Schema.Types.ObjectId, ref: 'FilmArticle', required: true},
    ...
    reply_to_id: {type: mongoose.Schema.Types.ObjectId, ref: 'FilmComment'}   //ref的值是模型(model)名称 //被回复的评论id
    ...
})
const FilmComment = mongoose.model('FilmComment', filmCommentSchema);
module.exports = FilmComment

//FilmArticle
const filmArticleSchema = new Schema({
    ...
    comment: [{type: mongoose.Schema.Types.ObjectId, ref: 'FilmComment'}],
    ...
})
let FilmArticle = mongoose.model('FilmArticle', filmArticleSchema);
module.exports = FilmArticle

const newFilmComment = req.body.reply_to_id ? new FilmCommentModel({
    article_id: req.body.article_id,
    content: content,
    create_time: Date.now(),
    reply_to_id: req.body.reply_to_id
}) : new FilmCommentModel({
    article_id: req.body.article_id,
    content: content,
    create_time: Date.now(),
})

await FilmCommentModel.create(newFilmComment)
const newFilmArticle = await FilmArticleModel.findByIdAndUpdate(req.body.article_id, {
    $push': {
        comment: newFilmComment._id
    },
}, {new: true}).populate({
    path: 'comment',
    model: 'FilmComment',
    populate: {
        path: 'reply_to_id',
        model: 'FilmComment',
        populate: {
            path: 'user_id',
            select: {
                password: 0, token: 0
            },
            model: 'User'
        }
    }
})
```

## 前后钩子
前：pre() 后：post(),在执行某些操作时可以执行的函数，在Schema上指定。
在以下操作中触发执行 中间件
schame的钩子有：
```
//--------pre 前置
schema.pre('init', function (next) {
    console.log('init');
    next();
});
schema.pre('validate', function (next) {
    console.log('validate');
    next();
});
schema.pre('save', function (next) {
    console.log('save');
    next();
});

schema.pre('find', function(next) {
    console.log('find');
    next();
});
//--------post 后置
schema.post('init', function (doc,next) {
    console.log('post-init');
    next();
});
schema.post('validate', function (doc,next) {
    console.log('post-validate');
    next();
});
schema.post('save', function (doc,next) {
    console.log('post-save');
    next();
});
schema.post('find', function (doc,next) {
    console.log('post-find');
    next();
});
```

## 插件
我们是可以通过插件形式来拓展Schema的功能。

比如文章，当我们修改文章时，一般都会添加一个最后编辑时间，虽然我们可以每次修改时都手动更新，但是我们可以通过插件来自动更新。
```
// plugins/plugins.js  
module.exports = {   
  lastModified(schema, options) {
    schema.add({ lastMod: Date })
    schema.pre('save', function (next) {    //前置钩子
      this.lastMod = new Date;   
      next()   
    }) 
    if (options && options.index) {
      schema.path('lastMod').index(options.index)
    }  
  }  
}


// articles/filmArticles.js

const plugins = require('../plugins/plugins');  

filmArticlesSchema.plugin(plugins.lastModified, {index: true}); 
```

## 聚合函数
MongoDB中聚合(aggregate)主要用于处理数据(诸如统计平均值,求和等)，并返回计算后的数据结果。有点类似sql语句中的 count(*)。
