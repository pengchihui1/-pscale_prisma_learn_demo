# 简介（Next.js 启动器） 

这是一个 [Next.js](https://nextjs.org/) 项目，它使用 [Prisma](https://www.prisma.io/) 连接到 [PlanetScale](https://planetscale.com/) 数据库和 [Tailwind CSS](https://tailwindcss.com/) 用于样式设置。

## 先决条件

- [Node.js](https://nodejs.org/en/download/)
- [PlanetScale CLI](https://github.com/planetscale/cli)

## 本地
```
git clone https://github.com/planetscale/nextjs-starter
yarn
1. mv .env.example .env 配置本地数据库连接 更正schema.prisma文件数据库连接诶
2. npx prisma migrate dev --name init创建表
3. 请求api，回到首页看到数据
```

## Windows下安装流程
```
scoop安装：https://scoop.sh/
安装pscale:https://github.com/planetscale/cli#installation

第一步：PowerShell
> Set-ExecutionPolicy RemoteSigned -Scope CurrentUser # Optional: Needed to run a remote script the first time
> irm get.scoop.sh | iex

第二步
scoop bucket add pscale https://github.com/planetscale/scoop-bucket.git
scoop install pscale mysql
scoop update pscale

pscale db create star-app --region 
pscale branch create star-app initial-setup

第三步
npx create-next-app@latest --use-npm
cd star-app
npm install prisma --save-dev
npm install @prisma/client
npx prisma init

第四步
连接本地库
npx prisma migrate dev --name init
多开窗口
pscale auth login
pscale connect prismatest initial-setup --port 5432
npx prisma db push

```

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

## 学到更多

要了解有关 PlanetScale 的更多信息，请查看以下资源：

- [PlanetScale 快速入门指南](https://docs.planetscale.com/tutorials/planetscale-quick-start-guide) - 了解如何开始使用 PlanetScale。

## 下一步是什么？

详细了解 PlanetScale 如何允许您对数据库表进行 [非阻塞模式更改](https://docs.planetscale.com/concepts/nonblocking-schema-changes)，而不会锁定或导致生产数据库停机。如果您有兴趣了解如何在连接到 PlanetScale 时保护您的应用程序，
请阅读[连接