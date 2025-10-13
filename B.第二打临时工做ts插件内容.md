# 0.结论

在不定制化处理的前提下，streamset可以对账户同步实现平替，

https://juejin.cn/post/7290725817825984524

# 1.指令

## 1.1 账户对接:

你是谷歌cto，写出相应的interface和service的axios代码获取api的代码，要求符合谷歌标准。

你是谷歌cto，获取相应的interface，要求使用map过滤，要求使用sql插入相应的数据库表中。



## 1.2 登录:

你是谷歌cto，写出相应的domain层代码，要求符合谷歌标准，要求使用typescript。

你是谷歌cto，使用相应非统一门户标签进行静态页面跳转的代码，要求符合谷歌标准。

你是谷歌cto，写出相应静态页面登录代码，要求通过用户名和密码获取uid，并返回拼接地址给前端，前端使用该地址跳转，将code返回前端，要求符合谷歌标准。

你是谷歌cto，写出相应静态页面登出代码，删除cookie，并返回拼接地址给前端，前端使用该地址跳转首页，要求符合谷歌标准。



## 1.3 git仓库:







启动node.js项目

安装指定版本node.js

引用:

https://www.cnblogs.com/18sui/p/17162016.html



//sso

```
rm -rf dist
npm run build
ls dist
npm run dev

```

//sync

```
./build.sh

npm cache clean --force
npm install

npm run build:sync
npm run start:sync

cd /ecis/common
npm run compile

cd ../../ecis/mysql
npm run compile

cd ../../ecis/extensions
npm run compile

cd ../../ecis/grpc
npm run compile

cd ../../ecis/grpc
npm run compile


cd ../../ecis/middleware
npm run compile

cd ../../ecis/server
npm run compile

cd ../../ecis/services
npm run compile

cd ../../ecis/wps
npm run compile
```

//mysql

```
root    Root12345678@1613

mysql -uroot -pRoot12345678@1613

ALTER USER 'root'@'%' IDENTIFIED BY 'Root12345678@1613';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;


CREATE USER 'root'@'%' IDENTIFIED BY 'Root12345678@1613';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;

ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'Root12345678@1613';
FLUSH PRIVILEGES;



SELECT user, host, plugin FROM mysql.user WHERE user = 'root';


SET GLOBAL validate_password.policy = LOW;
SET GLOBAL validate_password.length = 4;
ALTER USER 'root'@'%' IDENTIFIED BY 'root123';
FLUSH PRIVILEGES;
SELECT user, host, authentication_string FROM mysql.user WHERE user = 'root';


```

# 2. node.js的具体调用流程

```
整体调用流程

API 层：
接收 HTTP 请求，分发到 Controller。

Controller 层：
调用应用层的服务编排逻辑。

应用层：
调用服务层处理具体业务逻辑。

服务层：
调用仓储层获取数据或写入数据库。

仓储层：
执行具体的数据库操作。
```

## 2.1仓储层实现

### 2.1.1 在node.js中实现类似mybatisPlus的功能的代码

###### 使用Sequelize作为框架，

指令

```
你是谷歌cto，以mysql作示例，从最底层的utils向最顶层controller推荐，要求符合谷歌标准，要求使用Sequelize 
```



你是谷歌cto，集成常见的Sequelize对应版本的数据库，要求符合谷歌标准

```
{
  "name": "sync", // 项目名称
  "version": "0.1.0", // 项目版本
  "description": "Account sync for demo", // 项目描述
  "main": "dist/src/index.js", // 项目入口文件
  "license": "MIT", // 许可证类型
  "scripts": {
    "clear": "rm -rf dist", // 清理 dist 目录
    "compile": "npm i && npx tsc", // 安装依赖并使用 TypeScript 编译项目
    "build": "npm run compile", // 快捷脚本，编译项目
    "debug": "ts-node src/index.ts env", // 使用 ts-node 调试项目
    "start": "node ./dist/index.js", // 启动项目
    "nodemon": "nodemon -e ts,json -L src/index.ts", // 使用 nodemon 自动重启开发服务
    "test": "jest --detectOpenHandles", // 使用 Jest 测试代码
    "env": "node ../../scripts/env.js", // 环境变量管理脚本
    "dev": "npx tsc && node ./dist/index.js" // 开发模式下编译并启动项目
  },
  "dependencies": {
    "@grpc/grpc-js": "^1.6.7", // 用于 gRPC 通信
    "@koa/router": "^10.1.1", // Koa 的路由中间件
    "amqplib": "^0.10.4", // RabbitMQ 客户端库
    "axios": "^0.26.0", // HTTP 请求库
    "crypto": "^1.0.1", // 加密功能库
    "crypto-js": "^4.2.0", // 支持加密算法（如 AES、MD5 等）的 JavaScript 库
    "dayjs": "^1.11.1", // 日期处理库
    "formidable": "^3.5.1", // 用于解析上传文件的库
    "google-protobuf": "^3.20.1", // 用于处理 Protocol Buffers 数据的库
    "jsrsasign": "^11.1.0", // 用于签名和加密的 JavaScript 库
    "koa": "^2.13.4", // Node.js 的轻量级 Web 框架
    "koa-body": "^4.2.0", // Koa 的请求体解析中间件
    "koa-logger": "^3.2.1", // Koa 的日志中间件
    "koa-multer": "^1.0.2", // Koa 的文件上传中间件
    "moment": "^2.30.1", // 日期处理库（推荐用 dayjs 替代）
    "n-readlines": "^1.0.1", // 按行读取文件的库
    "node-schedule": "^2.1.0", // 定时任务库
    "querystring": "^0.2.1", // 用于处理 URL 查询字符串
    "sequelize": "^6.35.1", // ORM 库，用于对象关系映射
    "mysql2": "^3.5.2", // MySQL 驱动（与 Sequelize 搭配使用）
    "pg": "^8.11.0", // PostgreSQL 驱动（与 Sequelize 搭配使用）
    "pg-hstore": "^2.3.4", // 用于 PostgreSQL 的 JSON 格式支持
    "sqlite3": "^5.1.6", // SQLite 驱动（与 Sequelize 搭配使用）
    "tedious": "^15.1.0", // Microsoft SQL Server 驱动（与 Sequelize 搭配使用）
    "uuid": "^8.3.2", // 生成唯一标识符
    "winston": "^3.7.2", // 日志记录库
    "xlsx": "^0.18.5" // 处理 Excel 文件的库
  },
  "devDependencies": {
    "@types/amqplib": "^0.10.5", // RabbitMQ 的类型定义
    "@types/axios": "^0.14.0", // Axios 的类型定义
    "@types/crypto-js": "^4.2.2", // Crypto-JS 的类型定义
    "@types/jsrsasign": "^10.5.14", // jsrsasign 的类型定义
    "@types/koa": "^2.13.4", // Koa 的类型定义
    "@types/koa-logger": "^3.1.2", // Koa Logger 的类型定义
    "@types/mysql2": "^2.3.3", // MySQL2 的类型定义
    "@types/node-schedule": "^1.3.2", // Node-Schedule 的类型定义
    "@types/pg": "^8.6.6", // PostgreSQL 的类型定义
    "@types/uuid": "^8.3.4", // UUID 的类型定义
    "grpc_tools_node_protoc_ts": "^5.3.2", // gRPC 工具，用于生成 TypeScript 的类型
    "ts-node": "^10.9.2" // TypeScript 运行时
  }
}

```

你是谷歌cto，新建utils/db.ts，要求使用Sequelize，要求符合谷歌标准

```
import { Sequelize } from 'sequelize';

// 使用 Sequelize 初始化数据库连接
export const sequelize = new Sequelize('testdb', 'root', 'password', {
    host: 'localhost',
    dialect: 'mysql', // 数据库类型
    logging: false,   // 禁用 SQL 日志
    pool: {
        max: 10,      // 最大连接数
        min: 0,       // 最小连接数
        idle: 10000,  // 空闲连接最大存活时间（ms）
    },
});

// 测试数据库连接
export async function testConnection() {
    try {
        await sequelize.authenticate();
        console.log('数据库连接成功！');
    } catch (error) {
        console.error('无法连接到数据库:', error.message);
    }
}

```





你是谷歌cto，新建models/user.ts，要求符合谷歌标准

```
CREATE TABLE `users` (
    `id` INT UNSIGNED AUTO_INCREMENT PRIMARY KEY COMMENT '用户唯一标识',
    `username` VARCHAR(50) NOT NULL COMMENT '用户名',
    `email` VARCHAR(100) NOT NULL UNIQUE COMMENT '邮箱地址，唯一',
    `createdAt` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `updatedAt` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci COMMENT='用户表';

```

```
import { DataTypes, Model } from 'sequelize';
import { sequelize } from '../utils/db';

// 定义用户模型
export class User extends Model {
    id!: number;
    username!: string;
    email!: string;
    createdAt!: Date;
    updatedAt!: Date;
}

// 初始化 User 模型（映射数据库表）
User.init(
    {
        id: {
            type: DataTypes.INTEGER.UNSIGNED,
            autoIncrement: true,
            primaryKey: true,
        },
        username: {
            type: DataTypes.STRING(50),
            allowNull: false,
        },
        email: {
            type: DataTypes.STRING(100),
            allowNull: false,
            unique: true,
        },
    },
    {
        sequelize,
        tableName: 'users',
        timestamps: true,
    }
);

```

你是谷歌cto，新建repositories/userRepository.ts，要求符合谷歌标准

```
import { User } from '../models/user';

export class UserRepository {
    /**
     * 创建用户
     */
    static async createUser(data: { username: string; email: string }) {
        return await User.create(data);
    }

    /**
     * 获取所有用户
     */
    static async getAllUsers() {
        return await User.findAll();
    }

    /**
     * 根据 ID 获取用户
     */
    static async getUserById(userId: number) {
        return await User.findByPk(userId);
    }

    /**
     * 更新用户信息
     */
    static async updateUser(userId: number, data: { username?: string; email?: string }) {
        const user = await User.findByPk(userId);
        if (!user) throw new Error('用户不存在');
        return await user.update(data);
    }

    /**
     * 删除用户
     */
    static async deleteUser(userId: number) {
        const user = await User.findByPk(userId);
        if (!user) throw new Error('用户不存在');
        await user.destroy();
    }
}

```



你是谷歌cto，新建services/userService.ts ，要求符合谷歌标准

```
import { UserRepository } from '../repositories/userRepository';

export class UserService {
    /**
     * 获取所有用户
     */
    static async getUsers() {
        return await UserRepository.getAllUsers();
    }

    /**
     * 根据 ID 获取用户
     */
    static async getUserById(userId: number) {
        const user = await UserRepository.getUserById(userId);
        if (!user) throw new Error('用户不存在');
        return user;
    }

    /**
     * 创建新用户
     */
    static async createUser(data: { username: string; email: string }) {
        if (!data.username || !data.email) throw new Error('用户名和邮箱不能为空');
        return await UserRepository.createUser(data);
    }

    /**
     * 更新用户信息
     */
    static async updateUser(userId: number, data: { username?: string; email?: string }) {
        return await UserRepository.updateUser(userId, data);
    }

    /**
     * 删除用户
     */
    static async deleteUser(userId: number) {
        return await UserRepository.deleteUser(userId);
    }
}

```



你是谷歌cto，新建controllers/userController.ts，要求符合谷歌标准

```
import { Context } from 'koa';
import { UserService } from '../services/userService';

export class UserController {
    /**
     * 获取所有用户
     */
    static async getUsers(ctx: Context) {
        try {
            const users = await UserService.getUsers();
            ctx.body = { success: true, data: users };
        } catch (error) {
            ctx.status = 500;
            ctx.body = { success: false, error: error.message };
        }
    }

    /**
     * 根据 ID 获取用户
     */
    static async getUserById(ctx: Context) {
        try {
            const userId = Number(ctx.params.id);
            const user = await UserService.getUserById(userId);
            ctx.body = { success: true, data: user };
        } catch (error) {
            ctx.status = 404;
            ctx.body = { success: false, error: error.message };
        }
    }

    /**
     * 创建用户
     */
    static async createUser(ctx: Context) {
        try {
            const { username, email } = ctx.request.body;
            const newUser = await UserService.createUser({ username, email });
            ctx.status = 201;
            ctx.body = { success: true, data: newUser };
        } catch (error) {
            ctx.status = 400;
            ctx.body = { success: false, error: error.message };
        }
    }

    /**
     * 更新用户
     */
    static async updateUser(ctx: Context) {
        try {
            const userId = Number(ctx.params.id);
            const { username, email } = ctx.request.body;
            const updatedUser = await UserService.updateUser(userId, { username, email });
            ctx.body = { success: true, data: updatedUser };
        } catch (error) {
            ctx.status = 400;
            ctx.body = { success: false, error: error.message };
        }
    }

    /**
     * 删除用户
     */
    static async deleteUser(ctx: Context) {
        try {
            const userId = Number(ctx.params.id);
            await UserService.deleteUser(userId);
            ctx.status = 204;
        } catch (error) {
            ctx.status = 404;
            ctx.body = { success: false, error: error.message };
        }
    }
}

```

你是谷歌cto，新建routes/userRoutes.ts，要求符合谷歌标准

```
import Router from '@koa/router';
import { UserController } from '../controllers/userController';

const router = new Router({ prefix: '/api/users' });

// 路由与控制器方法绑定
router.get('/', UserController.getUsers);
router.get('/:id', UserController.getUserById);
router.post('/', UserController.createUser);
router.put('/:id', UserController.updateUser);
router.delete('/:id', UserController.deleteUser);

export default router;

```



你是谷歌cto，新建index.ts，要求符合谷歌标准

```
import Koa from 'koa';
import bodyParser from 'koa-body';
import userRoutes from './routes/userRoutes';
import productRoutes from './routes/productRoutes';
import { testConnection } from './utils/db';

const app = new Koa();

// 测试数据库连接
testConnection();

// 使用中间件
app.use(bodyParser());

// 加载用户路由
app.use(userRoutes.routes());
app.use(userRoutes.allowedMethods());

// 加载产品路由
app.use(productRoutes.routes());
app.use(productRoutes.allowedMethods());

// 启动服务
const PORT = 3000;
app.listen(PORT, () => {
    console.log(`Server is running at http://localhost:${PORT}`);
});

```

# 3. 接手node.js项目

启动

```
npm install
npm run build
npm run dev
npm start


curl -X POST https://localhost:8000/api/v1/gatherOrganizationSync \
     -H "Content-Type: application/xml" \
     -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
     -d '<?xml version="1.0" encoding="UTF-8"?>
         <SendAllOrganizationRequest>
           <Organization>
             <id>00410088002900000001</id>
             <parentid>00410000000000000000</parentid>
             <name>太原大南门营业部</name>
             <startdate>2010-01-01</startdate>
             <enddate>2010-05-06</enddate>
             <updatedate>2010-03-08</updatedate>
             <orgstatus>0</orgstatus>
             <sort>00410000000000000000/1</sort>
             <orgabbreviat>山西公司/运城分公司/河津县分公司</orgabbreviat>
             <misorganizationid>2741</misorganizationid>
             <type>上市省公司三级</type>
             <dept>省公司部门</dept>
           </Organization>
           <Organization>
             <id>00410088002900000000</id>
             <parentid>00410000000000000000</parentid>
             <name>大同工商银行营业厅</name>
             <startdate>2010-01-01</startdate>
             <enddate>2010-05-06</enddate>
             <updatedate>2010-03-08</updatedate>
             <orgstatus>0</orgstatus>
             <sort>00410000000000000000/1</sort>
             <orgabbreviat>山西公司/运城分公司/河津县分公司</orgabbreviat>
             <misorganizationid>2741</misorganizationid>
             <type>上市省公司三级</type>
             <dept>省公司部门</dept>
           </Organization>
           <!-- 可根据需要添加更多 Organization 元素 -->
         </SendAllOrganizationRequest>'

```



```
// 从 LDAP 数据访问层（DAO）模块导入函数，用于从 LDAP 服务器获取用户信息
import { getLdapUserFromServer } from '../dao/ldapUserDao';
// 导入应用配置文件，用于加载 LDAP 配置
import config from "../config/config";

/**
 * 获取 LDAP 用户信息
 * 
 * 功能：
 * 1. 从配置文件中加载 LDAP 相关的连接信息。
 * 2. 调用 DAO 层的函数，从 LDAP 服务器获取用户数据。
 * 3. 对返回的数据进行格式化，提取用户的关键信息。
 * 
 * @returns Promise<Array<{name: string, username: string, groups: string[]}>> 
 *          格式化后的用户信息列表。
 * @throws Error 当从 LDAP 获取数据时发生错误。
 */
export async function getLdapUserInfo() {
    // 加载 LDAP 服务器的 URL
    const url = config.ldap.url;
     // 获取请求中的 company_id 参数
    const companyId = ctx.params.company_id || ctx.query.company_id;
    if (!companyId) {
      console.error("Error: Missing company_id parameter.");
      ctx.status = 400;
      ctx.body = { success: false, message: "Missing required parameter: company_id" };
      return ctx;
    }
    console.log(`Received company_id: ${companyId}`);
    // 加载 LDAP 用户名和密码，用于绑定（认证）服务器
    const user = config.ldap.user;
    const password = config.ldap.password;

    // 定义 LDAP 查询的基础路径（Base DN）
    const baseDn = config.ldap.users.search;

    // 定义 LDAP 查询的过滤器，用于筛选符合条件的用户
    const filter = config.ldap.users.filter;

    // 定义需要从 LDAP 查询的用户属性
    const attributes = [
        'cn',                  // 用户的显示名称
        'sn',                  // 用户的姓氏
        'distinguishedName',   // 用户在 LDAP 树中的完整路径
        'objectGUID',          // 用户的唯一标识符
        'memberOf',            // 用户所属的组
        'name',                // 用户的名称
        'sAMAccountName'       // 用户的登录名
    ];

    try {
        // 调用 DAO 层函数，从 LDAP 服务器获取用户数据
        const users = await getLdapUserFromServer(
            url,              // LDAP 服务器地址
            user,             // LDAP 用户名
            password,         // LDAP 密码
            baseDn,           // 查询的基础路径
            filter,           // 查询过滤器
            attributes        // 需要查询的属性
        );

        // 对获取的用户数据进行格式化，仅保留关键信息
        return users.map((user: any) => ({
            name: user.cn,                  // 用户的显示名称
            username: user.sAMAccountName,  // 用户的登录名
            groups: user.memberOf,          // 用户所属的组
        }));
    } catch (err) {
        // 捕获并抛出错误，添加更多上下文信息
        throw new Error(`Error retrieving user info: ${err.message}`);
    }
}

```

![image-20241121160345758](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241121160345758.png)



```
整合"Path： /org/dev/v1/companies/{company_id}/members

Method： GET

接口描述：
[dev]获取企业成员列表"，通过传入company_id
```

```
Path： /org/dev/v1/companies/{company_id}/members

Method： GET

