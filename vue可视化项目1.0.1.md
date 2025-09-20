# a1. 环境配置

idea(建议老版)和vscode(建议新版)下载，jdk，maven安装和配置，新建项目。虚拟机配置和安装数据库。

## 1.1 setting文件内容

```txt
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="
            http://maven.apache.org/SETTINGS/1.0.0
            https://maven.apache.org/xsd/settings-1.0.0.xsd">

  <!-- ================================================ -->
  <!-- 本地仓库（可按需修改到你的磁盘路径）              -->
  <!-- ================================================ -->
  <localRepository>D:/codeSoft/maven-reposity/apache-maven-3.6.3/maven-repository</localRepository>

  <!-- ================================================ -->
  <!-- 镜像（Mirrors）                                  -->
  <!--   - 依赖走国内高速镜像                            -->
  <!--   - 插件走官方 Central                            -->
  <!-- ================================================ -->
  <mirrors>
    <!-- 依赖镜像：阿里云中央仓库 -->
    <mirror>
      <id>aliyun-deps</id>
      <mirrorOf>external:*</mirrorOf>
      <name>Aliyun Central Mirror for Dependencies</name>
      <url>https://maven.aliyun.com/repository/central</url>
    </mirror>
    <!-- 依赖镜像补充：清华 TUNA -->
    <mirror>
      <id>tuna-deps</id>
      <mirrorOf>external:*</mirrorOf>
      <name>TUNA Mirror for Dependencies</name>
      <url>https://mirrors.tuna.tsinghua.edu.cn/maven/</url>
    </mirror>
    <!-- 插件镜像：保持直连官方 Central -->
    <mirror>
      <id>central-plugins</id>
      <mirrorOf>external:!aliyun-deps,!tuna-deps</mirrorOf>
      <name>Maven Central for Plugins</name>
      <url>https://repo.maven.apache.org/maven2</url>
    </mirror>
  </mirrors>

  <!-- ================================================ -->
  <!-- 仓库配置 Profiles（默认激活）                    -->
  <!--   - 依赖仓库                                    -->
  <!--   - 插件仓库                                    -->
  <!-- ================================================ -->
  <profiles>
    <profile>
      <id>default</id>
      <activation><activeByDefault>true</activeByDefault></activation>

      <!-- 依赖仓库列表 -->
      <repositories>
        <!-- 官方 Central -->
        <repository>
          <id>central</id>
          <name>Maven Central</name>
          <url>https://repo.maven.apache.org/maven2</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>false</enabled></snapshots>
        </repository>
        <!-- Spring 官方 Release -->
        <repository>
          <id>spring-releases</id>
          <name>Spring Releases</name>
          <url>https://repo.spring.io/release</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>false</enabled></snapshots>
        </repository>
        <!-- OSSRH Snapshots -->
        <repository>
          <id>ossrh-snapshots</id>
          <name>OSSRH Snapshots</name>
          <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
          <releases><enabled>false</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </repository>
        <!-- 可选：企业私服 -->
        <!--
        <repository>
          <id>company-nexus</id>
          <name>Company Nexus</name>
          <url>https://nexus.company.com/repository/maven-public/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </repository>
        -->
      </repositories>

      <!-- 插件仓库列表 -->
      <pluginRepositories>
        <!-- 国内插件镜像：阿里云 -->
        <pluginRepository>
          <id>aliyun-plugins</id>
          <name>Aliyun Central Plugins</name>
          <url>https://maven.aliyun.com/repository/central</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>false</enabled></snapshots>
        </pluginRepository>
        <!-- 国内插件镜像：清华 TUNA -->
        <pluginRepository>
          <id>tuna-plugins</id>
          <name>TUNA Plugins Mirror</name>
          <url>https://mirrors.tuna.tsinghua.edu.cn/maven/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>false</enabled></snapshots>
        </pluginRepository>
        <!-- 官方 Central 插件回落 -->
        <pluginRepository>
          <id>central-plugins-fallback</id>
          <name>Official Maven Central Plugins</name>
          <url>https://repo.maven.apache.org/maven2</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>false</enabled></snapshots>
        </pluginRepository>
        <!-- Spring 官方插件仓库 -->
        <pluginRepository>
          <id>spring-plugins</id>
          <name>Spring Plugin Releases</name>
          <url>https://repo.spring.io/release</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>false</enabled></snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>

  <!-- 启用默认 profiles -->
  <activeProfiles>
    <activeProfile>default</activeProfile>
  </activeProfiles>

</settings>

```

## 1.2 pom文件内容

单个模块

```txt
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.17</version>
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>demo-app</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Demo Application</name>
    <description>Spring Boot Application compatible with JDK 8 to 11</description>

    <properties>
        <java.version>1.8</java.version> <!-- 若使用 JDK 11，可改为 11 -->
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- Web 开发 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- 单元测试 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- Lombok（可选） -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Spring Boot 插件 -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <!-- 编译插件，指定 JDK 版本 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                    <encoding>${project.build.sourceEncoding}</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

多个模块

父级pom

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
           http://maven.apache.org/POM/4.0.0
           https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <!-- 项目坐标 -->
    <groupId>com.example</groupId>
    <artifactId>springboot-parent</artifactId>
    <version>2.7.17</version>
    <packaging>pom</packaging>
    <name>Spring Boot 2.7 Parent</name>

    <!-- 统一属性 -->
    <properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <spring.boot.version>2.7.17</spring.boot.version>
        <maven.compiler.plugin.version>3.8.1</maven.compiler.plugin.version>
    </properties>

    <!-- 依赖管理：导入 Spring Boot BOM -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring.boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <!-- 插件管理：统一版本与配置 -->
    <build>
        <pluginManagement>
            <plugins>
                <!-- Java 编译插件 -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>${maven.compiler.plugin.version}</version>
                    <configuration>
                        <source>${java.version}</source>
                        <target>${java.version}</target>
                        <encoding>${project.build.sourceEncoding}</encoding>
                    </configuration>
                </plugin>
                <!-- Spring Boot 打包插件 -->
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <version>${spring.boot.version}</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>

    <!-- 聚合子模块 -->
    <modules>
        <module>web</module>
    </modules>

</project>

```



子级pom

```txt
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
           http://maven.apache.org/POM/4.0.0
           https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <!-- 引用父 POM -->
    <parent>
        <groupId>com.example</groupId>
        <artifactId>springboot-parent</artifactId>
        <version>2.7.17</version>
        <relativePath>../pom.xml</relativePath>
    </parent>

    <modelVersion>4.0.0</modelVersion>
    <artifactId>web</artifactId>
    <name>Spring Boot Web Module</name>
    <description>标准化的 Web 模块</description>

    <dependencies>
        <!-- Web 起步依赖，自动拉取 spring-webmvc 等 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 测试依赖 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- 使用父级管理的 Spring Boot 打包插件 -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

可能会报红，挂代理，pakcage一下把包拉下来即可。



## 1.3 数据库和redis安装

查看服务器版本，并离线安装相应的rpm包



可连网机器

```txt
cat /etc/redhat-release



sudo cp -r /etc/yum.repos.d /etc/yum.repos.d.bak

sudo curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

sudo yum clean all
sudo yum makecache

sudo yum clean all
sudo yum makecache


```

下载离线rpm包

```txt


sudo yum install -y yum-utils

sudo yum install -y https://repo.mysql.com/mysql57-community-release-el7-11.noarch.rpm

mkdir -p ~/mysql57-rpms
cd ~/mysql57-rpms

