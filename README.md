# Cosmic Whisper — Aetheria Oracle

塔罗牌 Web App，接入 GPT-4o-mini 做 AI 解读。

## 项目结构

```
cosmic-whisper/
├── index.html              ← 主页面（全部功能在这一个文件里）
├── netlify.toml             ← Netlify 配置
├── netlify/
│   └── functions/
│       └── tarot-ai.js      ← Serverless function（代理 OpenAI API）
└── tarot-cards/             ← 放你的78张牌图片（可选）
    ├── card-1.jpg
    ├── card-2.jpg
    └── ...
```

## 部署步骤（5 分钟搞定）

### 第一步：获取 OpenAI API Key
1. 去 https://platform.openai.com/api-keys
2. 点 "Create new secret key"
3. 复制保存（以 `sk-` 开头）

### 第二步：部署到 Netlify
1. 把整个 `cosmic-whisper` 文件夹用 Git 推到 GitHub
   ```bash
   cd cosmic-whisper
   git init
   git add .
   git commit -m "init"
   # 去 GitHub 新建一个 repo，然后：
   git remote add origin https://github.com/你的用户名/cosmic-whisper.git
   git push -u origin main
   ```
2. 去 https://app.netlify.com → "Add new site" → "Import an existing project" → 选你的 GitHub repo
3. Build settings 保持默认（不用填 build command），点 Deploy

### 第三步：配置 API Key（关键！）
1. 在 Netlify 后台 → 你的站点 → Site configuration → Environment variables
2. 点 "Add a variable"
3. Key 填：`OPENAI_API_KEY`
4. Value 填：你的 OpenAI API Key（`sk-...`）
5. 保存

### 第四步：自定义域名（可选）
你已经有 `cosmicwhisperkessi.netlify.app`，在 Netlify 后台 Domain management 里设置即可。

## 费用估算

GPT-4o-mini 每次解读约消耗 ~1000 tokens：
- 输入：~$0.00015/次
- 输出：~$0.0006/次
- **总计：约 $0.00075/次 ≈ ¥0.005/次**
- 1000 次解读 ≈ ¥5

## 如果不想用 AI

不配置 `OPENAI_API_KEY` 就行，API 调用会自动失败，然后 fallback 到内置的静态模板解读。网站照常运行，只是回答没有那么"定制化"。

## 如果以后想换回 Claude

在 `netlify/functions/tarot-ai.js` 里把 OpenAI 的调用换成 Anthropic 的即可，prompt 不用改。
