

# 1. 万能技术栈美企占比Prompt（可嵌入每个步骤）

### 一键化嵌入举例

#### 1. 立项与需求收集

```
请列出美企“项目Kickoff/需求管理”常用工具2组，每组1-3个，并标注美国公司使用率%：
表格格式输出，主流+创新型组合。
```

#### 2. 前台需求文档

```
请列出美企“原型/Mockup/用例设计”常用工具2组，每组1-3个，带美国公司使用率%：
输出主流方案和创新方案。
```

#### 3. 后台需求文档

```
美国公司接口定义/权限建模常用技术/工具，请推荐两组并给出各自使用率%，并指出创新型可选方案。
```

#### 4. 系统设计文档

```
美企常用架构图/系统设计工具推荐两组，每组1-3个，按公司使用率%降序输出。
```

#### 5. 数据设计文档

```
请列出美国主流的数据建模/数据库选型两组方案，分别标明美国公司使用率（%），表格输出。
```

#### 6. 开发流程



```
你是美国主流大厂CTO，针对开发流程，包括代码开发，包括前后端开发步骤，比如说前端从view层开始开发，后端从sql，repository层开始开发，给出具体步骤，并且是美国公司最广泛使用的步骤即可。请表格输出，并简要推荐组合逻辑。
```

收到——严格按“美国大厂主流开发流程”，**涵盖前后端各典型分层**，只保留**最广泛被采纳的实际步骤**，每步附“常用技术/工具”，并表格展现。最后补**主流组合逻辑推荐**。

------

### 美国公司主流开发流程分步（含前后端）

| 步骤                | 前端主流做法/工具             | 后端主流做法/工具                      | 使用率（%） |
| ------------------- | ----------------------------- | -------------------------------------- | ----------- |
| 1. 需求/任务分解    | Figma/Storybook拆分组件       | Jira任务分解，API接口定义(OpenAPI)     | 70–85%      |
| 2. 设计review       | Figma设计走查，交互批注       | 数据库ER图审查，架构review(Lucidchart) | 60–80%      |
| 3. 代码初始化       | Create React App/Next.js/Vite | Spring Boot/Express.js新项目模板       | 75–90%      |
| 4. view层开发       | React/Vue开发UI组件           |                                        | 90%+        |
| 5. 状态管理         | Redux/Context API             |                                        | 80%         |
| 6. 接口集成         | Axios/Fetch对接REST API       | API实现（RESTful/GraphQL）             | 90%         |
| 7. 本地mock/调试    | MSW/Mock Service Worker       | Postman/Swagger mock服务               | 65–80%      |
| 8. 业务逻辑开发     | UI事件/数据处理/表单校验      | Repository层/Service层/业务代码        | 80–90%      |
| 9. 数据访问         |                               | ORM（Prisma/TypeORM/Sequelize）        | 70%         |
| 10. 单元测试        | Jest/Testing Library          | Jest/Mocha/JUnit/pytest                | 80%         |
| 11. 代码提交        | Git + PR/Code Review          | Git + PR/Code Review                   | 95%+        |
| 12. 自动化集成      | GitHub Actions/CircleCI       | Jenkins/GitHub Actions/Travis CI       | 75–90%      |
| 13. 预发布/测试环境 | Vercel/Netlify/S3预览部署     | Docker Compose/K8s test环境            | 60–80%      |
| 14. QA/验收         | Cypress/E2E自动化测试         | 集成测试/自动化验收（Postman等）       | 60%         |
| 15. 正式上线        | CloudFront/CDN生产部署        | K8s/Docker生产发布                     | 70–80%      |
| 16. 监控/报警       | Sentry/Datadog/LogRocket      | Prometheus/Grafana/Sentry/Datadog      | 60–75%      |



------

### 推荐组合逻辑

- 前端：**Figma+React+Redux+MSW+Jest+GitHub Actions+Vercel+Sentry**
  - 典型硅谷SaaS与B2B公司主流，前后端解耦，Mock加快联调。
- 后端：**Spring Boot/Express+OpenAPI+Prisma/TypeORM+Jest/JUnit+GitHub Actions+K8s+Prometheus**
  - JavaScript（Express）与Java（Spring Boot）最普及，配合ORM提升数据层开发速度，全链路监控。

> 逻辑核心：
>
> 1. 需求-设计-开发-测试-集成-发布-监控完整闭环；
> 2. 所有环节工具均为美国大厂最常用、易团队协作与标准化、自动化程度高；
> 3. 分层明确、测试与CI/CD嵌入每步，保证高质量交付和高效协作。

------

如需**流程自动化Prompt**或**全栈一步到位技术栈配比**，可继续追问。



1. 需求分析，并制作前台和后台需求文档，系统设计文档



2. 制作数据库设计文档



3. 根据系统设计文档，新建相应的后端模块，按顺序编写bom模块，common模块

```
你是美国主流大厂CTO，针对common模块，编写或者改造相应的ResponseDTO和ResultCode代码。要求美国公司最广泛使用的即可，尽量使用装饰器使得代码简短。
```

常用的工具代码编写完成之后，可以install，对common模块进行安装

