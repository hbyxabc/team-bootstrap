---
title: 文档管理体系
status: approved
owner: Lead
updated: 2026-03-23
---

# 文档管理体系

## 一、设计原则

1. **单一信息源** — 同一信息只在一处维护，其他地方引用
2. **Token 效率** — Agent 读最少文档获得最完整信息
3. **可执行性** — 每份文档都有明确的消费者和用途
4. **自然淘汰** — 文档有明确的生命周期，过期即归档

---

## 二、目录结构

```
docs/
├── board.md                  # 任务看板（Lead 维护）
├── handoff.md                # 会话交接（Lead 维护）
├── doc-management.md         # 本文档（Lead 维护）
├── team-workflow.md          # 工作流程与协作（Lead 维护）
│
├── product/                  # 产品知识库（Product Designer 维护）
│   ├── decisions.md          # 决策记录 + 产品愿景（常驻，追加写入）
│   ├── modules/              # [规格] 文档（per 模块，常驻）
│   ├── requirements/         # [需求] 文档（per 变更，活跃期）
│   └── archive/              # 已归档的产品文档
│
├── design/                   # 设计知识库（Product Designer 维护）
│   ├── design-system.md      # 设计规范（常驻）
│   ├── design-index.md       # 设计索引 + .pen 映射（常驻）
│   ├── project-ui.pen      # 唯一设计稿
│   ├── deliverables/         # [设计] 文档（per 变更，活跃期）
│   └── archive/              # 已归档的设计文档
│
└── technical/                # 技术知识库（CTO 维护）
    ├── tech-index.md         # 技术索引（常驻）
    ├── architecture.md       # 系统架构（常驻）
    ├── deployment.md         # 部署流程（常驻）
    ├── deliverables/         # [方案]/[审查] 文档（per 变更，活跃期）
    └── archive/              # 已归档的技术文档
```

---

## 三、文档类型

### 常驻文档（永不归档，持续更新）

| 类型 | 位置 | 单位 | 说明 |
|------|------|------|------|
| [规格] | product/modules/ | per 模块 | **最新最全的产品需求文档**（非总结） |
| 设计规范 | design/design-system.md | 全局 | 颜色、字体、间距、组件规则 |
| 设计索引 | design/design-index.md | 全局 | .pen 映射 + 变更日志 |
| 技术参考 | technical/*.md | per 领域 | 架构、部署 |
| 产品决策 + 愿景 | product/decisions.md | 全局 | 追加写入，不修改。含产品愿景、北极星指标 |

### 活跃文档（有生命周期，最终归档）

| 类型 | 位置 | 单位 | 生命周期 |
|------|------|------|---------|
| [需求] | product/requirements/ | per 变更 | draft → review → approved → implemented → 归档 |
| [设计] | design/deliverables/ | per 变更 | draft → review → approved → implemented → 归档 |
| [方案] | technical/deliverables/ | per 变更 | draft → review → approved → implemented → 归档 |
| [审查] | technical/deliverables/ | per 审查 | 产出后 → 结论落地 → 归档 |

---

## 四、[规格] 文档标准

### 定位

**[规格] = 该模块当前最新、最全的产品需求文档。**

- 不是总结，而是完整的产品定义
- 读 [规格] 就能完整理解和实现该模块的任何功能
- 详细程度等于或高于任何一份 [需求] 文档
- 是 Agent 做该模块任务时的唯一产品输入

### 标准结构

```markdown
---
title: 模块名称
type: 规格
module: slug
product_version: v0.0.X
status: implemented
owner: Product Designer
updated: YYYY-MM-DD
---

## 一、概览
## 二、用户故事
## 三、功能清单
## 四、页面/组件结构
## 五、交互规格（精确到状态变化、动画参数）
## 六、业务规则（阈值、条件、优先级、公式）
## 七、数据流（API 端点、请求/响应、字段说明）
## 八、边界条件（空数据、极端值、错误处理）
## 九、移动端适配
## 十、与其他模块的关系
## 十一、SEO 规格（如适用）
## 十二、未来迭代方向
## 十三、与设计/代码的一致性
## 十四、验收标准
## 十五、变更记录
```

### 更新规则

- [需求] 文档实现上线后，将其细节**原样合并**进 [规格]
- 合并后 [需求] 文档归档
- [规格] 的变更记录追加本次合并的条目
- product_version 字段更新到当前版本

---

## 五、创建 vs 更新判断框架

```
要写文档了 →

  ┌─ 是规格文档吗？
  │   → YES: 找到现有 [规格]，直接更新，NEVER 新建
  │
  ├─ 是需求/方案/设计文档？
  │   ├─ 同一版本 + 同一目标 + 方案调整？ → 更新现有文档
  │   └─ 新版本 / 目标变了 / 推翻旧方案？ → 新建文档，旧版归档
  │
  └─ 跨多个模块？
      → 需求/方案用一份文档管（按变更命名）
      → 上线后分别回流到各受影响的 [规格]
```

### 判断维度

| 维度 | 同一变更（更新旧文档） | 不同变更（创建新文档） |
|------|----------------------|----------------------|
| 版本 | 同一版本内的迭代细化 | 跨版本的重新设计 |
| 目标 | 目标不变，方案调整 | 目标本身变了 |
| 范围 | 范围基本一致 | 范围显著扩大或缩小 |
| 决策链 | 延续之前的决策 | 推翻之前的决策 |

---

## 六、命名规范

### 文件命名

```
常驻文档：  {slug}.md                           # 如 architecture.md
规格文档：  [规格] {module-slug}.md              # 如 [规格] homepage.md
活跃文档：  [类型] {feature-slug} {YYYY-MM-DD}.md  # 如 [需求] topic-system-v3 2026-03-22.md
归档文档：  [归档] {original-name} {YYYY-MM-DD}.md
```

### 类型前缀

| 前缀 | 所属目录 | 说明 |
|------|---------|------|
| [规格] | product/modules/ | 模块产品定义 |
| [需求] | product/requirements/ | 变更需求 |
| [设计] | design/deliverables/ | 设计交付 |
| [方案] | technical/deliverables/ | 技术方案 |
| [审查] | technical/deliverables/ | 审查报告 |
| [归档] | */archive/ | 已归档文档 |

