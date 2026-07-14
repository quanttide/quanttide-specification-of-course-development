# 状态枚举

课程云使用两套独立的状态枚举，分别控制**内容研发流程**和**教学实施流程**，互不干扰。

## ContentStatus

ContentStatus 控制课程内容的研发状态。课程单位实体（Program / Course / Phase / Lesson 等）通过该状态标记内容的可用性。

| 值 | 含义 |
|----|------|
| `DRAFT` | 草稿，研发中，教学侧不可见 |
| `PUBLISHED` | 已发布，可供教学引用 |

### 规则

- 仅 `PUBLISHED` 的内容可被 Class 引用
- 删除 `PUBLISHED` 内容必须先下架（降为 `DRAFT`）
- 发布上级容器（如 Course）时，需检查子级（如 Lesson）状态，有 `DRAFT` 子级则提示确认

## ClassStatus

ClassStatus 控制班级的教学实施状态。Class 通过该状态标记教学阶段的推进。

| 值 | 含义 |
|----|------|
| `PREPARING` | 筹备中，可调整教学计划 |
| `ACTIVE` | 进行中，教学计划锁定仅可微调 |
| `ENDED` | 已结束，学员成绩冻结 |
