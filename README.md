# cili-data

cili 的**教材数据分发仓库**。只分发打包产物；打包脚本与原始数据（PDF/Markdown/提炼散文件）均不入库，仅本地使用。

## 数据内容

国家中小学智慧教育平台收录教材，经 `PDF → Markdown → 清洗 → AI 提炼` 处理后的章节化文本。

- 范围：T1 学科（数学、英语、物理、化学、生物学、语文、地理、信息技术、科学、历史、道德与法治、思想政治）
- 规模：约 1350 本 / 8800+ 章（以 `textbook/dist/manifest.json` 的计数为准）
- 每章带书名/教材信息/章节 frontmatter，供延迟加载与检索

## 仓库文件

| 文件 | 说明 |
|---|---|
| `textbook/dist/chapters.zip` | 精炼章节（成员 `<cid>/index.md`、`<cid>/chapters/NNN.md`） |
| `textbook/dist/catalog.json` | 书目元数据（学段/学科/版本/年级/册次/章数/字数） |
| `textbook/dist/manifest.json` | 数据集版本、计数与各包 SHA256 |

打包脚本、`textbook_md/`、`textbook_md_clear/` 原文与过程文件仅在本地保留，不入库。

> 说明：本仓库由 cili 启动时的 `TextbookPackSync` 后台线程使用：读取 `manifest.json` 比对版本 → 下载 zip → SHA256 校验 → 原子解压到 `data/cili/textbooks/`。

## 手动部署

```bash
# 解压到本地数据目录（cili 数据根）
python textbook/build_textbook_pack.py --install
# 构建全文检索库（SQLite FTS5 trigram，约 85 秒）
python textbook/build_textbook_pack.py --install --build-db
```

## 版权

教材原文版权归原出版方所有，本数据仅用于个人学习与研究。
