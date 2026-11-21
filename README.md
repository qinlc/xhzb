
<h1 align="center" style="margin: 30px 0 30px; font-weight: bold;">RuoYi v3.9.0</h1>
<h4 align="center">基于SpringBoot+Vue前后端分离的Java快速开发框架</h4>
<p align="center">
	<a href="https://gitee.com/y_project/RuoYi-Vue/stargazers"><img src="https://gitee.com/y_project/RuoYi-Vue/badge/star.svg?theme=dark"></a>
	<a href="https://gitee.com/y_project/RuoYi-Vue"><img src="https://img.shields.io/badge/RuoYi-v3.9.0-brightgreen.svg"></a>
	<a href="https://gitee.com/y_project/RuoYi-Vue/blob/master/LICENSE"><img src="https://img.shields.io/github/license/mashape/apistatus.svg"></a>
</p>

## 平台简介

若依是一套全部开源的快速开发平台，毫无保留给个人及企业免费使用。

* 前端采用Vue3、Element Plus。
* 后端采用Spring Boot、Spring Security、Redis & Jwt。
* 权限认证使用Jwt，支持多终端认证系统。
* 支持加载动态权限菜单，多方式轻松权限控制。
* 高效率开发，使用代码生成器可以一键生成前后端代码。
* 提供了技术栈（[Vue3](https://v3.cn.vuejs.org) [Element Plus](https://element-plus.org/zh-CN) [Vite](https://cn.vitejs.dev)）版本[RuoYi-Vue3](https://gitcode.com/yangzongzhuan/RuoYi-Vue3)，保持同步更新。

## 内置功能

1.  用户管理：用户是系统操作者，该功能主要完成系统用户配置。
2.  部门管理：配置系统组织机构（公司、部门、小组），树结构展现支持数据权限。
3.  岗位管理：配置系统用户所属担任职务。
4.  菜单管理：配置系统菜单，操作权限，按钮权限标识等。
5.  角色管理：角色菜单权限分配、设置角色按机构进行数据范围权限划分。
6.  字典管理：对系统中经常使用的一些较为固定的数据进行维护。
7.  参数管理：对系统动态配置常用参数。
8.  通知公告：系统通知公告信息发布维护。
9.  操作日志：系统正常操作日志记录和查询；系统异常信息日志记录和查询。
10. 登录日志：系统登录日志记录查询包含登录异常。
11. 在线用户：当前系统中活跃用户状态监控。
12. 定时任务：在线（添加、修改、删除)任务调度包含执行结果日志。
13. 代码生成：前后端代码的生成（java、html、xml、sql）支持CRUD下载 。
14. 系统接口：根据业务代码自动生成相关的api接口文档。
15. 服务监控：监视当前系统CPU、内存、磁盘、堆栈等相关信息。
16. 缓存监控：对系统的缓存信息查询，命令统计等。
17. 在线构建器：拖动表单元素生成相应的HTML代码。
18. 连接池监视：监视当前系统数据库连接池状态，可进行分析SQL找出系统性能瓶颈。

## 在线体验

- admin/admin123  

演示地址：http://vue.ruoyi.vip  
文档地址：http://doc.ruoyi.vip

## 升级内容

- springboot版本升级为3.5.0
- jdk版本升级为17
- mybatis版本升级为3.0.3
- spring-framework版本升级为6.2.7

## 环境搭建

本项目是基于 RuoYi-Vue 二次开发的「星海智伴」养老护理平台，模块名与启动类均已重命名为 `xhzb-*`，与若依模板不再一致，请按下面的实际路径配置。

### 一、环境要求

| 组件 | 版本 | 说明 |
| --- | --- | --- |
| JDK | 17 | 根 `pom.xml` 中 `java.version=17`，低于 17 无法编译 |
| Maven | 3.6+ | 建议在 IDEA 中配置 Maven 与本地仓库，依赖会自动下载 |
| MySQL | 8.0 | 建库字符集使用 `utf8mb4` |
| Redis | 5.0+ | 登录 Token、缓存与 AI 向量库均依赖 Redis |
| Node.js | 16+ | 建议 18 或 20，用于启动前端工程 `xhzb_ui` |
| Ollama | 可选 | 仅智能问答功能需要，见第五节 |

### 二、初始化数据库

1. 在 MySQL 中新建数据库 `xhzb`（`utf8mb4`）。

2. 依次导入 `sql` 目录下的两个脚本：

   | 顺序 | 脚本 | 用途 |
   | --- | --- | --- |
   | 1 | `sql/xhzb.sql` | 建库建表与初始化数据（约 200 KB） |
   | 2 | `sql/contract_job.sql` | 注册「合同状态定时更新」定时任务，需在主脚本导入后执行 |

   > ⚠️ `sql/xhzb.sql` 开头包含 `DROP DATABASE IF EXISTS \`xhzb\``，会先删除同名库再重建。**不要在已有数据的环境上直接执行**，导入前请先备份。

