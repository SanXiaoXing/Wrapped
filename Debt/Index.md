---
borrowData:
  - { date: "2026-03-02 09:51", amount: 100.00 }
  - { date: "2026-03-04 10:30", amount: 200.00 }
  - { date: "2026-03-05 14:08", amount: 500.00 }
  - { date: "2026-03-10 16:13", amount: 200.00 }
  - { date: "2026-03-12 15:37", amount: 200.00 }
  - { date: "2026-03-14 16:29", amount: 500.00 }
  - { date: "2026-03-16 10:49", amount: 300.00 }
  - { date: "2026-03-16 17:01", amount: 100.00 }
  - { date: "2026-03-17 16:12", amount: 300.00 }
  - { date: "2026-04-19 10:45", amount: 300.00 }
  - { date: "2026-04-19 17:34", amount: 500.00 }
  - { date: "2026-04-20 18:10", amount: 1500.00 }
  - { date: "2026-04-25 10:28", amount: 200.00 }
  - { date: "2026-05-01 18:10", amount: 200.00 }
  - { date: "2026-05-08 15:53", amount: 300.00 }
  - { date: "2026-05-08 16:23", amount: 200.00 }
  - { date: "2026-06-08 15:18", amount: 1000.00 }
repayData:
  - { date: "2026-03-02 14:19", amount: 100.00 }
  - { date: "2026-03-04 20:14", amount: 200.00 }
  - { date: "2026-03-10 17:05", amount: 200.00 }
  - { date: "2026-03-15 15:09", amount: 500.00 }
  - { date: "2026-03-31 23:10", amount: 300.00 }
  - { date: "2026-04-15 19:05", amount: 300.00 }
  - { date: "2026-04-26 18:05", amount: 800.00 }
  - { date: "2026-05-01 19:46", amount: 200.00 }
  - { date: "2026-05-12 22:16", amount: 500.00 }
---

# 宋赫

### 📊 借款与还款明细统计

#### 一、借款明细（转给宋赫的金额）

```dataviewjs
// 从页面 frontmatter 获取借款数据
const borrowData = dv.current().borrowData;

// 计算借款合计
const totalBorrow = borrowData.reduce((sum, item) => sum + item.amount, 0);

// 渲染表格
dv.table(
	["日期", "金额（元）"],
	borrowData.map(item => [
		item.date,
		item.amount.toFixed(2)
	])
);

// 显示合计
dv.paragraph(`**合计：${totalBorrow.toFixed(2)} 元**`);
```

#### 二、还款明细（转回的金额）

```dataviewjs
// 从页面 frontmatter 获取还款数据
const repayData = dv.current().repayData;

// 计算还款合计
const totalRepay = repayData.reduce((sum, item) => sum + item.amount, 0);

// 渲染表格
dv.table(
	["日期", "金额（元）"],
	repayData.map(item => [
		item.date,
		item.amount.toFixed(2)
	])
);

// 显示合计
dv.paragraph(`**合计：${totalRepay.toFixed(2)} 元**`);
```

#### 三、剩余欠款计算

```dataviewjs
// 从页面 frontmatter 获取数据
const borrowData = dv.current().borrowData;
const repayData = dv.current().repayData;

// 计算合计
const totalBorrow = borrowData.reduce((sum, item) => sum + item.amount, 0);
const totalRepay = repayData.reduce((sum, item) => sum + item.amount, 0);
const remainingDebt = totalBorrow - totalRepay;

// 显示计算结果
dv.paragraph(`- **总借款金额：** ${totalBorrow.toFixed(2)} 元`);
dv.paragraph(`- **总还款金额：** ${totalRepay.toFixed(2)} 元`);
dv.paragraph(`- **剩余欠款：** ${totalBorrow.toFixed(2)} - ${totalRepay.toFixed(2)} = **${remainingDebt.toFixed(2)} 元**`);
```

---

**计算公式：**
```
剩余欠款 = Σ(借款明细) - Σ(还款明细)
```
