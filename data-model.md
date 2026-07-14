# 数据模型

## 课程树：六级嵌套

```
Program
├── id: string
├── name: string
├── description: string
├── status: ContentStatus (draft | published)
└── courses: Course[]
    ├── id: string
    ├── name: string
    ├── description: string
    ├── status: ContentStatus
    ├── sortOrder: int
    └── phases: Phase[]
        ├── id: string
        ├── name: string
        ├── description: string
        ├── status: ContentStatus
        ├── sortOrder: int
        └── lessons: Lesson[]
            ├── id: string
            ├── title: string
            ├── description: string
            ├── duration: int (分钟)
            ├── status: ContentStatus
            ├── sortOrder: int
            └── scenes: Scene[]
                ├── id: string
                ├── name: string (URL slug)
                ├── title: string
                ├── steps: Step[]
                │   ├── order: int
                │   └── content: string
                ├── choices: Choice[]
                │   ├── label: string
                │   └── targetSceneId: string
                ├── verifyTip: string
                └── videoUrl: string
```

嵌套结构而非 ID 引用，减少查询次数。Lesson 的 Scene/Step 通过 `loadLesson()` 按需加载，不随树一起展开。

---

## ClassTeaching

### 字段

| 字段 | 类型 | 必填 | 默认 | 说明 |
|---|---|---|---|---|
| `id` | string | 是 | — | 唯一标识 |
| `name` | string | 是 | — | 班级名称 |
| `refName` | string | 是 | — | 引用的专业/课程名称（展示用） |
| `refType` | string | 否 | `"program"` | `"program"` / `"course"` |
| `refId` | string | 是 | — | 引用的 Program/Course ID |
| `status` | ClassStatus | 否 | `"preparing"` | 教学状态 |
| `startDate` | string | 是 | — | ISO 日期 |
| `endDate` | string | 是 | — | ISO 日期 |
| `studentCount` | number | 否 | `0` | 学员数 |
| `progress` | number | 否 | `0.0` | 教学进度 0.0 ~ 1.0 |

### ClassStatus 枚举

| 值 | 含义 |
|---|---|
| `"preparing"` | 筹备中，可调整教学计划 |
| `"active"` | 进行中，教学计划锁定仅可微调 |
| `"ended"` | 已结束，学员成绩冻结 |

### RefType 枚举

| 值 | 含义 |
|---|---|
| `"program"` | 引用整个专业作为教学内容 |
| `"course"` | 引用单个课程作为教学内容 |

### 数据关系

```
Class ──引用──> Program | Course
```

- Class 引用 Program/Course 的内容，不包含 Lesson 副本
- 课程单位侧内容更新后，Class 侧可选择性同步
- 一个 Program/Course 可被多个 Class 引用
- Class 不感知组织侧信息

---

## 状态枚举

### ContentStatus

| 值 | 含义 |
|---|---|
| `"draft"` | 草稿，研发中 |
| `"published"` | 已发布，可供教学引用 |
