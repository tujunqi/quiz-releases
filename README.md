# 概率统计刷题 · 版本发布仓库

App 内「检查更新」不读静态文件，直接查 GitHub Releases API：

```
https://api.github.com/repos/tujunqi/quiz-releases/releases/latest
```

`releases/latest` 返回**最新的非预发布**版本，App 拿它的 `tag_name` 与内置 `APP_VER` 比较，
下载链接取 `assets[]` 里 `.apk` 的 `browser_download_url`。所以本仓库不需要维护任何清单文件——
**发一个 Release，所有已安装的 App 就能检测到**。

## 为什么不用 raw.githubusercontent.com / jsDelivr

实测本机（国内网络）：

| 地址 | 结果 |
| --- | --- |
| `raw.githubusercontent.com` | TLS 握手失败（被墙） |
| `cdn.jsdelivr.net` | TLS 握手失败（被墙） |
| `api.github.com` | 200 ✅ |
| `github.com/.../releases/download/...` | 302 → 200 ✅ |

## 发版流程

在 `C:/Users/tu/Desktop/1111/probability-statistics-quiz` 下执行：

```bash
# 1. 打包新 APK，产物放进 releases/

# 2. 上传（资产文件名必须用 ASCII，GitHub 会吞掉中文字符）
cp "releases/概率统计刷题-v1.4-debug.apk" /tmp/quiz-v1.4.apk
gh release create v1.4 /tmp/quiz-v1.4.apk -R tujunqi/quiz-releases \
  -t "v1.4" -n "这次更新了什么，会原样显示在 App 的更新弹窗里"

# 3. 同步 App 内版本号：web/index.html 的 APP_VER，与 android Manifest 的 versionName/versionCode
```

## 灰度 / 回滚

- 新包先 `-p` 发成预发布，真机验证过再转正，期间同学仍停在稳定版：
  ```bash
  gh release create v1.5 /tmp/quiz-v1.5.apk -R tujunqi/quiz-releases -p -t "v1.5" -n "..."
  gh release edit v1.5 -R tujunqi/quiz-releases --latest   # 验证通过，转正
  gh release edit v1.5 -R tujunqi/quiz-releases --prerelease # 出问题，撤回
  ```
- 当前状态：`v1.2` 正式版，`v1.3` 预发布（未真机验证）。

## 国内下载提示

Release 资产走 `release-assets.githubusercontent.com`，速度不稳定。真要给同学用，
把 APK 同时丢一份到微信/QQ 群文件更省事；本仓库作为长期版本存档与更新探测源。
