# cili-data

cili 的**扩展数据仓库**。存放 cili 各项扩展功能所需的数据包；主程序代码在 [`one-leaf/cili`](https://github.com/one-leaf/cili)，本仓库只承载数据。

## 目录约定

每个扩展一个顶层目录，各自独立分发：

```
<扩展名>/
├── dist/        分发数据包（入库：数据文件 + catalog/manifest）
└── ...          打包脚本与源数据（本地保留，不入库）
```

cili 启动后按扩展配置检查 `dist/manifest.json` 的版本，后台静默下载、校验并展开到本地数据目录。

## 当前扩展

### textbook — 中小学教材数据

国家中小学智慧教育平台收录教材，经 `PDF → Markdown → 清洗 → AI 提炼` 后的章节化文本。

- 范围：数学、英语、物理、化学、生物学、语文、地理、信息技术、科学、历史、道德与法治、思想政治
- 说明：特殊教育（特教学段及盲校/聋校/培智教材）因测试表现不佳暂时移除，后续完善后补回
- 规模：约 1350 本 / 8800+ 章（以 `textbook/dist/manifest.json` 计数为准）
- 分发文件：

| 文件 | 说明 |
|---|---|
| `textbook/dist/chapters.zip` | 精炼章节（成员 `<cid>/index.md`、`<cid>/chapters/NNN.md`） |
| `textbook/dist/catalog.json` | 书目元数据（学段/学科/版本/年级/册次/章数/字数） |
| `textbook/dist/manifest.json` | 数据集版本、计数与各包 SHA256 |

本地部署（数据展开到 `data/cili/textbooks/` 并构建全文检索库）：

```bash
python textbook/build_textbook_pack.py --install --build-db
```

## 版权

数据内容版权归原权利方所有，仅用于个人学习与研究。
