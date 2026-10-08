# Keygate (LazyCat)

开源自托管软件许可证管理平台，打包为 LazyCat LPK v2。上游项目：[tabloy/keygate](https://github.com/tabloy/keygate)。

## 部署信息

- **包名**：`cloud.lazycat.app.keygate`
- **版本**：跟随上游镜像 SemVer tag 自动更新（当前 0.1.13；每日 UTC 03:23 检查）
- **交付模式**：lazycat（镜像复制到官方源，支持官方商店 + 喵喵商店双发布）
- **min_os_version**：1.5.0

## 服务组成

| 服务 | 镜像 | 说明 |
|------|------|------|
| `keygate` | ghcr.io/tabloy/keygate（自动更新，版本源） | 主应用（Go + Web 管理端，端口 9000） |
| `postgres` | postgres 18.x-alpine（自动更新） | 数据库 |

## 初始化与登录

1. 安装后打开应用 → 进入 `/setup` **初始化页面**：创建 owner 账号与第一个产品
2. 登录使用**邮箱验证码（OTP）**：
   - 未配置 SMTP 时：验证码打印在**应用日志**中（客户端应用详情 → 日志，或 `lzc-cli project log`）
   - 配置 SMTP 后：验证码发送到邮箱
3. 自动生成强密钥：JWT_SECRET / LICENSE_SIGNING_KEY / RELEASE_KEY_ENCRYPTION_KEY（64 hex）

## 设置向导参数

| 参数 | 说明 |
|------|------|
| admin_email | 管理员邮箱（可选；留空通过 setup 页面创建） |
| smtp_* | SMTP 服务器五项（可选；未配置时 OTP 打印到日志） |
| stripe_secret_key / stripe_webhook_secret | Stripe 计费（可选） |
| redis_url | Redis（可选，限流/缓存） |
| storage_* | S3 兼容对象存储（可选，发布分发） |

## 文件结构

```
package.yml              # 包元数据
lzc-manifest.yml         # 运行结构（keygate + postgres，双镜像 lazycat 更新）
lzc-deploy-params.yml    # 安装向导参数（15 项）
lzc-build.yml            # 构建配置
icon.png                 # 应用图标（上游 logo.svg）
.github/lazycat-action.yml      # Action 配置（双商店）
.github/workflows/lazycat.yml   # 发布工作流
.github/screenshots/            # 官方商店截图素材
```
