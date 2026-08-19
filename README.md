# utils-support-resource-parent

大体积数据文件分发仓库，通过 Maven 依赖引用，避免将词典、模型等资源打包进业务 JAR。

## 模块

| 模块 | 内容 | 大小 |
|------|------|------|
| `utils-support-resource-emoji` | Emoji 数据文件（emoji.json.xz） | 28 KB |
| `utils-support-resource-pinyin` | 拼音词典（pinyin.dict.xz, chinese.dict.xz, mutil_pinyin.dict.xz） | 91 KB |

## 使用方式

在 `pom.xml` 中添加依赖：

```xml
<dependency>
    <groupId>com.chua</groupId>
    <artifactId>utils-support-resource-emoji</artifactId>
    <version>4.0.0.41</version>
</dependency>
```

需要配置 GitHub Packages 仓库：

```xml
<repository>
    <id>github-resource</id>
    <url>https://maven.pkg.github.com/CHTK001/utils-support-resource-parent</url>
</repository>
```

## 发布

```bash
mvn deploy
```

需要 `~/.m2/settings.xml` 中配置 GitHub Token：

```xml
<server>
    <id>github-resource</id>
    <username>CHTK001</username>
    <password>${GITHUB_TOKEN}</password>
</server>
```
