## express搭后台

------

下面是我毕设的app.js的代码

```js
const express = require("express");
const path = require("path");
const app = express();
// 数据库连接和模型
require("./db/connect.js");
require("./db/utilModel.js");

// 设置解析body格式
app.use(express.urlencoded({ extended: true }));

// 解析 application/json
app.use(express.json());

// 启用gzip压缩 加快传输速率 要在静态资源之前才行
const compression = require('compression')
app.use(compression())
// 设置静态资源
// 到达dist下找index.html，这里的dist是在vue那边bulit
app.use(express.static(path.join(__dirname, './dist')))

// 解决跨域  https://segmentfault.com/a/1190000022512695
var cors = require("cors");
app.use(cors());

// && req.url!=='/register'
app.post("/register", require("./route/admin/register"));

// 登录拦截
app.use("/api", require("./middleware/loginGuard"));

// 引入路由模块
const api = require("./route/api");
// 为路由匹配请求路径
app.use("/api", api);

app.listen(8090, () => {
  console.log("localhost:8090");
});

```

