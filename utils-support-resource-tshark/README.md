# utils-support-resource-tshark

tshark 便携包**资源**（按平台），供 monitor 在无外网环境安装 tshark。

同类参照：`utils-support-resource-vnc` 也是把各平台二进制放在 `resources/<产品>/<平台>/`，
本模块沿用同一口径。

## 目录

```
src/main/resources/tshark/
  windows-x86_64/tshark-package.zip
  linux-x86_64/tshark-package.deb        # 或 tshark-package.tar.xz / .tar.gz
  darwin-aarch64/tshark-package.tar.gz
  darwin-x86_64/tshark-package.tar.gz
```

目录为空时 monitor 的 `bundledPackage()` 返回 null，继续走 `package-url` /
`package-path`，行为与没有本模块一致。

## 优先级（monitor 侧）

```
package-path(-平台)  >  本模块的 classpath 资源  >  package-url(-平台)
```

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
