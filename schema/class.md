# ClassTeaching

Class（班级）描述教学单位子领域的组织实施。Class 引用课程单位的内容进行教学，不重新定义教学内容。

## 字段

| 字段 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `id` | string | 是 | — | 唯一标识 |
| `name` | string | 是 | — | 班级名称 |
| `refName` | string | 是 | — | 引用的专业/课程名称（展示用） |
| `refType` | RefType | 否 | `PROGRAM` | 引用类型 |
| `refId` | string | 是 | — | 引用的 Program/Course ID |
| `status` | ClassStatus | 否 | `PREPARING` | 教学状态 |
| `startDate` | string | 是 | — | ISO 日期 |
| `endDate` | string | 是 | — | ISO 日期 |
| `studentCount` | int | 否 | `0` | 学员数 |
| `progress` | float | 否 | `0.0` | 教学进度 0.0 ~ 1.0 |

## 枚举

### RefType

| 值 | 含义 |
|----|------|
| `PROGRAM` | 引用整个专业作为教学内容 |
| `COURSE` | 引用单个课程作为教学内容 |

### ClassStatus

| 值 | 含义 |
|----|------|
| `PREPARING` | 筹备中，可调整教学计划 |
| `ACTIVE` | 进行中，教学计划锁定仅可微调 |
| `ENDED` | 已结束，学员成绩冻结 |

## 数据关系

```
Class ──引用──> Program | Course
```

- Class 引用 Program/Course 的内容，不包含 Lesson 副本
- 课程单位侧内容更新后，Class 侧可选择性同步
- 一个 Program/Course 可被多个 Class 引用
- Class 不感知组织侧信息

## JSON 示例

```json
{
  "id": "cls-1",
  "name": "浙理班级",
  "refName": "大数据微专业",
  "refType": "PROGRAM",
  "refId": "p1",
  "status": "ACTIVE",
  "startDate": "2026-07-01",
  "endDate": "2026-08-01",
  "studentCount": 45,
  "progress": 0.35
}
```
