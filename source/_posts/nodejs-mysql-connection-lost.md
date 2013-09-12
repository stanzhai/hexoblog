title: MySQL error: The server closed the connection (node.js)
date: 2013-09-11 10:51:24
categories: 问题修复
tags: nodejs
---

## 问题描述

最近使用nodejs连接mysql数据库，用到了mysql模块，不过使用一段时间后，会出现以下异常：

```
Error: Connection lost: The server closed the connection.
```

看错误信息是服务端主动关闭连接导致的。

<!--more-->

## 解决方案

设置定时器，每隔一段时间自动检查连接，如果链接断掉则进行重连。

主要代码如下：

```
var mysql = require('mysql');
var Common = require('./common');
var conf = Common.conf;
var logger = Common.logger;

var connectionState = false;
var connection = mysql.createConnection({
  host: conf.db.hostname,
  user: conf.db.user,
  password: conf.db.pass,
  database: conf.db.schema,
  insecureAuth: true
});
connection.on('close', function (err) {
  logger.error('mysqldb conn close');
  connectionState = false;
});
connection.on('error', function (err) {
  logger.error('mysqldb error: ' + err);
  connectionState = false;
});

function attemptConnection(connection) {
  if(!connectionState){
    connection = mysql.createConnection(connection.config);
    connection.connect(function (err) {
      // connected! (unless `err` is set)
      if (err) {
        logger.error('mysql db unable to connect: ' + err);
        connectionState = false;
      } else {
        logger.info('mysql connect!');

        connectionState = true;
      }
    });
    connection.on('close', function (err) {
      logger.error('mysqldb conn close');
      connectionState = false;
    });
    connection.on('error', function (err) {
      logger.error('mysqldb error: ' + err);

      if (!err.fatal) {
        //throw err;
      }
      if (err.code !== 'PROTOCOL_CONNECTION_LOST') {
        //throw err;
      } else {
        connectionState = false;
      }

    });
  }
}
attemptConnection(connection);

var dbConnChecker = setInterval(function(){
  if(!connectionState){
    logger.info('not connected, attempting reconnect');
    attemptConnection(connection);
  }
}, conf.db.checkInterval);

// Mysql query wrapper. Gives us timeout and db conn refreshal! 
var queryTimeout = conf.db.queryTimeout;
var query = function(sql,params,callback){
  if(connectionState) {
    // 1. Set timeout
    var timedOut = false;
    var timeout = setTimeout(function () {
      timedOut = true;
      callback('MySQL timeout', null);
    }, queryTimeout);

    // 2. Make query
    connection.query(sql, params, function (err, rows) {
      clearTimeout(timeout);
      if(!timedOut) callback(err,rows);
    });
  } else {
    // 3. Fail if no mysql conn (obviously)
    callback('MySQL not connected', null);
  }
}

// And we present the same interface as the node-mysql library!
// NOTE: The escape may be a trickier for other libraries to emulate because it looks synchronous
exports.query = query;
exports.escape = connection.escape;
```

使用方法：

```
var conn = require('./database');
var sql = 'SELECT foo FROM bar;';
conn.query(sql, [userId, plugId], function (err, rows) {
   // logic
};
```

## 参考资料

<http://stackoverflow.com/questions/13018227/reproduce-mysql-error-the-server-closed-the-connection-node-js/13020899>