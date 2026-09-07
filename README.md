# public-site · 公网法务与支持页

App Store Connect 强制要求 **Privacy Policy URL** 和 **Support URL**，且必须是公网可访问的网页 ——
App 内的 `privacy.vue` / `terms.vue` 页面**不算数**，审核员要在浏览器里打开。这个目录就是那两个 URL 的内容。

---

## 文件

```
index.html          → App Store 的 Support URL
privacy.html        → App Store 的 Privacy Policy URL（英文）
terms.html          → 英文服务协议
zh/privacy.html     → 中文隐私政策
zh/terms.html       → 中文服务协议
```

全部自包含：零外链、零 CDN、零 JavaScript。丢任何静态托管都能直接跑，也支持系统深色模式。

---

## ⚠️ 不要手改这里的 HTML

这些文件是**生成的**。数据源是 App 里那两个法务文件：

```
wingman-ios/src/locale/legal/en.ts      ← 英文隐私政策 + 服务协议
wingman-ios/src/locale/legal/zh-CN.ts   ← 中文隐私政策 + 服务协议
```

**App 内页面和这个网站用的是同一份数据。** 改法务文案只改上面两个 `.ts`，然后重新生成：

```bash
cd wingman-ios
node scripts/build-legal-site.mjs
```

手改 HTML 的话，下次一生成就被覆盖，而且 App 内和网页版会不一致 —— 那正是这套设计要避免的。

---

## 部署（三选一）

### 方案 A · GitHub Pages（推荐，免费，约 15 分钟）

1. 在 GitHub 新建一个 **public** 仓库，例如 `acewingman-site`
2. 把本目录所有文件推上去（`index.html` 必须在仓库根）
3. 仓库 → Settings → Pages → Source 选 `Deploy from a branch`，分支选 `main`、目录选 `/ (root)`
4. 等 1-2 分钟，拿到 URL：`https://<你的用户名>.github.io/acewingman-site/`

填进 App Store Connect：

| ASC 字段 | 填什么 |
|---|---|
| Privacy Policy URL | `https://<用户名>.github.io/acewingman-site/privacy.html` |
| Support URL | `https://<用户名>.github.io/acewingman-site/` |

> ⚠️ 仓库必须是 **public**，private 仓库的 Pages 需要付费方案，审核员打不开。

### 方案 B · Cloudflare Pages（免费，自带 CDN，更快）

`dash.cloudflare.com` → Workers & Pages → Create → Pages → 连 GitHub 仓库 → 构建命令留空、输出目录填 `/`。
URL 形如 `https://acewingman-site.pages.dev/`。

### 方案 C · 自有域名

买域名后指向上面任一托管即可。这样 App Store 商品页上显示的 Marketing URL 也能用同一个域名，最专业。

---

## 上线前自查

- [ ] 用**无痕窗口**打开 Privacy Policy URL，确认不需要登录就能看到全文
- [ ] 手机浏览器打开一遍（审核员常用手机）
- [ ] 三个内部跳转都通：`Support ↔ English ↔ 中文`
- [ ] 页脚邮箱 `enzosun94@gmail.com` 可点击且是你在用的邮箱
- [ ] 法务文案若有改动，已重新跑过 `build-legal-site.mjs` 并重新部署