接口描述：
[dev]获取企业成员列表
```

```
export function getLdapUser(ctx: Context) {
  // 获取请求中的 company_id 参数
  const companyId = ctx.params.company_id || ctx.query.company_id;
  if (!companyId) {
    console.error("Error: Missing company_id parameter.");
    ctx.status = 400;
    ctx.body = { success: false, message: "Missing required parameter: company_id" };
    return ctx;
  }
  console.log(`Received company_id: ${companyId}`);
  console.log("test getLdapUser1111111")
  console.log("require ldapjs begin")
  console.log(`config.ldap.url=${config.ldap.url}`)
  //创建LDAP client，把服务器url传入
  var client = ldap.createClient({
    url: config.ldap.url
  });
  console.log(`filter=${config.ldap.users.filter}`)
  console.log(`scope=${'base'}`)
  console.log(`timeLimit=${500}`)
  //创建LDAP查询选项
  //filter的作用就是相当于SQL的条件
  var opts: ldap.SearchOptions = {
    filter: config.ldap.users.filter, //查询条件过滤器，查找uid=kxh的用户节点
    scope: "sub",        //查询范围
    timeLimit: 500000,       //查询超时
    attributes: ['cn', 'sn', 'distinguishedName', 'objectGUID', 'memberOf', 'name', 'sAMAccountName']
  };
  console.log(`user=${config.ldap.user}`)
  console.log(`password=${config.ldap.password}`)
  //将client绑定LDAP Server
  //第一个参数：是用户，必须是从根节点到用户节点的全路径
  //第二个参数：用户密码
  client.bind(config.ldap.user, config.ldap.password, function (err: any, res1: any) {
    //开始查询
    //第一个参数：查询基础路径，代表在查询用户信息将在这个路径下进行，这个路径是由根节开始
    //第二个参数：查询选项 
    console.log(`bind is ok`)
    console.log(`search=${config.ldap.users.search}`)

    UserSearch(client, config.ldap.users.search, opts, companyId)
      .then(function (res) {
        console.log("res", res)
      })
  });
  console.log(`bind is over`)
  return ctx
}

