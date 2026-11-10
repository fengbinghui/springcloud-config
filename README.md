# springcloud-config

Spring Cloud Config 的**配置仓库示例**：把不同环境的配置集中存放在 Git 仓库中，由 Config Server 统一对外提供，客户端按 `application/profile` 拉取。

适用于「Spring Cloud Config Server + Git 后端」模式。

## 仓库结构

```text
springcloud-config/
├─ config-dev.yml     # 开发环境配置
├─ config-prod.yml    # 生产环境配置
└─ config-test.yml    # 测试环境配置
```

每个文件都可按 Spring Cloud Config 的命名规则扩展为：

- `{application}.yml` —— 默认 profile
- `{application}-{profile}.yml` —— 指定环境
- `{application}-{profile}.properties`

## 如何使用

### 1. 启动 Config Server（另一个工程）

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/fengbinghui/springcloud-config.git
          default-label: main
```

### 2. 客户端拉取配置

```yaml
spring:
  application:
    name: config
  profiles:
    active: dev
  cloud:
    config:
      uri: http://localhost:8888
```

访问 `GET /{application}/{profile}` 即可获取对应配置（例如 `/config/dev`）。

## 学习要点

- 配置与代码分离：同一份代码通过不同 profile 加载不同配置；
- 集中管理：多服务、多环境配置统一放在 Git 仓库，变更可追溯（commit 历史即配置变更历史）；
- 动态刷新：配合 Spring Cloud Bus / `@RefreshScope` 可实现配置热更新（本项目仅存放配置，不含服务端代码）。

> 本项目为 Spring Cloud 学习实践，配置文件内容为演示值，不代表真实环境参数。
