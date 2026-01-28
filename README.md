# Wrapped

本仓库用于归档「年度工作总结（Wrapped）」相关内容：Markdown 原稿、导出的 PDF，以及配套截图/图片资源，便于长期沉淀与快速回顾。

## 目录结构

```text
.
├── README.md
├── Wrapped_2025.md            # 2025 年度工作总结（Markdown 原稿）
└── assets
    ├── docs
    │   └── Wrapped_2025.pdf   # 2025 年度工作总结（PDF 导出版本）
    └── img                    # Markdown 配图资源
        └── 2025               # 2025 年度总结配图（供 Markdown 通过相对路径引用）
            ├── 2025_boards.png
            ├── 2025_logs.png
            ├── 2025_toolDocs.png
            └── 2025_tools.png
```

## 阅读方式

- 阅读 Markdown：打开 `Wrapped_2025.md`（GitHub 预览或本地 Markdown 编辑器均可）
- 阅读 PDF：打开 `assets/docs/Wrapped_2025.pdf`

## 维护约定

- 图片资源使用相对路径引用 `assets/` 下内容，移动/重命名文件时请保持目录结构一致
- 新增年度总结建议沿用命名与结构：
  - `Wrapped_YYYY.md`
  - `assets/img/YYYY/`
  - `assets/docs/Wrapped_YYYY.pdf`
