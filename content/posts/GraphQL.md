+++
title = "GraphQL"
date = 2019-03-14T16:52:19+08:00
tags = ["tools"]
categories = ["tools"]
draft = false
description = 'GraphQL'
+++

# GraphQL 入门

GraphQL是一个用于API的查询语言，使用你自己定义的类型系统在服务器端进行查询。

1. 一旦GraphQL运行，它可以用来发送GraphQL查询去验证和执行。

2. GraphQL的实现：

   - 使用Nodejs实现GraphQL服务器

     ```
     mkdir test && cd ./test
     npm install express --save
     npm install babel --save
     touch ./server.js
     touch ./index.js
     ```

     ```
     index.js:
     require('babel/register');
     require('./server.js');
     ```

     ```
     //server.js
     import express from 'express';

     let app = express();
     let PORT = 3000;

     app.post('/graphql', (req, res) => {
       res.send('Hello!');
     });

     let server = app.listen(PORT, function() {
       let host = server.address().address;
       let port = server.address().port;

       console.log('GraphQL listening at http://%s:%s', host, port);
       ```

   - 编写GrapQL Schema

     添加Schema（Schema 是GraphQL请求的入口，用户的GraphQL请求会对应到具体的Schema ）

     GraphQL请求：

     ```
     query getHightScore { score }
     query getHightScore(limit: 10) { score }
     ```

     加有查询条件的GraphQL请求就是Graphql的schema，通过schema可以定义服务器的显影内容。

     - 使用graphql

       ```
       npm install graphql --save
       ```

       作用：包装服务器schema并处理GraphQL请求。

       处理请求内容， body-parser

       ```
       npm install body-parser --save
       ```

       ​

- 创建schema

  ```
  //schema.js
  import {
    GraphQLObjectType,
    GraphQLSchema,
    GraphQLInt
  } from 'graphql';

  let count = 0;

  let schema = new GraphQLSchema({
    query: new GraphQLObjectType({
      name: 'RootQueryType',
      fields: {
        count: {
          type: GraphQLInt,
          resolve: function() {
            return count;
          }
        }
      }
    })
  });

  export default schema;

  ```

  ​

- 连接schema（将GraphQL schema 和服务器连接起来）

  ```
  //server.js
  import express from 'express';
  import schema from './schema';

  import { graphql } from 'graphql';
  import bodyParser from 'body-parser';

  let app = express();
  let PORT = 3000;

  //Parse post content as text
  app.use(bodyParser.text({ type: 'application/graphql' }));

  app.post('/graphql', (req, res) => {
    //GraphQL executor
    graphql(schema, req.body)
    .then((result) => {
      res.send(JSON.stringify(result, null, 2));
    })
  });

  let server = app.listen(PORT, function() {
    let host = server.address().address;
    let port = server.address().port;

    console.log('GraphQL listening at http://%s:%s', host, port);
  });
  ```

  ​

## 变异（mutation， 修改数据）

GraphQL中将对数据的修改操作称为mutation。

定义mutation：

```
let schema = new GraphQLSchema({
  query: ...
  mutation: //TODO
});
```

启动本地服务后，在浏览器界面输入查询语句即可。

![image-20180404112840070](https://ws1.sinaimg.cn/large/006tKfTcgy1fq0kkf84m5j31ao0rfq7f.jpg)
