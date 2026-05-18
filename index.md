# 📊 工作仪表板

> `#timeline` `#周报` `#项目进度` `#时间线`

---

## 🗓️ 周报列表

> 全部周报，按时间倒序排列，显示每份周报的已完成/待办任务数

```dataview
TABLE
  date_range AS "日期范围",
  month AS "月份",
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

> 从所有周报中提取已完成的任务，按月份和周报分组展示

```dataviewjs
// 按月份分组展示
const pages = dv.pages('"2026"').where(p => p.file.name.contains("Week"));
const monthGroups = {};

// 按月份分组
for (const page of pages) {
  const month = page.file.folder.split('/').pop();
  if (!monthGroups[month]) monthGroups[month] = [];
  monthGroups[month].push(page);
}

// 按月份倒序输出
for (const month of Object.keys(monthGroups).sort().reverse()) {
  dv.header(3, `📅 ${month}`);
  for (const page of monthGroups[month].sort(p => p.date_range, 'desc')) {
    dv.header(4, `📄 ${page.file.name}`);
    const tasks = page.file.tasks.filter(t => t.completed);
    if (tasks.length > 0) {
      dv.table(
        ["任务", "完成状态"],
        tasks.map(t => [t.text, "✅"])
      );
    } else {
      dv.paragraph("*无已完成任务*");
    }
  }
}
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
