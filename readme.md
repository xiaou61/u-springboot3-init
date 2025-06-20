# u-springboot3-init

本项目是基于 Spring Boot 3 的企业级项目模板，采用多模块分层设计，集成常用开发组件，适合快速二次开发和企业级项目起步。

## 目录结构

```
u-springboot3-init/
├── pom.xml                   # 父项目，统一依赖与版本管理
├── xiaou-common/             # 通用基础类与工具包
│   ├── xiaou-common-bom/     # common依赖项BOM管理
│   ├── xiaou-common-core/    # 核心工具与通用响应、异常、常量等
│   ├── xiaou-common-web/     # Web相关通用封装
│   ├── xiaou-common-mybatis/ # MyBatis-Plus相关扩展
│   └── xiaou-common-redis/   # Redis与Redisson相关扩展
├── xiaou-starter/            # 启动模块，主程序入口
├── xiaou-modules/            # 业务模块扩展（可自定义添加）
└── readme.md                 # 项目说明文档
```

## 各模块说明

### 1. 父项目（u-springboot3-init）

- 统一管理所有子模块依赖、版本、插件配置。
- 采用 Java 17，Spring Boot 3.4.4，集成常用三方库（Hutool、Lombok、MyBatis-Plus、Redisson、MapStruct等）。
- 通过 `dependencyManagement` 实现依赖版本一致性。

### 2. xiaou-common（通用基础包）

#### - xiaou-common-bom
- 统一管理 common 相关依赖版本，便于多模块依赖一致。

#### - xiaou-common-core
- 提供全局常量（如HttpStatus、GlobalConstants）、通用响应体（R.java）、分页对象（PageReqDto/PageRespDto）、全局异常（ErrorCode/ServiceException/GlobalExceptionHandler）、常用工具类（DateUtils、JsonUtils、StringUtils等）。
- 适用于所有业务模块的基础能力复用。

#### - xiaou-common-web
- 封装 Web 层常用依赖，默认使用 Undertow 作为 Web 容器（性能优于 Tomcat）。
- 依赖 core，便于 Web 层直接使用通用能力。

#### - xiaou-common-mybatis
- 集成 MyBatis-Plus，提供分页、性能分析、SQL解析等能力。
- 便于业务模块直接使用 MyBatis-Plus 相关功能。

#### - xiaou-common-redis
- 集成 Redisson、Lock4j、Caffeine 等，支持分布式锁、缓存等能力。
- 提供 Redis 工具类和配置属性，便于快速集成 Redis 相关功能。

### 3. xiaou-starter（启动模块）

- 主程序入口，包含 `Application.java` 启动类。
- 默认提供首页接口（`/`），返回欢迎信息。
- 依赖所有 common 子模块，开箱即用。
- 配置多环境支持（application.yml、application-dev.yml、application-prod.yml），可根据 `spring.profiles.active` 切换环境。

### 4. xiaou-modules（业务模块扩展）

- 预留业务模块扩展目录，便于后续按需添加具体业务实现。
- 推荐每个业务领域单独建模块，保持项目解耦和可维护性。

## 启动方式

1. 配置数据库、Redis等连接信息（见 `xiaou-starter/src/main/resources/application-*.yml`）。
2. 进入 `xiaou-starter` 目录，运行：
   ```bash
   mvn spring-boot:run
   ```
   或直接用 IDE 运行 `com.xiaou.web.Application` 主类。

3. 访问 http://localhost:8080/ ，可见欢迎信息。

## 配置说明

- `application.yml`：基础配置，默认激活 `dev` 环境。
- `application-dev.yml`：开发环境配置，含数据库、Redis等连接信息。
- `application-prod.yml`：生产环境配置，建议上线前完善安全配置。
- 支持多环境切换，便于开发、测试、生产部署。

## 设计亮点

- **分层解耦**：通用能力与业务实现分离，便于维护和扩展。
- **依赖管理**：BOM统一依赖版本，避免冲突。
- **开箱即用**：集成常用开发组件，减少重复造轮子。
- **多环境支持**：配置灵活，适应不同部署场景。
- **高扩展性**：业务模块可按需扩展，适合中大型项目演进。

## 适用场景

- 企业级后端服务快速搭建
- 需要统一规范、通用能力复用的项目
- 适合团队协作和持续集成

---