**每个目录只放对应类型前缀的文件。不允许混放。**

### Frontmatter 标准

```yaml
---
title: 文档标题
status: draft | review | approved | implemented | archived
owner: Product Designer | CTO | Dev | Lead
updated: YYYY-MM-DD
# 以下为 [规格] 文档专用：
type: 规格
module: slug
product_version: v0.0.X
---
```

---

## 七、生命周期管理

### 状态流转

```
draft → review → approved → implemented → archived
```

| 状态 | 含义 | 触发条件 |
|------|------|---------|
| draft | 起草中 | 创建时 |
| review | 待审批 | 提交审查时 |
| approved | 已批准 | 创始人/CTO 确认时 |
| implemented | 已实现 | 代码上线后 |
| archived | 已归档 | 内容合并到 [规格] 后 |

### 归档规则

| 文档类型 | 归档触发条件 | 归档操作 |
|---------|-------------|---------|
| [需求] | 功能上线 + 内容已合并到 [规格] | 移入 archive/，前缀改为 [归档] |
| [设计] | 功能上线 + .pen 已更新 | 移入 archive/ |
| [方案] | 功能上线 + 结论已落地 | 移入 archive/ |
| [审查] | 结论已落地到代码/规则 + 超过 7 天 | 移入 archive/ |

### [审查] 文档特殊规则

[审查] 是一次性输出，不是持续维护的文档。其价值在于**结论**，不在于报告本身。

1. 审查产出后，结论中的关键发现应落地到：
   - 代码修复 → 提 task
   - 规则沉淀 → 写入 `.claude/rules/`
   - 架构决策 → 写入 `decisions.md`
2. 结论落地后，[审查] 文档即可归档
3. 超过 7 天未归档的 [审查] 文档应主动检查是否可归档

---

## 八、索引维护

- `design/design-index.md`：维护 .pen 结构映射 + 变更日志。每次 .pen 修改后更新。
- `technical/tech-index.md`：维护常驻参考文档 + deliverables 列表。每次新增/归档后更新。
- 产品索引：`product/modules/` 目录即索引，无需单独索引文件。
- **索引必须与实际文件一致。** 归档后同步移除索引条目。

---

## 九、跨模块变更与同步

### 跨模块变更

需求/方案用一份文档管（按变更命名），上线后分别回流到各受影响的 [规格]。

### 文档同步

谁发现需要修改 → 立即通知文档 Owner → Owner 更新 → 判断连锁影响 → 通知下游 Owner。**不允许"先记着后面再改"。**

### .pen 管理

唯一文件 `docs/design/project-ui.pen`。Page=模块，Frame=组件，Sub-frame=状态。只有 Product Designer 可通过 `batch_design` 修改。每次修改记录变更日志到 design-index.md。

---

## 十、增量控制

1. **[规格] 只更新不新建** — 每个模块只有一份
2. **同一变更迭代在原文档更新** — 不创建 v2、v3 版本
3. **[审查] 结论落地后 7 天内归档**
4. **归档 > 3 个月可删除**
5. **版本发布时检查索引一致性**

---

## Changelog

| 日期 | 变更 |
|------|------|
| 2026-03-23 | v1.0 初版：从 team-rules.md 文档管理章节 + memory/feedback_doc_management.md 整合升级为独立完整体系 |