yumdownloader --resolve --destdir=. \
  mysql-community-server \
  mysql-community-client \
  mysql-community-common \
  mysql-community-libs
yumdownloader --resolve --destdir=. \
    perl perl-Getopt-Long perl-libs perl-macros \
    net-tools libaio
sudo yum localinstall -y *.rpm
```

启动并设置密码

```txt

sudo systemctl start mysqld
sudo systemctl enable mysqld
sudo grep 'temporary password' /var/log/mysqld.log

mysqladmin -u root -p'临时密码' password '新密码'

mysqladmin -u root -p'*8F0elI1dZty' password 'Root@2024'

```

检验编码集

```
mysql -uroot -pRoot@2024
show variables like 'character_set_%';

```

修改为utf-8,并配置远程权限。

```txt
>/etc/my.cnf

vi /etc/my.cnf
```

```txt
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
port=3306
bind-address=0.0.0.0

```

检查是否修改成功

```txt
service mysqld restart

mysql -uroot -pRoot@2024
show variables like 'character_set_%';
```

若配置不生效，则授权远程连接

```
grant all privileges on *.* to 'root'@'%' identified by 'Root@2024' with grant option;
flush privileges;
```

打开防火墙端口

```
sudo firewall-cmd --permanent --add-port=3306/tcp
sudo firewall-cmd --reload

```

**Redis安装**

```
sudo yum install -y gcc jemalloc-devel tcl wget


cd /usr/local/src
wget http://download.redis.io/releases/redis-7.2.4.tar.gz
tar xzf redis-7.2.4.tar.gz
cd redis-7.2.4


make
sudo make install


sudo mkdir -p /etc/redis
sudo cp redis.conf /etc/redis/
# 修改配置（推荐）
sudo sed -i 's/^supervised no/supervised systemd/' /etc/redis/redis.conf
sudo sed -i 's/^# requirepass .*/requirepass yourStrongPassword/' /etc/redis/redis.conf
sudo sed -i 's/^bind 127.0.0.1/bind 0.0.0.0/' /etc/redis/redis.conf  # 如需远程访问


sudo tee /etc/systemd/system/redis.service <<EOF
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
User=root
Group=root

[Install]
WantedBy=multi-user.target
EOF


sudo systemctl daemon-reload
sudo systemctl start redis
sudo systemctl enable redis


redis-cli -a yourStrongPassword ping


sudo systemctl status redis    # 查看服务状态
sudo systemctl restart redis   # 重启服务（配置修改后）
sudo systemctl stop redis      # 停止服务
sudo systemctl enable redis    # 开机自启


sudo vi /etc/redis/redis.conf

supervised systemd
bind 0.0.0.0
requirepass yourStrongPassword

sudo systemctl restart redis


sudo firewall-cmd --permanent --add-port=6379/tcp
sudo firewall-cmd --reload

```



## 1.4 代码结构提交远程仓库

因为是整体模板，需要把前端和后端一起提交上去

github新建一个仓库,然后git提交

进入项目根目录，执行下面命令

```
# 1. 配置身份
git config --global user.name  "sww"
git config --global user.email "1248002362%40qq.com"

# 2. 确认在项目根目录
cd /d/code/project-root/frontend

# 3. 初始化（如果还没 init）
git init

# 4. 添加远程（如果已经添加过可以跳过）
git remote add origin https://github.com/dddd56656/swwBaseProject.git

# 5. 暂存、提交
git add .
git commit -m "Initial commit"

# 6. 重命名分支为 main
git branch -M main

# 7. 推送并关联
git push -u origin main --force

账号:dddd56656
密码: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

```

代码提交到远程仓库成功

大陆用户请进行配置

```
git config --global http.proxy http://127.0.0.1:10808
git config --global https.proxy http://127.0.0.1:10808
```



## 1.5 前端框架搭建( Vite+Vue3+TypeScript+Pinia+Vue Router+Sass+Axios+环境变量+跨域代理)

安装nvm和node.js

https://github.com/coreybutler/nvm-windows/releases

```
nvm list available
nvm install 18
nvm use 18
node -v     
npm -v        



```

搭建可用框架

```
npm create vite@latest

```

![image-20250509085521613](1.vue可视化项目.assets/image-20250509085521613.png)



```shell
npm install

npm run dev

npm init @eslint/config@latest

```

![image-20250509085354178](1.vue可视化项目.assets/image-20250509085354178.png)





卸载vetur插件，安装ESlint插件，安装volar插件

![image-20250511214634616](1.vue可视化项目.assets/image-20250511214634616.png)



设置默认为eslint

![image-20250511214735556](1.vue可视化项目.assets/image-20250511214735556.png)



pckage新增脚本

```txt
    "lint": "eslint \"src/**/*.{vue,ts,js}\" --cache",
    "lint:fix": "eslint \"src/**/*.{vue,ts,js}\" --fix --cache",



```



```txt
# lint-staged安装
npm install --save-dev lint-staged
# husky安装
npm install --save-dev husky@latest
npx husky init                # 会自动生成 .husky/pre-commit

npm run prepare      # 或首次 clone 后自动执行

```

修改pre-commit

```txt
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

echo "🔍 lint‑staged 正在检查暂存文件…"
npx --no-install lint-staged || exit 1   # --no-install 省 30%‑40% 解析时间

```

package.json新增

```
  /* ──────────────── lint‑staged 仅扫暂存文件 ──────────────── */
  "lint-staged": {
    "*.{vue,ts,js}": [
      "npm run lint:fix -- --max-warnings 0"
    ],
    "*.{json,md,yml}": [
      "prettier --write"
    ]
  },
```

验证效果

```txt
# 1️⃣ 正常提交——应“✅ 通过”
mkdir -p src && echo "export const ok = 42;" > src/husky-pass.ts \
  && git add src/husky-pass.ts \
  && git commit -m "test: husky should pass"

# 2️⃣ 制造 ESLint 错误——应被“🚫 阻断”
echo "const unused = 1" > src/husky-fail.ts \
  && git add src/husky-fail.ts \
  && git commit -m "test: husky should fail"

```

新增构建插件

```
npm install --save-dev vite-plugin-checker

```

修改vite.config.ts

```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import checker from 'vite-plugin-checker'

// https://vite.dev/config/
export default defineConfig({
  plugins: [vue(),
    checker({
      vueTsc: true,   // 仍然跑 vue-tsc 类型检查
      eslint: false   // 屏蔽 eslint 通道，避免报错
    })
  ],
})

```

修改pakcage.json

```
"build": "vue-tsc -b && npm run lint && vite build",
```

.gitignore新增

```
# ESLint 缓存文件
.eslintcache
```

代码提交信息规范插件commitlint/cli的安装和使用

```
npm install --save-dev @commitlint/cli@^17.6.0 @commitlint/config-conventional@^17.6.0

```

新增commitlint.config.cjs

```
// commitlint.config.js

/**
 * CommitLint 配置文件
 * 该配置遵循 Conventional Commits 规范，适用于企业级提交校验标准
 */