function UserSearch(server: any, LdapConf: any, SearchOtps: any, companyId: any) {
  return new Promise(function (resolve, reject) {
    server.search(LdapConf, SearchOtps, function (err: any, res: any) {
      res.on('searchEntry', async function (entry: any) {
        console.log(`dept entry=${JSON.stringify(entry)}`);
        console.log('entry.pojo: ' + JSON.stringify(entry.pojo));
        //'cn', 'sn', 'distinguishedName', 'objectGUID', 'memberOf', 'name', 'sAMAccountName'
        let attributesList = entry.pojo.attributes
        let cn = "";
        let sn = "";
        let distinguishedName = "";
        let name = "";
        let objectGUID = "";
        let memberOf = "";
        let sAMAccountName = "";
        for (let p of attributesList) {
          console.log("p", p)
          if (p.type == "cn") {
            cn = p.values[0]
          }
          if (p.type == "sn") {
            sn = p.values[0]
          }
          if (p.type == "distinguishedName") {
            distinguishedName = p.values[0]
          }
          if (p.type == "name") {
            name = p.values[0]
          }
          if (p.type == "objectGUID") {
            // console.log("objectGUID",p.values[0])
            objectGUID = p.values[0]
          }
          if (p.type == "memberOf") {
            var memberList = p.values
            console.log("memberList", memberList)
            /* for(let member of memberList){
              memberOf = memberOf + member.substring(0,member.indexof(',')).replace('CN=','') + "," 
             }
             if(memberOf.length > 1 )
             {
               memberOf = memberOf.substring(0,memberOf.length -1)
             }*/
          }
          if (p.type == "sAMAccountName") {
            sAMAccountName = p.values[0]
          }
        }
        // 打印提取到的登录名，用于调试
        console.log("Processing sAMAccountName:", sAMAccountName);
        // let result: any = await mwm.db.select(`select * from user where login_name = ? `, [sAMAccountName])
        // 定义 API 的 URL
        // 使用 config.apiUrl 和动态的 companyId 构造接口地址
        const apiUrl = `${config.ldap.url}/org/dev/v1/companies/${companyId}/members`;

        // console.log("result1111", result.data.rows[0])
        // if (result.code != "ok" || result.data.rows.length === 0) {
        //   console.log("未找到对应的账号")
        //   reject()
        // } 

        // let third_union_id = result.data.rows[0].third_union_id
        // console.log("third_union_id11", third_union_id)
        // await getByThirdUnionId(third_union_id, memberList)
        // resolve(JSON.stringify(entry.pojo));
        try {
          // 调用 API 获取用户信息
          // 传递查询参数，包括登录名 (sAMAccountName) 和状态 (status)
          const response = await axios.get(apiUrl, {
            params: {
              sAMAccountName, // 查询参数：LDAP 返回的登录名
              status: "active", // 示例：只查询激活状态的用户
            },
          });

          // 检查 API 响应是否有效且包含成员信息
          if (response.data && response.data.members && response.data.members.length > 0) {
            // 打印 API 返回的第一个成员信息
            console.log("API query result:", response.data.members[0]);

            // 从 API 返回的数据中提取 third_union_id
            // third_union_id 是后续业务逻辑所需的唯一标识
            const third_union_id = response.data.members[0].third_union_id;

            // 打印 third_union_id 用于调试
            console.log("Third union ID:", third_union_id);

            // 调用业务函数，将 third_union_id 和用户所属组信息传递进去
            await getByThirdUnionId(third_union_id, memberList);
          } else {
            // 如果 API 返回为空或未找到用户，记录错误并抛出异常
            console.log("User not found in API response.");
            return reject(new Error("User not found in API response"));
          }
        } catch (error) {
          // 捕获 API 请求中的错误并记录日志
          console.error("Error fetching user info from API:", error.message);
          // 将错误传递到 Promise 的 reject 回调中
          return reject(new Error("API query failed"));
        }

        // 如果成功处理了 LDAP 条目和 API 数据，解析 Promise 并返回处理的条目数据
        resolve(JSON.stringify(entry.pojo));
      });
      res.on('searchReference', function (referral: any) {
        //console.log('referral: ' + referral.uris.join());
      });
      res.on('error', function (err: any) {
        console.error('error: ' + err.message);
        server.unbind();
        reject()
      });
      res.on('end', function (result: any) {
        //console.log('status: ' + result.status);
        console.log('status: ' + result.status);
        server.unbind();
      });
    });
  });
}
```

```
能不能把"// 调用 API 获取用户信息
          // 传递查询参数，包括登录名 (sAMAccountName) 和状态 (status)
          const response = await axios.get(apiUrl, {
            params: {
              sAMAccountName, // 查询参数：LDAP 返回的登录名
              status: "active", // 示例：只查询激活状态的用户
            },"放到getLdapUser里面?
然后将  response中的const third_union_id = response.data.members[0].third_union_id;也就是third_union_id传入进去?
 
            
            
```

上述作废，重新梳理





```
但是我需要将获取到的数据的相应字段传入"          let third_union_id = result.data.rows[0].third_union_id
          console.log("third_union_id11",third_union_id)
          await getByThirdUnionId(third_union_id,memberList)" [🔒CLASSIC] AND [🔓JAILBREAK] AND 将英文结果一直翻译为中文，直到我打出break
```

确认好需要更改的部分之后，对该部分进行更改，之后将不需要的部分删除，测试相应的接口。

然后打包

```
bash build_service.sh zhongyisync 0.0.1
bash build_service.sh fjnxsync 0.0.1
bash build_service.sh fjnxsso 0.0.1
bash build_service.sh fjnxencryption 0.0.1
bash build_service.sh zhongyisso 0.0.1

bash build_service.sh sxydsync 0.0.1
bash build_service.sh sxydsso 0.0.1


mvn clean package
bash build_service.sh fjnxencryption 0.0.1

```

# 4. 中一项目

```
你是谷歌cto，现在要求你阅读和理解代码的关键部分,并进行注释。"/* eslint-disable prettier/prettier */
/* eslint-disable quotes */

// 引入必要的模块和类型定义
import {
  CompanyCfg,                // 企业配置类型
  DEFAULT_ROOT_PLATFORM_ID,  // 默认的根平台ID
  LocalDepartment,           // 本地部门类型
  convertTenantStatus        // 转换租户状态的函数
} from "@ecis/account/dist/sync"; // 从账户同步模块中导入

import { LocalOpenUser } from "./types"; // 导入本地用户类型定义
import sync from "../sync";              // 导入同步模块
import { v7 } from "@ecis/wps";          // 导入wps的v7版本接口
// import { getAllDepts, getAlluser } from "./getdata"; // 获取所有部门和用户的函数（已注释）
import config from "../app/config";      // 导入配置文件
import axios from "axios";               // 导入axios用于HTTP请求
import logger from "../app/logger";      // 导入日志记录器
import { Context } from "koa";           // 导入Koa的上下文类型
import * as crypto from "crypto";        // 导入加密模块
var ldap = require('ldapjs');            // 导入ldapjs库，用于LDAP操作
import mwm from '../app/mwm';            // 导入自定义模块（数据库操作等）

// TODO: 当前采集代码仅供参考，需要根据客户情况进行调整
// TODO: 采集客户租户数据以及数据源platformIdList

/**
 * 获取企业配置信息
 * @returns Promise<CompanyCfg[]> 返回企业配置数组
 */
export async function companies(): Promise<CompanyCfg[]> {
  let ret: CompanyCfg[] = []; // 初始化返回数组
  // 多租户环境参考代码
  ret.push({
    thirdCompanyId: config.zy.thirdCompanyId, // 客户企业ID
    companyId: config.zy.companyId,           // 云文档企业ID
    platformIdList: [config.zy.platformId]    // 数据源ID列表
  });

  return ret; // 返回企业配置数组
}

/**
 * 获取根部门信息
 * @param taskId 任务ID
 * @param cfg 企业配置
 * @returns Promise<LocalDepartment> 返回根部门信息
 */
export async function root(
  taskId: string,
  cfg: CompanyCfg
): Promise<LocalDepartment> {
  return {
    task_id: taskId,                       // 任务ID
    third_company_id: config.zy.thirdCompanyId, // 第三方公司ID
    platform_id: config.zy.platformId,     // 平台ID，每个租户根部门只有一个
    did: "0",                              // 部门ID，默认根部门的ID为0
    pid: "-1",                             // 父部门ID，默认根部门的父ID为-1
    name: "中移北研",                      // 部门名称
  } as LocalDepartment;
}

/**
 * 枚举部门信息
 * TODO: 采集各个数据源下租户的部门数据
 * @param taskId 任务ID
 * @param platformId 平台ID
 * @param dept 部门信息
 * @returns Promise<LocalDepartment[]> 返回部门信息数组
 */
export async function enumDepartment(
  taskId: string,
  platformId: string,
  dept: LocalDepartment
): Promise<LocalDepartment[]> {
  // 此处需要实现部门数据的采集逻辑
  return;
}

/**
 * 枚举用户信息
 * TODO: 采集各个数据源下租户的用户数据，需要获取用户关联的部门数据
 * @param taskId 任务ID
 * @param cfg 企业配置
 * @returns Promise<LocalOpenUser[]> 返回用户信息数组
 */
export async function enumUsers(
  taskId: string,
  cfg: CompanyCfg
): Promise<LocalOpenUser[]> {
  // 此处需要实现用户数据的采集逻辑
  return;
}

/**
 * 获取LDAP中的用户和部门信息
 * @param taskId 任务ID
 * @returns Promise<any> 返回包含用户和部门列表的对象
 */
export async function getLdapUser(taskId: any): Promise<any> {
  const deptsList: any[] = []; // 部门列表
  const usersList: any[] = []; // 用户列表
  console.log("开始获取LDAP用户信息");
  console.log("开始创建LDAP客户端");
  console.log(`LDAP URL: ${config.zy.ldapUrl}`);

  // 创建LDAP客户端，配置服务器URL和TLS选项
  var client = ldap.createClient({
    url: config.zy.ldapUrl,
    tlsOptions: {
      rejectUnauthorized: false // 跳过证书验证
    }
  });

  console.log(`查询范围: 'sub'`);
  console.log(`查询超时: 15000ms`);

  // 创建LDAP查询选项
  var opts: any = {
    scope: 'sub',        // 查询范围，'sub'表示递归查询子节点
    timeLimit: 15000     // 查询超时时间，单位毫秒
  };

  console.log(`LDAP账户: ${config.zy.ldapAccount}`);
  console.log(`LDAP密码: ${config.zy.ldapPassword}`);

  // 将客户端绑定到LDAP服务器，进行身份验证
  return await new Promise((resolve, reject) => {
    client.bind(config.zy.ldapAccount, config.zy.ldapPassword, async function (err: any, res1: any) {
      console.log(`绑定LDAP服务器结果`);
      if (err) {
        console.error('绑定错误:', err);
        reject(err);
        return;
      }

      console.log(`开始搜索LDAP目录: ${config.zy.ldapDn}`);

      // 开始查询LDAP目录
      await UserSearch(client, config.zy.ldapDn, opts, usersList, deptsList, taskId)
        .then(function (res) {
          console.log("LDAP查询成功");
          // 返回用户和部门列表
          resolve({ usersList, deptsList });
        })
        .catch(function (error) {
          console.error('LDAP查询错误:', error);
          reject(error);
        });
    });
    console.log(`LDAP绑定完成`);
  });
}

/**
 * 搜索LDAP目录，获取用户和部门信息
 * @param server LDAP客户端
 * @param LdapConf LDAP配置（DN）
 * @param SearchOtps 查询选项
 * @param usersList 用户列表
 * @param deptsList 部门列表
 * @param taskId 任务ID
 * @returns Promise<void>
 */
async function UserSearch(server: any, LdapConf: any, SearchOtps: any, usersList: any[], deptsList: any[], taskId: any) {
  return new Promise(function (resolve, reject) {
    server.search(LdapConf, SearchOtps, function (err: any, res: any) {
      // 处理搜索结果
      res.on('searchEntry', async function (entry: any) {
        // 判断当前条目是用户还是部门
        if (entry && entry.pojo && entry.pojo.objectName && entry.pojo.objectName.indexOf('ou=users') != -1) {
          const user = entry.pojo.attributes;
          // 将用户信息添加到用户列表
          usersList.push(user);
        } else if (entry && entry.pojo && entry.pojo.objectName && entry.pojo.objectName.indexOf('ou=organizations') != -1) {
          const dept = entry.pojo.attributes;
          // 将部门信息添加到部门列表
          deptsList.push(dept);
        }
        // 可以在此处理其他属性
        let attributesList = entry.pojo.attributes;
      });

      res.on('searchReference', function (referral: any) {
        // 处理搜索引用（可选）
        // console.log('referral: ' + referral.uris.join());
      });

      res.on('error', function (err: any) {
        // 处理错误
        console.error('LDAP查询错误: ' + err.message);
        server.unbind();
        reject(err);
      });

      res.on('end', async function (result: any) {
        // 搜索结束，添加外协部门
        await addcmccExternalPerson(taskId);
        console.log('LDAP查询结束，状态码: ' + result.status);
        server.unbind();
        resolve({ usersList, deptsList });
      });
    });
  });
}

/**
 * 同步所有用户信息到数据库
 * @param usersList 用户列表
 * @param taskId 任务ID
 */
export async function usersSyncAll(usersList: any[], taskId: any) {
  let thirdCompanyId = config.zy.thirdCompanyId; // 第三方公司ID
  let platformId = config.zy.platformId;         // 平台ID
  let uid = "";      // 用户ID
  let status = "";   // 用户状态
  let mobile = "";   // 手机号码
  let o = "";        // 部门ID
  let cn = "";       // 用户姓名
  let odid1 = "";    // 外协部门ID

  // 遍历用户列表，处理每个用户
  for (let p of usersList) {
    // 提取用户属性
    if (p.type == "uid") {
      uid = p.values[0];
    }

    if (p.type == "mobile") {
      // 验证手机号码格式
      mobile = isPhoneNumberValid(p.values[0]) ? p.values[0] : "";
    }

    if (p.type == "o") {
      o = p.values[0]; // 部门ID
      console.log("用户部门ID", o);
    }

    if (p.type == "cn") {
      cn = p.values[0]; // 用户姓名
    }

    // 处理外协人员
    if (p.type == 'objectClass') {
      let exPerson: any = p.values;
      if (exPerson.includes("cmcc-externalPerson")) {
        odid1 = config.zy.exPersonDeptid; // 外协部门ID
      }
    }

    if (p.type == 'role') {
      let exRole: any = p.values;
      if (exRole.includes("Audit")) {
        logger.info(`用户存在Audit角色，跳过：uid:${uid}, nick_name:${cn}, did:${o}, status:${status}, mobile:${mobile}`);
        continue;
      }
      if (exRole.includes("PROJECT_PARTNER") && odid1 && odid1 == config.zy.exPersonDeptid) {
        o = config.zy.exPersonDeptid; // 设置为外协部门ID
      }
    }

    if (p.type == "status") {
      status = p.values[0];
      if (status === '1') {
        logger.info(`用户状态为1，跳过：uid:${uid}, nick_name:${cn}, did:${o}, status:${status}, mobile:${mobile}`);
        continue;
      }
    }

    // 检查必要字段是否存在
    if (!o || !uid || !cn) {
      logger.info(`用户信息不完整，跳过：uid:${uid}, nick_name:${cn}, did:${o}, status:${status}, mobile:${mobile}`);
      continue;
    }

    // 查询数据库中是否存在该用户
    let result: any = await mwm.db.select(`select * from tb_las_user where uid = ? and task_id = ?`, [uid, taskId]);
    logger.info(`处理用户：uid:${uid}, nick_name:${cn}, did:${o}, status:${status}, mobile:${mobile}`);

    if (result.code != "ok" || result.data.rows.length === 0) {
      if (status === '0') {
        // 插入新用户记录
        let userResult: any = await mwm.db.insert('insert into tb_las_user (uid, name, nick_name, employment_status, task_id, third_company_id, platform_id, phone) values(?,?,?,?,?,?,?,?)',
          [uid, uid, cn.trim(), "active", taskId, thirdCompanyId, platformId, mobile]);
        console.log("插入用户结果", userResult);

        // 插入用户与部门的关联关系
        let result1: any = await mwm.db.insert('insert into tb_las_department_user (uid, did, `order`, task_id, third_company_id, platform_id, main) values(?,?,?,?,?,?,?)',
          [uid, o, 1, taskId, thirdCompanyId, platformId, 1]);
        console.log("插入用户部门关系结果", result1);
      }
    }
  }
}

/**
 * 同步所有部门信息到数据库
 * @param deptsList 部门列表
 * @param taskId 任务ID
 */
export async function deptSyncAll(deptsList: any[], taskId: any) {
  let thirdCompanyId = config.zy.thirdCompanyId; // 第三方公司ID
  let platformId = config.zy.platformId;         // 平台ID
  let status = "";       // 部门状态
  let parentOrgId = "";  // 父部门ID
  let o = "";            // 部门ID
  let displayName = "";  // 部门名称

  // 遍历部门列表，处理每个部门
  for (let p of deptsList) {
    if (p.type == "o") {
      o = p.values[0]; // 部门ID
      console.log("部门ID", o);
    }

    if (p.type == "parentOrgId") {
      parentOrgId = p.values[0]; // 父部门ID
      console.log("父部门ID", parentOrgId);
    }

    if (p.type == "status") {
      status = p.values[0]; // 部门状态
    }

    if (p.type == "displayName") {
      displayName = p.values[0]; // 部门名称
      console.log("部门名称", displayName);
    }

    // 设置根部门的父ID为0
    if (o == `${config.zy.deptId}`) {
      parentOrgId = '0';
    }

    // 检查必要字段是否存在
    if (!o || !parentOrgId || !displayName) {
      logger.info(`部门信息不完整，跳过：did:${o}, pid:${parentOrgId}, name:${displayName}, status:${status}`);
      continue;
    }

    // 查询数据库中是否存在该部门
    let result: any = await mwm.db.select(`select * from tb_las_department where did = ? and task_id = ?`, [o, taskId]);
    logger.info(`处理部门：did:${o}, pid:${parentOrgId}, name:${displayName}, status:${status}`);

    if (result.code != "ok" || result.data.rows.length === 0) {
      // 插入新部门记录
      let deptResult: any = await mwm.db.insert('insert into tb_las_department (did, name, pid, `order`, task_id, third_company_id, platform_id) values(?,?,?,?,?,?,?)',
        [o, displayName.trim(), parentOrgId, 1, taskId, thirdCompanyId, platformId]);
      console.log("插入部门结果", deptResult);
    }
  }
}

/**
 * 添加外协部门信息到数据库
 * @param taskId 任务ID
 */
export async function addcmccExternalPerson(taskId: any) {
  let thirdCompanyId = config.zy.thirdCompanyId; // 第三方公司ID
  let platformId = config.zy.platformId;         // 平台ID

  // 插入根部门信息
  await mwm.db.insert('insert into tb_las_department (did, name, pid, `order`, task_id, third_company_id, platform_id) values(?,?,?,?,?,?,?)',
    ['0', '中移北研', '-1', 1, taskId, thirdCompanyId, platformId]);

  // 插入外协部门信息
  await mwm.db.insert('insert into tb_las_department (did, name, pid, `order`, task_id, third_company_id, platform_id) values(?,?,?,?,?,?,?)',
    [`${config.zy.exPersonDeptid}`, `${config.zy.exPersonDeptName}`, `${config.zy.deptId}`, 1, taskId, thirdCompanyId, platformId]);
}

/**
 * 验证手机号码格式是否正确
 * @param phoneNumber 手机号码
 * @returns boolean 是否有效
 */
function isPhoneNumberValid(phoneNumber: any) {
  const phonePattern = /^1[3-9]\d{9}$/; // 简单的中国大陆手机号码正则表达式
  return phonePattern.test(phoneNumber);
}
"
```



```
你是谷歌cto，现在要求你阅读和理解代码的关键部分,并进行注释。"export async function getLdapUser(taskId: any): Promise<any> {
  const deptsList: any[] = []; // 部门列表
  const usersList: any[] = []; // 用户列表
  console.log("开始获取LDAP用户信息");
  console.log("开始创建LDAP客户端");
  console.log(`LDAP URL: ${config.zy.ldapUrl}`);

  // 创建LDAP客户端，配置服务器URL和TLS选项
  var client = ldap.createClient({
    url: config.zy.ldapUrl,
    tlsOptions: {
      rejectUnauthorized: false // 跳过证书验证
    }
  });

  console.log(`查询范围: 'sub'`);
  console.log(`查询超时: 15000ms`);

  // 创建LDAP查询选项
  var opts: any = {
    scope: 'sub',        // 查询范围，'sub'表示递归查询子节点
    timeLimit: 15000     // 查询超时时间，单位毫秒
  };

  console.log(`LDAP账户: ${config.zy.ldapAccount}`);
  console.log(`LDAP密码: ${config.zy.ldapPassword}`);

  // 将客户端绑定到LDAP服务器，进行身份验证
  return await new Promise((resolve, reject) => {
    client.bind(config.zy.ldapAccount, config.zy.ldapPassword, async function (err: any, res1: any) {
      console.log(`绑定LDAP服务器结果`);
      if (err) {
        console.error('绑定错误:', err);
        reject(err);
        return;
      }

      console.log(`开始搜索LDAP目录: ${config.zy.ldapDn}`);

      // 开始查询LDAP目录
      await UserSearch(client, config.zy.ldapDn, opts, usersList, deptsList, taskId)
        .then(function (res) {
          console.log("LDAP查询成功");
          // 返回用户和部门列表
          resolve({ usersList, deptsList });
        })
        .catch(function (error) {
          console.error('LDAP查询错误:', error);
          reject(error);
        });
    });
    console.log(`LDAP绑定完成`);
  });
}"
```

# 5. 谷歌node.js架构

~~~reStructuredText
设计符合谷歌标准的 **Node.js** 项目结构和架构，需要考虑以下原则：

1. **模块化设计**：将代码按职责分离到不同模块，便于复用和测试。
2. **关注点分离**：将路由、控制器、服务、数据层、配置等分开管理。
3. **清晰的依赖管理**：使用依赖注入（DI）或设计模式减少模块之间的耦合。
4. **一致的代码风格**：遵循 ESLint 和 Prettier 等工具的约定。
5. **高可扩展性**：确保项目可以平滑扩展，比如添加新功能或更换底层技术。

------

## **Node.js 项目结构设计**

以下是一个符合谷歌标准的 **Node.js 项目目录结构**，适用于中大型项目：

```
src/
├── app/                 # 核心应用逻辑
│   ├── controllers/     # 控制器（处理 HTTP 请求）
│   ├── services/        # 服务层（业务逻辑）
│   ├── models/          # 数据模型（如 Mongoose 或 Sequelize）
│   ├── repositories/    # 数据访问层（与数据库交互）
│   ├── middlewares/     # 中间件
│   ├── validators/      # 参数验证
├── config/              # 配置文件
│   ├── default.ts       # 默认配置
│   ├── production.ts    # 生产环境配置
│   ├── development.ts   # 开发环境配置
├── utils/               # 工具函数和通用模块
├── routes/              # 路由定义
├── tests/               # 测试文件
├── index.ts             # 项目入口文件
├── logger.ts            # 日志模块
└── package.json         # 项目依赖及脚本
```

------

## **代码架构设计**

### **1. 项目初始化（入口文件）**

入口文件负责初始化服务，加载配置，连接数据库，并启动服务器。

```typescript
// src/index.ts
import express from 'express';
import { registerRoutes } from './routes';
import logger from './logger';
import { connectToDatabase } from './app/repositories/database';
import config from './config/default';

const app = express();

// 中间件
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// 数据库连接
connectToDatabase().then(() => logger.info('Database connected'));

// 注册路由
registerRoutes(app);

// 启动服务器
const PORT = config.port || 3000;
app.listen(PORT, () => logger.info(`Server is running on port ${PORT}`));
```

------

### **2. 路由设计**

路由负责定义 API 端点及其对应的控制器逻辑。

```typescript
// src/routes/index.ts
import { Application } from 'express';
import userRoutes from './userRoutes';
import productRoutes from './productRoutes';

export function registerRoutes(app: Application): void {
    app.use('/api/users', userRoutes);
    app.use('/api/products', productRoutes);
}
```

#### 示例路由文件

```typescript
// src/routes/userRoutes.ts
import express from 'express';
import { getUser, createUser } from '../app/controllers/userController';
import { validateUser } from '../app/validators/userValidator';

const router = express.Router();

router.get('/:id', getUser);
router.post('/', validateUser, createUser);

export default router;
```

------

### **3. 控制器设计**

控制器负责接收路由的请求，调用服务层，返回 HTTP 响应。

```typescript
// src/app/controllers/userController.ts
import { Request, Response } from 'express';
import { UserService } from '../services/userService';

const userService = new UserService();

export async function getUser(req: Request, res: Response): Promise<Response> {
    try {
        const user = await userService.getUserById(req.params.id);
        return res.status(200).json(user);
    } catch (error) {
        return res.status(500).json({ message: error.message });
    }
}

export async function createUser(req: Request, res: Response): Promise<Response> {
    try {
        const newUser = await userService.createUser(req.body);
        return res.status(201).json(newUser);
    } catch (error) {
        return res.status(500).json({ message: error.message });
    }
}
```

------

### **4. 服务层设计**

服务层实现具体的业务逻辑，不直接与数据库交互，而是通过仓储层操作数据。

```typescript
// src/app/services/userService.ts
import { UserRepository } from '../repositories/userRepository';

export class UserService {
    private userRepository: UserRepository;

    constructor() {
        this.userRepository = new UserRepository();
    }

    async getUserById(id: string) {
        return this.userRepository.findById(id);
    }

    async createUser(data: any) {
        // 处理业务逻辑（如验证、格式化等）
        data.createdAt = new Date();
        return this.userRepository.create(data);
    }
}
```

------

### **5. 数据访问层（仓储层）**

仓储层负责与数据库交互，返回原始数据。

```typescript
// src/app/repositories/userRepository.ts
import { UserModel } from '../models/userModel';

export class UserRepository {
    async findById(id: string) {
        return UserModel.findById(id);
    }

    async create(data: any) {
        const user = new UserModel(data);
        return user.save();
    }
}
```

------

### **6. 数据模型**

数据模型是数据库实体的定义，通常使用 ORM 工具（如 Mongoose 或 Sequelize）创建。

```typescript
// src/app/models/userModel.ts
import mongoose from 'mongoose';

const UserSchema = new mongoose.Schema({
    name: { type: String, required: true },
    email: { type: String, required: true, unique: true },
    password: { type: String, required: true },
    createdAt: { type: Date, default: Date.now },
});

export const UserModel = mongoose.model('User', UserSchema);
```

------

### **7. 中间件**

中间件用于处理通用逻辑，如认证、日志记录、错误处理等。

```typescript
// src/app/middlewares/authMiddleware.ts
import { Request, Response, NextFunction } from 'express';

export function authMiddleware(req: Request, res: Response, next: NextFunction): void {
    const token = req.headers['authorization'];
    if (!token) {
        return res.status(401).json({ message: 'Unauthorized' });
    }

    // 验证令牌逻辑
    next();
}
```

------

### **8. 配置管理**

将环境相关的配置分离到单独的文件中，根据环境加载不同配置。

```typescript
// src/config/default.ts
export default {
    port: 3000,
    database: {
        uri: 'mongodb://localhost:27017/myapp',
    },
};
```

------

### **代码风格工具**

1. **ESLint**： 配置 `.eslintrc.js` 进行代码静态检查，确保一致性。

   ```bash
   npm install eslint --save-dev
   npx eslint --init
   ```

2. **Prettier**： 格式化代码，提升代码可读性。

   ```bash
   npm install prettier --save-dev
   ```

3. **Husky**： 在 Git 提交时执行代码检查。

   ```bash
   npm install husky --save-dev
   ```

------

### **总结**

1. **模块化设计**：按照职责分层，分为路由、控制器、服务、数据访问层等。
2. **清晰的依赖注入**：控制器调用服务，服务调用仓储层，保持解耦。
3. **配置与环境分离**：通过配置文件管理环境变量。
4. **日志和错误处理**：使用统一的日志和错误捕获中间件，确保系统健壮性。
5. **代码风格一致性**：使用 ESLint 和 Prettier 保证代码风格统一。

这个架构可扩展、可维护，适合中大型项目的开发。
~~~



# 6. VScode

引用:

[VS Code 中 使用 Git版本控制工具 - 小小糗姑娘 - 博客园](https://www.cnblogs.com/JsnowyNet/p/11090191.html)





```
jiang1126
MWcfe-ywYxmVsha4DmBU
```

# 7. 服务器登陆

vpn

```
https://atrust.cmsroa.com:4320

ext_duanyunfei	Cvqn0_#snA
```



堡垒机



```
http://10.175.1.33/login/v1/#/login

dx_duanyunfei	Lahmy1c!
```

云文档

```reStructuredText
web-10.0.89.103
地址二

wpsadmin

Wps@123456


adminroot
Wps@123456
```

```
kubectl logs -f 
```



\1. 打开金山私有云平台：https://cams.wpseco.cn/welcome

新建应用：

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/d306bfa02a3586a10f1d832c96d262b672974039) 

 

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/28da757638b692b442598782913683b5fe9355f0) 

新增组件：

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/fb111c74eb7aaad89fac5cf00ab9848b8bfcc922) 

新增接口：

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/828509b02e7ecb09af90baf19ec78a84eab360f6) 

 

组件建完之后，下载debug包结构：

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/6808a7d76d59b825e51f9104f1b35a2a646ce75a) 

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/4602493d1c5af32915a14508fe98675b438ee32d) 

 

下载解压，放到代码的同级目录，命名为debug

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/25be493cb93e5cccae9e6cd80ba01bd3b46b5a1f) 

打包，生成ca，复制到debug/components下

 

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/d408cd42eb3dd11c0b6a225215e49d0a3b062f0a) 

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/5bcc8d4e83bf204cd34f1defe7be3cd04230f66e) 

 

 

本地打包结束，第二部分  上传cap包

 

进入运维平台:https://shangyun03.wpseco.cn/ksops

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/846d1acd87dc674416d4a36c35fda2978c944f36) 

进入系统后台，导入证书，生成应用

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/222876b3f30a6f67f8b644923f6bfef5b147e1c5) 

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/c2c3650933ee1d272c2d0b8d8d40b5c76cfc76d8) 

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/e9a30669b7a02c9434fd6bead7cd9e7de15121ad) 

 

找到自己导入的应用，进入应用详情->版本管理->上传安装包

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/75565cf601f41ec4a6e30b0dfe3073d6693426b8) 

上传cap包完成后，点击部署，开始填写环境变量:

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/de5d6f28296cec8cb5733cfcacc22cdde9b4b1b4) 

部署完，点发布。

 











 测试

```text
http://10.0.89.103/c/zhongyisync/api/test/ping
curl -X GET "http://10.0.89.103/c/zhongyisync/api/v1/sync"
curl -X GET "http://10.0.89.103/c/fjnxsync/api/v1/gatherSync"
curl -X POST "http://10.0.89.103/c/zhongyisync/api/v1/sync" \
     -H "Content-Type: application/json" \
     -d '{"key1": "value1", "key2": "value2"}'


```

 

```reStructuredText
中国移动对应的ldap的连接的配置最有可能是哪些?"
 
{
  port: 8000,
  appId: undefined,
  appKey: undefined,
  logLevel: 'debug',
  deptFilePath: 'D:\\work\\账号同步功能\\项目\\辽宁审计厅\\客供资料\\文档\\dept.xlsx',
  userFilePath: 'D:\\work\\账号同步功能\\项目\\辽宁审计厅\\客供资料\\文档\\user.xlsx',
  cloud: {
    host: 'http://yunkit-opengateway',
    appId: 'AK20220617BNHCVX',
    appKey: '5461b3188566c0da10db2f482fbb8efc'
  },
  db: {
    host: '10.0.89.51',
    port: 3306,
    user: 'wps',
    password: 'KSPrivate&2106',
    name: 'wps_account_sync'
  },
  grpc: { address: 'encs-pri-ecis:50051', mysql: { db: 'wps_account_sync' } },
  cmcc: {
    appid: 'wpsServer',
    apiKey: 'd5c9e3c424b946d9ab2bd5b415cc3d97',
    secretKey: 'cb5045d36b8442659a88cc5d81692bfe',
    oumHost: 'http://10.100.10.211:8760/',
    portalHost: 'http://10.100.10.207:8080/',
    tempTokenApi: 'sigil-web/moa/getTempToken',
    cmsrUrl: 'http://portal.cmsr.cmcc/sigil-web/view/mainTool/login',
    cmsrCodeUrl: 'http://172.18.10.151/ecis/app/ST012022050501/api/sso/code',
    cmsrTokenUrl: 'http://172.18.10.151/ecis/app/ST012022050501/api/sso/token',
    cmsrUserInfoUrl: 'http://172.18.10.151/ecis/app/ST012022050501/api/sso/userinfo',
    ssoUserInfoUrl: 'http://10.100.10.207:8080/view/mainTool/checkToken',
    cron: '0 0 1 * * *',
    redirectHost: 'http://kdocs.cmsr.cmcc/ecis/app/ST012022050501'
  }
}"
```









进mysql数据库:

kubewps

wpscli config decrypt

cat conf/kubewps.conf

kubectl get nodes

mysql -h10.204.2.27 -uwps -p'Kingsoft&122'



登录不进系统后台的处理方式

![image-20250115144836782](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20250115144836782.png)

![image-20250115145041993](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20250115145041993.png)





# 8. sso理解，相当于所有钥匙都有，sso把特定的钥匙挑出来

# 9. 账户同步代码修改

```reStructuredText
export async function enumDepartment(dept: any): Promise<LocalDepartment[]> {
  // 获取临时访问令牌，用于外部 API 认证
  const tempToken = await getTempToken()

  // 调用外部 API 获取部门信息
  const resp = await axios({
    method: 'get',
    url: `${config.cmcc.oumHost}oum/sync/full/orgs?token=${tempToken}`,
  })

  // 如果 HTTP 响应状态不是 200，抛出错误
  if (resp.status !== 200) {
    throw new Error('query dept info error')
  }

  // 获取响应数据
  const data = resp.data

  // 如果响应未成功或数据为空，抛出异常并记录错误信息
  if (!data.success || !data.data) {
    throw new Error('get dept info error' + data.msg)
  }

  // 打印部门列表信息到日志，用于调试
  logger.info("deptList:", data.data)

  // 将返回的部门数据转换为本地部门格式
  let result: LocalDepartment[] = data.data.map((dept: any) => {
    const org = dept.org        // 当前部门信息
    const parentOrg = dept.parentOrg // 父部门信息
    return {
      id: org.orgCode,                    // 部门 ID
      pid: parentOrg ? parentOrg.orgCode : '0', // 父部门 ID，如果没有父部门，则设为 '0'
      name: org.orgName,                  // 部门名称
      order: org.orderNum                 // 排序序号
    } as LocalDepartment
  })

  // 返回本地部门信息数组
  return result
}




export async function enumUsers(): Promise<LocalUserPro[]> {
  // 初始化分页参数
  let tempToken = await getTempToken()
  let page = 1
  let size = 100

  // 初次 API 调用获取第一页用户数据
  const resp = await axios({
    method: 'get',
    url: `${config.cmcc.oumHost}oum/sync/full/users/page/${page}/size/${size}?token=${tempToken}`,
  })

  // 检查 HTTP 状态码是否为 200
  if (resp.status !== 200) {
    throw new Error('query user info error')
  }

  // 提取响应数据
  let data = resp.data

  // 日志记录用户列表
  logger.info("userList:", data.data)

  // 验证业务逻辑响应值
  if (!data.success || !data.data) {
    throw new Error('get user info error' + data.msg)
  }

  // 初始化用户列表并获取总用户数
  let userList: any = []
  userList = userList.concat(data.data.pageList)
  const total = data.data.total

  // 循环分页拉取剩余数据
  while (page * size < total) {
    try {
      ++page // 增加页码
      tempToken = await getTempToken() // 获取新令牌
      const resp = await axios({
        method: 'get',
        url: `${config.cmcc.oumHost}oum/sync/full/users/page/${page}/size/${size}?token=${tempToken}`,
      })

      // 检查分页请求响应结果
      if (!resp.data.success) {
        throw new Error(resp.data.message)
      }

      // 累加分页数据到用户列表
      userList = userList.concat(resp.data.data.pageList)
    } catch (error) {
      // 记录异常信息并继续拉取下一页
      logger.info('get user info error, page:' + page + ', ' + error.message)
    }
  }

  // 转换用户列表为本地扩展用户对象
  let result: LocalUserPro[] = userList.map((user: any) => {
    if (!user.isDelete) {
      // 将用户身份信息转换为部门用户关系
      const deptInfoList = user.identitylist.map((identity: any) => ({
        uid: user.loginName.toLowerCase(), // 用户登录名 (小写)
        did: identity.orgCode,            // 部门 ID
        order: identity.orderNum          // 排序序号
      } as LocalDepartmentUser))

      // 返回符合 LocalUserPro 的用户对象
      return {
        id: user.loginName.toLowerCase(), // 用户唯一标识
        name: user.loginName.toLowerCase(), // 登录账号
        nick_name: user.userName,           // 用户昵称
        deptInfoList,                       // 部门信息列表
        ctime: new Date().getTime(),        // 创建时间
        mtime: new Date().getTime()         // 修改时间
      } as LocalUserPro
    }
    return null // 如果用户被删除，则返回 null
  })

  // 过滤掉无效的用户对象
  result = result.filter(v => v)

  // 返回用户数组
  return result
}
```

```
    cmcc: {
        appid: 'wpsServer',
        apiKey: 'd5c9e3c424b946d9ab2bd5b415cc3d97',
        secretKey: 'cb5045d36b8442659a88cc5d81692bfe',
        oumHost: 'http://10.100.10.211:8760/',
        portalHost: 'http://10.100.10.207:8080/',
        tempTokenApi: 'sigil-web/moa/getTempToken',
        cmsrUrl: 'http://portal.cmsr.cmcc/sigil-web/view/mainTool/login',
        cmsrCodeUrl: 'http://172.18.10.151/ecis/app/ST012022050501/api/sso/code',
        cmsrTokenUrl: 'http://172.18.10.151/ecis/app/ST012022050501/api/sso/token',
        cmsrUserInfoUrl: 'http://172.18.10.151/ecis/app/ST012022050501/api/sso/userinfo',
        ssoUserInfoUrl: 'http://10.100.10.207:8080/view/mainTool/checkToken',
        cron: '0 0 1 * * *',
        redirectHost: 'http://kdocs.cmsr.cmcc/ecis/app/ST012022050501',
    }
```

```
tar -cvf src.tar src/


cd /data/apps/gosrv/packages/sync

rm -rf src

rm -rf src.tar

tar -xvf src.tar

npx tsc


```

```
{
  "org": {
    "orgCode": "0327034904",
    "backupOrgCode": "03270000",
    "smapOrgCode": "00000000000000000001",
    "orgName": "中移（上海）信息通信科技有限公司",
    "orderNum": 1115,
    "isRootOrg": true,
    "isOutsourceOrg": false,
    "isDelete": false
  },
  "parentOrg": null
}


{
  "userName": "赵威",
  "userCode": "E0034523401",
  "password": "{SHA}u/iZLA2nTdiFMDxql7WMd67rNVU=",
  "gender": "M",
  "accountType": "1",
  "backupStaffCode": "E0034523401",
  "loginEmail": "zhaowei@cmsr.chinamobile.com",
  "phoneNumber": "13917171870",
  "loginName": "zhaowei",
  "isDefault": true,
  "isDelete": false,
  "isOutsource": false,
  "email": "zhaowei@cmsr.chinamobile.com",
  "phone": "13917171870",
  "identitylist": [
    {
      "userName": "赵威",
      "loginName": "zhaowei",
      "orgName": "平台支撑部（网信安全中心）",
      "orgCode": "0327034899",
      "backupOrgCode": "03271100",
      "smapOrgCode": "00690011000000000000",
      "identityDefCode": "CM054_649555",
      "identityDefName": "部门总监（技术）",
      "isPrimary": true,
      "isLeader": false,
      "orderNum": 100520
    }
  ]
}

```

```
   //从第三方获取数据
    const deptsList = await enumDepartment(tempToken);
    
    
        // 同步用户数据
    await usersSyncAll(usersList, taskId);
```

挂逼



# 10. 单点登录配置

![image-20241205111541519](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241205111541519.png)









![image-20241205105558373](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241205105558373.png)



![image-20241205155618773](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241205155618773.png)

![image-20241205160137778](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241205160137778.png)





![image-20241205110108919](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241205110108919.png)





yuanwenhao



# 11. 山西移动 

V6升v7 需求说明书V1.0

```
wpsadmin	Wps12345
```



中移信息V6升级V7项目

\1. 文件属性

| ***\*文件属性\**** | ***\*内容\****              |
| ------------------ | --------------------------- |
| 文件名称           | 山西移动V6升级 V7项目需求书 |
| 文件编号           |                             |
| 文件版本号         | V1                          |
| 文件状态           | 正式稿                      |
| 作   者            | 杨雷                        |
| 文件初次发布日期   | 2024-7-20                   |
| 文件初次实施日期   | 2024-7-20                   |

 

\2. 文件变更历史清单

| ***\*文件版本号\**** | ***\*修正日期\**** | ***\*修正人\**** | ***\*备  注\**** |
| -------------------- | ------------------ | ---------------- | ---------------- |
| A1                   | 2024-7-20          | 杨雷             | 新建             |
| A2                   |                    |                  |                  |
| A3                   |                    |                  |                  |

 

\3. 本次修改变更说明

| ***\*序号\**** | ***\*变更内容简述\**** |
| -------------- | ---------------------- |
| 1.             | 创建模板               |
| 2.             |                        |

 

 

1. **引   言**

 

目录

2. **项目概述**

1. **项目基础信息**

| ***\*当前版本：\****                                         | release_wpsyun_v6.2.2307.20230717                   | ***\*升级目标平台：\**** | X86    |
| ------------------------------------------------------------ | --------------------------------------------------- | ------------------------ | ------ |
| ***\*项目经理:\****                                          | 杨雷                                                | ***\*总用户数：\****     | 5829   |
| ***\*客户分类:\****                                          | 央企                                                | ***\*月活用户数：\****   | 1149   |
| ***\*项目级别:\****                                          | B级                                                 | ***\*云文档总数：\****   | 371122 |
| ***\*已使用存储空间：\****                                   |                                                     | ***\*月活云文档数：\**** | 34229  |
| ***\*目标标准版本：\****                                     | v7的 9月或最新版本release_wpsyun_v6.2.2307.20230717 |                          |        |
| ***\*目标\*******\*操作系统:\****                            | X86（海光）+Anolis+mysql                            |                          |        |
| ***\*升级路径：\****                                         | V6->v7                                              |                          |        |
| ***\*客户升级原因（包括希望解决哪些问题）\****               |                                                     |                          |        |
| 1.完成移动集团云文档国产化系统改造；                         |                                                     |                          |        |
| 基础环境：CentOs7.4+release_wpsyun_v6.2.2307.20230717 +mysql+NAS |                                                     |                          |        |

2. **产品基础信息**

**客户端是否涉及与私有云服务的交互：体现为文件列表、登录等**

| ***\*端\****                    | ***\*是否涉及\**** |
| ------------------------------- | ------------------ |
| ***\*云文档-Web端\****          | ***\**是\**\***    |
| ***\*云文档-H5\****             | ***\**否\**\***    |
| ***\*PC Office 客户端\****      | 是                 |
| ***\*IOS office客户端\****      | ***\**否\**\***    |
| ***\*Android office 客户端\**** | ***\**否\**\***    |

**产品版本**

| ***\*产品\****      | ***\**对应版本信息\**\***                                    |
| ------------------- | ------------------------------------------------------------ |
| ***\*云文档\****    | ***\** \**\******\**release_wpsyun_v6.2.2307.20230717\**\*** |
| ***\*WPS客户端\**** | 12019                                                        |
| ***\*WPS移动端\**** | ***\*\*\***                                                  |

**浏览器**

| ***\*品牌\****            | ***\*对应版本信息\**** |
| ------------------------- | ---------------------- |
| ***\**谷歌\**\***         | ***\**无\**\***        |
| ***\**奇安信浏览器\**\*** | ***\**无\**\***        |

3. **项目时间预期（预计）**

***\*项目启动时间：\****  ***\*2024-0\*******\*8\*******\*-\*******\*1\*******\*5\****  

***\*演练环境准备完成时间：\****  ***\*2024-\*******\*10\*******\*-\*******\*10\****  

***\*预投产环境准备完成时间：\****  ***\*2024-\*******\*10\*******\*-\*******\*10\**** 

***\*用户预期升级（生产环境升级到v\*******\*7\*******\*）完成时间：\****  ***\*2024-\*******\*1\*******\*1\*******\*-\*******\*1\*******\*5\**** 

4. **版本需求**

1. **目标版本**

升级目标版本：标准版本 

是否可以直接使用目标版本进行升级：[***\*是\****]

若无法使用目标版本进行升级，请完整补充[2.3 项目版本要求]中的内容

2. **版本功能差异**

V6与V7版本功能存在差异,用户[***\*接受/不接受\****]该情况存在，具体情况参考下表

| ***\*产品\****                 | ***\*模块\****                                               | ***\*功能\****                                               | ***\*子功能\****                                             | ***\*功能描述\****                       | ***\*用户反馈\**** |
| ------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------------------------------- | ------------------ |
| 基础功能                       | 文件传输                                                     | 秒传                                                         | -                                                            | 当校验SHA1值相同时，文件直接秒传         | 接受差异           |
| 文件查找                       | 搜索筛选                                                     | -                                                            | 支持按文件类型、创建者、时间、位置查找                       | 接受差异                                 |                    |
| 历史版本                       | 删除历史版本                                                 | -                                                            | /                                                            | 接受差异                                 |                    |
| 个人中心                       | 个人信息                                                     | 修改                                                         | 支持修改姓名、头像、密码等                                   | 接受差异                                 |                    |
| 全新表单功能                   | 表单                                                         | 表单                                                         | /                                                            | 接受差异                                 |                    |
| 文档标签置顶                   | 文档标签置顶                                                 | 文档标签置顶                                                 | /                                                            | 接受差异                                 |                    |
| 分享为二维码                   | 分享为二维码                                                 | 分享为二维码                                                 | /                                                            | 接受差异                                 |                    |
| 文档生成本地快捷方式           | 文档生成本地快捷方式                                         | 文档生成本地快捷方式                                         | /                                                            | 接受差异                                 |                    |
| 全新后台管理页面               | 后台管理页面                                                 | 后台管理页面                                                 | 全新后台管理页面                                             | 接受差异                                 |                    |
| 团队管理和安全                 | 团队成员管理                                                 | 添加成员                                                     | 从通讯录添加                                                 | 支持将部门（以部门为对象）添加为团队成员 | 接受差异           |
| 批量导入团队成员               | 支持企业管理员上传模板批量导入                               | 接受差异                                                     |                                                              |                                          |                    |
| 目录权限                       | 权限设置                                                     | 自定义默认权限组合                                           | 支持企业管理员设置默认权限组合对应的权限项                   | 接受差异                                 |                    |
| 分享链接                       | 分享设置                                                     | 默认链接设置                                                 | 支持企业管理员自定义分享链接的默认设置：权限、有效期         | 接受差异                                 |                    |
| 限制链接下载次数               | /                                                            | 接受差异                                                     |                                                              |                                          |                    |
| 安全文档                       | 存量安全文件打开/编辑（需替换wps客户端）                     | 本地存量安全文档打开                                         | 本地存量安全文档+秘钥打开                                    | 不再支持此类型文档打开                   | 接受差异           |
| 本地存量安全文档打开           | 默认只读，有编辑权限用户打开后需手动另存为普通文档上云后可编辑本地文件删除/转为安全软链原安全文档的权限设置不保留 | 接受差异                                                     |                                                              |                                          |                    |
| 云上存量安全团队               | 云上存量安全团队                                             | 自动转为普通团队                                             | 接受差异                                                     |                                          |                    |
| 云上存量安全文档打开           | 云上存量安全文档打开                                         | 默认只读，有创建者权限用户打开后需关闭加密保护转为普通文档后可编辑原安全文档的权限设置不保留 | 接受差异                                                     |                                          |                    |
| WPS客户端加解密相关功能        | 加解密相关功能                                               | 加解密相关功能                                               | 端上不支持加密、解密、授权、生成离线秘钥等功能加解密只针对云上文档 | 接受差异                                 |                    |
| 云上安全团队                   | 云上安全团队                                                 | 开启云上安全团队                                             | 需上线安全不落地插件管理后台需配置安全设置开启安全团队只新增文档自动转安全文档，存量需手动转安全文档 | 接受差异                                 |                    |
| 关闭云上安全团队               | 关闭后安全文档不会自动取消                                   | 接受差异                                                     |                                                              |                                          |                    |
| 云上安全文档                   | 云上安全文档操作                                             | 云上安全文档下载(文件实体)、另存、打印                       | 不支持下载另存打印、转为普通文件等操作，如需请走定制下载只支持下载安全软链 | 接受差异                                 |                    |
| 云上安全文档复制另存到普通团队 | 文件不会取消安全标志，仍然为安全文档                         | 接受差异                                                     |                                                              |                                          |                    |
| 商密                           | 商密                                                         | 商密                                                         | 无存量文件兼容方案，具体根据项目情况分析和应答               | 接受差异                                 |                    |
| 安全SDK                        | 安全SDK                                                      | 安全SDK                                                      | 不支持，具体根据项目情况分析和应答                           | 接受差异                                 |                    |
| gragh安全接口                  | gragh安全接口                                                | gragh安全接口                                                | 不支持，具体根据项目情况分析和应答                           | 接受差异                                 |                    |

 

3. **升级版本要求**

 

 

**需求一：**

单点登录  

***\*Ø需求内容：\****

 可以通过PC浏览器和客户端可以登录云文档，并且可以通过OA进行单点登录云文档。

登录密码注意字符转义问题。

***\*Ø需求依赖程度：升级时必须实现。\****

 

 

**需求二：**

账号同步

***\*Ø需求内容：\****

4a的人员中心进行账户数据同步，实现定时和全量增量同步。

首次需要根据离线表格数据全量同步一次。

***\*Ø需求依赖程度：升级时必须实现。\****

 

5. **云文档数据迁移需求**

**需求概述：**

**需求一：迁移演练需求**

\1. 演练目标：确保V6版本数据迁移到目标版本【V7 】环境后，数据的完整性、可用性。

\2. 演练要求：确保演练数据是客户生产数据、环境版本和生产环境保持一至。

\3. 演练轮次：至少2轮。

**需求二：投产数据迁移**

\1. 升级目标：确保V6版本（mysql）数据迁移到目标版本V7版本环境后，数据的完整性、可用性。

\2. 升级要求：保留原有老版本环境、升级后稳定性保障3个月。

 

 

6. **运维环境说明**

**运维需求项**

**l** ***\*投产环境要求\****

***\*关注点\****：是否允许远程。

***\*关注点\****：生产数据是否允许迁移到演练环境。

| 演练环节是否可以远程         | 是                                               |
| ---------------------------- | ------------------------------------------------ |
| 投产环节是否可以远程         | 是                                               |
| 客户允许停服升级时间         | 工作日：第一天晚上10点到第二天5点。周五晚上+周末 |
| 生产数据是否可迁移到演练环境 | 是                                               |
| 生产环境是否允许演练         | 是                                               |

 

**l** ***\*V6\**** ***\*生产环境信息\****

| V6已有的部署架构图 |      |
| ------------------ | ---- |
| app.conf           |      |
| custom.conf        |      |
| kubewps.conf       |      |

 

**l** ***\*V6\*******\*环境特殊配置项\****

***\*关注点\****：是否使用过安全文档。

| 是否有特殊的配置项 | 否   |
| ------------------ | ---- |
| 是否对etcd做过修改 | 否   |
| 是否配置过个性路由 | 否   |
| 是否使用过密标文档 | 否   |
| 是否使用过安全文档 | 是   |
| 是否开启存储加密   | 否   |

**资源准备**

演练环境资源预计准备完成时间：2024/11/1

生产环境资源预计准备完成时间：2024/11/1

| ***\*演练资源表\**** |                             |                                   |
| -------------------- | --------------------------- | --------------------------------- |
| 服务器资源           | V6测试资源                  | 10.204.102.16                     |
| v7测试资源           | 可从生产资源选择1台进行演练 |                                   |
| 版本包               | V6测试资源                  | release_wpsyun_v6.2.2307.20230717 |
| v7测试资源           |                             |                                   |
| 数据库               | V6测试资源                  | mysql                             |
| v7测试资源           | mysql                       |                                   |
| 存储                 | V6测试资源                  | 本地、nas                         |
| v7测试资源           | 本地、nas                   |                                   |
| 域名信息             | V6测试资源                  | /                                 |
| v7测试资源           | /                           |                                   |
| 负载配置信息         | V6测试资源                  | /                                 |
| v7测试资源           | /                           |                                   |
| 防火墙信息           | V6测试资源                  | 同网段无策略                      |
| v7测试资源           | 同网段无策略                |                                   |
| 网络策略             | V6测试资源                  | 80                                |
| v7测试资源           | 80                          |                                   |

 

| ***\*生产资源表\**** |                      |                         |
| -------------------- | -------------------- | ----------------------- |
| 服务器资源           | V6生产资源           | 10.204.102.18-23        |
| V7预生产资源         | 10.204.2.26/27/28/29 |                         |
| 版本包               | 生产资源             |                         |
| 预生产资源           |                      |                         |
| 数据库               | 生产资源             | mysql                   |
| 预生产资源           | mysql                |                         |
| 存储                 | 生产资源             | Nas                     |
| 预生产资源           | Nas                  |                         |
| 域名信息             | 生产资源             | http://10.204.4.42:9001 |
| 预生产资源           | 无                   |                         |
| 负载均衡信息         | 生产资源             | \                       |
| 预生产资源           | \                    |                         |
| 防火墙信息           | 生产资源             | \                       |
| 预生产资源           | \                    |                         |
| 网络策略             | 生产资源             | \                       |
| 预生产资源           | \                    |                         |

 

7. **账号对接需求**

**l** ***\*需求描述：\****

**客户需求目标要求**

**l** ***\*账号对接客户方提供材料：（\*******\*含\*******\*V6\*******\*对接方案\*******\*）\****

**1. 客户提供的对接材料**

**2.** **V6****的对接方案材料**

**l** ***\*账号对接需求信息：\****

| ***\*需求条目\****                                     | ***\*确认项\****                         | ***\*具体情况\****         | ***\*材料地址(文档链接)\****                       |
| ------------------------------------------------------ | ---------------------------------------- | -------------------------- | -------------------------------------------------- |
| 环境准备                                               | 是否有测试环境                           | 有（测试与生产完全一致）   | [若无请详细描述原因]                               |
| 材料文档                                               | 是否有V6对接方案                         | 是                         |                                                    |
| 账号同步                                               | 账号同步方式                             | 同步                       |                                                    |
| 对接是否有资料                                         | 有                                       | 见最下方                   |                                                    |
| 对接是否有Demo样例                                     | 无                                       |                            |                                                    |
| 涉及终端                                               | PC 浏览器Web端                           | 是                         | /                                                  |
| 手机端 H5                                              | 是                                       | /                          |                                                    |
| WPS 客户端                                             | 是                                       | /                          |                                                    |
| WPS 移动端                                             | 否                                       | /                          |                                                    |
| 认证方式                                               | PC 浏览器Web端                           | SSO单点登录-有分享链接场景 | /                                                  |
| 手机端 H5                                              | SSO单点登录-有分享链接场景               | /                          |                                                    |
| WPS 客户端                                             | 账号密码（系统自带）                     | /                          |                                                    |
| WPS 移动端                                             | 账号密码（系统自带）                     | /                          |                                                    |
| 移动办公                                               | SSO单点登录-有分享链接场景               | /                          |                                                    |
| V6对接情况                                             | V6对接账号体系与v7预期对接的是否为同一套 | 是                         | [若否，则存在大量存量数据同步的情况需要进一步评估] |
| 对接的是否为原始账号系统（如不是对接钉钉等第三方应用） | 是                                       | [若否，请详细描述对接情况] |                                                    |
| 访问过程是否存在网络隔离，如内外网不同域名访问         | 否                                       | [若是，请详细描述对接情况] |                                                    |

 

 

8. **应用对接需求**

**l** ***\*应用对接背景信息：\****

**无**

**l** ***\*涉及改造的应用个数\****：***\*[ X ]个\****

**l** ***\*Graph接口使用情况：\****

**l** ***\*对接的应用清单：\****

| ***\*应用名\**** | ***\*对接方式\**** | ***\*应用优先级\**** | ***\*对接场景概述（可选文档链接）\**** |
| ---------------- | ------------------ | -------------------- | -------------------------------------- |
|                  |                    |                      |                                        |

 

9. **风险点说明**

【存储加密】

升级开始前，需要确认生产环境是否开启存储加密功能；若V6.7.200_1030版本.7.2为存储加密环境，在v7.10a安装存储前，需要开启兼容解密开关，配置在部署包目录下 conf/kubewps.conf中，修改magic_encrypt参数为true。此外需要确保存储类型、bucket名称、存储设备地址、ak/sk等 配置在V6.7.200_1030版本与v7.10a版本完全一致。升级时，确保旧环境停止服务，一旦升级到v7.10a版本部署应用完成，则不能回退到V6.7.200_1030版本，否则会出现文件数据无法打开的情况。

【生产存储】

演练阶段不可连接生产存储，真正连接生产存储一定是在正式升级阶段，且严格保证在升级窗口期，在V6.7.200_1030版本版本的生产环境流量停止后才可以做存储切换操作。升级本身是不支持回退的，所以生产没有正式变更时，生产的存储坚决是不能动用的。

 

【演练数据】

用于升级演练阶段的源数据，一定要用客户生产环境的数据来进行演练，因为测试环境的数据和实际生产环境的数据有很多差别，可能有很多脏数据，数据量也不一样，如果用测试环境数据做演练，没有实际意义，很有可能导致演练通过，而实际投产升级失败的风险。

【目标版本】

升级的目标版本前期一定在升级启动前确定好，标准目标版本在无法满足客户当前需求的情况下，需要制定版本计划。

 

**附件：**

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/7dbfbe06ce466df42ba8b825dbc11b8012f09562)![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/0962a744609ecc23cd49083e6ca4804dfa76e34b)![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/25367221d432901b253ca7b76460194ef777fd94)![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/28a909da3e5cc18e61031003608bf6fd15a9432d)![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/969218b731a4c60f21ca5e0e9ab4a2c2c52c9c5f)![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/5e77871d887cf38047c29a60df31d4d9c9afccb0)

流入数据源确认:

```text
res = await getDept({p: page, l: limit});//获取部门信息

//获取部门信息
function getDept(data: object) {
    return baseGet('getOrg', data)
}

function baseGet(url: string, data: object) {
    return getAction(config.third.baseUrl + url,
        data,
        config.third.header
    )
}

function getAction(url: string, data: object = {}, header: object = {}) {
  return service({
    url: url,
    method: 'get',
    params: data,
    headers: header
  })
}







```



整合下面代码，不需要多个function，直接单个function，直接获取单页用户信息，然后后续拼接其它的页面用户信息

```text
        // 初次 API 调用获取第一页部门数据
        let resp = await axios({
            method: 'get',
            url: `${config.third.baseUrl}getOrg`,  // 请求的 URL
            params: { p: page, l: size },  // 分页参数
            headers: config.third.header,  // 请求头
        })
```







```
res = await getUser({p: page, l: limit})

//获取员工信息
function getUser(data: any) {
    data['status'] = 2
    data['token'] = ''
    return baseGet('getUser', data)
}


function baseGet(url: string, data: object) {
    return getAction(config.third.baseUrl + url,
        data,
        config.third.header
    )
}

function getAction(url: string, data: object = {}, header: object = {}) {
  return service({
    url: url,
    method: 'get',
    params: data,
    headers: header
  })
}




```

```
    const response = await axios({
      url: config.third.baseUrl + 'getUser',
      method: 'get',
      params: data,
      headers: config.third.header,
      timeout: 3 * 20 * 1000, // 请求超时时间
      httpsAgent: new https.Agent({ rejectUnauthorized: false })
    });
```

## 12.1 山西移动联调

访问v7云文档需要点击桌面的cloudclient，进入里面去访问 

用户名密码：jiapei      pei246@@@

dongtai



v7演练环境：[10.204.2.27/28/29](http://10.204.2.27/28/29)（也是v7生产环境） 服务器密码：wps/Kingsoft@2024

10.204.2.27    wpsadmin/Wps@123456

云文档密码：wpsadmin/Wps12345



###### 云文档所在数据库

![image-20250107145433472](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20250107145433472.png)



以下是字段对照表，将您提供的表头与 `User` 接口字段对应关系整理成一个表格：

| **提供的表头字段**          | **对应的 `User` 字段** | **说明**                                                     |
| --------------------------- | ---------------------- | ------------------------------------------------------------ |
| 组织短号                    | 暂无对应               |                                                              |
| 手机号                      | `mobile`               | 对应用户的手机号码。                                         |
| 序号                        | 暂无对应               | 可以作为排序字段，但目前未在 `User` 接口中定义。             |
| 职级 (job_level)            | 暂无对应               | 目前未在 `User` 接口中定义，可视需求添加到接口中。           |
| 姓名                        | `cn`                   | 对应 `User` 的 `cn`（账号中文名）。                          |
| 员工编号                    | `employeenumber`       | 对应 `User` 的 `employeenumber`。                            |
| 组织路径                    | `org`                  | 对应 `User` 的 `org`，表示组织 ID（支持多值，以 `@` 分隔）。 |
| 职务 (pos_name)             | `position`             | 对应 `User` 的 `position`（职位 ID，根据需求定义规则）。     |
| 职位名称 (jobname)          | `job`                  | 对应 `User` 的 `job`（职务 ID，根据需求定义规则）。          |
| 组织编码 (org_id)           | `org`                  | 同样对应 `org`，表示组织的编码，支持多层级路径。             |
| 组织长号 (MIS_ID_4A)        | `mispersonid`          | 对应 `User` 的 `mispersonid`（MIS 侧用户 ID）。              |
| 分配关系 ID (ASSIGNMENT_ID) | 暂无对应               |                                                              |
| 权限 (PRIMARY_FLAG)         | `role`                 | 不确定是不是这个                                             |
| 职位 ID (JOB_ID)            | 暂无对应               | 可根据需要扩展到 `User` 接口中。                             |

| 组织短号 | 手机号 | 序号 | 职级(job_level) | 姓名 | 员工编号 | 组织路径 | 职务(pos_name) | 职位名称(jobname) | 组织编码(org_id) | 组织长号(MIS_ID_4A) | 分配关系ID(ASSIGNMENT_ID) | 权限(PRIMARY_FLAG) | 职位ID(JOB_ID) |
| -------- | ------ | ---- | --------------- | ---- | -------- | -------- | -------------- | ----------------- | ---------------- | ------------------- | ------------------------- | ------------------ | -------------- |
|          |        |      |                 |      |          |          |                |                   |                  |                     |                           |                    |                |

role不填，人员类型全部内部，移动办公小号不填。sort排序号用默认值1。org，组织id用组织长号。uid能不能用用户名的拼音或者员工编号替代?

中间表结构，uid，

中间表操作(这里删除的是buildin类型的数据，是内建的):

use ecis_xxxx_db

delete from tb_las_user where uid = 'chenglong';

delete from tb_las_department_user where uid = 'chenglong';

delete from tb_las_department WHERE name = '神头网格';
delete from tb_las_department  WHERE name = '耿镇网格';

delete from  tb_las_department   WHERE name = '台怀镇网格';

云文档表操作:(这里删除的是我们之前人为导入的存量数据，如果生产环境中没有，可以不删除。在有大量存量数据情况下，如果不进行这一步，云文档会有一大堆操作需要人工进行，属于云文档插件问题，后续该插件会进行修复)

use wps_yundoc_mini;

delete from tb_dept where source = 'sync';

delete from tb_account where source = 'sync';

云文档插件的bug，需要进行下面的sql

// 在ecis_xxxx_ecisaccountsync_self库下执行以下sql
ALTER TABLE tb_lock MODIFY COLUMN `lock_key` varchar(100) NOT NULL COMMENT '锁唯一键';







```
//部门推送同步
router.post('/api/v1/gatherDepartmentSync', las.handleDepartmentGather);


export async function handleDepartmentGather(ctx: Context) {
    ctx.body = { code: 200 }
    try {
        let cfgs = await gather.companies()
        const taskId = dayjs(new Date()).format('YYYYMMDDHHmmss')
        // 多租户
        // 多租户支持：遍历每个公司配置，执行同步
        for (const cfg of cfgs) {
            console.log("============>同步数据到中间表开始");
            // const now = new Date()
            // 调用同步部门数据的方法
            await gather.startDepartment(taskId, cfg, ctx);
            console.log("============>同步数据到中间表结束");
            // const now2 = new Date()
            // //采集消耗的秒数
            // const consumeTime = (now2.getTime() - now.getTime()) / 1000
            // console.log("=======>gather.end, taskID:", taskId, "consumeTime:", consumeTime)
            // await gather.wps4Sign(taskId, cfg.thirdCompanyId, consumeTime)
        }

        // 返回成功响应
        ctx.type = 'application/xml';
        ctx.body = `<SendAllOrganizationResponse>
        <Ret>
        <code>true</code>
        <message>操作成功</message>
        </Ret>
      </SendAllOrganizationResponse>`;

    } catch (e) {
        console.log("============>同步数据到中间表错误:", e);

        // 返回失败的响应
        ctx.type = 'application/xml';
        ctx.body = `<SendAllOrganizationResponse>
        <Ret>
        <code>false</code>
        <message>操作失败, ${e.message}</message>
        </Ret>
      </SendAllOrganizationResponse>`;
    } finally {
        console.log("============>同步数据到中间表结束");
    }

    return ctx;  // 返回处理结果
}



```



```

```



# 12. 安全扫描

```
1056703140@qq.com
密码：yangjunbo123
```





![image-20241206160429884](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206160429884.png)



![image-20241206160525041](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206160525041.png)



![image-20241206161151134](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206161151134.png)









###### git 切换账户

```
删除windows凭证
```

![image-20241206164840870](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206164840870.png)





![image-20241206163246375](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206163246375.png)









![image-20241206163141473](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206163141473.png)

![image-20241206163953614](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206163953614.png)



修改代码，commit and push



![image-20241206164508110](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206164508110.png)

要求全部组件<=中危





![image-20241206165417454](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206165417454.png)



![image-20241206171628692](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206171628692.png)

最高版本+0.01

要求全部组件都发布



![image-20241206174846292](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206174846292.png)





![image-20241206172954420](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241206172954420.png)

最高版本+0.01

![image-20241210194452058](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241210194452058.png)





```text
171 shanddongsync是Java写的，构建成功且安全扫描通过
172 全是静态包
173 名字带测试，不用搞
174 缺单点代码，缺账户对接代码，构建成功且安全扫描通过
175 无代码仓库
176 无代码仓库
177 无代码仓库
178 构建成功且安全扫描通过
179 无代码仓库
180 构建成功且安全扫描通过
---------------------------------
181 sso是Java写的，构建成功且安全扫描通过

182 空仓库

183，184，185 名字带测试，不用搞

186 ccbuass是Java写的,ecc找不到代码，构建成功且安全扫描通过
187 空仓库
188 构建成功且安全扫描通过
189 beijingsync是Java写的，构建成功且安全扫描通过
190 构建成功且安全扫描通过


--------------------------




```

```
克隆最新分支

git clone -b pro_security --single-branch --depth 1 https://kgit.wpsit.cn/code/202311/shandongdianli.git

```

```
{
  "appId": "bydsso",
  "id": "bydapiservice",
  "name": "比亚迪api",
  "version": "VERSION",
  "type": "service",
  "source": "file://IMAGEFILE",
  "internal": false
}


{
  "appId": "bydsso",
  "id": "ssologin",
  "name": "比亚迪账号同步",
  "version": "VERSION",
  "type": "service",
  "source": "file://IMAGEFILE",
  "internal": false
}

```



# 13. 云文档单点登录



云文档单点登录

文档由 冯史瑶 于 2024-09-11 17:16 创建，由 冯史瑶 于 2024-09-11 17:16 更新



文档标签：

无

文档分类：

无

赞 0

踩 0



210

一、云文档的授权登录流程

云文档的授权登录对接只支持标准的Oauth授权码协议，流程如下：

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/download)

上述流程主要涉及1.获取授权code、2.获取令牌token、3.获取用户信息三个接口，关于这三个接口，可参考：[Oauth授权码协议介绍](https://p.wpseco.cn/wiki/doc/62593124cbe4a916499d565b)

如果客户的认证方式不是标准的Oauth协议，则需要开发对应的适配服务，由这个适配服务提供标准的Oauth授权协议的三个接口。

 

二、云文档的单点登录配置

通过云文档系统后台配置

系统后台地址：https://${host}/micsweb/sys/contact-sync

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/download)

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/download)

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/download)

 

通过i网关接口配置

1. 创建单点登录配置

接口文档：https://p.wpseco.cn/wiki/doc/662b148e0582ede3d238fd19#heading-11

1. 修改单点登录配置

接口文档：https://p.wpseco.cn/wiki/doc/662b148e0582ede3d238fd19#heading-23

1. 查询单点登录配置

接口文档：https://p.wpseco.cn/wiki/doc/662b148e0582ede3d238fd19#heading-19

1. 删除单点登录配置

接口文档：https://p.wpseco.cn/wiki/doc/662b148e0582ede3d238fd19#heading-15

 

三、常见问题与注意事项

单点登录获取授权令牌接口和获取用户信息接口的返回结构

```plaintext
// 获取授权令牌接口返回示例
{
    "code": "", // 业务字段，随意
    "msg": "",  // 业务字段，随意
    "access_token": "xxxx",//授权令牌字段名根据实际配置来
}

// 获取用户信息接口返回示例
{
    "code": "", // 业务字段，随意
    "msg": "",  // 业务字段，随意
    "union_id": "xxxx",//用户标识字段名根据实际配置来
}
```

csrf错误

一般是跨域引起的错误，需要在应用管理平台上配置跨域白名单

地址：https://${host}/c/manage/#/system/whiteList

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/download)

 

账号不存在





一般为单点登录第三步获取用户信息接口返回的用户标识union_id的值在云文档中未找到对应的用户（基于用户三方ID查找）

```
在页面中进行配置
/c/zhongyisso/api/v1/authorization/redirectToWps
http://encs-pri-cams-engine/c/zhongyisso/api/v1/authorization/callback
http://encs-pri-cams-engine/c/zhongyisso/api/v1/authorization/userInfo
接收code,access_token,union_id


```



# 14. 绿发二期



```
https://shangyun03.wpseco.cn/ksops/#/login
wpsadmin/Wps@123456
```





![image-20241212105502534](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20241212105502534.png)



```
curl -X POST "https://shangyun03.wpseco.cn/c/iflyservice/api/v1/app/add" \
-H "Content-Type: application/json" \
-d '{
    "code": "APP12345",
    "third_ak": "exampleThirdAk",
    "third_sk": "exampleThirdSk",
    "company_id": 1,
    "user_id": 1,
    "name": "LP-TEST",
    "description": "测试应用",
    "logo": "DD1AFB29d29hL2Rldi9pbWFnZS80ZmQ1MGE0ZjJiYzZjY2FmM2YwZjVkNGM1ZWEwZTk4YTp3cHMzOndvYTow",
    "pc_home_page": "",
    "pc_open_mode": 0,
    "mobile_home_page": "",
    "extra_info": "{\"key\": \"value\"}"
}'

