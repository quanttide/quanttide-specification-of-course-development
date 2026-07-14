# 课程树

课程树是课程内容的结构化描述。Program（专业）、Course（课程）、Phase（阶段）、Lesson（课时）、Scene（场景）、Step（步骤）六级嵌套，表示教学内容的组织关系。

## Program

| 字段 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `id` | string | 是 | — | 唯一标识 |
| `name` | string | 是 | — | 专业名称 |
| `description` | string | 否 | `""` | 专业描述 |
| `status` | ContentStatus | 否 | `DRAFT` | 发布状态 |
| `courses` | Course[] | 否 | `[]` | 包含的课程列表 |

## Course

| 字段 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `id` | string | 是 | — | 唯一标识 |
| `name` | string | 是 | — | 课程名称 |
| `description` | string | 否 | `""` | 课程描述 |
| `status` | ContentStatus | 否 | `DRAFT` | 发布状态 |
| `sortOrder` | int | 否 | `0` | 同级排序序号 |
| `phases` | Phase[] | 否 | `[]` | 包含的阶段列表 |

## Phase

| 字段 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `id` | string | 是 | — | 唯一标识 |
| `name` | string | 是 | — | 阶段名称 |
| `description` | string | 否 | `""` | 阶段描述 |
| `status` | ContentStatus | 否 | `DRAFT` | 发布状态 |
| `sortOrder` | int | 否 | `0` | 同级排序序号 |
| `lessons` | Lesson[] | 否 | `[]` | 包含的课时列表 |

## Lesson

| 字段 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `id` | string | 是 | — | 唯一标识 |
| `title` | string | 是 | — | 课时标题 |
| `description` | string | 否 | `""` | 课时描述 |
| `duration` | int | 否 | `45` | 课时时长（分钟） |
| `status` | ContentStatus | 否 | `DRAFT` | 发布状态 |
| `sortOrder` | int | 否 | `0` | 同级排序序号 |
| `scenes` | Scene[] | 否 | `[]` | 包含的场景列表 |

> Scene/Step 通过 `loadLesson(lessonId)` 按需加载，不随课程树一起展开。

## Scene

| 字段 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `id` | string | 是 | — | 唯一标识 |
| `name` | string | 是 | — | URL slug |
| `title` | string | 否 | `""` | 场景标题 |
| `steps` | Step[] | 否 | `[]` | 操作步骤列表 |
| `choices` | Choice[] | 否 | `[]` | 场景间跳转选项 |
| `verifyTip` | string | 否 | `""` | 验证提示 |
| `videoUrl` | string | 否 | `""` | 关联视频路径 |

## Step

| 字段 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `order` | int | 是 | — | 步骤序号 |
| `content` | string | 是 | — | 操作描述 |

## Choice

| 字段 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `label` | string | 是 | — | 按钮文案 |
| `targetSceneId` | string | 是 | — | 目标场景 ID |

## ContentStatus

ContentStatus 控制课程内容的研发状态。上四级实体（Program / Course / Phase / Lesson）均通过该状态标记可用性。

| 值 | 含义 |
|----|------|
| `DRAFT` | 草稿，研发中，教学侧不可见 |
| `PUBLISHED` | 已发布，可供教学引用 |

### 规则

- 仅 `PUBLISHED` 的内容可被 Class 引用
- 删除 `PUBLISHED` 内容必须先下架（降为 `DRAFT`）
- 发布上级容器时，需检查子级状态，有 `DRAFT` 子级则提示确认

## JSON 示例

```json
{
  "id": "p1",
  "name": "大数据微专业",
  "status": "published",
  "courses": [
    {
      "id": "c1",
      "name": "氛围编程",
      "sortOrder": 0,
      "phases": [
        {
          "id": "ph1",
          "name": "安装编程工具",
          "sortOrder": 0,
          "lessons": [
            {
              "id": "l1",
              "title": "安装编程环境",
              "duration": 30,
              "sortOrder": 0,
              "scenes": [
                {
                  "id": "s1",
                  "name": "zed-setup",
                  "title": "Zed 的安装与初始化",
                  "steps": [
                    { "order": 1, "content": "访问 zed.dev 下载 Zed" },
                    { "order": 2, "content": "安装并打开 Zed" }
                  ],
                  "choices": [
                    { "label": "继续", "targetSceneId": "s2" }
                  ],
                  "verifyTip": "Zed 能正常编辑即完成",
                  "videoUrl": ""
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```
