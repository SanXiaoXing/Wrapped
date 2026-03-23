# 📊 工作仪表板

> `#timeline` `#周报` `#项目进度` `#时间线`

---

## 🗓️ 周报列表

> 全部周报，按时间倒序排列，显示每份周报的已完成/待办任务数

```dataview
TABLE
  date_range AS "日期范围",
  length(filter(file.tasks, (t) => t.completed)) AS "✅完成",
  length(filter(file.tasks, (t) => !t.completed)) AS "⏳待办"
FROM "2026"
WHERE contains(file.name, "Week")
SORT date_range DESC
LIMIT 20
```

---

## 📝 待处理任务

> 从所有周报中提取未完成的勾选任务

```dataview
TASK
FROM "2026"
WHERE contains(file.name, "Week") AND !completed
GROUP BY file.link
SORT rows.date_range[0] DESC, key DESC
LIMIT 20
```

---

## ✅ 已完成任务

> 从所有周报中提取已完成的任务

```dataview
TASK
FROM "2026"
WHERE contains(file.name, "Week") AND completed
GROUP BY file.link
SORT rows.date_range[0] DESC, key DESC
LIMIT 20
```

---

## 📅 时间线

> 按年份组织，周报按时间正序排列，快速导航

### 2026 年

```dataview
TABLE
  date_range AS "日期范围"
FROM "2026"
WHERE contains(file.name, "Week")
SORT date_range ASC
```
