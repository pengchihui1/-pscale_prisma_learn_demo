# 简介（Next.js 启动器） 

这是一个 [Next.js](https://nextjs.org/) 项目，它使用 [Prisma](https://www.prisma.io/) 连接到 [PlanetScale](https://planetscale.com/) 数据库和 [Tailwind CSS](https://tailwindcss.com/) 用于样式设置。

## 先决条件
DATABASE_URL='postgresql://postgres@localhost:5432/prismatest'
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

set-executionpolicy remotesigned -scope currentuser
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')

第二步
scoop bucket add pscale https://github.com/planetscale/scoop-bucket.git
scoop install pscale mysql
scoop install mysql

如果发生了中断，下一次使用命令安装的时候会显示已安装，把C:\Users\<user name>下的scoop文件夹删掉就好了（重新打开vscode）

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

安装mysql
第五步（方式一）
pscale auth login
pscale connect prismatest testdatabase  --port 3309
npx prisma db push
第五步（方式二）
1. 复制数据库地址：DATABASE_URL='mysql://yn4j3bxzuy2y:pscale_pw_MHVgglNNEzXbliXZmJ-DCOK-xr95thuf9-2b_zmQ7Eg@vohp4fs7h07v.us-east-3.psdb.cloud/prismatest?sslaccept=strict'
2. npx prisma db push

```

## mysql安装流程
```
官网下载MSI客户端安装：https://cloud.tencent.com/developer/article/1636375
环境变量bin目录配置

root用户密码为空，设置密码(mysql版本8开头)
1. set password ="root";
2. mysql -u root -p; 回车输入密码

root用户有密码但是要设置为空(mysql版本8开头)
select user,host from user where user='root';
1. use mysql; 使用mysql数据库
2. update user set authentication_string='' where user='root'; 设置认证为空
3. alter user "root"@"localhost" identified by '';  设置密码为空
4. flush privileges; 修改生效
注意：authentication_string 作为一个认证字符串
注意：MySQL建用户的时候会指定一个host，默认是127.0.0.1/localhost只能本机访问； 
其它机器用这个用户帐号访问会提示没有权限，host改为%，表示允许所有机器访问。

成功后！
mysql -u root -p 直接回车 进入mysql
```

## PlanetScale简介
1. PlanetScale 推出基于Vitess 的分布式MySQL 数据库服务
2. PlanetScale 一个很有意思的云 MySQL 解决方案
3. MySQL 已经证明自己在键值查找方面比Redis更快

### 在 Vercel 上部署

[![使用 Vercel 部署](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/planetscale/nextjs-starter&env= PLANETSCALE_PRISMA_DATABASE_URL)

### 在 Netlify 上部署

\*注意：此存储库中的 `Netlify.toml` 文件包含用于在初始部署时自定义 `PLANETSCALE_PRISMA_DATABASE_URL` 属性的配置。

[![部署到 Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github. com/planetscale/nextjs-starter)