然后发现pom包出现高危漏洞

```
你是谷歌cio，这是我现在的两个pom，我总感觉有问题，能帮我修改一下吗?要求美国公司使用率百分比排序最高
```

然后修改成功后，发现很多地方版本杂乱，且没有用装饰器，好的，重新编写和改造好之后，重新打包，成功。

继续修改成国际主流，遇到问题，[ERROR] Target option 1.5 is no longer supported. Use 1.6 or later

mvn -v

java -v 

查看环境变量，正确后，可正确允许



好的吧，发现想直接和国际接轨，工程量会很大。目前只能先用国内的方式了。

通用包，common，bom，正常写utill代码，和responseDTO，然后写eureka，pom

包报红，排除切换jdk的版本，发现jdk切换没有生效，最上面的先生效

![image-20250614141911816](3.第三份工作，github做开源项目内容.assets/image-20250614141911816.png)



然后发现还是报红，但是可以运行，发现是idea无法识别引用，需要显式指定插件版本

![image-20250614143718077](3.第三份工作，github做开源项目内容.assets/image-20250614143718077.png)



然后提交代码，

```
# 1. 配置身份
git config --global user.name  "sww"
git config --global user.email "1248002362%40qq.com"

# 2. 确认在项目根目录
cd /d/code/project-root/frontend

# 3. 初始化（如果还没 init）
git init

# 4. 添加远程（如果已经添加过可以跳过）
git remote add origin https://github.com/dddd56656/swwBaseProject-front.git

# 5. 暂存、提交
git add .
git commit -m "Initial commit"

# 6. 重命名分支为 main
git branch -M main

# 7. 推送并关联
git push -u origin main --force

账号:dddd56656
密码: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


git config --global http.proxy http://127.0.0.1:10808
git config --global https.proxy http://127.0.0.1:10808
```



发现gitignore没发挥作用，删除已提交的代码追踪

```
git rm -r --cached .idea/
git commit -m "remove .idea from repo"

```

然后开始写config模块，新建一个repository来放配置文件,又被墙了...

下一个Proxifier，然后使用，成功



接下来写广告模块，先在test里面写代码生成器，自动化生成代码，节约时间。然后发现插件导入失败，这是低版本idea的bug没修，手动添加版本即可，添加完就可以删除。然后因为改了很多pom，重新reimport一下。寄，发现是pom的组合插件出了问题。需要升级。

你是谷歌cio，在我给你的pom插件组合基础上升级，列出五种升级组合版本，要求给出使用率百分比排序，要求不需要改变原有代码，要求符合谷歌标准，



```
你是谷歌cio，在我给你的pom插件组合基础上升级，列出五种升级组合版本，要求给出使用率百分比排序，要求不需要改变原有代码，要求符合谷歌标准，


你是谷歌cio，基于第一个对我的pom进行升级，要求符合谷歌cto标准
```

然后发现了一些缺包，找相应的GitHub地址，然后有readme看readme,没有就编译install，然后整完之后，清空缓存，重启idea。install,找报错，比run更加精准。然后自动导包，

![image-20250618025716844](3.第三份工作，github做开源项目内容.assets/image-20250618025716844.png)



编写广告单表代码，注册到eureka并对外暴露。然后新建font模块，  强行拉取依赖。



```
for f in $(find . -name pom.xml); do
  dir=$(dirname "$f")
  (cd "$dir" && mvn clean install -U)
done

```







#### 7. 部署与上线流程

```
请给出美国公司主流与创新型“容器化/部署/监控”技术组合，并给出各项使用率（%），表格输出。
```

------

## 终极总模板（万能复制）

> 你是美国主流大厂CTO，针对【**具体环节/技术目标/场景**】，请输出两组当前美国公司主流使用的技术栈/工具组合（每组1-3项），并给出每项工具在美国公司的最新使用率（%），主流与创新型各一组。请表格输出，并简要推荐组合逻辑。

------

# 2. 快速改造项目

你是谷歌cio，给出谷歌内部快速改造项目的具体步骤，要求尽可能简单，我智商很低，要求是开发人员的具体步骤，要求是改造，意思是原本有一个项目，对原有项目进行改造，开发人员之前没有接触过，，要求是纯新人，啥也不懂，需求也不懂那种，要求符合谷歌标准。



好的，人类，这里是一个**符合谷歌标准**、专为**“纯新人”开发人员**设计的“已有项目快速改造”**最简化执行流程**。
 这是内部工程团队常用的“Bootstrapped Rebuild Plan”，**目标是把“啥也不懂”的人变成可以动手改项目的人**。

------

## ✅ 项目改造最小化流程（新人版）

> 面向场景：已有项目，需要新人开发者快速上手并执行**功能性或架构性改造**。
>  假设：对业务逻辑不熟、对代码不了解、没有背景知识。

------

### 🔧 第一步：环境与文档检查

简单来说就是快速把项目跑起来，并且能够自动化部署

后端:

```
for f in $(find . -name pom.xml); do
  dir=$(dirname "$f")
  (cd "$dir" && mvn clean install -U)
done
```

然后慢慢修   或者有仓库的话，直接找人要个maven

