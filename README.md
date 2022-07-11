# 简介（Next.js 启动器） 

这是一个 [Next.js](https://nextjs.org/) 项目，它使用 [Prisma](https://www.prisma.io/) 连接到 [PlanetScale](https://planetscale. com/) 数据库和 [Tailwind CSS](https://tailwindcss.com/) 用于样式设置。

## 先决条件

- [Node.js](https://nodejs.org/en/download/)
- [PlanetScale CLI](https://github.com/planetscale/cli)

## 本地
```
1. mv .env.example .env 配置本地数据库连接 更正schema.prisma文件数据库连接诶
2. npx prisma migrate dev --name init创建表
3. 请求api，回到首页看到数据
```

## 线上设置数据库

使用以下命令创建一个新数据库：

```
pscale 数据库创建 <DATABASE_NAME>
```

> 创建数据库时会自动创建一个分支“main”，因此您可以在以下步骤中将其用于“BRANCH_NAME”。

## 设置启动 Next.js 应用程序

克隆启动器存储库。

```
git 克隆 https://github.com/planetscale/nextjs-starter
```

安装依赖项。

```
npm 安装
```

接下来，您需要通过 CLI 创建数据库用户名和密码以连接到您的应用程序。如果您希望在此步骤中使用仪表板，您可以在 [连接字符串文档](/concepts/connection-strings#creating-a-password) 中找到这些说明，然后返回此处完成设置。

首先，通过将 `.env.example` 文件重命名为 `.env` 来创建你的 `.env` 文件：

```
mv .env.example .env
```

接下来，使用 PlanetScale CLI，为您的数据库分支创建一个新的用户名和密码：

```
pscale 密码创建 <DATABASE_NAME> <BRANCH_NAME> <PASSWORD_NAME>
```

> `PASSWORD_NAME` 值代表正在生成的用户名和密码的名称。您可以为一个分支拥有多个凭据，因此这为您提供了一种对它们进行分类的方法。要在仪表板中管理您的密码，请转到您的数据库概览页面，单击“设置”，然后单击“密码”。

记下返回给您的值，因为您将无法再次看到此密码。

```文本
密码 production-password 已成功创建。
请保存以下值，因为它们不会再次显示

  名称 用户名 访问主机 URL 角色纯文本
 --------- -------------- --------------- -------------------- ------ ------------ ------------------------------------------
  生产密码 xxxxxxxxxxxxx xxxxxx.us-east-2.psdb.cloud 可以读写 pscale_pw_xxxxxxx
```

您将使用这些属性来构造您的连接字符串，这将是您的 .env 文件中的 PLANETSCALE_PRISMA_DATABASE_URL 的值。使用以下格式的连接字符串更新 `PLANETSCALE_PRISMA_DATABASE_URL` 属性：

```文本
mysql://<USERNAME>:<PLAIN_TEXT_PASSWORD>@<ACCESS_HOST_URL>/<DATABASE_NAME>?sslaccept=strict
```

使用 Prisma 将数据库模式推送到您的 PlanetScale 数据库。

`npx 棱镜数据库推送`

运行种子脚本以使用“产品”和“类别”数据填充您的数据库。

`npm 运行种子`

## 运行应用程序

使用以下命令运行应用程序：

`npm 运行开发`

在 [localhost:3000](localhost:3000) 打开浏览器以查看正在运行的应用程序。

## 部署

在您的应用程序在本地运行后，就该部署它了。为此，您需要将数据库分支（默认为`main`）提升为生产分支（[阅读分支文档了解更多信息](https://docs.planetscale.com/concepts/branching) ）。

```
pscale 分支提升 <DATABASE_NAME> <BRANCH_NAME>
```

现在您的分支已升级到生产环境，您可以使用之前生成的现有密码在本地运行，也可以创建一个新密码。无论如何，您在下面的部署步骤中都需要密码。

选择以下部署按钮之一，并确保在此设置过程中更新 `PLANETSCALE_PRISMA_DATABASE_URL` 变量。

### 在 Vercel 上部署

[![使用 Vercel 部署](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/planetscale/nextjs-starter&env= PLANETSCALE_PRISMA_DATABASE_URL)

### 在 Netlify 上部署

\*注意：此存储库中的 `Netlify.toml` 文件包含用于在初始部署时自定义 `PLANETSCALE_PRISMA_DATABASE_URL` 属性的配置。

[![部署到 Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github. com/planetscale/nextjs-starter)

＃＃ 学到更多

要了解有关 PlanetScale 的更多信息，请查看以下资源：

- [PlanetScale 快速入门指南](https://docs.planetscale.com/tutorials/planetscale-quick-start-guide) - 了解如何开始使用 PlanetScale。

＃＃ 下一步是什么？

详细了解 PlanetScale 如何允许您对数据库表进行 [非阻塞模式更改](https://docs.planetscale.com/concepts/nonblocking-schema-changes)，而不会锁定或导致生产数据库停机。如果您有兴趣了解如何在连接到 PlanetScale 时保护您的应用程序，
请阅读[连接