```

```
ALTER TABLE tb_app_info
ADD COLUMN appid VARCHAR(64) NOT NULL COMMENT '应用 ID',
ADD COLUMN logo_store_key TEXT COMMENT 'Logo 存储密钥';
```

# 15. 福建农信

项目基本信息

1. ***\*项目名称\****

福建农信账号对接



1. ***\*涉及产品\****

2. - 云文档
   - 金山协作
   - WPS office端



1. ***\*项目类型\****

2. - 全新项目
   - 升级项目



1. ***\*存量数据情况\****

2. - 无存量数据
   - 有存量数据，无需保留
   - 有存量数据，为自建账号，用户部门需保留
   - 有存量数据，为同步账号，用户部门需保留（迁移过来的）



1. ***\*用户数量\****

20000



1. ***\*部门数量\****

XXX

***\*环境信息\****

1. ***\*目标版本（精确到小版本）\****

云文档：release_dc_xc_arm_x86_v7.0.2411a.20241114



1. ***\*平台架构\****

2. - X86   
   - ARM



1. ***\*数据库\****
2. 自带MySQL
3. 其他数据库  人大金仓数据库



1. ***\*是否可以远程联调\****
2. 可远程
3. 不能远程



1. ***\*需对接环境情况\****

一套测试环境：用于业务功能测试和功能验证

一套生产环境：用于办公环境投产使用

***\*需求描述\****

***\*账号同步需求\****

1. 1. ***\*账号同步数据源\****

***\*数据源：\****福建农信统一运营门户系统

***\*数据获取方式：\****

通过接口获取全量用户、机构的文件信息，然后通过文件信息下载全量用户、组织文件。通过全量的用户、组织文件完成同步。



1. 1. ***\*数据规范\****

***\*数据同名情况\****

- - - 部门下存在同名子部门
    - 部门下存在同名用户（可能存在相同中文名的情况，但登录名是唯一的）
    - 用户账号存在重复情况

注：

云文档不支持同级下存在重名的部门，如有该情况数据源需要修改；

云文档不支持用户账号重复，如有该情况数据源需要修改；



***\*数据格式规范\****

- - - 部门名包含特殊字符（特殊字符  \/;:*?"<>|%&）
    - 用户账号不符合规范（只允许输入字母、数字、点、减号、下划线、@符号，且以字母或者数字开头）

注：部分特殊字符云文档可能不支持，如果有特殊字符需要提前处理



***\*一人多部门情况\****

- - - 用户有一人存在多部门情况
    - 有主部门标识

注：云文档支持一人多部门情况，如果对现实部门信息有特殊要求需要提供主部门信息

 

1. 1. ***\*同步方式\****
   2. 云文档通过接口获取全量用户、机构的文件信息，然后通过文件信息下载全量用户、组织文件。通过全量的用户、组织文件完成同步



1. 1. ***\*同步规则\****

orgparent为空的部门是顶级部门

之前接口下载的文件中有的数据，后来没有了，就是删除。

删除用户禁用处理。



1. 1. ***\*用户信息接口资料\****

接口文档：https://365.kdocs.cn/l/cpJbdSOFT0ne?openfrom=docs

组织、用户信息同步接口：

(3.4、3.5、3.6、3.7 其实是要开发4个接口+2个文件)

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/QWJZOEY5ABQBE)



***\*用户同步所需字段对应关系\****

| ***\*云文档字段\**** | ***\*是否必须\**** | ***\*数据源字段\**** | ***\*补充说明\****                     |
| -------------------- | ------------------ | -------------------- | -------------------------------------- |
| ***\*账号\****       | 是                 | loginName            |                                        |
| ***\*姓名\****       | 是                 | userName             |                                        |
| ***\*所在部门ID\**** | 是                 | depCode              |                                        |
| ***\*三方ID\****     | 是                 | userId               |                                        |
| ***\*用户状态\****   | 是                 | userStatus           | 用户状态0禁用  1启用已改密 2启用未改密 |
| ***\*用户排序\****   | 否                 | /                    |                                        |
| ***\*手机号\****     | 否                 | /                    | /                                      |
| ***\*邮箱\****       | 否                 | /                    | /                                      |
| ***\*职务\****       | 否                 | /                    | /                                      |
| ***\*直属领导\****   | 否                 | /                    | /                                      |



1. 1. ***\*部门信息接口资料\****

***\*部门同步所需字段对应关系\****

| ***\*云文档字段\**** | ***\*是否必须\**** | ***\*数据源字段\**** | ***\*补充说明\****                     |
| -------------------- | ------------------ | -------------------- | -------------------------------------- |
| ***\*部门名称\****   | 是                 | orgName              |                                        |
| ***\*部门ID\****     | 是                 | orgCode              |                                        |
| ***\*父部门ID\****   | 是                 | orgParent            |                                        |
| ***\*部门状态\****   | 是                 | /                    | /                                      |
| ***\*部门排序\****   | 否                 | orgOrder             | 升序  但很多机构没值，不是必选字段降序 |



***\*单点登录需求\****

1. 1. ***\*登录场景\****

2. 浏览器直接访问web云文档登录

3. 浏览器直接访问H5云文档登录

4. WPSoffice登录

5. 金山协作登录

6. IM工作台跳转

7. IM聊天会话免登录打开云文档

8. 其他业务系统免登录跳转



场景1：PC端，浏览器访问web云文档，通过定制界面调用客户登录接口登录

场景2：PC端，用户打开金山协作，通过定制界面调用客户登录接口登录

场景3：PC端，用户打开WPS，通过定制界面调用客户登录接口登录

场景4：PC端，用户点击统一运营门户上云文档应用图标，免密进入云文档界面

场景5：移动端，用户点击统一运营APP上云文档应用图标，免密进入云文档界面





1. 1. ***\*单点认证对接方式\****

接口

1. ***\**\*非标定制：\*\**\***

场景1、2、3属于同一定制，前端定制界面并调用客户登录接口，对于账号登录失败场景，我们提示语统一为“用户名或密码错误”。

前端定制可以参考长虹电子代码，UI如下：

只留账号、密码输入框，福建农信居中显示在登录框上方。

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/FBR4KEY5ABAHE)

客户接口参考文档https://365.kdocs.cn/l/cpJbdSOFT0ne?openfrom=docs中的3.13 用户登录接口

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/BT6MQEY5ADAEA)

注：登录信息有加密处理



1. ***\**\*免密登录\*\**\***

场景4、5 免密登录参考客户资料中的 3.16 章节，客户系统会传token给云文档

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/N7T42EY5ADAEK)





1. 1. ***\*登出场景\****

浏览器上进入云文档，点击退出时，需要能退出到登录界面。

（客户未提供登出接口，点击退出时，跳转到定制的登录界面，让用户再次输入用户名密码登录）



**需求提取及表格整理**

------

**项目基本信息**

| 项目属性             | 内容                                                     |
| -------------------- | -------------------------------------------------------- |
| **项目名称**         | 福建农信账号对接                                         |
| **涉及产品**         | 云文档、金山协作、WPS Office端                           |
| **项目类型**         | 全新项目、升级项目                                       |
| **存量数据情况**     | 有存量数据，为同步账号，用户部门需保留（迁移过来的）     |
| **用户数量**         | 20000                                                    |
| **部门数量**         | XXX                                                      |
| **目标版本**         | 云文档：release_dc_xc_arm_x86_v7.0.2411a.20241114        |
| **平台架构**         | X86、ARM                                                 |
| **数据库**           | 自带 MySQL、人大金仓数据库                               |
| **是否可以远程联调** | 可远程                                                   |
| **对接环境情况**     | 测试环境：业务功能测试和验证；生产环境：办公环境投产使用 |

------

**需求描述**

**账号同步需求**

**1. 数据源信息：**

| 数据属性         | 内容                                                   |
| ---------------- | ------------------------------------------------------ |
| **数据源**       | 福建农信统一运营门户系统                               |
| **数据获取方式** | 接口获取全量用户、机构文件信息；通过文件信息完成同步。 |

------

**2. 数据规范：**

| 规范点             | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| **数据同名情况**   | 1. 部门下存在同名子部门。2. 部门下存在同名用户（登录名唯一）。3. 用户账号重复情况。 |
| **数据格式规范**   | 1. 部门名包含特殊字符：`/;:*?"<>                             |
| **一人多部门情况** | 支持一人多部门，有主部门标识。                               |

