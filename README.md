# 网盘系统 (NetworkDisk)

> 企业级云存储与文件管理平台 — 基于 Spring Cloud 微服务架构，集成 AI 智能文档分析能力

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2.2-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Vue](https://img.shields.io/badge/Vue-3.3.4-4FC08D.svg)](https://vuejs.org/)
[![Vite](https://img.shields.io/badge/Vite-8.0.16-646CFF.svg)](https://vitejs.dev/)
[![License](https://img.shields.io/badge/license-private-red.svg)](LICENSE)

---

## 📖 目录

- [项目介绍](#项目介绍)
- [功能特性](#功能特性)
- [技术栈](#技术栈)
- [项目结构](#项目结构)
- [系统架构](#系统架构)
- [快速开始](#快速开始)
- [配置说明](#配置说明)
- [API 文档](#api-文档)
- [部署指南](#部署指南)

---

## 项目介绍

卡码网盘是一个功能完备的企业级网络云盘系统，提供文件存储、管理、分享、回收站等基础功能，同时集成了 AI 驱动的智能文档分析能力（文档索引、摘要生成、智能问答）。

项目采用 **Spring Cloud 微服务架构**，后端基于 Java 21 + Spring Boot 3.2.2，前端基于 Vue 3 + Element Plus，服务间通过 Dubbo RPC 通信，使用 Nacos 实现服务注册与配置管理。

### 核心模块

| 模块 | 端口 | 说明 |
|------|------|------|
| `networkdisk-gateway` | 8081 | API 网关（Spring Cloud Gateway） |
| `networkdisk-auth` | 8090 | 用户认证与授权（Sa-Token） |
| `networkdisk-business` | - | 业务聚合模块（父 POM） |
| ├ `networkdisk-app` | 8082 | 业务主应用 |
| ├ `networkdisk-files` | 8082 | 文件管理服务 |
| ├ `networkdisk-user` | 8086 | 用户管理服务 |
| ├ `networkdisk-share` | 8085 | 文件分享服务 |
| ├ `networkdisk-recycle` | 8084 | 回收站服务 |
| ├ `networkdisk-engine` | - | 文件存储引擎 |
| ├ `networkdisk-ai` | 8087 | AI 智能服务 |
| └ `networkdisk-notice` | - | 通知服务 |
| `networkdisk-common` | - | 公共组件库 |

---

## 功能特性

### 📁 文件管理
- ✅ 文件/文件夹上传、下载、预览
- ✅ **大文件分片上传**（chunk upload），支持断点续传
- ✅ **秒传功能**：基于 MD5 检测重复文件，秒级完成
- ✅ 文件夹树形导航 + 面包屑路径
- ✅ 文件复制、移动、重命名
- ✅ 全文检索（Elasticsearch）
- ✅ 文件分类浏览（全部 / 图片 / 视频 / 文档 / 音乐 / 其他）

### 🔄 回收站
- ✅ 软删除机制，支持文件还原
- ✅ 永久删除（物理删除）

### 🔗 文件分享
- ✅ 创建分享链接，支持提取码
- ✅ 分享有效期设置（永久 / 1天 / 7天 / 30天）
- ✅ 分享管理（查看、取消、复制链接）
- ✅ 分享文件保存到自己的网盘

### 👤 用户系统
- ✅ 注册 / 登录（图形验证码）
- ✅ Sa-Token 认证（Token 30天有效期）
- ✅ 密码修改与重置
- ✅ 搜索历史记录

### 🤖 AI 智能功能
- ✅ **文档索引**：支持 PDF、Word、Excel、PPT、TXT、HTML 等格式
- ✅ **文档摘要**：自动生成文档内容摘要
- ✅ **智能标签**：自动提取文档关键词标签
- ✅ **文件问答（RAG）**：基于文档内容的自然语言提问

### 🛡️ 系统保障
- ✅ 网关层限流（Sentinel）
- ✅ 分布式锁（Redisson）
- ✅ 敏感日志脱敏
- ✅ 全局 CORS 支持

---

## 技术栈

### 后端

| 类别 | 技术 | 版本 |
|------|------|------|
| 语言 | Java | 21 |
| 框架 | Spring Boot | 3.2.2 |
| 微服务 | Spring Cloud | 2023.0.0 |
| 微服务 | Spring Cloud Alibaba | 2022.0.0.0 |
| RPC | Apache Dubbo | 3.2.10 |
| 注册/配置 | Nacos | server |
| 网关 | Spring Cloud Gateway | - |
| ORM | MyBatis Plus | 3.5.5 |
| 连接池 | Druid | 1.2.20 |
| 分库分表 | ShardingSphere | 5.2.1 |
| 缓存 | Redis + Redisson + JetCache + Caffeine | - |
| 搜索引擎 | Elasticsearch + Easy-Es | 7.17.6 |
| 消息队列 | RocketMQ | - |
| 认证 | Sa-Token | 1.37.0 |
| 限流 | Sentinel | - |
| 定时任务 | XXL-Job | 2.4.0 |
| 文件存储 | FastDFS + 阿里云 OSS | - |
| 向量存储 | PostgreSQL + pgvector | 14+ |
| 文档解析 | Apache Tika | 2.9.2 |
| 工具集 | Lombok / MapStruct / Hutool / Fastjson2 / Guava | - |

### 前端

| 类别 | 技术 | 版本 |
|------|------|------|
| 运行时 | Vue 3 (Composition API) | 3.3.4 |
| 路由 | Vue Router | 4.2.4 |
| 状态管理 | Pinia | 2.1.6 |
| UI 库 | Element Plus | 2.3.8 |
| HTTP | Axios | 1.4.0 |
| 构建 | Vite | 8.0.16 |
| 分片上传 | simple-uploader.js + spark-md5 | - |

---

## 项目结构

```
NetworkDisk-main/
│
├── pom.xml                          # 根 POM（依赖管理）
├── REQUIREMENTS.md                  # 需求规格说明书
│
├── networkdisk-auth/                # 🔐 认证服务
│   └── src/main/java/com/disk/auth/
│       ├── controller/              # REST 接口
│       ├── domain/                  # 领域层（服务、实体、转换器）
│       └── infrastructure/          # 基础设施（常量、异常）
│
├── networkdisk-gateway/             # 🚪 API 网关
│   └── src/main/java/com/disk/gateway/
│       └── config/                  # 路由、CORS、限流配置
│
├── networkdisk-business/            # 💼 业务模块（父 POM）
│   ├── networkdisk-app/             #   主应用聚合
│   ├── networkdisk-files/           #   文件管理
│   ├── networkdisk-user/            #   用户管理
│   ├── networkdisk-share/           #   文件分享
│   ├── networkdisk-recycle/         #   回收站
│   ├── networkdisk-engine/          #   存储引擎
│   ├── networkdisk-ai/              #   AI 智能服务
│   └── networkdisk-notice/          #   通知服务
│
├── networkdisk-common/              # 🧩 公共组件库
│   ├── networkdisk-api/             #   API 接口定义（Dubbo）
│   ├── networkdisk-base/            #   基础工具、异常、枚举
│   ├── networkdisk-cache/           #   多级缓存
│   ├── networkdisk-config/          #   Nacos 配置中心
│   ├── networkdisk-datasource/      #   数据源（MySQL、Druid）
│   ├── networkdisk-es/              #   Elasticsearch
│   ├── networkdisk-file/            #   文件存储抽象
│   ├── networkdisk-job/             #   XXL-Job 定时任务
│   ├── networkdisk-limiter/         #   Sentinel 限流
│   ├── networkdisk-lock/            #   分布式锁
│   ├── networkdisk-mq/              #   RocketMQ
│   ├── networkdisk-rpc/             #   Dubbo RPC
│   ├── networkdisk-sa-token/        #   Sa-Token 认证
│   ├── networkdisk-web/             #   Web 层组件
│   └── networkdisk-delay-message/   #   延迟消息
│
├── disk-by-cursor/                  # 🖥️ 前端项目
│   ├── src/
│   │   ├── api/                     #   API 请求封装
│   │   ├── components/              #   通用组件
│   │   ├── router/                  #   路由配置
│   │   ├── stores/                  #   Pinia 状态
│   │   ├── utils/                   #   工具函数
│   │   └── views/                   #   页面组件
│   ├── package.json
│   └── vite.config.js
│
├── es-docker-compose/               # 🐳 ES + Kibana Docker 部署
│   └── docker-compose.yml
│
└── fdfs-docker-compose/             # 🐳 FastDFS Docker 部署
    ├── docker-compose.yml
    ├── nginx.conf
    └── storage.conf
```

---

## 系统架构

```
                          ┌──────────────────┐
                          │   Nacos (8848)   │
                          │ 服务注册 & 配置中心 │
                          └────────┬─────────┘
                                   │
     ┌─────────────────────────────┼─────────────────────────────┐
     │                             │                             │
     ▼                             ▼                             ▼
┌──────────┐              ┌──────────────┐            ┌────────────────┐
│ 前端 Vue │─────────────▶│   Gateway    │───────────▶│   Auth (8090)  │
│(Vite Dev)│              │   (8081)     │            │   Sa-Token     │
└──────────┘              └──────┬───────┘            └────────────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                  │
              ▼                  ▼                  ▼
     ┌────────────┐    ┌──────────────┐    ┌─────────────┐
     │  Business  │    │   AI (8087)  │    │  Notice     │
     │  (8082)    │    │  文档问答     │    │  消息通知    │
     └─────┬──────┘    └──────┬───────┘    └─────────────┘
           │                  │
           │     Dubbo RPC    │
           └──────────────────┘
           │                  │
    ┌──────┴──────────────────┴──────────┐
    │                                     │
    ▼           ▼           ▼            ▼
┌───────┐ ┌────────┐ ┌──────────┐ ┌───────────┐
│ MySQL │ │ Redis  │ │ RocketMQ │ │FastDFS/OSS│
└───────┘ └────────┘ └──────────┘ └───────────┘
    │                                     │
    ▼                                     ▼
┌──────────────┐              ┌──────────────────┐
│Elasticsearch │              │ PostgreSQL        │
│  全文检索     │              │  + pgvector 向量库 │
└──────────────┘              └──────────────────┘
```

---

## 快速开始

### 前置条件

| 软件 | 版本要求 | 说明 |
|------|----------|------|
| JDK | 21+ | 后端编译运行 |
| Maven | 3.8+ | 项目构建 |
| Node.js | 18+ | 前端构建 |
| MySQL | 8.0+ | 业务数据存储 |
| Redis | 7.0+ | 缓存 |
| Nacos | 2.x | 服务注册/配置中心 |
| RocketMQ | 5.x | 消息队列 |
| Elasticsearch | 7.17.6 | 全文检索 |
| FastDFS | latest | 文件存储 |
| PostgreSQL | 14+ | 向量存储（AI 模块） |

### 1. 启动中间件

#### 启动 Elasticsearch + Kibana
```bash
cd es-docker-compose
docker-compose up -d
```

#### 启动 FastDFS
```bash
cd fdfs-docker-compose
docker-compose up -d
```

#### 启动其他中间件
确保 Nacos、MySQL、Redis、RocketMQ、PostgreSQL 已启动并可用。

### 2. 后端启动

```bash
# 编译整个项目
mvn clean install -DskipTests

# 启动认证服务 (端口 8090)
cd networkdisk-auth
mvn spring-boot:run

# 启动业务服务 (端口 8082)
cd networkdisk-business/networkdisk-app
mvn spring-boot:run

# 启动 AI 服务 (端口 8087)
cd networkdisk-business/networkdisk-ai
mvn spring-boot:run

# 启动网关 (端口 8081)
cd networkdisk-gateway
mvn spring-boot:run
```

### 3. 前端启动

```bash
cd disk-by-cursor
npm install
npm run dev
```

前端开发服务器默认运行在 `http://localhost:5173`，API 请求自动代理到后端各服务。

### 4. 访问系统

- **前端页面**：`http://localhost:5173`
- **网关入口**：`http://localhost:8081`
- **Kibana（ES 管理）**：`http://localhost:5601`

---

## 配置说明

### AI 模块配置

AI 模块使用 OpenAI 兼容接口，默认对接阿里云通义千问。配置文件位于 `networkdisk-ai/src/main/resources/application.yml`：

```yaml
com:
  disk:
    ai:
      provider:
        type: openai-compatible
        base-url: https://dashscope.aliyuncs.com/compatible-mode/v1
        api-key: <your-api-key>
        chat-model: qwen-plus
        embedding-model: text-embedding-v3
        embedding-dimension: 768
      index:
        chunk-size: 1200
        chunk-overlap: 200
        retrieval-top-k: 6
      pgvector:
        enabled: true
        url: jdbc:postgresql://127.0.0.1:5432/networkdisk_ai
        username: postgres
        password: <your-password>
```

### 切换 LLM 提供商

任何兼容 OpenAI API 格式的服务均可替换，例如：
- **OpenAI**：`base-url: https://api.openai.com/v1`
- **DeepSeek**：`base-url: https://api.deepseek.com/v1`
- **本地模型**（Ollama / vLLM）：修改 `base-url` 对应地址即可

### PostgreSQL pgvector 初始化

```sql
-- 创建数据库
CREATE DATABASE networkdisk_ai;

-- 启用 pgvector 扩展
CREATE EXTENSION IF NOT EXISTS vector;
```

---

## API 文档

### 认证接口 (Auth)

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/v1/auth/send-captcha` | 获取图形验证码 |
| POST | `/api/v1/auth/register` | 用户注册 |
| POST | `/api/v1/auth/login` | 用户登录 |
| POST | `/api/v1/auth/logout` | 退出登录 |

### 文件接口 (Files)

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/v1/files/list` | 文件列表 |
| POST | `/api/v1/files/upload` | 文件上传 |
| GET | `/api/v1/files/download` | 文件下载 |
| GET | `/api/v1/files/preview` | 文件预览 |
| POST | `/api/v1/files/folder` | 创建文件夹 |
| PUT | `/api/v1/files/rename` | 重命名 |
| DELETE | `/api/v1/files/delete` | 删除文件 |
| POST | `/api/v1/files/copy` | 复制文件 |
| POST | `/api/v1/files/transfer` | 移动文件 |
| GET | `/api/v1/files/search` | 搜索文件 |
| POST | `/api/v1/files/chunk-upload` | 分片上传 |
| POST | `/api/v1/files/chunk-merge` | 合并分片 |
| GET | `/api/v1/files/breadcrumbs` | 面包屑导航 |
| GET | `/api/v1/files/folder-tree` | 文件夹树 |

### 分享接口 (Share)

| 方法 | 路径 | 说明 |
|------|------|------|
| POST | `/api/v1/shares` | 创建分享 |
| GET | `/api/v1/shares/list` | 分享列表 |
| DELETE | `/api/v1/shares` | 取消分享 |
| GET | `/api/v1/shares/check` | 验证提取码 |
| GET | `/api/v1/shares/info` | 分享详情 |

### 回收站接口 (Recycle)

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/v1/recycles/list` | 回收站列表 |
| PUT | `/api/v1/recycles/recycle/restore` | 还原文件 |
| DELETE | `/api/v1/recycles/recycle` | 永久删除 |

### AI 接口 (AI)

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/v1/ai/capabilities` | 获取 AI 能力列表 |
| POST | `/api/v1/ai/files/index` | 文档索引 |
| POST | `/api/v1/ai/files/summary` | 生成文档摘要 |
| POST | `/api/v1/ai/files/tags` | 生成文档标签 |
| POST | `/api/v1/ai/files/question` | 文档问答（RAG） |

---

## 部署指南

### 生产环境部署

1. **修改配置**：将各模块 `application.yml` 中的数据库连接、Redis 地址等配置改为生产环境地址
2. **打包**：`mvn clean package -DskipTests`
3. **启动 jar**：`java -jar <module>/target/<module>.jar --spring.profiles.active=prod`
4. **前端构建**：`cd disk-by-cursor && npm run build`，将 `dist/` 目录部署到 Nginx

### Docker 部署（中间件）

项目提供了两个 docker-compose 文件用于快速部署依赖中间件：

- [es-docker-compose/docker-compose.yml](es-docker-compose/docker-compose.yml) — Elasticsearch 7.17.6 + Kibana
- [fdfs-docker-compose/docker-compose.yml](fdfs-docker-compose/docker-compose.yml) — FastDFS Tracker + Storage + Nginx

---

## 开发规范

### 代码结构（DDD 分层）

```
controller/          # REST 控制器，处理 HTTP 请求
domain/
  ├── context/       # 用例上下文（请求参数封装）
  ├── entity/        # 领域实体（DO）
  ├── convertor/     # 对象转换器（MapStruct）
  ├── request/       # 入参 DTO（ParamVO）
  ├── response/      # 出参 DTO（VO）
  └── service/       # 领域服务
facade/              # Dubbo RPC 接口实现
infrastructure/      # 基础设施（常量、枚举、工具类、外部集成）
```

### 技术约定

- 对象转换统一使用 **MapStruct**，转换器放在 `domain/convertor/`
- 跨服务调用统一使用 **Dubbo**，接口定义在 `networkdisk-api` 模块
- 分布式锁使用 **Redisson**，配置在 `networkdisk-lock` 模块
- 限流规则通过 **Sentinel** 配置，组件封装在 `networkdisk-limiter`
- 用户 ID 对外传输前必须通过 `IdUtil.encrypt()` 加密

---

## 待完成功能

- [ ] 短信/邮箱验证码注册
- [ ] 通知服务（SMS 集成）
- [ ] 文件协作编辑
- [ ] AI 多文件对话
- [ ] 移动端适配
- [ ] CI/CD 流水线
- [ ] 单元测试覆盖率提升

---
