## sequelize

------

[Sequelize 中文文档 | Sequelize 中文网](https://www.sequelize.com.cn/)

1. 安装

 ```js
  $ npm install --save sequelize
  $ npm install --save mysql
 ```

2. 连接MySQL

```JS
// 连接数据库
const { Sequelize, DataTypes } = require('sequelize');
// 必须要有数据库先,参数1是数据库名，参数2为用户名，参数3为密码
const sequelize = new Sequelize('the_library', 'root', '123456', {
  host: 'localhost',
  dialect: 'mysql',/* 选择 'mysql' | 'mariadb' | 'postgres' | 'mssql' 其一 */
  logging: console.log,
    //连接池
    pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000,
  }
});
//由于连接是异步的,所以返回一个promise，测试连接是否成功
sequelize.authenticate().then(()=>{
	console.log('连接成功');
}).catch(()=>{})

module.exports={
	sequelize
}
```

3.定义模型

```js
/*2. 数据库管理员表admin
名			类型	长度	小数点	NULL	用途	键
admin_id	bigint	20			0	否	账号	✔
password	varchar	15			0	否	密码	
username	varchar	15			0	是	用户名	
rold_id		int		2			0	否	账户类型
*/
const { Sequelize, DataTypes } = require('sequelize');
adminModel={
	modekname:"admin",
	attribute:{
		admin_id:{
			type:DataTypes.INTEGER,
			primaryKey: true,
			allowNull: false,
			autoIncrement: true,
			comment: '账号'
		},
		password:{
			type:DataTypes.STRING,
			allowNull: false,
			comment: '密码'
		},
		username:{
			type:DataTypes.STRING,
			allowNull: false,
			comment: '用户名'
		}
		// ,
		// rold_id:{
		// 	type:DataTypes.ENUM,
		// 	// allowNull: false,
		// 	values: ["0", "1", "2","3","4"],
		// 	comment: '账户类型'
		// }
	},
	option:{
		// 把时间戳禁用了
		timestamps: false,
		// indexes: [
		//    // 在 email 上创建唯一索引
		//    {
		//      unique: true,
		//      fields: ['index']
		//    }]
	}
}

//定义模型
// 1、定义好模型了
const Admin=sequelize.define(表名，规则，约束)
```

4.同步模型

```js
// 然后同步呀{ force: true } sync({ force: true })
//当{force:true}是，无论没有有创建表都删除重来
	sequelize.sync().then(()=>{
		console.log("同步用户模型");
	})
```

5.查找数据

```js
(async () => {
  // 查找所有
  const allUser = await Admin.findAll()

  // 按id查找
  const oneUser = await Admin.findById(id)

  // 按条件查询
  const someUser = await Admin.findAll({
    where: {
      // 模糊查询
      name: {
        $like: '%小%',
      },

      // 精确查询
      password: 'root',
    }
  })

  // 分页查询
  const size = 10 // 每页10条数据
  const page = 1 // 页数
  const pageUser = await Admin.findAndCountAll({
    where: {
      name: {
        $like: '%小%',
      },
    },
    limit: size,
    offset: size * (page - 1),
  })
})();
```

6.改

```js
(async () => {
// 方法一
await Admin.upert(data)  // data 里面如果带有 id 则更新，不带则新建

// 方法二
const admin = await Admin.findById(id)
admin.update(data)
})()
```

7.删

```js
(async () => {
// 方法一
// 删除所有名字带’小‘的用户
await Admin.destroy({
  where: {
    username: '小',
  },
})

// 方法二
const admin = await Admin.findById(id)
admin.destroy()
})()
```

8.连表查询

​	8.1三表查询

这是我毕业设计的三表连表查询，主要思路是三张表的模型，BookInfo，ReaderCard，LendList，可LendList模型(借书表)通过reader_id与ReaderCard（读者）相关联，又通过book_id与BookInfo(图书信息表)关联，从而达到一个三表关联查询。

```js
 BookInfo.belongsToMany(ReaderCard,{through:LendList,foreignKey:'book_id'})
 ReaderCard.belongsToMany(BookInfo,{through:LendList,foreignKey:'reader_id'})
   const result=await ReaderCard.findAll({
	    include:[{
			model:BookInfo
		}
		],
	   where:{
		   reader_id:id
	   },
	   raw:true 
   })
```

​	 8.2 两表查询

```js
SysUser.hasOne(MonitorSetting,{foreignKey: 'uId',sourceKey: 'id'});
MonitorSetting.belongsTo(SysUser, {foreignKey: 'uId',targetKey: 'id'});
```



[Sequelize实现一对一，一对多，多对多关联_遛狗的代码的博客-CSDN博客](https://blog.csdn.net/Brave_Coder/article/details/79734451?utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-4.baidujs&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-4.baidujs)