module.exports = {
  // 使用社区通用规范
  extends: ['@commitlint/config-conventional'],

  // 自定义规则
  rules: {
    // 1. type 类型必须在以下列表中，保证语义清晰
    'type-enum': [
      2,
      'always',
      [
        'feat',     // 新功能 feature
        'fix',      // 修复 bug
        'docs',     // 文档变更
        'style',    // 代码格式（不影响功能，如空格、缩进）
        'refactor', // 代码重构（不包括修复和功能）
        'perf',     // 性能优化
        'test',     // 添加或修改测试
        'build',    // 构建工具变更（webpack、vite、npm scripts 等）
        'ci',       // CI 配置变更（GitHub Actions、GitLab CI 等）
        'chore',    // 其他日常事务（构建过程辅助脚本、依赖升级等）
        'revert'    // 回滚提交
      ]
    ],

    // 2. type 必须小写（例如 "Feat" 会报错）
    'type-case': [2, 'always', 'lower-case'],

    // 3. scope（作用域）必须小写，保持统一（如 "auth", "api"）
    'scope-case': [2, 'always', 'lower-case'],

    // 4. subject（简短描述）不能为空
    'subject-empty': [2, 'never'],

    // 5. subject 不允许以句号结尾（如 "fix: update foo." 会报错）
    'subject-full-stop': [2, 'never', ['.']],

    // 6. subject 必须是句首大写（Sentence case）或全部小写（可按需修改）
    'subject-case': [2, 'always', 'sentence-case'],

    // 7. header（type + scope + subject）总长度不超过 72 字符
    'header-max-length': [2, 'always', 72],

    // 8. body 每行不能超过 72 字符，便于终端阅读和 changelog 显示
    'body-max-line-length': [2, 'always', 72],

    // 9. footer 每行不能超过 72 字符（如 BREAKING CHANGE、issue refs）
    'footer-max-line-length': [2, 'always', 72],

    // 10. footer 前必须有一个空行（与 body 分隔）
    'footer-leading-blank': [2, 'always'],
  }
};

```

```
echo "npx --no -- commitlint --edit \$1" > .husky/commit-msg

```

测试

```
git commit -m "Feat(auth): add OAuth2 login support"

```

示例

```
git commit -m "feat(auth): add OAuth2 login support"

```

通过修改tsconfig.app.json配置加快构建速度和暴露隐藏bug

修改tsconfig.app.json

```
  "compilerOptions": {,
    "isolatedModules": true,
    "types": ["vite/client"]  },

```

Vue Router，pinia插件安装和使用

npm install vue-router@4 pinia

新增src/router/index.ts

```txt
// 引入Vue Router的核心方法与类型
import { createRouter, createWebHistory } from 'vue-router'
import type { RouteRecordRaw } from 'vue-router'


// 定义所有路由规则，便于权限控制、自动化管理
const routes: RouteRecordRaw[] = [
  {
    path: '/',                // 首页路由
    name: 'Home',
    component: () => import('@/views/Home.vue'), // 懒加载，提升首屏性能
  },
  {
    path: '/about',           // 关于页路由
    name: 'About',
    component: () => import('@/views/About.vue'),
  },
]

// 创建并导出router实例，统一在main.ts注册
export const router = createRouter({
  history: createWebHistory(), // 使用HTML5 history模式，支持SEO与回退
  routes,
})

```

新增src/store/index.ts   (mkdir -p src/store && touch src/store/index.ts)

```
// 引入并创建Pinia实例，便于主入口集成
import { createPinia } from 'pinia'

// 导出Pinia实例
export const pinia = createPinia()

```

新增src/store/useUserStore.ts    (mkdir -p src/store && touch src/store/useUserStore.ts)

```txt
// src/store/useUserStore.ts
import { defineStore } from 'pinia'
import http from '@/utils/http'
import type { UserInfo } from '@/types/user'

export const useUserStore = defineStore('user', {
  state: () => ({
    name: '未登录用户' as string,
    isLoggedIn: false as boolean,
  }),
  actions: {
    async login(name: string) {
      // 演示写死，实际可以用 http.post 登录
      this.name = name
      this.isLoggedIn = true
    },
    logout() {
      this.name = '未登录用户'
      this.isLoggedIn = false
    },
    async fetchUser() {
      // 泛型声明：确保 data 是 UserInfo 类型
      const data = await http.get<UserInfo>('/user/info')
      this.name = data.name
      this.isLoggedIn = data.isLoggedIn
    }
  }
})

```

新增  src/views/AppHome.vue    (mkdir -p src/views && touch src/views/AppHome.vue)

```txt
<template>
  <div class="home-box">
    <h1>首页</h1>
    <p>欢迎：{{ user.name }}</p>
    <button v-if="!user.isLoggedIn" @click="user.login('张三')">登录</button>
    <button v-else @click="user.logout()">登出</button>
    <button @click="refreshUser">刷新用户信息</button>
    <router-link to="/about">关于我们</router-link>
  </div>
</template>

<script setup lang="ts">
import { useUserStore } from '@/store/useUserStore'
const user = useUserStore()
const refreshUser = () => user.fetchUser()
</script>

<style lang="scss" scoped>
@use 'sass:color';
// 无需再次 import，$primary-color 已注入
.home-box {
  padding: 20px;
  background: color.adjust($primary-color, $lightness: 40%);
  color: $primary-color;
  border: 1px solid $primary-color;
  border-radius: 8px;
}
</style>

```

新增tsconfig.app.json

```
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}

```

新增src/views/AppAbout.vue    (touch src/views/AppAbout.vue)

```
<template>
  <div class="about-box">
    <h1>关于我们</h1>
    <p>本网站是由 Vue 3 + Vite + TypeScript 构建的现代前端项目。</p>
    <router-link class="link" to="/">返回首页</router-link>
  </div>
</template>

<script setup lang="ts">
// 此页面目前不需要逻辑，留空 script 即可
</script>

<style lang="scss" scoped>
@use 'sass:color';
// 变量 $primary-color 已自动全局注入，无需重复 @use

.about-box {
  padding: 24px;
  max-width: 600px;
  margin: 0 auto;
  border: 1px solid $primary-color;
  border-radius: 8px;
  color: $primary-color;
  background: color.adjust($primary-color, $lightness: 42%);
  // 或 color.scale($primary-color, $lightness: 42%);
}

.link {
  display: inline-block;
  margin-top: 16px;
  color: color.adjust($primary-color, $lightness: -10%);
  // 或 color.scale($primary-color, $lightness: -10%);
  font-weight: bold;
  text-decoration: none;

  &:hover {
    text-decoration: underline;
  }
}
</style>

```

新增vite.config.ts

```
import path from 'path'  // ← 这一行很重要！

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src'),  // 确保 __dirname 和 'src' 用对
    },
  },
})
```

修改App.vue

```
<template>
  <router-view />
</template>
```

sass,axios插件的安装和使用,环境变量配置，跨域

安装

```
npm install -D sass          # 只需安装 sass，Vite 已自动支持 .scss/.sass
npm install axios

```

新建src/styles/_variables.scss   (touch src/styles/_variables.scss)

```txt
$primary-color: #42b983;
$font-size-base: 16px;

```

src/styles/global.scss      (touch src/styles/global.scss  )

```txt
@use './variables' as *;

html, body, #app {
  margin: 0;
  padding: 0;
  width: 100vw;
  min-width: 0;
  height: 100vh;
  min-height: 0;
  background: #f5f6fa;
  box-sizing: border-box;
  overflow-x: hidden;
  font-family: 'Roboto', Arial, sans-serif;
  font-size: $font-size-base;
  color: $primary-color;
}

// 保证所有元素都继承box-sizing，避免抖动
*, *::before, *::after {
  box-sizing: inherit;
}

// 响应式自适应只调整字体，不对body加padding！
@media (max-width: 600px) {
  html { font-size: 90%; }
}

@media (min-width: 601px) and (max-width: 1200px) {
  html { font-size: 100%; }
}

@media (min-width: 1201px) {
  html { font-size: 110%; }
}

