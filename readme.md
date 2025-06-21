# u-springboot3-init

> **A Spring Boot 3.x multi-module starter template for back-end services.**  
> 开箱即用、按需组合、可快速二次开发的后端通用模板。

---

## ✨ 主要特性

- **Spring Boot 3.x + Java 17**：跟随 Spring 最新生态，利用现代语法特性。
- **多模块分层**：`common` 抽象通用能力、`starter` 负责启动、`modules` 承载业务逻辑，职责清晰、易于扩展。
- **依赖版本统一**：父 `pom` + `bom` 管理所有三方库版本，避免冲突。
- **开箱即用组件**：MyBatis-Plus、Redisson、MapStruct、Hutool、Lock4j、Sa-Token 等常用库一应俱全。
- **多环境配置**：`dev / prod` 双环境示例，可自由新增。
- **优雅异常 & 统一响应**：内置 `GlobalExceptionHandler`、`R<T>` 响应体及通用错误码。
- **最佳实践示例**：含首页 `IndexController`、分页 DTO、公用工具类等常用代码范例。

---

## 🗂️ 目录结构

```
u-springboot3-init
│
├── pom.xml                  # 根 POM，统一依赖 / 插件 / 版本
├── readme.md                # 项目说明（本文档）
│
├── xiaou-common             # 通用能力层
│   ├── xiaou-common-bom     # <dependencyManagement> 统一版本
│   ├── xiaou-common-core    # 常量 / 响应体 / 异常 / 工具类
│   ├── xiaou-common-web     # Web & Undertow 基础封装
│   ├── xiaou-common-mybatis # MyBatis-Plus 及插件整合
│   └── xiaou-common-redis   # Redis & Redisson & 缓存封装
│
├── xiaou-starter            # 项目启动模块（入口）
│   └── src/main/resources   # application.yml & 多环境配置
│
└── xiaou-modules            # 业务模块集合（预留）
```

---

## 📦 模块功能说明

| 模块 | Maven 坐标 | 作用 | 主要内容 |
| ---- | ---------- | ---- | -------- |
| **父项目** | `com.xiaou:u-springboot3-init` | 版本 & 依赖集中管理 | Java 17、Spring Boot 3.4.4、常用插件版本定义 |
| **xiaou-common-bom** | `com.xiaou:xiaou-common-bom` | common 系列依赖版本对齐 | 将 core / web / mybatis / redis 版本纳入统一 bom |
| **xiaou-common-core** | `com.xiaou:xiaou-common-core` | 业务无关的核心工具 | 常量、统一响应 `R<T>`、分页 DTO、异常体系、Hutool & MapStruct 工具桥接 |
| **xiaou-common-web** | `com.xiaou:xiaou-common-web` | Web 通用封装 | Undertow 容器、全局 CORS、JSON 配置等 |
| **xiaou-common-mybatis** | `com.xiaou:xiaou-common-mybatis` | 数据层封装 | MyBatis-Plus Starter、分页插件、P6Spy SQL 日志 |
| **xiaou-common-redis** | `com.xiaou:xiaou-common-redis` | 分布式缓存 & 锁 | Redisson、Lock4j、Caffeine、本地缓存工具 |
| **xiaou-starter** | `com.xiaou:xiaou-starter` | 启动与样例 | `Application.java` 入口、首页 `/` 接口、环境配置示例 |
| **xiaou-modules** | *(空)* | 业务实现 | 推荐一业务一模块，互相隔离，按需依赖 *common* 能力 |

---

## 🚀 快速开始

### 1. 环境要求

| 工具 | 版本 |
| ---- | ---- |
| JDK  | 17+  |
| Maven| 3.9+ |
| MySQL| 8.x (示例配置) |
| Redis| 6.x / 7.x |

> **Windows 用户**请确保 `JAVA_HOME` & `MAVEN_HOME` 已正确配置。

### 2. 克隆项目

```bash
git clone https://github.com/your-org/u-springboot3-init.git && cd u-springboot3-init
```

### 3. 修改配置

编辑 `xiaou-starter/src/main/resources/application-dev.yml`：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/your_db?useSSL=false&serverTimezone=Asia/Shanghai
    username: root
    password: root
  redis:
    host: 127.0.0.1
    port: 6379
```

> 生产环境请完善 `application-prod.yml`（示例已提供 Redisson 单机配置）。

### 4. 启动

```bash
# ⽗ POM 会聚合编译所有模块
mvn -pl xiaou-starter spring-boot:run
# 或直接运行主类
mvn -pl xiaou-starter -am package -DskipTests
java -jar xiaou-starter/target/xiaou-starter.jar
```

访问 <http://localhost:8080/>

```
欢迎使用 u-springboot3-init，请通过前端地址访问。
```

---

## 🔧 常用脚本 & 指令

| 场景 | 命令 |
| ---- | ---- |
| 打包多模块 | `mvn clean package -DskipTests` |
| 仅打包 starter | `mvn -pl xiaou-starter -am clean package` |
| 变更版本号 | `mvn versions:set -DnewVersion=1.1.0` |

---

## 🧩 如何扩展业务模块

1. 在 `xiaou-modules` 下新建模块目录，例如 `user-service`。
2. 创建 `pom.xml`，父 `<parent>` 指向根项目，并依赖 `xiaou-common-*` 所需模块。
3. 在 `src/main/java` 下编写 Controller / Service / Mapper。
4. 可在 `xiaou-starter/pom.xml` 中添加 `<dependency>` 引入新模块，或让 starter 以 *spring.factories* 自动装配。

```xml
<dependency>
  <groupId>com.xiaou</groupId>
  <artifactId>user-service</artifactId>
  <version>${revision}</version>
</dependency>
```

> 按领域拆分模块，可显著提升可维护性与团队协作效率。

---

## 📖 核心类速查

| 类/接口 | 包 | 作用 |
| ------- | --- | ---- |
| `R<T>` | `com.xiaou.common.domain` | 统一响应封装，链式 `ok / fail / warn` 方法 |
| `ErrorCode` | `com.xiaou.common.exception` | 全局错误码枚举 |
| `GlobalExceptionHandler` | `com.xiaou.common.exception` | 统一异常捕获 & 日志打印 |
| `PageReqDto / PageRespDto` | `com.xiaou.common.page` | 通用分页请求 / 响应 DTO |
| `RedisUtils` | `com.xiaou.redis.utils` | Redis 通用操作封装 |
| `MybatisPlusConfig` | `com.xiaou.mybatis.config` | MyBatis-Plus 分页配置示例 |

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🙏 致谢

本项目参考了业界众多优秀实践，感谢社区作者的分享。如果本模板对您有帮助，请 **Star ⭐** 支持！
