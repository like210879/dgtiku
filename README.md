# 电工考证刷题 · iOS 安装包（巨魔 / TrollStore 用）

这个文件夹把现有的单文件网页版包成一个 iOS App（WKWebView 壳，离线运行，不依赖网络）。
IPA 里的可执行文件只能在 macOS 的 Xcode 里编译，所以这里用 **GitHub 免费的 macOS 机器**自动编译，产出一个 **未签名 IPA**，巨魔可以直接安装。

---

## 一、你需要准备

- 一个 GitHub 账号（免费注册：https://github.com/signup）
- iPhone 上已装好巨魔（TrollStore）
- 电脑能上网

## 二、把项目传到 GitHub（5 分钟）

1. 打开 https://github.com/new ，仓库名填 `dgtiku`，**选 Public**（公开仓库才有免登录的下载直链），点 **Create repository**。
2. 在新仓库页面点 **uploading an existing file**。
3. 把本文件夹 **`ios_app` 里面的全部内容**拖进网页上传框：
   - `www/`（内含 `index.html`，刷题应用本体）
   - `assets/`（内含 `icon-only.png`，App 图标）
   - `.github/`（自动化脚本，Windows 资源管理器可直接拖）
   - `package.json`、`capacitor.config.json`、`README.md`
4. 点 **Commit changes**。

## 三、一键编译（8–12 分钟）

1. 打开仓库的 **Actions** 标签页。
2. 左侧点 **构建 iOS 安装包** → 右侧 **Run workflow** → 绿色 **Run workflow**。
3. 等状态变绿勾。
4. 编译成功后，工作流会自动把 IPA 发布成一个 **Release**，直链固定为：

   ```
   https://github.com/<你的用户名>/dgtiku/releases/download/v1.0/dgtiku.ipa
   ```

   例：用户名是 `likc1988`，链接就是
   `https://github.com/likc1988/dgtiku/releases/download/v1.0/dgtiku.ipa`

   公开仓库的这个链接**不需要登录**，手机 Safari 直接点就能下（下完在「文件」里找）。
   仓库页面右侧 **Releases** 里也能看到同一个文件。

5. 想要 Actions 里那个附件 zip（`dgtiku-ipa`），也可以，但那个需要登录 GitHub 才能下载，用 Release 直链更省事。

## 四、装到 iPhone

1. 手机 Safari 打开上面的 Release 直链 → 下载 `dgtiku.ipa`。
2. 打开「文件」App，长按该文件 → **共享** → **TrollStore**（或先开巨魔，点右上角 + 选文件）。
3. 巨魔里点 **Install**，桌面出现「电工刷题」图标，点开即用。

## 五、这个 App 的实际情况

- **完全离线**：600 道题都在 App 本地，飞行模式可刷。
- **进度存本机**：分题型断点续做、错题本（分判断/单选/多选）、收藏夹、统计都存在 App 自己的容器里，关掉重开都在。
- **和网页版数据不互通**：网页版存浏览器，App 存独立容器，各记各的。
- **未签名**：这正是巨魔的用法，不需要证书、不会过期；不能上架 App Store，也不能装到没有巨魔的设备。

## 六、编译失败了怎么办

把 Actions 里红色那一步的日志（`xcodebuild` 报错那几行）发出来，我改脚本再跑。常见三类：
- Capacitor 版本与 iOS 部署目标不匹配；
- 图标资源缺必需尺寸；
- 原生依赖需要在 CI 里补 `pod install`。

## 七、其他

- `make_icon.py`：图标生成脚本（改配色或文字后可重跑）。
- 想换成别的应用名：改 `capacitor.config.json` 的 `appName` 和工作流里 `CFBundleDisplayName` 那一行。
- 想重编同一版本：再点一次 Run workflow，Release 里的 `dgtiku.ipa` 会被新的覆盖。
