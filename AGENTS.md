# AGENTS.md — utils-support-resource-parent

> **模块边界与依赖铁律：[`../规则.md`](../规则.md)（`D:\ch\project\规则.md`），动手前必须先读并遵守。违反 R1/R2 = S0 严重 bug。**
> 全局规则：`C:\Users\Administrator\.config\opencode\AGENTS.md`；源码编辑红线：`C:\Users\Administrator\.agents\GLOBAL-SOURCE-EDITING-RULES.md`。

## 本仓库定位

公共资源与文件仓储（词典、模型等），通过 Maven 分发。**资源与打包约定仓库，不含业务规则。**

## 三条铁律（摘要，全文以 `规则.md` 为准）

1. **R1 禁止业务穿插**：不得把业务 starter 的逻辑搬进来；上层也不得另建一套资源加载实现。
2. **R2 禁止穿透持久层**：跨模块取数只走契约。
3. **R4 缺能力就下沉**：通用资源加载/校验能力扩到底层，保持唯一实现。

## 交付闸门

`mvn -o -q compile -DskipTests` EXIT=0；编码 `BOM=0 / U+FFFD=0`；资源路径变更必须同步上层引用。

## 提交

只提交自己的文件；资源变更需标注受影响模块。
