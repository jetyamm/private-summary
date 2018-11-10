#config中的index.js文件配置代理
1、配置config文件
```js
  proxyTable: {
      '/api': {
        target: 'https://m.guokr.com/science_api/channels', // 你请求的第三方接口
        changeOrigin: true, // 在本地会创建一个虚拟服务端，然后发送请求的数据，并同时接收请求的数据，这样服务端和服务端进行数据的交互就不会有跨域问题
        pathRewrite: { // 路径重写，
          '^/api': '' // 替换target中的请求地址，也就是说以后你在请求http://api.jisuapi.com/XXXXX这个地址的时候直接写成/api即可。
        }
      }
  }
```
2、直接通过axios请求（http与https均可以请求）


------------------------

#使用express脚手架实现服务器代理
```js
npm install express-generator -g // npm install 是安装任何第三方包的命令，  express-generator -g 指在全局范围下安装名为 express-generator 的第三方包。
express myapp     //指在当前命令行目录下创建一个名为myapp的文件夹用来做应用目录
cd myapp           //到达我们的工作目录
npm install 
npm start  // 启动服务器
```

服务器代理是为了处理跨域操作，前端访问后端时，请求的url只要协议、域名、端口号有一个不一致，则会跨域，这时候就需要通过三种方法来实现跨域
* jsonp：后端直接返回一段jsonp的字符串给前端（通过script标签src的无视跨域特性来实现请求）
* crors：后端直接给前端开启访问权限
* 服务器代理：通过服务器访问服务器无需跨域的这个特点，前端可以通过构建一个本地服务器，来进行对后端进行访问请求，再通过本地服务器返回数据到前端。

------------------
# mongodb封装函数
```js
const mongodb = require('mongodb');

let MongodbClient = mongodb.MongoClient;

const DB_CONN_STR = 'mongodb://localhost:27017';

const DB_NAME = 'xiaowei';

/**
 * 操作数据库--插入
 * 
 * @param {String} collectionName  集合名
 * @param {Object,Array} data 插入的一条或多条数据
 * @param {Function} callback 回调函数, 接收一个结果参数
 */
function insert(obj) {
    let { collectionName, data, callback } = obj;
    MongodbClient.connect(DB_CONN_STR, (err, client) => {
        if (err) {
            console.log('连接数据库失败');
            return;
        }
        let db = client.db(DB_NAME);
        if (data && data.length) {
            db.collection(collectionName).insertMany(data, (err, result) => {
                callback(err, result);
                client.close();
            });
        } else {
            db.collection(collectionName).insertOne(data, (err, result) => {
                callback(err, result);
                client.close();
            });
        }

    })
}

/**
 * 操作数据库--查询
 * 传入对象, 对象属性如下
 * @param {String} collectionName  集合名
 * @param {Object} condition 查询条件
 * @param {Function} callback 回调函数, 接收一个结果参数
 * @param {[Number]} limit 查询数据数量
 * @param {[Number]} skip 开始查询的索引值
 * @param {[Object]} sort 排序条件
 */
function find(obj) {
    let { collectionName, condition, callback, limit, skip, sort } = obj
    MongodbClient.connect(DB_CONN_STR, (err, client) => {
        if (err) {
            console.log('连接数据库失败');
            return;
        }
        let db = client.db(DB_NAME);
        let temp = db.collection(collectionName).find(condition);
        if (limit !== undefined) {
            temp = temp.limit(limit);
            if (skip !== undefined) {
                temp = temp.skip(skip);
            }
        }
        if (sort !== undefined) {
            temp = temp.sort(sort);
        }
        temp.toArray((err, result) => {
            callback(err, result);
            client.close();
        });

    })
}

/**
 * 操作数据库--更新
 * 
 * @param {String} collectionName  集合名
 * @param {Object} condition  条件
 * @param {Object} data 插入的一条或多条数据
 * @param {Function} callback 回调函数, 接收一个结果参数
 */
function updata(obj) {
    let { collectionName, condition, data, callback } = obj;
    MongodbClient.connect(DB_CONN_STR, (err, client) => {
        if (err) {
            console.log('连接数据库失败');
            return;
        }
        let db = client.db(DB_NAME);
        db.collection(collectionName).update(condition, data, (err, result) => {
            callback(err, result);
            client.close();
        });
    })
}


/**
 * 操作数据库--删除
 * 
 * @param {String} collectionName  集合名
 * @param {Object} condition  条件
 * @param {Function} callback 回调函数, 接收一个结果参数
 */
function remove(obj) {
    let { collectionName, condition, callback } = obj;
    MongodbClient.connect(DB_CONN_STR, (err, client) => {
        if (err) {
            console.log('连接数据库失败');
            return;
        }
        let db = client.db(DB_NAME);
        db.collection(collectionName).remove(condition, (err, result) => {
            callback(err, result);
            client.close();
        });
    })
}

module.exports = { insert, find, updata, remove };
```
-------------------------

#sql封装函数
```js
var mysql = require('mysql');
var pool  = mysql.createPool({
  connectionLimit : 10,
  host            : 'localhost',
  user            : 'root',
  password        : '',
  database        : 'oneShop'
});
/**
 * 
 * @param {String} sql	数据库执行语句
 * @param {Object} options	参数
 * @param {Function} callback	回调函数
 */
var query = function (sql,options,callback) {
    pool.getConnection(function (err, connection) {
        if (err) throw err; // not connected!
        // Use the connection
        connection.query(sql, options,function (error, results, fields) {
            // When done with the connection, release it.
            connection.release();
            // Handle error after the release.
            if (error) throw error;
            // Don't use the connection here, it has been returned to the pool.
            //console.log(results);
            callback(results)
        });
    });
};
module.exports = {
    query: query
}
```