前端:

```
npm install
```

或者 有bash.sh,直接跑脚本





#### 1. 克隆项目

```bash
git clone <项目地址>
cd <项目目录>
```

#### 2. 本地跑起来

- 找到 `README.md` 或 `docs/`，确保能跑起来（即 `npm start` / `bazel run` / `docker-compose up` 成功）
- 如果没有文档，看有没有 `start.sh`、`main.py`、`Makefile` 等。
- **改造项目前必须能成功运行原始项目！**

#### 3. 查 CI/CD 状态

- 是否有 `.github/workflows/` 或 `BUILD.bazel`？
- CI 是用 Jenkins、GitHub Actions、Cloud Build 还是其他？
- 目标：确定项目部署方式和自动测试方式。

------

### 🧭 第二步：需求确认与改造目标

简单来说就是把项目改造成自己的项目，删除或者替换一些东西。

首先是改名

```
find . -type d -name "*lagou*" | while read dir; do
  newdir=$(echo "$dir" | sed 's/lagou/sww/g')
  mv "$dir" "$newdir"
done

```

你是谷歌cio，给出缺陷反馈和优化反馈，给出优化结构并注释，要求符合谷歌标准,要求符合eslint标准，要求使用官方瓦片图层，要求最大化注释

# 2.1. 前端开发

你是谷歌cto，分析下面需求，要求符合谷歌标准，数据应该是mock.js插件，然后通过api层代码从mock.js拿取，传递到store层，再传递到view层



你是谷歌cto，根据上面需求仅写出相应的mock层代码，要求使用Mock 数据和mock.js插件，要求符合谷歌标准，要求能够通过api层代码从mock.js拿取，传递到store层，再传递到view层，要求最大化注释，要求只需要写出mock层代码



你是谷歌cto，根据写出上面mock层代码对应的api层代码，要求符合谷歌标准，要求能够通过api层代码从mock.js拿取，传递到store层，再传递到view层，要求最大化注释，要求只需要写出api层代码



你是谷歌cto，根据写出相应的store层代码，要求符合谷歌标准，要求能够通过api层代码从mock.js拿取，传递到store层，再传递到view层，要求最大化注释，要求只需要写出store层代码



你是谷歌cto，根据写出相应的view层代码，要求符合谷歌标准，要求能够通过api层代码从mock.js拿取，传递到store层，再传递到view层，要求最大化注释，要求只需要写出view层代码



你是谷歌cto，根据写出相应的router层代码，要求符合谷歌标准





你是谷歌cio，根据下面需求写出相应的view层代码，要求符合谷歌标准

#需要注意版本是否匹配，vue2推荐用mock.js

npm install mockjs --save-dev

npm install --save msw

你是谷歌cio，给出对应的前端store层代码，要求使用率最高，要求符合谷歌标准，要求符合eslint标准，要求最大化注释









#### 4. 读取 issue 或 PR 描述

- 去 GitHub / GitLab 的 Issues 或 Jira 找“这个改造”是啥内容。
- 不懂就贴文字问团队负责人：“这个改造的目标具体是？有没有原型？”

#### 5. 制作最简改造任务单

```md
## 改造任务
目标：让 XX 功能支持 YY 场景
输入：当前版本为 XXX，已有模块 ABC
输出：增加支持 ZZ 的功能
验收：修改后通过测试 + UI 界面能操作
```

------

### 🔍 第三步：代码结构认知

简单来说，就是在原有统一结构的基础上，进行代码编写。



#### 6. 画一张代码结构图

- `src/` 下各目录是干什么的？
- 哪些是前端？哪些是后端？API 调用从哪发起？
- 推荐用 [excalidraw.com](https://excalidraw.com/) 或 Miro 快速画出结构图。

#### 7. 把主流程打印出来

- 找“入口函数”，比如 `main()`、`App.tsx`、`router.js` 等
- 插入 `console.log()` 或 `print()` 看执行流程。
- **记录：主流程跑哪几个模块，调用哪些接口。**

------

### 🔨 第四步：实际改造开发

简单来说，就是在充分掌握原系统后，对系统进行升级





#### 8. 复制旧功能写新逻辑

- 找到与需求最相关的已有模块（如表单、接口、页面）
- 复制一份做新逻辑，**不要直接删旧代码**，先做隔离
- **逐步调整逻辑，确保每次小步修改都能跑通测试**

#### 9. 添加最小单元测试

- 使用项目已有测试框架（如 Jest、Go test、PyTest）
- 写一个“输入→输出”的测试例子验证功能正确

------

### ✅ 第五步：测试验证与合并（Day 7）

简单来说，就是改完之后，需求达成，完成提交和部署。



#### 10. 本地 + 自动化测试

- 跑一遍全部测试（一般是 `npm test` 或 `bazel test //...`）
- 如果没写测试，就手动点界面，录像或截图交验。

#### 11. 提交 PR

```bash
git checkout -b feat/xx改造
git add .
git commit -m "feat: 改造 XX 模块支持 YY"
git push origin feat/xx改造
```







# 3. 遇到问题



## 1.报插件版本不对

去报错依赖位置写死版本

