# Casdoor
> 开源的身份和访问管理(IAM)以及单点登录(SSO)平台，托管在GitHub仓库https://github.com/casdoor/casdoor. 代码采用前后端分离架构，后端使用Go语言，前端基于React框架

Casdoor的代码结构设计模块化，支持多种认证协议(OAuth 2.0, OIDC, SAML, CAS等)、国际化(i18n)以及扩展性。后端负责处理API请求、数据库交互和第三方集成，前端(web/目录)提供
用户界面。

```bash
➜ tree . -L 1 -d
.
├── authz               # 处理授权逻辑，主要实现基于角色(RBAC)或属性的访问控制(ABAC)
├── captcha             # 管理CAPTCHA验证，用于注册和登录时的防机器人保护
├── conf                # 存储后端配置文件
├── controllers         # 定义RESTful API端点及处理逻辑
├── cred                # 管理用户凭证(如密码、API)
├── deployment          # 存放部署相关文件
├── email               # 处理电子邮件相关的功能
├── faceId              # 实现基于面部识别的认证
├── form                # 管理表单相关逻辑
├── i18n                # 支持国际化 (i18n)
├── idp                 # 实现第三方身份提供者(identity Provider)集成
├── ldap                # 支持LDAP(轻量目录访问协议)集成
├── notification        # 管理通知功能
├── object              # 定义数据模型和核心业务逻辑
├── pp                  # Payment Provider 支付相关提供者
├── proxy               # 实现代理相关功能
├── radius              # 支持远程认证拨入用户服务协议
├── routers             # 定义后端API路由
├── scim                # 支持跨域身份管理协议
├── storage             # 管理文件存储功能
├── swagger             # 存储API文档 swagger.yaml 文件描述所有API端点的详细信息
├── sync                # 处理与外部系统的用户数据同步
├── sync_v2             # sync的升级版本
├── util                # 存放通用工具函数
├── web                 # 前端代码
└── xlsx                # 处理Excel文件相关功能

27 directories
```

### 核心概念

Casdoor中四个核心概念: Origination,User,Application和Provider

- Origination:
> 组织时用户和应用程序的容器

- User:
> 在Casdoor中，用户可以登录到一个应用程序,每个用户只能属于一个组织，但可以登录该组织拥有的多个应用程序.

Casdoor 有两种类型的用户:

```
管理员用户 built-in: 组织用户 built-in/admin: 在Casdoor平台上拥有完全管理权限的全局管理员
普通用户 其他组织用户: my-company/alice: 可以注册、登录、登出、更改自己个人资源的普通用户
```

- Application:
> 应用程序代表需要由Casdoor保护的网络服务，每个应用程序都可以有自己定制的注册页面、登录页面等，根登录页面/login是仅供Casdoor内置应用程序app-built-in的登录页面.
> 应用程序是用户登录到Casdoor的"入口"或"界面", 用户必须通过一个应用程序的登录页面才能登录Casdoor.


- Provider
> Casdoor使用Provider的概念来管理所有这些第三方连接器。


### Casdoor内置
```
1. built-in 内置组织
2. built-in/admin: 内置管理员用户
3. app-built-in 内置应用，由built-in组织管理，代表Casdoor本身
```

### 数据库
> Casdoor 使用XORM与数据库进行交互，基于Xorm Drivers,当前支持的数据库包括:
```
MySQL
MariaDB
PostgreSQL
CockroachDB
SQL Server
Oracle
SQLite 3
TiDB
```
默认使用MySQL
