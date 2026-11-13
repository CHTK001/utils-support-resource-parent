# utils-support-resource-tshark

tshark 便携包**资源**（按平台），供 monitor 在无外网环境安装 tshark。

同类参照：`utils-support-resource-vnc` 也是把各平台二进制放在 `resources/<产品>/<平台>/`，
本模块沿用同一口径。

## 目录

```
src/main/resources/tshark/
  windows-x86_64/tshark-package.zip      # 已内置（见下）
  linux-x86_64/                          # 待放：deb / tar.xz / tar.gz
  darwin-aarch64/
  darwin-x86_64/
```

## 已内置：Windows 精简包

| 项 | 值 |
|---|---|
| 版本 | TShark (Wireshark) 4.4.19 |
| 大小 | 41.3 MB（解压 134.6 MB） |
| sha256 | `e9143e53a70010099d9f67fbff0f10e6a82297d1abbee1e22e70381657ab2944` |
| 内容 | `tshark.exe`、`dumpcap.exe`、全部非 GUI 依赖 DLL（libwireshark/libwiretap/libwsutil/glib/gcrypt/gnutls/…）、`tls` 与 `protobuf` 插件目录 |
| 排除 | `Wireshark.exe`、Qt6*、opengl32sw、dxcompiler/dxil/d3dcompiler、WinSparkle（GUI 专用，tshark 不需要） |

实测链路：资源取包 → 解压 59 个文件 → 找到 `tshark.exe` → 版本 `4.4.19` → `-D` 列出网卡（有 Npcap 时 `captureReady=true`）。

目录为空时 monitor 的 `bundledPackage()` 返回 null，继续走 `package-url` /
`package-path`，行为与没有本模块一致。

## 优先级（monitor 侧）

```
package-path(-平台)  >  本模块的 classpath 资源  >  应用目录下的 tshark/<平台>/  >  package-url(-平台)
```

## 不加依赖也能用

不想让 monitor 背 41MB 依赖时，把 jar 里的资源解到应用工作目录即可：

```bash
unzip utils-support-resource-tshark-4.0.0.43.jar 'tshark/*' -d /opt/monitor
# 结果：/opt/monitor/tshark/windows-x86_64/tshark-package.zip
```

monitor 启动时会先查 classpath，再查 `./tshark/<os>-<arch>/`，命中就直接解包装到
`~/spring-support/tshark`，不需要重新打包、也不需要改依赖。

## 产物从哪来

| 平台 | 产物 | 说明 |
|---|---|---|
| Windows x64 | Wireshark portable zip | 官方安装包解出的 `Wireshark/` 目录压成 zip，需带同目录 DLL |
| Linux x64 | `tshark_<ver>_amd64.deb` | 发行版/官方 PPA 产物；monitor 会取出 `data.tar.*` 再解 |
| Linux x64（自建） | `tar.xz` | 含 `usr/bin/tshark` 与依赖 `libwiretap*` 等 |
| macOS | `tar.gz` | 从 .pkg 取 `payload` 重打，或用 Homebrew 目录打包 |

放入后记录校验值：

```
sha256sum src/main/resources/tshark/linux-x86_64/tshark-package.deb
```

## 体积与许可

三平台合计约 170MB。GitHub 单文件上限 100MB（更大需 LFS），Gitee 有仓库配额，
所以**不建议三个平台的包同时进主仓**：按部署平台放对应的一个即可；对外分发时注意
Wireshark 是 GPLv2，需同时提供对应源码或书面提供。