------

**3. 同步方式：**

- **同步过程**：
   云文档通过接口获取全量用户、机构的文件信息，完成数据同步。
- **同步规则**：
  1. `orgparent` 为空的部门是顶级部门。
  2. 之前接口下载的数据，如果后续不存在，则视为删除。
  3. 删除用户时，处理为**禁用**。

------

**用户同步字段对应关系**

| **云文档字段** | **是否必须** | **数据源字段** | **补充说明**                   |
| -------------- | ------------ | -------------- | ------------------------------ |
| **账号**       | 是           | `loginName`    |                                |
| **姓名**       | 是           | `userName`     |                                |
| **所在部门ID** | 是           | `depCode`      |                                |
| **三方ID**     | 是           | `userId`       |                                |
| **用户状态**   | 是           | `userStatus`   | 0 禁用，1 启用已改密，2 未改密 |
| **用户排序**   | 否           | /              |                                |
| **手机号**     | 否           | /              |                                |
| **邮箱**       | 否           | /              |                                |
| **职务**       | 否           | /              |                                |
| **直属领导**   | 否           | /              |                                |

------

**部门同步字段对应关系**

| **云文档字段** | **是否必须** | **数据源字段** | **补充说明**         |
| -------------- | ------------ | -------------- | -------------------- |
| **部门名称**   | 是           | `orgName`      |                      |
| **部门ID**     | 是           | `orgCode`      |                      |
| **父部门ID**   | 是           | `orgParent`    |                      |
| **部门状态**   | 是           | /              |                      |
| **部门排序**   | 否           | `orgOrder`     | 升序，部分数据可为空 |