```

main.ts新增

```
import './styles/global.scss'

```

vite.config.ts新增

```
  css: {
    preprocessorOptions: {
      scss: {
      additionalData: `@use "@/styles/variables.scss" as *;`
      }
    }
  },
```

新建src/utils/http.ts    (touch src/utils/http.ts),处理不当会报错("Property 'isLoggedIn' does not exist on type 'AxiosResponse<UserInfo, any>'.ts(2339)")

```
import axios  from 'axios'
import type { AxiosRequestConfig, AxiosResponse, AxiosError } from 'axios'

const http = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 10000,
  headers: { 'Content-Type': 'application/json' }
})

http.interceptors.request.use(
  config => config,
  error => Promise.reject(error)
)

http.interceptors.response.use(
  (response: AxiosResponse) => {
    if (response.data && typeof response.data === 'object' && 'data' in response.data) {
      return response.data.data
    }
    return response.data
  },
  (error: AxiosError) => Promise.reject(error)
)

// 重点是这一行的泛型
const get = <T>(url: string, config?: AxiosRequestConfig): Promise<T> => {
  return http.get<unknown, T>(url, config)
}

const post = <T>(url: string, data?: unknown, config?: AxiosRequestConfig): Promise<T> => {
  return http.post<unknown, T>(url, data, config)
}

export default { get, post }

```

新建.env  .env.production    (touch .env   &&   touch .env.production )

```
VITE_API_BASE_URL=https://dev-api.example.com
VITE_APP_TITLE=My Vite App

```



```
VITE_API_BASE_URL=https://prod-api.example.com
VITE_APP_TITLE=My Vite App [PROD]

```



修改vite.config.ts，新增

```
  server: {
    proxy: {
      // 只要是 /api 开头的请求，代理到后端
      '/api': {
        target: 'https://dev-api.example.com',   // 后端接口地址
        changeOrigin: true,
        rewrite: path => path.replace(/^\/api/, ''),
      }
    }
  },
```

新建src/types/user.ts      (mkdir -p src/types &&  touch src/types/user.ts  )

```txt
// src/types/user.ts
export interface UserInfo {
  name: string
  isLoggedIn: boolean
}

```



ElementPlus插件安装和使用

```
npm install element-plus --save
```

src/main.ts新增

```
// 关键节点：全局注册 Element Plus，包含样式
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css' // 核心样式
app.use(ElementPlus) // 注册全局

```

mkdir -p src/layouts src/components src/views src/router src/store
touch src/layouts/MainLayout.vue

```
<template>
  <!--
    主布局：企业级前端系统标准骨架
    1. 左侧为多级导航菜单（AppAsideMenu 组件，参考 Element Plus 多级菜单官方样式）
    2. 上方为头部工具栏（AppHeaderBar 组件，可放用户信息/全局操作/设置）
    3. 中间为主内容区（router-view 占位，渲染路由对应页面）
    4. el-container 嵌套结构保证响应式和样式隔离
  -->
  <el-container class="main-layout">
    <!-- 侧边栏：宽度固定，可后续接入收缩/权限等功能 -->
    <el-aside width="220px" class="aside-menu">
      <!-- 多级菜单组件（所有菜单内容均可配置扩展） -->
      <AppAsideMenu />
    </el-aside>

    <!-- 右侧主区：头部 + 主内容区 -->
    <el-container>
      <!-- 头部栏（顶部工具区，可放Logo、面包屑、用户等） -->
      <el-header height="56px" class="header-bar">
        <AppHeaderBar />
      </el-header>
      <!-- 主内容区：所有页面的内容都渲染在这里 -->
      <el-main class="main-content">
        <router-view />
        <!-- router-view 占位，动态加载当前路由组件 -->
      </el-main>
    </el-container>
  </el-container>
</template>

<script setup lang="ts">
/**
 * 主布局文件 MainLayout.vue
 * - 负责所有页面的基础框架搭建
 * - 内部依赖两个核心基础组件（AppAsideMenu、AppHeaderBar），各自单独维护
 * - 可通过 provide/inject、Pinia、props、slots 等方式进行全局交互
 */
import AppAsideMenu from '@/components/AppAsideMenu.vue'   // 左侧多级菜单
import AppHeaderBar from '@/components/AppHeaderBar.vue'   // 顶部工具栏
</script>

<style scoped>
/* 主布局整体撑满视口 */
.main-layout {
  height: 100vh; /* 100%视口高度，撑满浏览器 */
}

