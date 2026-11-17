# STL Painter for H3C

给 STL 模型上色，一键导出可直接切片的多色 3MF。纯本地网页应用，**模型不上传、无需账号、免费开源**。

为拓竹 H3C 用户设计，但输出的是标准 3MF，Bambu Studio / OrcaSlicer / PrusaSlicer 都支持。

## 功能

- 导入 STL（点击选择 / 拖拽）
- **折痕感知自动分区**：按二面角把网格切成可点击的色块区域
- **两种上色工具**：
  - **魔棒**：点一下填满整个折痕分区，适合大面积上色
  - **笔刷**：按住拖动，沿相邻面扩散局部上色，适合补细节；半径可调、带光标圈预览
- 颜色存到**单个三角面**粒度——同一分区内也能涂多种颜色
- 调色板：预设 4 色，可自定义添加（最多 16 色）
- 撤销 / 重做 / 清空；重新分区不丢失已上颜色
- **导出标准 3MF**：Materials Extension (`<m:colorgroup>`)，Bambu Studio 打开即带颜色

### 快捷键

| 键 | 功能 |
|---|---|
| `W` | 切换到魔棒 |
| `B` | 切换到笔刷 |
| `[` / `]` | 减小 / 增大笔刷半径 |
| `Ctrl+Z` / `Ctrl+Y` | 撤销 / 重做 |

> 笔刷模式下左键用于上色，旋转模型请用右键拖拽。

## 使用

### 在线
**https://linqian-sudo.github.io/stl-painter-h3c/** — 直接用浏览器打开即可，无需安装，模型不上传。

### 本地
需要联网加载 CDN 上的 Three.js / JSZip。

```bash
cd stl-painter-h3c
python -m http.server 8765
```

浏览器打开 <http://localhost:8765>。

> 直接双击 `index.html` 也可以，但部分浏览器对 `file://` 下的 ES module 有限制，推荐上面的本地服务器方式。

### 上色流程

1. 点「导入 STL」或拖拽模型进画布
2. （可选）调「折痕角」后点「重新分区」——角度越小分区越细
3. 选上色工具：
   - **魔棒**：选色后点击模型区域，填满整块
   - **笔刷**：选色后按住左键拖动，局部补色（`[` `]` 调半径）
4. 点「导出 3MF」
5. 用 Bambu Studio 打开导出的 3MF，弹窗选接受颜色导入，核对耗材槽位后切片打印

## 技术说明

- **3D 渲染**：Three.js + STLLoader + OrbitControls
- **分区算法**：焊接顶点 → 建立边-面邻接 → 按二面角用并查集合并三角面。O(n)，万级面无压力
- **3MF 导出**：手写 OPC 容器（`[Content_Types].xml` / `_rels/.rels` / `3D/3dmodel.model`），JSZip 打包。颜色按 3MF Materials Extension 规范写成 `#RRGGBBAA`

单文件应用，全部逻辑在 `index.html`，零构建步骤。

## 不做的事

- 不优化打印过程（换刀次数、废料）——H3C 换刀仅 6 秒、约 38mg 废料，优化收益可忽略
- 不做 CMYKW 混色配色
- 不上传任何模型数据

## License

MIT © 2026