------

**单点登录需求**

**登录场景**

1. **PC端**：
   - 浏览器访问 Web 云文档。
   - 访问金山协作。
   - 访问 WPS Office。
   - 统一运营门户应用图标点击免密登录。
2. **移动端**：
   - 统一运营 APP 应用图标点击免密登录。
3. **IM 集成**：
   - IM 工作台跳转。
   - IM 聊天会话免登录打开云文档。

------

**单点认证对接方式**

1. **接口对接**：

   - 登录失败提示语统一为：`用户名或密码错误`。

   - 前端定制

     ：界面包含账号、密码输入框，福建农信居中显示。

     - 参考文档：https://365.kdocs.cn/l/cpJbdSOFT0ne
     - 用户登录接口：3.13章节。

   - 登录信息有**加密处理**。

2. **免密登录**：

   - 客户系统传 `token` 至云文档。
   - 参考文档：3.16章节。

------

**登出场景**

- 浏览器上点击退出，跳转到定制的登录界面，要求用户重新输入用户名和密码。
- 注：客户未提供登出接口。

------

**总结表格：核心需求提取**

| **模块**         | **需求内容**                                                 |
| ---------------- | ------------------------------------------------------------ |
| **项目基本信息** | 项目名称：福建农信账号对接用户数：20000目标版本：release_dc_xc_arm_x86_v7.0.2411a.20241114 |
| **数据同步**     | 数据源：福建农信统一运营门户系统数据规范：特殊字符、同名账号处理同步规则：全量同步与删除逻辑 |
| **单点登录**     | 登录场景：PC端、移动端、IM集成认证对接：接口对接，支持免密登录登出：无登出接口，返回登录界面 |
| **字段映射**     | 用户字段与部门字段的对应关系详细列出                         |

------

**谷歌标准下的建议**

1. **分层设计**：将账号同步、单点登录等功能分配到不同的服务层（例如领域层、应用层、基础设施层）。
2. **安全合规**：确保登录信息和接口传输中的敏感数据（如`token`）经过加密处理。
3. **高可用设计**：为同步和登录功能引入重试机制，防止外部接口故障导致数据同步中断。
4. **日志与监控**：增加日志记录和监控功能，便于后续问题排查和性能优化。

------

以上内容提取了您的需求并整理为结构化表格，同时给出了符合谷歌标准的架构建议。如有补充，请继续提供！





云文档通过接口获取全量用户、机构的文件信息，然后通过文件信息下载全量用户、组织文件。通过全量的用户、组织文件完成同步

orgparent为空的部门是顶级部门
之前接口下载的文件中有的数据，后来没有了，就是删除。
删除用户禁用处理。

用户信息全量同步接口
接口名称	http://ip:port/sp-app-batch/api/syncout/userdirect

传参: 

| **参数名** | **类型(长度)** | **含义** | **是否必须** | **备注**                                                     |
| ---------- | -------------- | -------- | ------------ | ------------------------------------------------------------ |
| systemCode | String(32)     | 系统编号 | 是           |                                                              |
| orgCode    | String(9)      | 机构编号 | 否           | 当传空或ALL,会同步所有用户，当传机构编号时，同步该机构及下级机构的所有用户 |

返回:

| **参数名** | **类型(长度)** | **含义** | **是否必须** |
| ---------- | -------------- | -------- | ------------ |
| fileName   | String(40)     | 文件名   | 是           |
| fileId     | String(40)     | 文件ID   | 是           |

用户信息全量文件下载接口

  http://ip:port/yusp-file/api/file/download?systemCode=xxxx  

传参:

| **参数名**  | **类型(长度)** | **含义** | **是否必须** | **备注**                           |
| ----------- | -------------- | -------- | ------------ | ---------------------------------- |
| fileName    | String(40)     | 文件名   | 是           |                                    |
| fileId      | String(40)     | 文件ID   | 是           |                                    |
| storageType | String(10)     | 存储类型 | 是           | 区分ESB文件同步标志，送“localdisk” |
| systemCode  | String(32)     | 系统编号 | 是           | systemCode                         |

 返回:

| **参数名**       | **类型(长度)** | **含义**          | **是否必须** |
| ---------------- | -------------- | ----------------- | ------------ |
| <findAllPubUser> |                | 根标签            | 是           |
| <PubUser>        |                |                   |              |
| userId           | String（36）   | 人员ID            | 是           |
| userName         | String(40)     | 人员名称          | 是           |
| loginName        | String(40)     | 登录名            | 是           |
| userStatus       | String(1)      | 用户状态          | 是           |
| mobile           | String(11)     | 手机号码          | 否           |
| mail             | String(50)     | 邮件系统帐号      | 否           |
| certType         | String(10)     | 证件类型          | 是           |
| idCard           | String(18)     | 证件号码          | 是           |
| sex              | String(1)      | 性别              | 是           |
| address          | String(120)    | 联系地址          | 否           |
| depCode          | String(9)      | 所属部门\机构代码 | 是           |
| corporCode       | String(9)      | 法人机构号        | 是           |
| teller           | String(30)     | 柜员号            | 是           |
| updateTime       | String(8)      | 更新时间          | 是           |
| </PubUser>       |                |                   |              |
| <findAllPubUser> |                | 根标签            |              |

机构信息全量同步接口

http://ip:port/sp-app-batch/api/syncout/orgdirect  

传参

| **参数名** | **类型(长度)** | **含义** | **是否必须** | **备注**                                                     |
| ---------- | -------------- | -------- | ------------ | ------------------------------------------------------------ |
| orgCode    | String(9)      | 机构代码 | 是           | 如果为"ALL"则查询所有机构，其他值则查询该机构下的所有机构(900000000  省联社 800000000 村镇 700000000 公司方) |
| systemCode | String(32)     | 系统编号 | 是           |                                                              |

 返回值

| **参数名** | **类型(长度)** | **含义** | **是否必须** |
| ---------- | -------------- | -------- | ------------ |
| fileName   | String(40)     | 文件名称 | 是           |
| fileId     | String(40)     | 文件ID   | 是           |

机构信息全量文件下载

 http://ip:port/yusp-file/api/file/download  

输入

| **参数名**  | **类型(长度)** | **含义** | **是否必须** | **备注**      |
| ----------- | -------------- | -------- | ------------ | ------------- |
| fileName    | String(40)     | 文件名   | 是           |               |
| fileId      | String(40)     | 文件ID   | 是           |               |
| storageType | String(10)     | 存储类型 | 是           | 送“localdisk” |
| systemCode  | String(32)     | 系统编号 | 是           | systemCode    |

 

返回

文件内容

| **参数名**       | **类型(长度)** | **含义**              | **是否必须** | **说明**                                       |
| ---------------- | -------------- | --------------------- | ------------ | ---------------------------------------------- |
| <findAllPubOrg>  |                | 根标签                |              |                                                |
| <PubOrg>         |                |                       |              |                                                |
| orgName          | String(120)    | 部门机构名称          | 是           |                                                |
| shortName        | String(120)    | 机构简称              | 是           |                                                |
| orgCode          | String(9)      | 部门机构代码          | 是           |                                                |
| orgType          | String(4)      | 组织机构类型          | 是           | 0001部门  0002机构  0003村镇银行  0010自定义   |
| orgParent        | String(9)      | 上级机构代码          | 否           |                                                |
| orgPhone         | String(20)     | 部门机构电话          | 否           |                                                |
| orgAddress       | String(120)    | 部门机构地址          | 否           |                                                |
| remark           | String(50)     | 备注说明              | 否           |                                                |
| orgOrder         | String(5)      | 排序号                | 否           |                                                |
| usableFlag       | String(1)      | 是否可用              | 是           | 0禁用  1可用  2撤并                            |
| orgPath          | String(600)    | 组织机构路径          | 是           |                                                |
| corporCode       | String(9)      | 所属法人机构号        | 是           |                                                |
| belongCode       | String(9)      | 承接机构              | 否           |                                                |
| belongDate       | String(8)      | 承接日期              | 否           |                                                |
| updateTime       | String(14)     | 更新时间              | 是           |                                                |
| bankType         | String(3)      | 银行类别（码值）      | 否           | 1农信银行  2农信社  3省联社  4其他  5村镇银行  |
| majorBank        | String(15)     | 主发银行              | 否           |                                                |
| orgNature        | String(5)      | 机构性质              | 否           | 1营业网点  2非营业网点  3虚拟网点 ( 总账汇总 ) |
| principalType    | String(5)      | 负责人类型            | 否           | 1法定代表人  2负责人                           |
| orgChief         | String(36)     | 负责人编号            | 否           |                                                |
| principalDuty    | String(200)    | 负责人岗位            | 否           |                                                |
| orgLevel         | String(5)      | 机构层级              | 否           | 0未定级  1一级  2二级  3三级  4 四级           |
| socialTrustCode  | String(18)     | 社会信用编号          | 否           |                                                |
| belongIns        | String(255)    | 归属管理机构/管辖机构 | 否           |                                                |
| branchId         | String(10)     | 网点号                | 否           |                                                |
| </PubOrg>        |                |                       |              |                                                |
| </findAllPubOrg> |                | 根标签                |              |                                                |

需要插入的表的表结构

-- Table structure for tb_las_department

-- ----------------------------
DROP TABLE IF EXISTS `tb_las_department`;
CREATE TABLE `tb_las_department`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键索引',
  `did` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '部门id',
  `task_id` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '任务id',
  `third_company_id` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '租户id',
  `platform_id` varchar(60) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '平台id, 用来区分多种数据源，platform_id + did 唯一, 根部门例外',
  `pid` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '父部门id',
  `name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '部门名称',
  `order` int(0) NULL DEFAULT NULL COMMENT '排序',
  `source` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT 'sync' COMMENT '来源, buildin/sync',
  `ctime` timestamp(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  `mtime` timestamp(0) NOT NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0) COMMENT '修改时间',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `uk_task_did`(`did`, `task_id`, `third_company_id`, `platform_id`) USING BTREE COMMENT 'did唯一索引',
  INDEX `idx_pid`(`pid`, `task_id`, `third_company_id`, `platform_id`) USING BTREE COMMENT '父部门id索引'
) ENGINE = InnoDB AUTO_INCREMENT = 14 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci COMMENT = '三方部门表' ROW_FORMAT = Dynamic;

获取到数据源的json结构

| **参数名**       | **类型(长度)** | **含义**              | **是否必须** | **说明**                                       |
| ---------------- | -------------- | --------------------- | ------------ | ---------------------------------------------- |
| <findAllPubOrg>  |                | 根标签                |              |                                                |
| <PubOrg>         |                |                       |              |                                                |
| orgName          | String(120)    | 部门机构名称          | 是           |                                                |
| shortName        | String(120)    | 机构简称              | 是           |                                                |
| orgCode          | String(9)      | 部门机构代码          | 是           |                                                |
| orgType          | String(4)      | 组织机构类型          | 是           | 0001部门  0002机构  0003村镇银行  0010自定义   |
| orgParent        | String(9)      | 上级机构代码          | 否           |                                                |
| orgPhone         | String(20)     | 部门机构电话          | 否           |                                                |
| orgAddress       | String(120)    | 部门机构地址          | 否           |                                                |
| remark           | String(50)     | 备注说明              | 否           |                                                |
| orgOrder         | String(5)      | 排序号                | 否           |                                                |
| usableFlag       | String(1)      | 是否可用              | 是           | 0禁用  1可用  2撤并                            |
| orgPath          | String(600)    | 组织机构路径          | 是           |                                                |
| corporCode       | String(9)      | 所属法人机构号        | 是           |                                                |
| belongCode       | String(9)      | 承接机构              | 否           |                                                |
| belongDate       | String(8)      | 承接日期              | 否           |                                                |
| updateTime       | String(14)     | 更新时间              | 是           |                                                |
| bankType         | String(3)      | 银行类别（码值）      | 否           | 1农信银行  2农信社  3省联社  4其他  5村镇银行  |
| majorBank        | String(15)     | 主发银行              | 否           |                                                |
| orgNature        | String(5)      | 机构性质              | 否           | 1营业网点  2非营业网点  3虚拟网点 ( 总账汇总 ) |
| principalType    | String(5)      | 负责人类型            | 否           | 1法定代表人  2负责人                           |
| orgChief         | String(36)     | 负责人编号            | 否           |                                                |
| principalDuty    | String(200)    | 负责人岗位            | 否           |                                                |
| orgLevel         | String(5)      | 机构层级              | 否           | 0未定级  1一级  2二级  3三级  4 四级           |
| socialTrustCode  | String(18)     | 社会信用编号          | 否           |                                                |
| belongIns        | String(255)    | 归属管理机构/管辖机构 | 否           |                                                |
| branchId         | String(10)     | 网点号                | 否           |                                                |
| </PubOrg>        |                |                       |              |                                                |
| </findAllPubOrg> |                | 根标签                |              |                                                |

 

需要插入的表的表结构

-- Table structure for tb_las_user
-- ----------------------------
DROP TABLE IF EXISTS `tb_las_user`;
CREATE TABLE `tb_las_user`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键索引',
  `task_id` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '任务id',
  `third_company_id` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '租户id',
  `platform_id` varchar(60) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '平台id, 用来区分多种数据源，platform_id + uid 唯一',
  `uid` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '用户id',
  `def_did` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '默认部门',
  `def_did_order` int(0) NULL DEFAULT 0 COMMENT '用户在默认部门下的排序',
  `account` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '登录名，对应account',
  `nick_name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '用户昵称，对应nick_name',
  `password` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '密码',
  `avatar` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '头像',
  `email` varchar(80) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '邮箱',
  `gender` varchar(60) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '用户性别',
  `title` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '职称',
  `work_place` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '办公地点',
  `leader` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '上级主管ID',
  `employer` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '员工工号',
  `employment_status` varchar(60) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT 'notactive' COMMENT '就职状态[active, notactive, disabled]',
  `employment_type` varchar(60) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '就职类型[permanent, intern]',
  `phone` varchar(60) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '手机号',
  `telephone` varchar(60) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '座机号',
  `source` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT 'sync' COMMENT '来源, buildin/sync',
  `custom_fields` varchar(5000) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '自定义字段，json数组',
  `ctime` timestamp(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  `mtime` timestamp(0) NOT NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0) COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `uk_task_company_name`(`account`, `task_id`, `third_company_id`) USING BTREE COMMENT 'name唯一索引',
  UNIQUE INDEX `uk_task_uid`(`uid`, `task_id`, `platform_id`) USING BTREE COMMENT 'uid唯一索引',
  INDEX `idx_task_company`(`task_id`, `third_company_id`) USING BTREE COMMENT 'task索引'
) ENGINE = InnoDB AUTO_INCREMENT = 17 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci COMMENT = '三方用户表' ROW_FORMAT = Dynamic;