/* 侧边栏区域样式，参考 Element Plus 官方侧栏 */
.aside-menu {
  background: var(--el-color-primary-light-9, #f5f7fa); /* 默认淡色，也可自定义主题色 */
  min-height: 100vh;
  border-right: 1px solid #ebeef5; /* 视觉分隔线 */
  /* 可按需添加 transition 支持折叠动画 */
}

/* 顶部工具栏，样式贴近 Element Plus 官方 Demo */
.header-bar {
  background: #fff;
  border-bottom: 1px solid #ebeef5;
  box-shadow: 0 1px 4px rgba(0,21,41,0.04); /* 细微阴影增强层次 */
  z-index: 100;
}

/* 主内容区，推荐采用留白设计，便于扩展仪表盘/表格/图表等 */
.main-content {
  padding: 24px;
  background: #f5f6fa; /* 与 Element Plus 设计语言一致 */
  min-height: 100%;
  /* 可以加overflow-y: auto，支持内容溢出滚动 */
}
</style>

```



touch src/components/AppAsideMenu.vue 

```
<template>
  <!--
    AppAsideMenu 组件 —— 系统侧边多级导航菜单
    - 采用 Element Plus <el-menu> 及 <el-sub-menu> 实现多级菜单结构
    - 支持路由自动高亮，icon与菜单文本自定义
    - 可作为权限系统和国际化菜单的基础
    - 推荐后期用配置数据+递归渲染（此Demo为手写静态结构，易理解）
  -->
  <el-menu
    :default-active="activeMenu"            <!-- 当前路由对应菜单自动高亮 -->
    class="el-menu-vertical-demo"           <!-- 自定义样式class -->
    background-color="#f5f7fa"              <!-- 菜单背景色，与EP官方一致 -->
    text-color="#333"                       <!-- 默认文字颜色 -->
    active-text-color="#409EFF"              <!-- 选中项高亮色（EP主色） -->
    :collapse="false"                       <!-- 是否折叠菜单，支持响应式 -->
    router                                  <!-- 启用路由模式，点击菜单自动跳转 -->
  >
    <!-- 一级菜单，含图标及文本 -->
    <el-sub-menu index="1">
      <template #title>
        <el-icon><Menu /></el-icon>
        <span>主导航</span>
      </template>
      <!-- 二级菜单：首页（直接跳转到 /） -->
      <el-menu-item index="/">首页</el-menu-item>
      <!-- 二级菜单：带三级子菜单的演示 -->
      <el-sub-menu index="1-2">
        <template #title>
          <el-icon><Setting /></el-icon>
          <span>系统设置</span>
        </template>
        <!-- 三级菜单项，可按需扩展 -->
        <el-menu-item index="/setting1">设置1</el-menu-item>
        <el-menu-item index="/setting2">设置2</el-menu-item>
      </el-sub-menu>
    </el-sub-menu>
    <!-- 其它一级或多级菜单项，可继续添加 -->
    <!--
    <el-menu-item index="/profile">
      <el-icon><User /></el-icon>
      <span>个人中心</span>
    </el-menu-item>
    -->
  </el-menu>
</template>

<script setup lang="ts">
/**
 * AppAsideMenu.vue
 * - 项目侧边多级菜单组件
 * - 支持动态路由高亮、图标自定义、权限拓展
 * - 推荐后续递归化和配置驱动
 */
import { useRoute } from 'vue-router'
import { computed } from 'vue'
import { Menu, Setting } from '@element-plus/icons-vue' // Element Plus官方icon

// 1. 获取当前路由信息（用于菜单高亮）
const route = useRoute()

// 2. 计算当前激活菜单项（以route.path为基准，确保跳转/刷新自动同步高亮）
const activeMenu = computed(() => route.path)

// 3. 推荐扩展：
//    - 菜单项数组 + 递归渲染（适配权限/多语言）
//    - 支持 collapse 响应式收缩（如引入左侧折叠功能）
//    - 可注入用户角色实现动态菜单
</script>

<style scoped>
/* 侧边菜单整体样式 */
.el-menu-vertical-demo {
  border-right: none;   /* 视觉简洁，无右侧边线 */
  min-height: 100vh;    /* 高度撑满侧边 */
  width: 100%;          /* 占满aside宽度 */
  background: inherit;  /* 跟随外层背景，可自定义主题 */
}
/* 图标与文本的间距优化（EP官方推荐8px） */
.el-menu .el-icon {
  margin-right: 8px;
}
</style>

```



touch src/components/AppHeaderBar.vue

```
<template>
  <!--
    AppHeaderBar 组件 —— 顶部全局工具栏
    1. 左侧：可放Logo、系统标题、面包屑等
    2. 右侧：用户区（头像+用户名+下拉菜单）、全局操作（如设置、切换主题、通知等）
    3. 推荐所有交互通过props/inject/Pinia进行解耦
    4. 业务区块可通过slot拓展
  -->
  <div class="header-bar-inner">
    <!-- 左侧区域：Logo/系统名（可改成slot） -->
    <div class="left">
      <span class="title">企业管理平台</span>
    </div>
    <!-- 右侧区域：用户/操作区 -->
    <div class="right">
      <!-- 用户头像，可换成后端头像或本地上传 -->
      <el-avatar size="small" style="margin-right: 8px;">
        <!-- 推荐用后端下发URL或第三方图像生成器 -->
        <img src="https://api.dicebear.com/8.x/pixel-art/svg?seed=user" alt="avatar" />
      </el-avatar>
      <!-- 用户名（可用pinia或props注入） -->
      <span style="margin-right: 16px;">{{ user.name }}</span>
      <!-- 用户操作下拉菜单：个人中心、登录/登出、设置等 -->
      <el-dropdown>
        <span class="el-dropdown-link">
          操作 <el-icon><ArrowDown /></el-icon>
        </span>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item @click="toProfile">个人中心</el-dropdown-item>
            <!-- 登录/登出根据状态切换 -->
            <el-dropdown-item divided v-if="user.isLoggedIn" @click="logout">登出</el-dropdown-item>
            <el-dropdown-item v-else @click="login">登录</el-dropdown-item>
            <el-dropdown-item @click="openSettings">设置</el-dropdown-item>
            <!-- 可继续添加国际化/主题切换等全局操作 -->
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </div>
</template>

<script setup lang="ts">
/**
 * AppHeaderBar.vue
 * - 顶部全局工具栏组件
 * - 最大化注释，Google级工程可扩展性
 * - 所有用户与操作均解耦（可接pinia、props或inject）
 */
import { useUserStore } from '@/store/useUserStore' // 推荐全局pinia管理用户信息
import { ArrowDown } from '@element-plus/icons-vue'

const user = useUserStore()

// 用户登录（可换成弹窗或跳OAuth）
const login = () =>   user.login({
    id: '1',                // 假定ID
    name: '张三',
    email: 'zhangsan@example.com', // 假定邮箱
    avatarUrl: ''           // 可留空
  })
// 用户登出
const logout = () => user.logout()
// 跳个人中心（路由跳转或弹窗，具体业务接入）
const toProfile = () => {
  // 这里可用router.push('/profile')或emit事件
}
// 打开设置（建议弹窗或跳转设置页）
const openSettings = () => {
  // 打开设置弹窗/页面
}
</script>

<style scoped>
.header-bar-inner {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 100%; /* 撑满header高度 */
}
.title {
  font-weight: 700;
  font-size: 20px;
  color: #333;
  letter-spacing: 1px;
}
.right {
  display: flex;
  align-items: center;
}
/* el-avatar/下拉等可自行美化 */
</style>

```

touch src/views/AppHome.vue

```
<template>
  <!--
    AppHome 组件 —— 首页内容区
    1. 欢迎区：显示当前用户信息与典型操作按钮
    2. 典型按钮示例：演示EP主按钮/默认/危险等
    3. Table样例：可直接扩展为业务表格
    4. 所有内容布局留有扩展空间，便于后续嵌入图表/分析/卡片等
  -->
  <div class="home-box">
    <!-- 欢迎栏及右侧操作按钮 -->
    <el-row :gutter="24" class="mb-4">
      <el-col :span="12">
        <h2>欢迎，{{ userStore.displayName }}！</h2>
      </el-col>
      <el-col :span="12" style="text-align: right;">
        <el-button type="primary" @click="refreshUser">刷新用户信息</el-button>
        <el-button type="success" @click="addChart" class="ml-2">显示图表</el-button>
      </el-col>
    </el-row>

    <!-- 典型按钮示例区 -->
    <el-card class="mb-4">
      <h3>典型按钮示例</h3>
      <el-button type="primary" class="mr-2">主要按钮</el-button>
      <el-button>默认按钮</el-button>
      <el-button type="danger" class="ml-2">危险按钮</el-button>
    </el-card>

    <!-- Table 样例区 -->
    <el-card>
      <h3>Table 样例</h3>
      <el-table :data="tableData" style="width: 100%">
        <el-table-column prop="date" label="日期" width="180" />
        <el-table-column prop="name" label="姓名" width="180" />
        <el-table-column prop="address" label="地址" />
      </el-table>
    </el-card>
  </div>
</template>

<script setup lang="ts">
/**
 * AppHome.vue
 * - 首页内容区/仪表盘
 * - 展示典型按钮、表格，预留后续扩展入口
 * - 与用户Pinia状态联动（如用户信息刷新）
 * - 兼容新Store结构，业务store全部按modules分文件
 */
import { ref } from 'vue'
import { useUserStore } from '@/store/modules/user' // 新结构的用户Store

const userStore = useUserStore()

/**
 * 刷新用户信息
 * 调用store action（一般实际会发起API请求）
 */
const refreshUser = () => userStore.fetchUser()

/**
 * 显示图表
 * 实际项目可弹窗/跳转或渲染图表组件
 */
const addChart = () => {
  // TODO: 集成图表组件（如ECharts）
}

// 表格数据样例，可直接扩展为API动态获取
const tableData = ref([
  { date: '2023-05-22', name: '张三', address: '上海市普陀区金沙江路' },
  { date: '2023-05-21', name: '李四', address: '北京市海淀区西二旗' },
  { date: '2023-05-20', name: '王五', address: '广州市天河区体育西路' }
])
</script>

<style scoped>
.home-box {
  padding: 20px;
  background: #fff;
  border-radius: 8px;
}
.mb-4 {
  margin-bottom: 24px;
}
.ml-2 {
  margin-left: 8px;
}
.mr-2 {
  margin-right: 8px;
}
</style>

```

touch src/router/index.ts

```
/**
 * index.ts
 * 路由主入口（Google级模块化，最大化注释）
 * 1. 按modules分文件解耦业务路由，方便维护和权限扩展
 * 2. 自动合并所有模块路由，支持无限扩展
 * 3. 挂载全局主布局，所有页面默认走统一布局
 */
import { createRouter, createWebHistory, RouteRecordRaw } from 'vue-router'

// 1. 按模块导入路由
import homeRoutes from './modules/home'
import systemRoutes from './modules/system'
// import profileRoutes from './modules/profile' // 可继续扩展

// 2. 主路由结构（包含主布局和子页面）
//   - 所有页面都在MainLayout下渲染，子路由负责页面内容
const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    component: () => import('@/layouts/MainLayout.vue'),
    children: [
      ...homeRoutes,      // 首页
      ...systemRoutes,    // 系统设置
      // ...profileRoutes, // 其它模块
      // 可扩展更多业务模块
    ]
  }
  // 可扩展如login/404等特殊路由，不走主布局
]

