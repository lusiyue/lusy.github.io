# gitbook

1.安装（注意，node是10.23.0版本的，不然出错）

```js
$ npm install -g gitbook-cli
```

2.初始化

```js
$ gitbook init
```

​	出现两个文件

- README.md（书籍的介绍在这个文件里）

- SUMMARY.md（书籍的目录结构在这里配置）

3.配置文件，book.json,没有就创建

```js
{
  "title": "我的笔记",
  "author": "lusy",
  "description": "日常工作中用到的一些技术总结",
  "language": "zh-hans", 
  "styles": {
    "website": "custom_style.css"//自己的样式
},
  "plugins": [//插件
    "github",
    "expandable-chapters-small",
    "splitter",
    "-lunr",
    "-search",
    "search-pro",
    "code",
    "back-to-top-button",
    "toggle-chapters"
  ],
  "pluginsConfig": {
    "github": {
      "url": "https://gitee.com/siyue_lu"
    }
  }
}
```

4.安装插件

```js
$ gitbook install
```

5.运行生成html

```
$ gitbook serve
```

6.遇到的bug

​	寻找.gitbook\versions\3.2.3\lib\output\website\copyPluginAssets.js

```js
//两处 confirm: true改成
confirm: false
```