获取到数据源的json结构

| **参数名**       | **类型(长度)** | **含义**          | **是否必须** |
| ---------------- | -------------- | ----------------- | ------------ |
| <findAllPubUser> |                | 根标签            | 是           |
| <PubUser>        |                |                   |              |
| userId           | String（36）   | 人员ID            | 是           |
| userName         | String(40)     | 人员名称          | 是           |
| loginName        | String(40)     | 登录名            | 是           |
| userStatus       | String(1)      | 用户状态          | 是           |
| mobile           | String(11)     | 手机号码          | 否           |
| mail             | String(50)     | 邮件系统帐号      | 否           |
| certType         | String(10)     | 证件类型          | 是           |
| idCard           | String(18)     | 证件号码          | 是           |
| sex              | String(1)      | 性别              | 是           |
| address          | String(120)    | 联系地址          | 否           |
| depCode          | String(9)      | 所属部门\机构代码 | 是           |
| corporCode       | String(9)      | 法人机构号        | 是           |
| teller           | String(30)     | 柜员号            | 是           |
| updateTime       | String(8)      | 更新时间          | 是           |
| </PubUser>       |                |                   |              |
| <findAllPubUser> |                | 根标签            |              |





需要插入的表

-- ----------------------------
DROP TABLE IF EXISTS `tb_las_department_user`;
CREATE TABLE `tb_las_department_user`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键索引',
  `task_id` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '任务id',
  `third_company_id` varchar(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '租户id',
  `platform_id` varchar(60) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '平台id, 用来区分多种数据源，platform_id + id 唯一',
  `uid` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '用户id',
  `did` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '部门id',
  `order` int(0) NULL DEFAULT NULL COMMENT '用户在部门下的排序',
  `main` int(0) NULL DEFAULT 0 COMMENT '是否是主部门，1：是 0：不是',
  `ctime` timestamp(0) NOT NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `uid`(`uid`, `did`, `task_id`, `third_company_id`, `platform_id`) USING BTREE COMMENT '唯一索引',
  INDEX `idx_did`(`did`, `task_id`) USING BTREE COMMENT '部门id索引',
  INDEX `idx_uid`(`uid`, `task_id`) USING BTREE COMMENT 'uid索引'
) ENGINE = InnoDB AUTO_INCREMENT = 45 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci COMMENT = '三方部门用户关系表' ROW_FORMAT = Dynamic;



单点需求:

后端需要负责的部分1，2，3就是相应的登录页面对应的接口，

这里需要





4，5就是相应单点免密登录对应的接口





单点需求分析:**场景需求分析**

场景 1-3：

- **PC端，浏览器、金山协作、WPS 等通过定制界面调用客户登录接口完成登录。**
- 流程：
  1. 用户通过定制界面（如浏览器）点击登录按钮。
  2. 系统引导用户跳转到客户身份认证服务。
  3. 用户完成登录并授权，系统生成授权码返回给云文档。
  4. 云文档根据授权码获取访问令牌，然后获取用户信息。

场景 4-5：

- **统一门户或 APP 点击应用图标，直接免密进入云文档。**
- 流程：
  1. 用户已登录统一门户或 APP（用户身份已认证）。
  2. 系统直接生成访问令牌（无需授权码）并跳转到云文档页面。
  3. 用户信息由统一门户或 APP 提供，云文档验证后直接登录。



这里传的是用户唯一id，用来获取对应的token和union_id

```
指向云服务的 OAuth 登录接口
const redirectUri = `${config.host}/cloud/login/api/v1/oauth/code/login?auth_type=oauth&company_id=${config.companyId}&platform_id=${config.platformId}&code=${uid}&state=${state}`;

```



所以这里的云文档实际上是储存数据，和进行登录的一套存储系统，所谓的单点和数据同步，实际上是对用户和数据的一个迁移，单点实际上是在中间存储了一套相应对应的token和uid?

```text
你的理解是对的。在这种场景下，**单点登录（SSO）** 本身的意义更多体现在 **用户体验的提升** 和 **系统间的整合** 上，而并不是单纯地解决数据集成问题。实际上，在你描述的这种架构中，数据和认证本身就是集成在云文档系统内的，所以单点登录的意义可能并不像在跨平台或跨系统场景下那么突出。

### 为什么这里的单点登录意义有限？

1. **数据已经在云文档系统中统一管理**：
   - 云文档系统既存储了用户的认证信息（`uid`、`token`），又存储了用户的文档数据。这种架构下，用户的认证和数据是集中在一个系统内的。
   - 由于数据和认证不需要在多个系统中进行同步或对接，单点登录的作用实际上只是简化了认证的流程（比如引入了第三方登录方式）。

2. **单点登录的真正意义在跨系统场景**：
   - 单点登录（SSO）真正的价值是在跨系统场景中实现无缝的用户切换。
   - 比如，如果云文档需要与其他系统（如邮件系统、协作平台、CRM 系统等）整合，那么用户只需登录一次就可以访问所有系统，这时候单点登录的作用就非常明显。
   - 在你描述的架构中，云文档本身已经承担了存储和管理的角色，登录之后几乎所有操作都集中在云文档系统内完成，单点登录的意义相对较小。

3. **集成了第三方认证，但依然是单系统内部管理**：
   - 目前的流程更多是为了简化用户认证流程（例如使用微信、WPS 等第三方登录），而不是跨系统的用户认证共享。
   - 云文档系统通过第三方认证获取用户的基本信息（如 `unionid`），然后在本地生成 `uid` 和 `token`，完全独立地管理用户和数据。这种情况下，SSO 的作用仅仅是实现了第三方认证的接入，而不是跨系统的认证。

---

### 单点登录的实际价值在哪里？

单点登录的核心价值体现在以下几个方面：

#### 1. **跨系统认证共享**：
   - 如果云文档系统需要整合多个独立的系统，比如：
     - 一个协作办公平台（如 IM 聊天系统）。
     - 一个项目管理工具（如 Jira、Trello）。
     - 一个在线文档编辑工具（如 Google Docs、WPS）。
   - 在这种情况下，单点登录可以让用户在一个系统中登录后，无需重新认证即可访问其他系统。

#### 2. **提升用户体验**：
   - 单点登录可以简化用户的操作流程。例如，用户在云文档系统中登录后，可以无缝跳转到第三方系统完成相关操作（比如通过 WPS 编辑文档）。
   - 对用户来说，只需要记住一个登录凭据，减少了重复登录的麻烦。

#### 3. **安全性提升**：
   - 单点登录通常配合 OAuth2 或其他标准协议实现，将用户的认证集中在一个系统内，减少了在多个系统中管理凭据的风险。
   - 例如，云文档系统可以通过第三方认证平台（如 Google、WeChat、WPS）完成用户认证，而不用直接管理用户密码，从而降低了数据泄露的风险。

#### 4. **适配复杂架构**：
   - 在分布式系统架构中，单点登录可以通过集中管理用户身份，减少系统之间的复杂交互逻辑。
   - 例如，一个用户登录后，多个子系统可以通过共享的 `token` 或 `session` 获取用户信息，而不需要单独实现用户认证流程。

---

### 在你的场景中，为什么单点登录看起来意义不大？

根据你描述的架构，云文档系统本身已经承担了所有的认证和数据存储工作，因此并不存在跨系统认证和资源管理的问题：

1. **云文档系统集成了用户认证和数据存储**：
   - 认证和数据都在同一个系统内，无需在多个系统间同步用户状态或共享数据。
   - 单点登录的核心（跨系统认证共享）并没有真正发挥作用。

2. **第三方登录只是简化了用户认证流程**：
   - 当前的流程只是通过第三方登录（微信、WPS 等）获取用户身份，然后在云文档系统中生成本地的 `uid` 和 `token`。
   - 用户登录后的所有操作（文档管理、数据存储）都集中在云文档系统内完成，第三方认证的作用仅仅是取代了用户名/密码的认证方式。

---

### 单点登录在这种架构中的作用是什么？

虽然在单一系统内单点登录的作用有限，但它仍然有以下几个优势：

1. **统一接入多种第三方认证**：
   - 云文档系统可以集成多种认证方式（如微信、WPS、Google 等），用户可以根据自己的习惯选择登录方式，而不用维护多套认证体系。
   - 通过 OAuth2 或类似协议，可以简化认证接入流程，同时增强安全性。

2. **为未来的跨系统扩展做准备**：
   - 如果未来云文档系统需要与其他系统（如协作平台、CRM 系统）对接，现有的单点登录框架可以复用，减少重复开发的成本。
   - 例如，如果需要让用户从云文档系统直接跳转到协作平台完成任务，单点登录可以提供无缝的认证体验。

3. **简化用户登录体验**：
   - 用户只需要登录一次（使用第三方登录），即可访问所有云文档服务，无需重复输入用户名/密码。

---

### 总结

在你的架构中，单点登录的主要作用是 **统一用户认证**，简化了用户通过第三方登录进入云文档系统的流程。但由于数据和认证都已经集成在云文档系统内，单点登录的跨系统协作功能并没有发挥太大的作用。

当前的设计：
1. 实现了通过第三方（如微信、WPS）登录，获取用户的认证信息（`unionid`）。
2. 在云文档系统内生成并管理本地用户 ID（`uid`）和登录凭据（`token`）。
3. 数据存储和管理都在云文档系统内部完成，用户的登录状态仅限于云文档系统内。

单点登录的作用更多体现在 **用户体验** 和 **统一认证**，以及为未来可能的跨系统扩展提供基础。
```



```
是的，**Token 的验证由认证中心决定**。认证中心是整个单点登录（SSO）体系中的 **信任根（Trust Anchor）**，它负责用户的身份认证和 Token 的签发，同时提供 Token 的验证服务。只要认证中心验证 Token 有效，任何子系统都可以基于这个结果直接允许用户访问。

这种架构的核心在于：**所有子系统信任认证中心**，子系统本身只负责解析和验证 Token，而不需要独立实现用户身份认证。

---

## **1. Token 的验证是由认证中心决定的**

认证中心主要承担以下职责：

### **(1) Token 的签发**
- 当用户在认证中心完成登录后，认证中心会生成一个 Token（如 JWT 或 Access Token），这个 Token 是用户身份的凭证。
- Token 中通常包含：
  - **用户身份信息**（如 `uid`、用户名、角色等）。
  - **Token 的有效期**（过期时间 `exp`）。
  - **签名**（确保 Token 未被篡改）。

  示例 JWT：
  ```json
  {
    "sub": "123456", // 用户 ID
    "name": "John Doe", // 用户名
    "roles": ["admin", "user"], // 用户角色
    "iat": 1688123456, // 签发时间
    "exp": 1688127056  // 过期时间
  }
```

### **(2) Token 的验证**
认证中心负责验证 Token 的合法性，主要包括：
1. **签名验证**：检查 Token 是否由认证中心签发，是否被篡改。
2. **有效期检查**：检查 Token 是否过期。
3. **状态检查**（可选）：检查 Token 是否被吊销（例如用户主动退出登录的情况）。

验证通过后，认证中心可以返回用户信息或直接标记 Token 有效。

### **(3) 提供验证服务**
- 子系统通过调用认证中心的验证接口，确定 Token 是否有效。
- 认证中心会解析 Token，并返回验证结果（有效或无效），以及与 Token 关联的用户信息。

---

## **2. 子系统通过认证中心验证后直接允许访问**

子系统依赖认证中心的验证结果来判断是否允许用户访问系统：

### **(1) 子系统的验证逻辑**
子系统通常会从请求中提取 Token，并通过以下两种方式验证其合法性：

#### **1. 本地验证（如果 Token 是 JWT）**
- 如果 Token 是 JWT，子系统可以通过公共密钥（Public Key）验证 Token 的签名，并解析其中的用户信息。
- 优点：无需每次请求都调用认证中心，减少网络开销。
- 示例代码（Node.js + `jsonwebtoken`）：
  ```javascript
  const jwt = require('jsonwebtoken');

  const token = req.headers.authorization?.split(" ")[1]; // 从请求头获取 Token
  try {
    const decoded = jwt.verify(token, 'your-public-key'); // 验证签名
    console.log(decoded); // { sub: '123456', name: 'John Doe', roles: ['admin'] }
    // 如果验证通过，直接允许访问
    next();
  } catch (err) {
    console.error('Invalid token:', err);
    res.status(401).send('Unauthorized');
  }
  ```

#### **2. 调用认证中心的验证接口**
- 如果 Token 是短 Token（如随机字符串），子系统需要向认证中心发送请求，验证 Token 是否有效。
- 优点：适用于非自包含的 Token（如 OAuth2 Access Token），支持更多的动态验证场景（如吊销检查）。
- 示例代码：
  ```javascript
  const axios = require('axios');

  const token = req.headers.authorization?.split(" ")[1]; // 从请求头获取 Token
  try {
    const response = await axios.post('https://auth.example.com/validate', { token });
    if (response.data.valid) {
      console.log(response.data.user); // 用户信息
      next(); // 验证通过，允许访问
    } else {
      res.status(401).send('Unauthorized');
    }
  } catch (err) {
    console.error('Token validation failed:', err);
    res.status(500).send('Internal Server Error');
  }
  ```

---

### **(2) 子系统的直接访问流程**

当 Token 验证通过后，子系统可以直接允许用户访问，无需再重复登录。

#### **完整流程**
1. **用户访问子系统**
   - 用户携带 Token（例如通过 `Authorization` Header 或 Cookie）访问子系统。

2. **子系统验证 Token**
   - 子系统解析 Token（JWT），或者调用认证中心验证 Token。
   - 如果 Token 无效，子系统引导用户重新登录（跳转到认证中心）。

3. **获取用户信息**
   - 如果 Token 验证通过，子系统从 Token 中提取用户信息（如用户 ID、角色等）。

4. **执行业务逻辑**
   - 根据用户的角色或权限，允许用户访问相应的资源。

---

### **(3) 登录成功后直接访问其他系统**

SSO 的核心是 **Token 的共享**，登录成功后，其他子系统可以通过共享的 Token 实现直接访问。

#### **用户访问流程示意图：**
```plaintext
用户 -------> 认证中心 (登录，获取 Token)
         <------- 返回 Token
用户 -------> 子系统 A（携带 Token）
子系统 A -------> 验证 Token（本地验证或调用认证中心）
         <------- 验证通过，允许访问
用户 -------> 子系统 B（再次携带 Token）
子系统 B -------> 验证 Token（本地验证或调用认证中心）
         <------- 验证通过，允许访问
​```

#### **实现要点**
- **Token 是共享的**：
  - 用户登录认证中心后，所有子系统都可以使用同一个 Token 来验证用户身份。
  - 子系统只需要通过 Token 验证用户，而无需重复登录。

- **登录状态由认证中心管理**：
  - Token 的生成、验证和状态管理（如吊销、续期）都由认证中心负责。
  - 子系统信任认证中心的验证结果。

- **用户体验无缝化**：
  - 用户只需要登录一次，就可以在所有子系统中访问资源，无需重复登录。

---

## **3. 认证中心的实现重点**

为了实现上述流程，认证中心需要具备以下功能：

### **(1) Token 的签发**
- 提供登录接口，验证用户的用户名和密码。
- 登录成功后，生成 Token 并返回给用户（如 JWT 或 Access Token）。

### **(2) Token 的验证**
- 提供一个验证接口，允许子系统验证 Token 是否有效。
- 验证内容包括：
  - 签名是否正确。
  - Token 是否过期。
  - Token 是否被吊销。

### **(3) 用户信息接口**
- 提供一个用户信息接口，允许子系统通过 Token 获取用户的详细资料（如用户名、角色等）。

---

## **4. 总结**

### **Token 的验证是由认证中心决定的：**
- **认证中心生成和验证 Token**，是整个 SSO 的核心，所有子系统都依赖认证中心的验证结果。
- 子系统通过解析 Token 或调用认证中心的验证接口，判断用户身份。

### **验证通过则直接访问系统：**
- 如果 Token 验证通过，子系统可以直接允许用户访问，无需再次登录。
- 子系统可以选择本地解析 Token（如 JWT），或者调用认证中心接口进行验证。

### **完整的 SSO 流程**
1. 用户在认证中心登录，获取 Token。
2. 用户访问子系统时携带 Token。
3. 子系统通过解析或验证 Token 确认用户身份。
4. 用户在所有子系统中共享登录状态，无需重复登录。

这套流程实现了真正的单点登录（SSO），确保用户体验流畅的同时，提高了系统的安全性和扩展性。
```

~~~text
是的，**单点登录（SSO）** 的本质可以理解为 **将原有的登录和访问接口中生成和管理的 `access_token` 独立拆解出来**，并由一个 **集中式认证中心** 统一生成和管理这个 `access_token`（或者其他形式的 Token），从而实现用户身份在多个系统中的共享。

---

### **1. 为什么说单点登录是拆解了 access_token？**

在传统的登录和访问接口设计中，每个系统都有自己的登录接口，登录后生成的 `access_token` 仅在该系统内部有效，以下是原有设计的特点：

#### **传统的分散式登录流程：**
1. **每个系统独立登录**：
   - 用户访问子系统 A，输入用户名和密码进行登录。
   - 子系统 A 验证用户身份，生成 `access_token`，并将该 Token 用于后续的访问。

2. **Token 不共享**：
   - 如果用户需要访问子系统 B，则需要重复登录子系统 B。
   - 子系统 B 独立生成自己的 `access_token`，与子系统 A 的 Token 完全不同。

3. **缺点**：
   - 用户体验差：需要在每个系统中单独登录，登录状态无法共享。
   - 系统冗余：每个子系统都需要实现独立的登录逻辑、用户管理逻辑、Token 签发逻辑。
   - 安全风险：多个系统各自管理用户密码，增加了泄漏的可能性。

---

#### **单点登录的改进：**
单点登录通过将 **用户认证（登录逻辑）和 Token 签发** 独立出来，集中由认证中心（Identity Provider, IdP）负责，解决了传统分散式登录的痛点。

- 用户只需要在认证中心登录一次，认证中心生成的 `access_token` 被所有系统共享，避免了重复登录的问题。

- 具体的变化：
  - **登录的集中化**：所有系统的登录逻辑从各自的实现中抽离，集中到认证中心。
  - **Token 的独立化**：认证中心负责生成和验证 Token，子系统只需要依赖 Token 进行用户身份认证。

---

### **2. 单点登录的核心：将登录和 Token 统一管理**

单点登录将 **登录和 Token 管理独立拆解**，由认证中心完成。具体拆解过程如下：

#### **(1) 将登录逻辑集中到认证中心**
- 原有的每个子系统独立登录接口，被集中到认证中心的登录接口中。
- 用户在认证中心完成登录后，认证中心生成一个统一的 `access_token`。

#### **(2) 子系统依赖 Token 验证用户**
- 原有每个子系统内部生成 `access_token` 的逻辑被拆解。
- 子系统改为依赖认证中心生成的 Token 来验证用户身份：
  1. 子系统直接解析 Token（如果是 JWT）。
  2. 或调用认证中心的接口，验证 Token 的合法性。