// 3. 创建路由实例，使用HTML5 History模式
const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router

```







mkdir src/router/modules

touch src/router/modules/home.ts

```
/**
 * home.ts
 * 首页相关路由配置（可按需扩展更多子页面）
 */
import { RouteRecordRaw } from 'vue-router'

const homeRoutes: RouteRecordRaw[] = [
  {
    path: '/',
    name: 'Home',
    component: () => import('@/views/AppHome.vue'),
    meta: {
      title: '首页',
      icon: 'Menu' // 方便和菜单系统关联
    }
  }
]

export default homeRoutes

```



touch src/router/modules/system.ts

```
/**
 * system.ts
 * 系统设置相关路由（可对应Element Plus多级菜单中的系统设置）
 */
import { RouteRecordRaw } from 'vue-router'

const systemRoutes: RouteRecordRaw[] = [
  {
    path: '/system',
    name: 'System',
    component: () => import('@/layouts/MainLayout.vue'), // 复用主布局
    meta: {
      title: '系统设置',
      icon: 'Setting'
    },
    children: [
      {
        path: 'setting1',
        name: 'SystemSetting1',
        component: () => import('@/views/SystemSetting1.vue'),
        meta: { title: '设置1' }
      },
      {
        path: 'setting2',
        name: 'SystemSetting2',
        component: () => import('@/views/SystemSetting2.vue'),
        meta: { title: '设置2' }
      }
    ]
  }
]

export default systemRoutes

```



mkdir src/store/types && touch src/store/types/user.ts

```
/**
 * 用户模块相关类型
 * 按Google级标准独立维护，便于多人协作和类型扩展
 */

export interface UserInfo {
  id: string
  name: string
  email: string
  avatarUrl?: string
  isLoggedIn: boolean
  // 可扩展如角色、权限、token等
}

```

mkdir src/store/modules && touch src/store/modules/user.ts

```
/**
 * 用户模块 Pinia Store
 * - 最大化注释，适合中大型项目
 * - 所有类型全部引自 types
 * - 推荐actions仅做状态变更和业务分发，异步API独立service层
 */
import { defineStore } from 'pinia'
import type { UserInfo } from '../types/user'

/**
 * 推荐：state用工厂函数返回，避免全局状态污染
 */
export const useUserStore = defineStore('user', {
  // 1. State: 用户基本信息，登录状态等
  state: (): UserInfo => ({
    id: '',
    name: '访客',
    email: '',
    avatarUrl: '',
    isLoggedIn: false
  }),

  // 2. Actions: 所有业务相关操作（登录、登出、拉取用户信息等）
  actions: {
    /**
     * 用户登录
     * @param info 用户信息对象（建议后端返回后再保存）
     */
    login(info: Omit<UserInfo, 'isLoggedIn'>) {
      this.id = info.id
      this.name = info.name
      this.email = info.email
      this.avatarUrl = info.avatarUrl
      this.isLoggedIn = true
    },

    /**
     * 用户登出（重置所有用户信息）
     */
    logout() {
      this.id = ''
      this.name = '访客'
      this.email = ''
      this.avatarUrl = ''
      this.isLoggedIn = false
    },

    /**
     * 拉取用户信息（通常配合API异步获取，推荐实际业务中封装独立service）
     */
    async fetchUser() {
      // TODO: 调用真实接口，以下为模拟数据
      const res = {
        id: '1',
        name: '张三',
        email: 'zhangsan@example.com',
        avatarUrl: 'https://api.dicebear.com/8.x/pixel-art/svg?seed=user'
      }
      this.login(res)
    }
  },

  // 3. Getters: 推荐分文件复杂业务单独抽取
  getters: {
    /**
     * 用户昵称首字母大写
     */
    displayName: (state): string => state.name ? state.name.charAt(0).toUpperCase() + state.name.slice(1) : '访客'
  }
})

```

nprogress插件的安装和使用，登录业务的代码编写,统一设置token，处理页面访问登录状态和登录过期。pinia-plugin-persistedstate插件的安装和使用。

```
npm install nprogress
npm i --save-dev @types/nprogress
```

新建services/user.ts    mkdir src/services && touch src/services/user.ts

```
/**
 * 用户相关API & 业务逻辑（service层）
 * - 只负责异步API、数据转换、复杂流程
 * - 类型全部解耦，只用import type { UserInfo } from ...
 */

import http from '@/utils/http'
import type { UserInfo } from '@/store/types/user'

/**
 * 用户登录
 * @param username 用户名
 * @param password 密码
 * @returns 用户信息
 */
export function loginApi(username: string, password: string) {
  return http.post<UserInfo>('/api/login', { username, password })
}

/**
 * 获取当前用户信息
 */
export function fetchUserApi() {
  return http.get<UserInfo>('/api/user')
}

/**
 * 用户登出
 */
export function logoutApi() {
  return http.post<void>('/api/logout')
}

```

对应的store层代码

```
/**
 * 用户Pinia Store（只做状态与同步变更）
 * - Google标准：全部类型解耦，最大注释
 * - 只做本地状态管理/变更/展示，异步交由service
 */

import { defineStore } from 'pinia'
import type { UserInfo } from '../types/user'
import { loginApi, fetchUserApi, logoutApi } from '@/services/user'

export const useUserStore = defineStore('user', {
  /**
   * 1. State：本地用户信息
   */
  state: (): UserInfo => ({
    id: '',
    name: '访客',
    email: '',
    avatarUrl: '',
    isLoggedIn: false
  }),

  /**
   * 2. Actions：同步变更+异步业务（推荐复杂业务分离出service层）
   */
  actions: {
    /**
     * 本地登录变更（仅存数据，不调接口）
     * @param info 登录成功后的用户信息
     */
    setLogin(info: Omit<UserInfo, 'isLoggedIn'>) {
      this.id = info.id
      this.name = info.name
      this.email = info.email
      this.avatarUrl = info.avatarUrl ?? ''
      this.isLoggedIn = true
    },

    /**
     * 本地登出变更（重置所有状态）
     */
    setLogout() {
      this.id = ''
      this.name = '访客'
      this.email = ''
      this.avatarUrl = ''
      this.isLoggedIn = false
    },

    /**
     * 登录流程（调API + 本地写入）
     */
    async login(username: string, password: string) {
      const user = await loginApi(username, password)
      this.setLogin(user)
      // 可选：本地存储token等
    },

    /**
     * 拉取当前用户信息并写入本地
     */
    async fetchUser() {
      const user = await fetchUserApi()
      this.setLogin(user)
    },

    /**
     * 登出流程（调API + 本地清理）
     */
    async logout() {
      await logoutApi()
      this.setLogout()
      // 可选：清理token
    }
  },

  /**
   * 3. Getters：只做本地展示/状态派生
   */
  getters: {
    /**
     * 用户名首字母大写
     */
    displayName: (state): string =>
      state.name ? state.name.charAt(0).toUpperCase() + state.name.slice(1) : '访客'
  }
    persist: true // 开启自动持久化，所有state字段自动同步到本地

})

