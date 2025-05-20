# 智慧教学辅助系统 – 后端项目

## 项目简介和用途

智慧教学辅助系统 是一个基于Spring Boot 的 Java 后端示例项目，采用经典的SSM架构（Spring、SpringMVC、MyBatis）整合开发，旨在提供部门、员工等信息的管理功能。项目通过 Spring Boot 简化配置，展示了控制器、服务层和数据访问层的分层设计，同时集成了登录认证、文件上传等功能。此项目适合作为学习 Spring Boot 与 MyBatis 整合的案例，也可用作小型企业后台管理系统的基础骨架。

## 技术栈说明

* **Spring Boot**：使用Spring Boot快速搭建项目骨架，简化配置和依赖管理。
* **Spring MVC**：负责请求映射和处理，控制器层使用注解（如@RestController、@GetMapping）定义接口。
* **MyBatis**：作为 ORM 框架，用于对象–关系映射和数据库访问，Mapper 接口通过注解或 XML 映射SQL。
* **Lombok**：用于简化实体类（POJO）的编写，如自动生成 Getter/Setter、构造方法和日志对象。
* **数据库**：MySQL（使用 `com.mysql.cj.jdbc.Driver` 驱动），数据库中创建 `tlias` 库，包含 `dept` 和 `emp` 表（见下表）。
* **安全与工具**：集成 JWT 作为认证方式（登录成功后颁发令牌）；使用 Spring AOP 进行日志记录/事务处理；通过 PageHelper 分页查询；文件上传功能接入阿里云 OSS 存储。
* **其他**：使用 Maven 构建项目（Spring Boot 父 POM 管理依赖）、Spring AOP 异常处理机制等，配置文件采用 `application.properties/yml` 统一管理应用参数。

## 模块说明

* **部门管理（Dept）** – 包含 `DeptController`（部门管理接口）、`DeptService` 及其实现类、`DeptMapper`（MyBatis 映射接口）和实体类 `Dept`。该模块提供部门信息的增删改查功能。例如，`DeptController` 通过 `DeptService.list()` 方法获取部门列表并返回给前端。
* **员工管理（Emp）** – 包含 `EmpController`、`EmpService`/`EmpServiceImpl`、`EmpMapper` 和实体类 `Emp`。主要负责员工信息的 CRUD 操作以及分页查询。`EmpController` 定义了新增、删除、更新和分页查询等接口，通过调用 `EmpService` 层方法实现业务逻辑。
* **用户登录** – 包含 `LoginController`，用于处理用户登录请求。前端提交用户名密码后，`LoginController` 调用 `EmpService.login(emp)` 验证用户信息；若验证成功，则使用 `JwtUtils.generateJwt(...)` 生成 JWT 令牌并返回给前端，否则返回错误提示。
* **文件上传** – 包含 `UploadController` 和辅助工具类 `AliOSSUtils`。`UploadController` 接收前端上传的图片文件，调用 `AliOSSUtils.upload()` 方法将文件上传至阿里云 OSS，并返回文件访问 URL。项目也预留了本地存储方案，但默认使用云存储以提高可靠性。
* **其他类** – 例如 `Result` 响应封装类统一返回结果；工具类层包括 `JwtUtils`（生成/解析 JWT）和 `AliOSSUtils`（阿里云 OSS 操作）等。这些模块和类一起构成了后端的完整功能。

## 开发环境要求

* **JDK**：需要 Java 1.8 或更高版本环境。
* **构建工具**：Maven 3.x，用于依赖管理和项目构建。
* **数据库**：MySQL 5.x（5.7+）或以上版本。需创建名为 `tlias` 的数据库，并执行 `dept` 和 `emp` 表结构（如下所示）。例如，`dept` 表用于存储部门信息；`emp` 表用于存储员工信息（包含用户名、密码、姓名、性别、岗位、入职日期、所在部门ID等字段）。在 `application.properties` 中配置好数据库连接（示例：`spring.datasource.url=jdbc:mysql://localhost:3306/tlias`）。
* **其他**：开发时推荐使用支持 Spring Boot 的 IDE（如 IntelliJ IDEA、Eclipse），配置好 Lombok 插件。后端项目无需前端环境（前端可使用独立的 Vue/Nginx 项目），运行时按常规 Spring Boot 应用启动即可。启动项目后，接口可通过 Postman 等工具测试访问。

