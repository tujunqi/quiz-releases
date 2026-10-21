# 概率统计刷题 · 版本发布仓库

供 App 内「检查更新」读取版本声明，并托管各版本 APK。

## 文件

- `version.json` —— 当前线上版本声明，App 检查更新时拉取这一个文件
- GitHub Releases —— 各版本 APK 下载

## 发版流程（在 `C:/Users/tu/Desktop/1111/probability-statistics-quiz` 下执行）

1. 打包新 APK，产物归档到 `releases/`
2. `gh release create v1.4 "releases/概率统计刷题-v1.4-debug.apk" -t "v1.4" -n "更新说明"`
3. 修改本仓库 `version.json` 的 `version` / `url` / `desc`，push 到 main
4. 同步 App 内的 `APP_VER` 常量，避免版本号不一致

## 更新检查地址

国内可达性优先用 jsDelivr 镜像，GitHub raw 作为兜底：

- `https://cdn.jsdelivr.net/gh/tujunqi/quiz-releases@main/version.json`
- `https://raw.githubusercontent.com/tujunqi/quiz-releases/main/version.json`
