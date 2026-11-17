# 考研英语标注系统（网页版）

纯静态单页应用，无需服务器。手机、平板、电脑浏览器通用，改动可直接写回仓库里的 `annotations.js`。

## 目录结构

| 文件 | 作用 |
| --- | --- |
| `index.html` | 页面主体（样式 + 全部逻辑），入口文件 |
| `data.js` | 题库数据（`window.EXAM_DATA`） |
| `dict.js` | 词典数据（`window.EXAM_DICT`） |
| `annotations.js` | 标注数据（`window.ANNOTATIONS`）——网页的可写数据文件 |
| `confusable.js` | 形近词库（当前页面未引用，留作备用） |
| `.nojekyll` | 关闭 GitHub Pages 的 Jekyll 处理，保证静态文件原样发布 |

## 改动怎么保存

右上角 **「连接文件」** 按钮，二选一：

**1. GitHub 仓库（手机 / 平板 / 多设备共用，推荐）**

改动通过 GitHub Contents API 直接提交到仓库的 `annotations.js`，每次保存生成一个 commit，Pages 自动重新发布。所有设备读到同一份数据。

需要填：仓库（`用户名/仓库名`，在 Pages 上会自动猜出来）、分支（默认 `main`）、文件路径（默认 `annotations.js`）、访问令牌。

**2. 本地文件（仅桌面 Chrome / Edge）**

通过 File System Access API 直接读写电脑上的 `annotations.js`，适合配合 OneDrive 里的原始文件使用。

**3. 都没连接时**

改动会暂存在浏览器 `localStorage`，刷新不丢，但不会写进文件；状态栏显示「仅本机」。

## Token 怎么建

**细粒度 Token（推荐）**：GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token
- Repository access 选 **Only select repositories** → 选中本仓库
- Permissions → Repository permissions → **Contents: Read and write**（其余全部 No access）
- 有效期按需，比如 90 天

**classic Token**：勾选 `repo` 即可。

> Token 存在浏览器本地（可勾选「不记住」改为仅本次有效）。它只对这一个仓库有写权限，泄露风险可控；不用了随时在 GitHub 上 Revoke。

## 本地预览

```powershell
python -m http.server 8080
# 浏览器打开 http://localhost:8080
```

注意：直接双击 `file://` 打开时，浏览器会因跨域策略拦截 GitHub API，需要起本地服务器或直接用线上地址。

## 在线访问

GitHub Pages 地址：`https://<用户名>.github.io/<仓库名>/`

手机上建议「添加到主屏幕」：Safari 分享菜单 → 添加到主屏幕；Chrome 菜单 → 添加到主屏幕。这样会以全屏方式打开，接近 App 体验。

## 更新题库

在电脑上改 `data.js` / `dict.js` 后推送：

```powershell
git add -A
git commit -m "更新题库"
git push
```

`annotations.js` 通常不用手动改，网页会自己提交。