### 三、配置并启动后端

配置分为两个文件，都在 `xhzb-admin/src/main/resources/` 下：

- `application.yml`：项目名、Token、MyBatis-Plus、Swagger 等公共配置
- `application-dev.yml`：数据源、Redis、端口（`spring.profiles.active` 默认为 `dev`）

`application-dev.yml` 中的默认值：

```yaml
server:
  port: 8080                     # 后端 HTTP 端口

spring:
  datasource:
    driverClassName: com.mysql.cj.jdbc.Driver
    url: ${DB_URL:jdbc:mysql://192.168.100.168:3306/xhzb?...}   # 默认指向开发内网库
    username: ${DB_USERNAME:root}
    password: ${DB_PASSWORD:heima123}
  data:
    redis:
      host: ${REDIS_HOST:192.168.100.168}
      port: ${REDIS_PORT:6378}   # 注意：这里是 6378，不是默认的 6379
      password: ${REDIS_PASSWORD:}
```

修改方式有两种，任选其一：

1. 直接改 `application-dev.yml` 里的默认值；
2. 不改动代码，用环境变量覆盖：`DB_URL`、`DB_USERNAME`、`DB_PASSWORD`、`REDIS_HOST`、`REDIS_PORT`、`REDIS_PASSWORD`。

启动类为：

```
xhzb-admin/src/main/java/com/xhzb/XhzbApplication.java
```

在 IDEA 中运行该类的 `main` 方法，控制台出现下面的启动成功图形即表示后端启动完成（同时会打印 Tomcat 监听 8080 端口）：

```sh
(♥◠‿◠)ﾉﾞ  若依启动成功   ლ(´ڡ`ლ)ﾞ  
 .-------.       ____     __        
 |  _ _   \      \   \   /  /    
 | ( ' )  |       \  _. /  '       
 |(_ o _) /        _( )_ .'         
 | (_,_).' __  ___(_ o _)'          
 |  |\ \  |  ||   |(_,_)'         
 |  | \ `'   /|   `-'  /           
 |  |  \    /  \      /           
 ''-'   `'-'    `-..-'    
```

### 四、配置并启动前端

前端是独立工程，位于根目录下的 `xhzb_ui`，**不在后端 Maven 模块中**，需要单独安装依赖：

```bash
cd xhzb_ui

# 二选一，不要混用两种包管理器
npm install       # 使用仓库中的 package-lock.json
# 或
pnpm install      # 使用仓库中的 pnpm-lock.yaml

npm run dev
```

启动后访问 `http://localhost:9001`（端口在 `xhzb_ui/vite.config.js` 的 `server.port` 中定义，为 9001）。

前端通过 Vite 代理转发 `/dev-api` 前缀的请求到后端，代理目标取自 `.env.development` 中的 `VITE_APP_PROXY_TARGET`：

```bash
VITE_APP_PROXY_TARGET = 'http://192.168.100.168:9000'
```

如果后端跑在本机默认 8080 端口，请把它改成 `http://localhost:8080`，否则页面能打开但所有接口请求会 404。

### 五、AI 能力（可选）

智能问答与知识库检索依赖大模型和向量库，默认走本机 Ollama（`application.yml` 中 `spring.ai`）：

- 地址：`http://127.0.0.1:11434`
- 对话模型：`qwen2.5:7b`，向量模型：`mxbai-embed-large`
- 向量库存储在 Redis 中，首次启动会自动建索引

不启用 AI 功能时，其余业务模块可正常使用。如需切换为百炼（DashScope）兼容接口，通过 `AI_CHAT_PROVIDER`、`AI_EMBEDDING_PROVIDER`、`OPENAI_API_KEY` 环境变量覆盖。

### 六、默认账号

- 账号：`admin` 密码：`admin123`

### 七、常见问题

| 现象 | 定位 |
| --- | --- |
| 启动报数据库连接失败 | 检查 `application-dev.yml` 中的数据源，默认指向 `192.168.100.168`，需改为本机 |
| 启动报 Redis 连接失败 | 注意默认端口为 `6378`，与常见的 `6379` 不同 |
| 前端页面能打开但接口 404 | 检查 `.env.development` 的 `VITE_APP_PROXY_TARGET` 是否指向本机 8080 |
| 前端端口不是 9001 | 检查 `vite.config.js` 的 `server.port`，或启动时 9001 被占用 |