```

layout层代码



mkdir src/layouts && touch src/layouts/MainLayout.vue && touch src/layouts/AuthLayout.vue

```
<template>
  <!-- 主应用外壳，包含头部、侧边栏、主内容和全局通知等 -->
  <div class="main-layout">
    <!-- 头部导航栏（可插槽自定义） -->
    <AppHeader />
    <div class="layout-body">
      <!-- 侧边栏（如有权限菜单） -->
      <AppSidebar />
      <!-- 主内容区：用slot注入实际页面内容 -->
      <main class="main-content">
        <slot />
      </main>
    </div>
    <!-- 全局底部 -->
    <AppFooter />
  </div>
</template>

<script setup lang="ts">
// 引用复用组件
import AppHeader from '@/components/AppHeader.vue'
import AppSidebar from '@/components/AppSidebar.vue'
import AppFooter from '@/components/AppFooter.vue'
</script>

<style scoped>
.main-layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: #f5f6fa;
}
.layout-body {
  flex: 1;
  display: flex;
  flex-direction: row;
}
.main-content {
  flex: 1;
  padding: 24px;
  background: #fff;
  min-height: 0;
  overflow: auto;
}
</style>

```



```
<template>
  <!-- 登录/注册/忘记密码专用外壳，居中简约 -->
  <div class="auth-layout">
    <div class="auth-box">
      <slot />
    </div>
  </div>
</template>

<script setup lang="ts">
/**
 * AuthLayout
 * 用于登录/注册等不需要主导航的简约页面外壳
 */
</script>

<style scoped>
.auth-layout {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(120deg, #409eff 0%, #f5f6fa 100%);
}
.auth-box {
  width: 350px;
  padding: 32px 24px;
  background: #fff;
  border-radius: 18px;
  box-shadow: 0 2px 16px #409eff33;
}
</style>

```



对应componets层代码

mkdir src/components && touch src/components/AppHeader.vue && touch src/components/AppSidebar.vue && touch  src/components/AppFooter.vue && touch src/components/UserAvatar.vue

```
<template>
  <header class="app-header">
    <!-- 应用LOGO -->
    <div class="logo">MyApp</div>
    <!-- 右侧用户栏 -->
    <div class="header-actions">
      <UserAvatar />
      <!-- 更多操作按钮 -->
    </div>
  </header>
</template>

<script setup lang="ts">
import UserAvatar from './UserAvatar.vue'
</script>

<style scoped>
.app-header {
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 32px;
  background: #409eff;
  color: #fff;
}
.logo {
  font-size: 22px;
  font-weight: bold;
  letter-spacing: 2px;
}
.header-actions {
  display: flex;
  align-items: center;
  gap: 20px;
}
</style>

```



```
<template>
  <aside class="app-sidebar">
    <!-- 侧边菜单栏，可根据权限/路由动态生成 -->
    <nav>
      <ul>
        <li><router-link to="/">首页</router-link></li>
        <li><router-link to="/about">关于</router-link></li>
        <!-- 可扩展更多 -->
      </ul>
    </nav>
  </aside>
</template>

<script setup lang="ts">
/**
 * AppSidebar
 * 侧边菜单，可接入动态权限/路由管理
 */
</script>

<style scoped>
.app-sidebar {
  width: 200px;
  background: #fff;
  border-right: 1px solid #eee;
  padding-top: 18px;
  min-height: 100%;
}
.app-sidebar ul {
  list-style: none;
  padding: 0;
}
.app-sidebar li {
  margin: 12px 0;
}
.app-sidebar a {
  color: #333;
  text-decoration: none;
  padding: 8px 16px;
  display: block;
  border-radius: 8px;
}
.app-sidebar a.router-link-active {
  background: #409eff22;
  color: #409eff;
}
</style>

```



```
<template>
  <footer class="app-footer">
    © 2025 MyApp. All rights reserved.
  </footer>
</template>

<script setup lang="ts">
/**
 * AppFooter
 * 全局底部信息栏
 */
</script>

<style scoped>
.app-footer {
  height: 48px;
  text-align: center;
  color: #888;
  background: #fafbfc;
  line-height: 48px;
  font-size: 14px;
}
</style>

```



```
<template>
  <div class="user-avatar">
    <img :src="avatarUrl" :alt="userName" />
    <span class="user-name">{{ userName }}</span>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useUserStore } from '@/store/modules/user'

// 读取当前用户信息
const userStore = useUserStore()
const avatarUrl = computed(() => userStore.avatarUrl || 'https://api.dicebear.com/8.x/pixel-art/svg?seed=user')
const userName = computed(() => userStore.displayName)
</script>

<style scoped>
.user-avatar {
  display: flex;
  align-items: center;
  gap: 8px;
}
.user-avatar img {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  object-fit: cover;
}
.user-name {
  font-size: 15px;
  color: #333;
}
</style>

```

mkdir src/router  && mkdir src/router/modules && touch src/router/index.ts && touch src/router/modules/other.ts && touch src/router/modules/base.ts

```txt
import { createRouter, createWebHistory } from 'vue-router'
import NProgress from 'nprogress'
import 'nprogress/nprogress.css'
import { baseRoutes } from './modules/base'
import { otherRoutes } from './modules/other'
import { useUserStore } from '@/store/modules/user'
import { ElMessageBox } from 'element-plus'

const routes = [...baseRoutes, ...otherRoutes]

const router = createRouter({
  history: createWebHistory(),
  routes
})

let loginTipActive = false // 防止多次弹窗&跳转

router.beforeEach((to, _from, next) => {
  NProgress.start()
  const userStore = useUserStore()
  console.log('[Router] 跳转前，isLoggedIn=', userStore.isLoggedIn)

  // 只在需要登录的页面做拦截
  if (to.meta.requiresAuth && !userStore.isLoggedIn) {
    if (!loginTipActive) {
      loginTipActive = true

      // 弹框提示，支持“立即登录”按钮，2秒后自动跳转
      ElMessageBox({
        title: '未登录提示',
        message: '您还未登录，请先登录后访问该页面。',
        showCancelButton: true,
        confirmButtonText: '立即登录',
        cancelButtonText: '取消',
        closeOnClickModal: true,
        closeOnPressEscape: true,
        callback: (action: string) => {
          // “立即登录” 或关闭弹窗时自动跳转
          if (action === 'confirm') {
            router.replace('/login')
          }
          loginTipActive = false // 恢复标志位
        }
      })

      // // 2秒后自动跳转并关闭弹窗
      // setTimeout(() => {
      //   if (loginTipActive) {
      //     router.replace('/login')
      //     ElMessageBox.close()
      //     loginTipActive = false
      //   }
      // }, 5000)
    }
    // 关键：此处 next(false) 阻止后续跳转（不放行当前路由），等待弹窗逻辑
    next(false)
    return
  }

  next()
})

router.afterEach(() => {
  NProgress.done()
})

export default router

```



```txt
/**
 * 其它业务模块路由（router/modules/other.ts）
 */

import type { RouteRecordRaw } from 'vue-router'

