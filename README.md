# Cubase MusicXML → Ample Riffer GRIFF v0.1

这是一个纯本地、单文件浏览器工具，用于把 Cubase Chord Track 通过 MusicXML 写入 Ample Sound Riffer 4 的顶部 Chord Lane。

## 使用方法

1. 双击打开 `cubase_musicxml_to_griff.html`。
2. 选择 Cubase 导出的完整 `.xml` / `.musicxml`。
3. 如果要继续编辑已有 Riffer 工程，选择原来的 `.griff`；如果不选，则生成仅含和弦轨的新 `.griff`。
4. 输入要更新的起始/结束小节（含首尾）。
5. 选择和弦处理模式：
   - 原样保留：保留 m7 / maj7 / 7 等 MusicXML 和弦性质。
   - 基础三和弦：去掉 7 / Maj7 / 6 / add 等扩展，保留 major/minor/sus/dim/aug 的基础性质。
   - 强力和弦：全部写为 Riffer 原生 `5` 类型。
   - 八度音：全部写为 Riffer 原生 `oct` 类型。
6. 检查预览，点击“生成并下载 .griff”。

## 增量更新规则

- 只替换指定小节区间内的 Chord Lane。
- 原 `.griff` 中其它 riff 节点、音符、技巧、CC 等内容不会主动修改或删除。
- 旧和弦跨过更新区间边界时，会切开并保留边界外的部分。
- MusicXML 中从更新区间之前延续进来的和弦，会从选区起点继续写入。
- 浏览器始终生成新文件，不会覆盖你选择的原 `.griff`。

## 指法策略

- 如果原 `.griff` 里已经存在同样的 Root / Type / Extension / Bass 组合，优先复用该和弦的 6 弦指法。
- 找不到可复用指法时，v0.1 使用标准六弦吉他 EADGBE 的 fallback 指法。
- Power Chord 与 Octave 有专门的 fallback 指法。

## 当前限制

- v0.1 主要针对 Cubase 导出的 `score-partwise` MusicXML 和 Ample Guitar Riffer 4。
- 原样模式目前覆盖常用 major/minor/7/maj7/m7/6/sus2/sus4/aug/dim/power，以及部分 9/11 映射；遇到未映射的特殊 MusicXML chord kind 会明确报错，不会静默猜测。
- 备用指法生成按标准 6 弦调弦设计。特殊调弦、7/8 弦、Bass 等以后可以增加“按原 GRIFF 乐器/调弦生成”的支持。
- MusicXML 如果包含变拍号，时间位置会按 MusicXML 正确累计，但 Riffer 顶层只有一个 `Time-Signature` 元数据字段，因此应在 Riffer 中额外核对。

## 已验证

使用当前 `Lonely Race` Cubase MusicXML 与已知可用 `.griff` 测试：

- 全曲原样转换：100 个和弦事件。
- 局部小节三和弦转换：只修改选区。
- 强力和弦：写入 `ChordType="5"`。
- 八度音：写入 `ChordType="oct"`。
- 人工加入的非 Chord riff 节点在增量转换后仍完整保留。
