# lzh0394's Scoop Bucket

[![CI](https://github.com/lzh0394/bucket/actions/workflows/ci.yml/badge.svg)](https://github.com/lzh0394/bucket/actions/workflows/ci.yml)
[![Excavator](https://github.com/lzh0394/bucket/actions/workflows/excavator.yml/badge.svg)](https://github.com/lzh0394/bucket/actions/workflows/excavator.yml)

个人 [Scoop](https://scoop.sh) bucket，用于存放自用与自维护的 Windows 软件清单。
脚手架来自 [ScoopInstaller/BucketTemplate](https://github.com/ScoopInstaller/BucketTemplate)。

## 安装

```pwsh
scoop bucket add lzh0394 https://github.com/lzh0394/bucket
scoop install lzh0394/<manifest-name>
```

## 本仓库包含的清单

| Manifest | 说明 |
| --- | --- |
| [`en-croissant`](https://github.com/franciscoBSalgueiro/en-croissant) | 开源国际象棋数据库 / GUI / 分析工具。`checkver` + `autoupdate` 已配好，版本跟进由 Excavator 自动完成。 |

## 新增一个清单

复制模板并填写，然后用 scoop 自带的维护工具在本地校验：

```pwsh
Copy-Item bucket\app-name.json.template bucket\<app-name>.json

pwsh bin\checkurls.ps1   <app-name>   # 所有 URL 是否可下载
pwsh bin\checkhashes.ps1 <app-name>   # hash 是否与上游一致（并可直接修正）
pwsh bin\formatjson.ps1  <app-name>   # 统一 JSON 格式
pwsh bin\checkver.ps1    <app-name>   # 能否识别到新版本
```

`bin\*.ps1` 都是 scoop 核心工具（`scoop prefix scoop` 下的 `bin\`）的转发壳，
需要 `SCOOP_HOME` 或本机已安装 scoop。

## 自动化

- **Excavator**（`.github/workflows/excavator.yml`）：每 4 小时由
  [ScoopInstaller/GithubActions](https://github.com/ScoopInstaller/GithubActions)
  按 manifest 里的 `checkver` / `autoupdate` 规则自动跟进上游新版本。
- **CI**（`.github/workflows/ci.yml`）：在 `windows-latest` 上用 Pester 校验全部 manifest。
- **Pull Requests / Issue 处理**：自动检查 PR 的 JSON 规范、必填字段（`description`、`license`）、
  hash、`checkver` 与 `autoupdate` 是否可用。

## License

[Unlicense](LICENSE)。