export const otherRoutes: RouteRecordRaw[] = [
  {
    path: '/about',
    name: 'About',
    component: () => import('@/views/About.vue'),
    meta: { layout: 'main', requiresAuth: true, title: '关于我们' }
  }
]

```



```
/**
 * 基础路由模块（router/modules/base.ts）
 * - 管理无需权限的公共页面路由
 */

import type { RouteRecordRaw } from 'vue-router'

export const baseRoutes: RouteRecordRaw[] = [
  {
    path: '/login',
    name: 'Login',
    component: () => import('@/views/Login.vue'),
    meta: { layout: 'auth', public: true, title: '登录' }
  },
  {
    path: '/',
    name: 'Home',
    component: () => import('@/views/Home.vue'),
    meta: { layout: 'main', requiresAuth: true, title: '首页' }
  }
  // 可扩展更多公共页面
]

```



mkdir src/views && touch src/views/AppHome.vue && touch src/views/AppLogin.vue && touch src/views/AppAbout.vue

```
<template>
  <MainLayout>
    <h1>欢迎，{{ userStore.displayName }}</h1>
    <p>这是首页内容。</p>
  </MainLayout>
</template>

<script setup lang="ts">
// 引入主layout
import MainLayout from '@/layouts/MainLayout.vue'
import { useUserStore } from '@/store/modules/user'

// 读取用户信息
const userStore = useUserStore()
</script>

<!-- 样式可选 -->

```

```
<template>
  <AuthLayout>
    <form class="login-form" @submit.prevent="handleLogin">
      <h2>登录</h2>
      <input v-model="username" placeholder="用户名" />
      <input v-model="password" type="password" placeholder="密码" />
      <button type="submit">登录</button>
      <div class="error" v-if="errorMsg">{{ errorMsg }}</div>
    </form>
  </AuthLayout>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import AuthLayout from '@/layouts/AuthLayout.vue'
import { useUserStore } from '@/store/modules/user'

const userStore = useUserStore()
const username = ref('')
const password = ref('')
const errorMsg = ref('')

const handleLogin = async () => {
  try {
    await userStore.login(username.value, password.value)
    window.location.href = '/'
  } catch  {
    errorMsg.value = '用户名或密码错误'
  }
}
</script>

<style scoped>
.login-form {
  display: flex;
  flex-direction: column;
  gap: 18px;
}
.error {
  color: red;
  margin-top: 8px;
}
</style>

```

```
<template>
  <MainLayout>
    <h1>关于我们</h1>
    <p>本项目遵循Google级代码分层规范。</p>
  </MainLayout>
</template>

<script setup lang="ts">
import MainLayout from '@/layouts/MainLayout.vue'
</script>

```



修改src/main.ts

```
/**
 * 应用主入口（main.ts）
 * - 注册Pinia、Router、ElementPlus等
 * - 最大化注释，团队友好
 */

import { createApp } from 'vue'
import App from './App.vue'
import router from '@/router'
import { pinia } from '@/store'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'


// 创建vue app
const app = createApp(App)

// 注册全局插件
app.use(pinia)
app.use(router)
app.use(ElementPlus)

// 挂载应用
app.mount('#app')

```

懒得写后端接口，postman可以定义伪接口。参照:https://www.cnblogs.com/surpassme/p/16489009.html

修改.env

```
VITE_API_BASE_URL=/
VITE_APP_TITLE=My Vite App
```

看数据变化的插件，Vue.js devtools



安装pinia-plugin-persistedstate

```
npm install pinia-plugin-persistedstate

```

修改pinia单例文件

```
// 引入并创建Pinia实例，便于主入口集成
import { createPinia } from 'pinia'
import piniaPersist from 'pinia-plugin-persistedstate'

// 导出Pinia实例
export const pinia = createPinia()
pinia.use(piniaPersist)

```

token过期，修改https

```
import axios  from 'axios'
import type { AxiosRequestConfig, AxiosResponse, AxiosError } from 'axios'
import router from '@/router' // 若用SPA跳转，否则可用location.href
import { useUserStore } from '@/store/modules/user'

const http = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 10000,
  headers: { 'Content-Type': 'application/json' }
})

http.interceptors.request.use(
  config => config,
  error => Promise.reject(error)
)

http.interceptors.response.use(
  (response: AxiosResponse) => {
    if (response.data && typeof response.data === 'object' && 'data' in response.data) {
      return response.data.data
    }
    return response.data
  },
  (error: AxiosError) => {
    // === 统一处理token过期/失效 ===
    // 1. 如果是http层401
    if (error.response && error.response.status === 401) {
      // 2. 清理用户store，自动登出
      const userStore = useUserStore()
      userStore.setLogout?.()
      // 3. 跳转登录页，防止死循环/多次重定向用replace
      router.replace('/login')
      // 或 window.location.href = '/login'
    }
    // 4. 也可以根据后端返回自定义错误码处理
    // if (error.response && error.response.data && error.response.data.code === 'TOKEN_EXPIRED') {
    //   ...
    // }
    return Promise.reject(error)
  })

// 重点是这一行的泛型
const get = <T>(url: string, config?: AxiosRequestConfig): Promise<T> => {
  return http.get<unknown, T>(url, config)
}

const post = <T>(url: string, data?: unknown, config?: AxiosRequestConfig): Promise<T> => {
  return http.post<unknown, T>(url, data, config)
}

export default { get, post }

```



权限业务，角色管理业务，权限规则业务实现，有可复用分页，输入页码跳转，loading功能，表单增删改查，对话框，

分析业务界面:

角色管理

权限管理

权限规则管理

用户管理



插件安装:Element Plus + vue-table-3 + vue-final-modal + vue-formily + nprogress

```
npm install vxe-table@next xe-utils vue-final-modal @formily/vue


cd src
mkdir -p types service store/module
touch types/permission.ts types/role.ts types/rule.ts
touch services/permission.ts services/role.ts services/rule.ts
touch store/modules/permission.ts store/modules/role.ts store/modules/rule.ts

```



这里我刚开始的时候以为interface是跟Java的静态类型差不多，但是后来发现，ts是没有对象类型的，这玩意叫结构约定，不占内存





后面就是写service，store，view，component,router。第一个写出来之后，就是通过第一个慢慢调整layout和main.ts。这样把整体的架子给搭起来，后续就基本是复用写好的界面。什么商城啊，学堂啊，会议啊，头条啊，基本全是这个底子走的。

业务的话，就是看自己的需求，基本上推荐用轮子是最好的，开箱即用。看业务，选择插件，编写代码，调整layout，router，https，main.ts等。基本新版vue3写业务差不多就这些。



商品列表，表格多选，批量上架和下架，excel表格的导入导出，添加商品，富文本编辑器，内容拖拽，商品规格，单规格处理，处理规格模板，编辑规格，生成多规格数据，多规格表格，删除规格行，批量设置，视图处理，删除规格列和行，动态添加规格列，处理添加规格获得焦点，添加新规格，规格拖拽排序。





部署打包，预览打包结果。GitHub，vercel,webify免费云服务器部署







# 2. react

## 2.1 vue和react的本质，就是通过虚拟dom进行提前diff，通过dom树自带增删改查修改一部分内容

## 2.2 redux的原理和使用，Mobx的原理和使用

## 2.3 钩子函数的原理和使用，Formik和yup的使用，css in js的使用。chakra-ui的使用，react优化性能的方法，reactSSR的原理和使用。next.js的原理和使用。gatsby和strapi的使用。



## 2.4 电商网站前端









































