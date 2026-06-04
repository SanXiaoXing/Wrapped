---
checkData:
  - { date: "2026-06-01", completed: true, note: "保持情绪稳定，没有被他人课题影响" }
  - { date: "2026-06-02", completed: true, note: "拒绝了不属于自己的请求" }
  - { date: "2026-06-03", completed: false, note: "" }
---

# 课题分离

> 「课题分离」源于阿德勒心理学：区分**自己的课题**与**他人的课题**，只为自己的人生负责。
> 每天勾选表示当天做到了课题分离，并在备注中记录心得。

---

## 📊 当月统计

```dataviewjs
try {
	const rawData = dv.current().checkData;
	if (!rawData || rawData.length === 0) {
		dv.paragraph("*暂无打卡数据，请在 frontmatter 的 checkData 中添加。*");
	} else {
		const checkData = rawData.map(d => ({
			date: String(d.date),
			completed: d.completed === true,
			note: String(d.note || "")
		}));

		const now = new Date();
		const curYear = now.getFullYear();
		const curMonth = now.getMonth() + 1;
		const todayDate = now.getDate();
		const daysInMonth = new Date(curYear, curMonth, 0).getDate();

		const pad = n => String(n).padStart(2, "0");
		const todayStr = `${curYear}-${pad(curMonth)}-${pad(todayDate)}`;

		// 当月数据
		const monthData = checkData.filter(d => {
			const p = d.date.split("-");
			return parseInt(p[0]) === curYear && parseInt(p[1]) === curMonth;
		});

		const completedDays = monthData.filter(d => d.completed).length;
		const rate = ((completedDays / daysInMonth) * 100).toFixed(1);

		const todayItem = checkData.find(d => d.date === todayStr);
		const todayCompleted = todayItem && todayItem.completed;

		// 连续打卡：从今天往前数连续完成的天数
		let streak = 0;
		const checkDate = new Date(curYear, curMonth - 1, todayDate);
		for (let i = 0; i < 365; i++) {
			const ds = `${checkDate.getFullYear()}-${pad(checkDate.getMonth() + 1)}-${pad(checkDate.getDate())}`;
			const item = checkData.find(x => x.date === ds);
			if (item && item.completed) {
				streak++;
				checkDate.setDate(checkDate.getDate() - 1);
			} else {
				break;
			}
		}

		dv.paragraph(`- **当月已完成：** ${completedDays} / ${daysInMonth} 天（${rate}%）`);
		dv.paragraph(`- **今日状态：** ${todayCompleted ? "✅ 已完成" : "⬜ 未完成"}`);
		dv.paragraph(`- **连续打卡：** ${streak} 天 🔥`);
	}
} catch (e) {
	dv.paragraph(`⚠️ 渲染出错：${e.message}`);
}
```

---

## 📅 当月日历

> ✅ 已做到　⬜ 未做到　**加粗**为今日

```dataviewjs
try {
	const rawData = dv.current().checkData;
	if (!rawData || rawData.length === 0) {
		dv.paragraph("*暂无数据*");
	} else {
		const checkData = rawData.map(d => ({
			date: String(d.date),
			completed: d.completed === true,
			note: String(d.note || "")
		}));

		const now = new Date();
		const curYear = now.getFullYear();
		const curMonth = now.getMonth() + 1;
		const todayDate = now.getDate();
		const pad = n => String(n).padStart(2, "0");
		const todayStr = `${curYear}-${pad(curMonth)}-${pad(todayDate)}`;

		const firstDayOfWeek = new Date(curYear, curMonth - 1, 1).getDay();
		const offset = (firstDayOfWeek + 6) % 7;
		const daysInMonth = new Date(curYear, curMonth, 0).getDate();

		const rows = [];
		let row = new Array(7).fill(" ");

		for (let day = 1; day <= daysInMonth; day++) {
			const col = (offset + day - 1) % 7;
			if (col === 0 && day > 1) {
				rows.push([...row]);
				row = new Array(7).fill(" ");
			}
			const dateStr = `${curYear}-${pad(curMonth)}-${pad(day)}`;
			const item = checkData.find(d => d.date === dateStr);
			const mark = item && item.completed ? "✅" : "⬜";
			const isToday = dateStr === todayStr;
			row[col] = isToday ? `**${day}${mark}**` : `${day}${mark}`;
		}
		rows.push([...row]);

		dv.table(["一", "二", "三", "四", "五", "六", "日"], rows);
	}
} catch (e) {
	dv.paragraph(`⚠️ 渲染出错：${e.message}`);
}
```

---

## 📝 当月打卡记录

```dataviewjs
try {
	const rawData = dv.current().checkData;
	if (!rawData || rawData.length === 0) {
		dv.paragraph("*暂无打卡记录*");
	} else {
		const checkData = rawData.map(d => ({
			date: String(d.date),
			completed: d.completed === true,
			note: String(d.note || "")
		}));

		const now = new Date();
		const curYear = now.getFullYear();
		const curMonth = now.getMonth() + 1;

		const monthData = checkData.filter(d => {
			const parts = d.date.split("-");
			return parseInt(parts[0]) === curYear && parseInt(parts[1]) === curMonth;
		});

		if (monthData.length === 0) {
			dv.paragraph("*本月暂无打卡记录*");
		} else {
			const sorted = [...monthData].sort((a, b) => b.date.localeCompare(a.date));
			dv.table(
				["日期", "状态", "备注"],
				sorted.map(d => [
					d.date,
					d.completed ? "✅ 已完成" : "⬜ 未完成",
					d.note || "—"
				])
			);
		}
	}
} catch (e) {
	dv.paragraph(`⚠️ 渲染出错：${e.message}`);
}
```

---

## ✍️ 全部历史记录

```dataviewjs
try {
	const rawData = dv.current().checkData;
	if (!rawData || rawData.length === 0) {
		dv.paragraph("*暂无历史记录*");
	} else {
		const checkData = rawData.map(d => ({
			date: String(d.date),
			completed: d.completed === true,
			note: String(d.note || "")
		}));

		const completed = checkData.filter(d => d.completed).length;
		const total = checkData.length;
		dv.paragraph(`**累计：** ${completed} / ${total} 天`);

		const sorted = [...checkData].sort((a, b) => b.date.localeCompare(a.date));
		dv.table(
			["日期", "状态", "备注"],
			sorted.map(d => [
				d.date,
				d.completed ? "✅" : "⬜",
				d.note || "—"
			])
		);
	}
} catch (e) {
	dv.paragraph(`⚠️ 渲染出错：${e.message}`);
}
```

---

## 💡 使用说明

1. **新增打卡**：在文件最上方的 `checkData` 数组中按格式添加一行
   ```yaml
   - { date: "YYYY-MM-DD", completed: true, note: "你的心得" }
   ```
2. **修改状态**：把 `completed` 改为 `true` / `false`
3. **添加备注**：在 `note` 字段中填写文字
4. **保存后** 上方日历、统计、记录会自动刷新
