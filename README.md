# 🎯 duanlian.shenzjd.com

一个基于 Git 和 Cloudflare Workers 的极简短链接系统。

## 核心原理

利用 Git 空提交的 commit hash 作为短链标识：

1. **创建短链**：通过 GitHub API 创建空提交，目标 URL 作为 commit message
2. **访问短链**：读取 GitHub 的 `.patch` 文件，从 Subject 中提取目标 URL
3. **重定向**：302 跳转到目标地址

## 功能

- 🌐 Web 页面一键生成短链（无需终端）
- 🔑 GitHub OAuth 登录（仅 repo owner 可操作）
- 📋 一键复制短链

## 部署

### 1. 创建 GitHub OAuth App

1. 打开 [GitHub Developer Settings](https://github.com/settings/developers) → **New OAuth App**
2. 填写：
   - **Homepage URL**: `https://duanlian.shenzjd.com`
   - **Authorization callback URL**: `https://duanlian.shenzjd.com/callback`
3. 记录 **Client ID** 和 **Client Secret**

### 2. 部署 Worker

```bash
# 安装 wrangler
npm install -g wrangler

# 登录 Cloudflare
npx wrangler login

# 设置密钥（只需一次）
npx wrangler secret put GITHUB_CLIENT_ID
npx wrangler secret put GITHUB_CLIENT_SECRET

# 部署
npx wrangler deploy
```

### 3. 绑定域名

Cloudflare Dashboard → Workers → duanlian → Settings → Domains & Routes → 添加自定义域名。

### 本地开发

```bash
# 创建 .dev.vars 文件
echo 'GITHUB_CLIENT_ID=你的ClientID' > .dev.vars
echo 'GITHUB_CLIENT_SECRET=你的ClientSecret' >> .dev.vars

# 启动
npx wrangler dev
```

## 使用

1. 访问 `https://duanlian.shenzjd.com`
2. 点击 **使用 GitHub 登录**
3. 输入目标链接，点击 **生成**
4. 复制生成的短链

也支持命令行创建：

```bash
git commit --allow-empty -m "https://example.com"
git push origin main
```

## 项目结构

```
.
├── README.md
├── worker.js          # Cloudflare Worker（前端 + OAuth + API + 重定向）
└── wrangler.toml      # 部署配置
```

## 许可证

MIT
