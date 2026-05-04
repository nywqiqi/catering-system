# 餐饮管理系统 (Catering System)

基于 Spring Boot 3 + MyBatis 的餐饮后台管理系统，提供菜品、桌面、用户、订单的 RESTful API。

## 技术栈

| 技术 | 版本 |
|------|------|
| Java | 17 |
| Spring Boot | 3.5.9 |
| MyBatis | 3.0.5 |
| MySQL | 8.x |
| Spring Security | 6.x |
| PageHelper | 2.1.1 |
| SpringDoc OpenAPI | 2.8.8 |
| Lombok | — |

## 项目结构

```
src/main/java/com/nyw/cateringsystem/
├── bean/            # 数据库实体
│   ├── Dish.java
│   ├── Order.java
│   ├── OrderInfo.java
│   ├── TableInfo.java
│   └── User.java
├── config/          # 配置类
│   ├── OpenApiConfig.java    # Swagger 文档配置
│   └── SecurityConfig.java   # Spring Security 配置
├── consts/          # 枚举常量
│   ├── CodeEnum.java
│   ├── RoleEnum.java
│   └── StatusEnum.java
├── controller/      # 控制器层
│   ├── DishController.java
│   ├── OrderController.java
│   ├── OrderInfoController.java
│   ├── TableController.java
│   └── UserController.java
├── converter/       # Entity ↔ DTO 转换器
├── dto/             # 数据传输对象
├── exception/       # 自定义异常及全局异常处理
├── repository/      # MyBatis Mapper 接口
├── result/          # 统一响应体 R<T>
├── service/         # 业务接口及实现
└── util/            # 工具类
```

## 功能模块

| 模块 | 功能 | 端点 |
|------|------|------|
| 菜品管理 | 菜品增删改查、按分类/名称搜索、上下架 | `/dishes/**` |
| 桌面管理 | 桌面增删改查、按桌号搜索 | `/tables/**` |
| 用户管理 | 用户增删改查、登录、按角色查询、修改密码 | `/users/**` |
| 订单管理 | 订单增删改查、按桌面查询、结账 | `/orders/**` |
| 订单明细 | 订单内菜品明细的增删改查 | `/orderInfos/**` |

## 快速开始

### 环境要求

- JDK 17+
- Maven 3.8+
- MySQL 8.0+

### 数据库初始化

创建数据库并执行建表：

```sql
CREATE DATABASE IF NOT EXISTS catering_system DEFAULT CHARACTER SET utf8mb4;
```

### 配置文件

修改 `src/main/resources/application.yml` 中的数据库连接信息：

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/catering_system
    username: root
    password: your_password
```

### 启动应用

```bash
mvn spring-boot:run
```

应用启动后访问：

- **Swagger UI**: http://localhost:8080/swagger-ui.html
- **API 文档 (JSON)**: http://localhost:8080/v3/api-docs

## API 文档

项目集成了 SpringDoc OpenAPI，启动后通过 Swagger UI 即可查看和测试所有接口。

接口按模块分为 5 个分组：

- **菜品管理** — 菜品的增删改查接口
- **桌面管理** — 餐桌的增删改查接口
- **用户管理** — 用户的增删改查及登录接口
- **订单管理** — 订单的增删改查及结账接口
- **订单明细管理** — 订单内菜品明细的增删改查接口

## 统一响应格式

所有接口返回统一响应体 `R<T>`：

```json
{
  "code": 200,
  "msg": "操作成功",
  "timestamp": 1714800000000,
  "data": {}
}
```

## 开发说明

- **安全配置**：当前开发环境下，所有请求无需认证。生产环境请修改 `SecurityConfig.java` 启用认证授权。
- **密码加密**：用户密码使用 BCrypt（strength=12）加密存储。
- **分页**：查询列表接口使用 PageHelper 分页，默认页大小为 5。
- **日志**：MyBatis SQL 日志已开启 `TRACE` 级别，可在 `application.yml` 中调整。