---

### **3. 单点登录的核心工作流**

下面是单点登录的典型工作流程：

#### **(1) 用户登录认证中心**
1. 用户访问认证中心，输入用户名和密码完成登录。
2. 认证中心验证用户身份后，生成一个 `access_token` 或 JWT，并返回给用户。

#### **(2) 用户访问子系统 A**
1. 用户携带认证中心生成的 Token 访问子系统 A。
2. 子系统 A 从请求中提取 Token，并验证其合法性：
   - 如果是 JWT，子系统 A 可以直接解析并验证签名。
   - 如果是短 Token，子系统 A 调用认证中心的验证接口检查合法性。

3. 验证通过后，子系统 A 提取用户信息（如 `uid`）并允许访问。

#### **(3) 用户访问子系统 B**
1. 用户使用同一个 Token 访问子系统 B，无需重复登录。
2. 子系统 B 按照与子系统 A 相同的逻辑验证 Token，确认用户身份。

---

### **4. 单点登录的关键技术点**

单点登录的实现依赖以下关键技术点：

#### **(1) 集中认证中心**
- 认证中心负责用户登录和 Token 签发。
- 它是整个单点登录系统的核心，其他子系统必须信任认证中心。

#### **(2) Token 的设计**
- 推荐使用 **JWT（JSON Web Token）** 作为 Token：
  - 自包含：JWT 本身包含用户信息（如 `uid`、角色等），子系统可以直接解析。
  - 签名机制：JWT 包含认证中心的数字签名，防止被篡改。
  - 分布式验证：子系统可以本地验证 JWT，无需每次都调用认证中心。

#### **(3) 子系统的验证逻辑**
- 子系统无需独立实现登录逻辑，只需验证由认证中心签发的 Token。
- 验证方法：
  1. 如果是 JWT，直接解析和验证签名。
  2. 如果是短 Token，调用认证中心的验证接口。

#### **(4) 会话共享**
- 子系统不需要存储用户的登录状态（Token 即是状态）。
- 如果需要用户会话，可以基于 Token 在子系统内部创建会话。

---

### **5. 单点登录和传统 Token 的对比**

| 特性                   | 传统登录（每个系统独立）         | 单点登录（SSO）                   |
|------------------------|----------------------------------|-----------------------------------|
| **登录逻辑**            | 每个子系统独立实现               | 集中在认证中心                   |
| **Token 管理**          | 每个子系统独立生成               | 认证中心统一生成和管理            |
| **重复登录**            | 用户每次访问不同系统都需登录      | 用户一次登录即可访问所有系统      |
| **用户信息共享**        | 系统间无法共享用户信息           | 所有系统共享同一个 Token          |
| **安全性**              | 每个子系统存储用户密码，风险较高  | 密码集中管理，减少泄漏风险        |
| **开发复杂度**          | 每个系统需要实现完整的登录逻辑    | 子系统只需验证 Token              |

---

### **6. 子系统访问系统的关键：Token 的验证和解析**

通过将 `access_token` 抽离到认证中心，子系统的访问逻辑被简化为 **验证 Token** 和 **解析用户身份**。以下是子系统的关键实现步骤：

#### **(1) 获取 Token**
- 子系统通过 `Authorization` Header 或其他方式获取用户携带的 Token：
  ```http
  Authorization: Bearer <access_token>
```

**(2) 验证 Token**

- 如果 Token 是 JWT，可以直接解析和验证：
  ```javascript
  const jwt = require('jsonwebtoken');
  const token = req.headers.authorization?.split(' ')[1];
  try {
    const decoded = jwt.verify(token, 'your-public-key');
    console.log(decoded); // 用户信息
    // 验证通过，允许访问
  } catch (err) {
    console.error('Invalid token:', err);
    res.status(401).send('Unauthorized');
  }
  ```

- 如果 Token 是短 Token，调用认证中心验证接口：
  ```javascript
  const response = await axios.post('https://auth.example.com/validate', { token });
  if (response.data.valid) {
    console.log(response.data.user); // 用户信息
    next(); // 验证通过，允许访问
  } else {
    res.status(401).send('Unauthorized');
  }
  ```

**(3) 解析用户信息**

- 验证通过后，提取用户的 `uid`、角色等信息，用于权限管理或业务逻辑。

---

**7. 总结**

**单点登录确实将原有的登录和访问接口的 `access_token` 拆解了出来，集中由认证中心负责生成和管理**。这种设计带来了以下好处：

1. **登录逻辑集中**：认证中心负责所有登录和 Token 的生成，子系统只需验证 Token。
2. **Token 共享**：用户在认证中心登录后生成的 Token，可以在多个系统中共享使用，无需重复登录。
3. **用户体验提升**：用户一次登录即可访问所有系统，实现无缝的单点登录。
4. **系统安全性提高**：子系统不需要存储用户密码，减少了密码泄漏的风险。

在这种架构中，认证中心是关键，它负责验证用户身份并保证 Token 的合法性。一旦认证中心验证通过，用户可以直接访问所有子系统，无需重复登录。
~~~



前端页面嵌入后端里面

新增接口，判断ua头走html登录，还是免密跳转



最终流程总结:

1. 用户访问系统 A，未登录。
   ↓
2. 系统 A 重定向到认证中心，用户输入用户名和密码完成登录。
   ↓
3. 认证中心返回授权码（code）到系统 A 的 `callBack` 接口。
   ↓
4. 系统 A 通过授权码（code）向认证中心查询 `access_token`。
   ↓
5. 系统 A 使用 `access_token` 调用 `userInfo` 接口获取用户信息。
   ↓
6. 系统 A 将用户信息加密成 JWT，返回给前端或存储。
   ↓
7. 用户访问系统 B，携带 JWT，系统 B 解析 JWT 验证用户身份，无需重新登录。
~~~

最后打成cap包，先在测试环境部署

https://shangyun03.wpseco.cn/ksops

# 16. 先获取相应的controller层对应的方法

Qv2ray-v2.7.0-linux-x64.AppImage

chmod +x Qv2ray-v2.7.0-linux-x64.AppImage
./Qv2ray-v2.7.0-linux-x64.AppImage

chmod +x Hiddify-rpm-x64.rpm
sudo yum install -y Hiddify-rpm-x64.rpm --skip-broken



~~~

### 交付

![image-20250103092807525](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20250103092807525.png)

![image-20250103093235772](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20250103093235772.png)



![image-20250102173125829](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20250102173125829.png)

![image-20250102175316465](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20250102175316465.png)

​    

# 16. 湖南农信

## 1.账户同步

### 1.1.1.   API授权（获取token接口1200200001911）

IAM提供API授权功能，应用通过鉴权才能访问IAM API，保障数据安全性，授权机制如下：

1、IAM管理员对应用访问API进行授权，生成应用授权ID与授权密钥。

2、应用通过授权ID与授权密钥，调用IAM授权接口，向IAM申请授权令牌。

3、应用携带授权令牌访问IAM API。

#### 1.1.1.1. 获取密钥

IAM管理员对应用访问API范围进行授权，返回应用授权ID与密钥。根据实际情况配置加密与加签、应用访问API范围。

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/clip_image002.jpg)

#### 1.1.1.2. 申请令牌

应用通过授权ID与授权密钥，调用IAM授权接口，向IAM申请授权令牌。

请求地址：/api/getToken 

请求协议：http/https

请求方式：POST

参数格式：application/x-www-form-urlencoded

字符编码：utf-8

请求BODY参数

| 参数名称 | 参数类型 | 是否必填 | 参数说明                      |
| -------- | -------- | -------- | ----------------------------- |
| ClientId | string   | 是       | 应用ID                        |
| SgnInf   | string   | 是       | 签名值，算法参考signature方法 |
| CrnTm    | string   | 是       | 当前时间戳，13位毫秒级时间戳  |

返回参数（BODY中参数示例，其他ESB相关参数请阅读ESB平台文档）：

| 参数 | 子参数          | 参数类型  | 参数说明                       |
| ---- | --------------- | --------- | ------------------------------ |
| BODY |                 | XmlObject | 返回数据                       |
|      | TokenInf        | string    | 授权令牌                       |
|      | RefreshTokenInf | string    | 授权刷新令牌（秒），默认1800秒 |
|      | TokenHeadInf    | string    | 令牌头信息                     |
|      | VldTm           | string    | 令牌有效时间（秒），默认300秒  |

状态码：

| 状态码 | 描述                                   |
| ------ | -------------------------------------- |
| 000000 | 成功                                   |
| 401    | 应用ID或应用密钥错误                   |
| 500    | 授权类型错误，时间戳校验错误，签名错误 |

#### 1.1.1.3. 集成示例

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/clip_image004.gif)

应用处理逻辑如下：

\1.    应用通过IAM API授权机制申请访问令牌；

\2.    应用定时调用API、更新组织与账号数据；

\3.    应用初次全量同步、后续增量同步；

\4.    应用需要处理账号新增、修改、删除、授权逻辑；

 

同步生成全量快照并拉取数据：![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/clip_image006.jpg)

异步生成快照数据并拉取数据：

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/clip_image008.jpg)

页面配置：

![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/clip_image010.jpg)

### 1.1.2.   应用组织拉取API（1200200002602）

IAM提供标准应用组织同步API，应用定时拉取同步，支持全量与增量同步。

 

请求地址：见ESB平台文档

请求协议：http/https

请求方式：POST

参数类型：application/xml

字符编码：utf-8

 

请求体参数（BODY）：

| 参数名称 | 参数类型 | 是否必填 | 参数说明                                                     |
| -------- | -------- | -------- | ------------------------------------------------------------ |
| AuthCd   | String   | 是       | 授权码，格式：“Bearer”+token，参考第3章节                    |
| ClientId | String   | 是       | 客户端id，统一认证提供                                       |
| AplIndCd | String   | 是       | 应用id，统一认证提供                                         |
| SgnInf   | String   | 是       | 签名                                                         |
| CrnTm    | String   | 是       | 当前时间戳，13位毫秒级时间戳                                 |
| AllInd   | String   | 否       | 是否全量生成简略快照数据。1-生成全量快照数据；0-不生成全量快照数据。默认不生成 |
| AsyncInd | String   | 否       | 生成全量快照数据的时候是不是异步生成，需要和上面的AllInd参数配合使用才生效。  1-异步生成；0-同步生成。默认是同步生成。如果是异步生成会返回给调用方一个TaskId用于后续判断异步生成快照数据的任务是否完成。 |
| TaskId   | String   | 否       | 判断异步生成快照是否完成。当AllInd为1，AsyncInd为1的时候会返回该参数。 |

备注：

\1.  如果需要拉取数据详情就只需要传必填项。

\2.  taskId只要传了就只会校验taskId是否合法，以及对应异步任务的状态。

\3.  同步生成快照数据需传参数：appId, isAll-true，然后接口返回之后可以拉取数据详情。

\4.  异步生成快照数据需传参数：appId, isAll-true, async-true，然后根据返回的taskId查询任务的状态，当任务执行完成可以拉取数据详情。

\5.  每次全量生成快照数据的时候会将之前没有拉取的数据标记为已更新。

 

返回参数（BODY中参数示例，其他ESB相关参数请阅读ESB平台文档）：

| 参数名称 | 子参数      | 参数类型  | 是否必填 | 参数说明                                                     |
| -------- | ----------- | --------- | -------- | ------------------------------------------------------------ |
| code     |             | String    | 是       | 状态码，  000000为成功                                       |
| msg      |             | String    | 否       | 成功或者失败相应的提示信息                                   |
| BODY     |             | XmlObject | 否       | 组织数据                                                     |
|          | EventId     | String    | 否       | 任务id，异步任务产生的taskId                                 |
|          | YaskSt      | boolean   | 是       | 异步任务状态                                                 |
|          | RsnDsc      | Int       | 是       | 描述                                                         |
|          | HasNextInd  | boolean   | 否       | 是否有下一批数据，true表示仍有数据，需要继续传必传参数来拉取数据 |
|          | OrgDataArry | XmlArray  | 否       | 组织数据，详细数据结构见下表                                 |

 

组织数据data字段：

| 参数名称      | 参数类型   | 是否必填 | 参数说明                                     |
| ------------- | ---------- | -------- | -------------------------------------------- |
| EventTm       | String     | 是       | 变更数据对应的变更事件时间戳                 |
| AplIndCd      | String     | 是       | 应用ID                                       |
| OrgNm         | String     | 是       | 部门名称                                     |
| OrgCd         | String     | 是       | 部门编码                                     |
| ParentOrgCd   | String     | 是       | 上级部门编码                                 |
| OrgSt         | String     | 是       | 组织状态码，0-正常 1-禁用                    |
| SortId        | String     | 否       | 排序                                         |
| OrgOthrAllInf | JsonObject | 是       | 应用其他属性：值是Json串，里面的字段数量可变 |

 

接口响应状态码（状态码请阅读ESB平台文档）：

| 状态码 | 描述                 |
| ------ | -------------------- |
| 000000 | 成功                 |
| 401    | 无效token            |
| 404    | 路径找不到           |
| 500    | 没有权限，不允许访问 |
| 9001   | 参数错误             |

state状态码：

| 状态码 | 描述     |
| ------ | -------- |
| 30     | 成功     |
| 0      | 等待中   |
| 40     | 失败     |
| 10     | 正在执行 |



### 1.1.3.   应用账号拉取API（1200200002603）

IAM提供标准应用账号同步API，应用定时拉取同步，支持全量与增量同步。

 

请求地址：见ESB平台文档

请求协议：http/https

请求方式：POST

参数类型：application/xml

字符编码：utf-8

 

请求体参数（BODY）：

| 参数名称 | 参数类型 | 是否必填 | 参数说明                                                     |
| -------- | -------- | -------- | ------------------------------------------------------------ |
| AuthCd   | String   | 是       | 授权码，格式：“Bearer”+token，参考第5.1.1章节                |
| ClientId | String   | 是       | 客户端id，统一认证提供                                       |
| AplIndCd | String   | 是       | 应用id，统一认证提供                                         |
| SgnInf   | String   | 是       | 签名                                                         |
| CrnTm    | String   | 是       | 当前时间戳，13位毫秒级时间戳                                 |
| AllInd   | String   | 否       | 是否全量生成简略快照数据。1-生成全量快照数据；0-不生成全量快照数据。默认不生成 |
| AsyncInd | String   | 否       | 生成全量快照数据的时候是不是异步生成，需要和上面的AllInd参数配合使用才生效。  1-异步生成；0-同步生成。默认是同步生成。如果是异步生成会返回给调用方一个TaskId用于后续判断异步生成快照数据的任务是否完成。 |
| TaskId   | String   | 否       | 判断异步生成快照是否完成。当AllInd为1，AsyncInd为1的时候会返回该参数。 |

备注：

\1.  如果需要拉取数据详情就只需要传必填项。

\2.  taskId只要传了就只会校验taskId是否合法，以及对应异步任务的状态。

\3. 同步生成快照数据传参数：appId, isAll-true。

\4. 异步生成快照数据传参数：appId, isAll-true, async-true，然后根据返回的taskId查询任务的状态，当任务执行完成可以拉取数据详情。

\5. 每次全量生成快照数据的时候会将之前没有拉取的数据标记为已更新。

 

 

返回参数（BODY中参数示例，其他ESB相关参数请阅读ESB平台文档）：

| 参数名称 | 子参数      | 参数类型  | 是否必填 | 参数说明                                                     |
| -------- | ----------- | --------- | -------- | ------------------------------------------------------------ |
| code     |             | String    | 是       | 状态码，  000000为成功                                       |
| msg      |             | String    | 否       | 成功或者失败相应的提示信息                                   |
| BODY     |             | XmlObject | 否       | 账号数据                                                     |
|          | EventId     | String    | 否       | 任务id，异步任务产生的taskId                                 |
|          | YaskSt      | boolean   | 是       | 异步任务状态                                                 |
|          | RsnDsc      | Int       | 是       | 描述                                                         |
|          | HasNextInd  | boolean   | 否       | 是否有下一批数据，true表示仍有数据，需要继续传必传参数来拉取数据 |
|          | UsrDataArry | XmlArray  | 否       | 账号数据，详细数据结构见下表                                 |

 

账号数据data字段：

| 参数名称      | 参数类型   | 是否必填 | 参数说明                     |
| ------------- | ---------- | -------- | ---------------------------- |
| EventTm       | String     | 是       | 变更数据对应的变更事件时间戳 |
| AplIndCd      | String     | 是       | 应用ID                       |
| AcctVerfind   | String     | 是       | 账号唯一标识                 |
| Pwd           | String     | 否       | 账号密码，默认为空           |
| WrbtSctrNm    | JsonObject | 是       | 所属部门                     |
| OrgSt         | String     | 是       | 组织状态码，0为正常 1为禁用  |
| OrgOthrAllInf | JsonObject | 是       | 应用账号其他属性             |
| AppRolesInf   | JsonObject | 否       | 应用账号拥有的应用角色       |
| AppPrvgInf    | JsonObject | 否       | 应用账号拥有的应用权限       |

 

code状态码：

| 状态码 | 描述                 |
| ------ | -------------------- |
| 000000 | 成功                 |
| 401    | 无效token            |
| 404    | 路径找不到           |
| 500    | 没有权限，不允许访问 |
| 9001   | 参数错误             |

state状态码：

| 状态码 | 描述     |
| ------ | -------- |
| 30     | 成功     |
| 0      | 等待中   |
| 40     | 失败     |
| 10     | 正在执行 |

```
            String sign= SignBuilder.create(apiInfoBO.getClientId(), apiInfoBO.getClientSecret())
                    .requestPath("/api-auth/oauth/token")
                    .requestMethod("POST")
                    .params(signParam)
                    .sign();
                    
                    
    public static SignBuilder create(String clientId, String clientSecret) {
        return new SignBuilder(clientId, clientSecret);
    }
        public String sign() {
        return MD5Util.getMD5String(rawDataBeforeSign() + clientSecret);
    }
        public static String getMD5String(String str) {
        MessageDigest messageDigest = null;
        try {
            messageDigest = MessageDigest.getInstance("MD5");
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
            return null;
        }
        messageDigest.update(str.getBytes());
        return byteArray2HexString(messageDigest.digest());
    }
        /**
     * MD5加密结果（由byte转换成String）
     * @param bytes md5加密后得到的数组
     * @return md5加密结果
     */
    private static String byteArray2HexString(byte[] bytes) {
        StringBuilder sb = new StringBuilder();
        for (byte b : bytes) {
            sb.append(HEX_DIGITS[(b & 0xf0) >> 4]).append(HEX_DIGITS[(b & 0x0f)]);
        }
        return sb.toString();
    }
    private String rawDataBeforeSign() {
        allParam.put(CLIENT_ID_KEY, clientId);

        if (timestamp != null && !allParam.containsKey(TIMESTAMP_KEY)) {
            allParam.put(TIMESTAMP_KEY, timestamp);
        }

        StringBuilder sb = new StringBuilder();
        for (Object value : allParam.values()) {
            if (value != null) {
                sb.append(String.valueOf(value).trim());
            }
        }
        if (requestPath != null) {
            sb.append(requestPath);
        }

        if (requestMethod != null) {
            sb.append(requestMethod.toUpperCase());
        }

        System.out.println("签名原文：" + sb);
        return sb.toString();
    }
    
```

# 17. 浙江中烟

需要确认好![img](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/07ed13b649fc7878af007b6e1a9bc44d.png)

![image-20250121153415443](202410%E6%96%B0%E5%B7%A5%E4%BD%9C%E5%86%85%E5%AE%B9.assets/image-20250121153415443.png)



