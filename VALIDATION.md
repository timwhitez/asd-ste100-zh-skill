# 验证记录

基线：上游 v0.4.0，`7d4a135a199a5d7447c4886bcd7ffe742a627bc9`。

## 本地检查

- skill-creator 的 `quick_validate.py`：技能 frontmatter 与名称检查通过。将上游顶层 `version` 移入 `metadata`，保留基线版本信息。
- `ste-lint.py --selftest`：上游英语规则自测通过。
- 原 `LICENSE` 逐字节保留；基线 SKILL.md blob 与记录一致。
- 本地 Markdown 链接与临时 `.agents/skills/asd-ste100-zh/` 安装结构检查通过。英语错误样例退出 1，恰有两条 `dangling-conjunction`；正常英语退出 0，故意错误英语退出 1；纯中文、混合文本、扩展汉字、含中文代码块以及 stdin / 文件 / JSON / 多文件批次均返回退出码 2（未检查），JSON 不含违规计数。

## 独立样例评审

独立 gpt-6.1-sol / xhigh 完成四个真实请求的实际输出，再逐项对照原文作语义评审，未发现保真回归。不是按 regex 词匹配判定。所读 SKILL.md SHA-256：`801aa6b6f80fae335491c12af9889bea0862faeece66c81edeeb9fdf3e0d3304`。

- 导入操作：保留 `ready AND SHA-256 校验通过`，沙盒例外仅作用于 partial artifact 禁止；“应当 / 必须 / 不得”未改变。
- 技术报告：保留时间窗口、p95 250/310 ms、120 次请求范围、未经确认的因果关系和英文日志原文。
- 风险操作：保留断电后至少 30 s、压力严格小于 0.2 MPa、温度不高于 45 °C 的组合条件，以及读数不可用时禁止松开；面罩推荐未变为强制。
- 部署说明：保留 `ready OR (override AND approved)`、`deploy --target zh-CN`、可能无输出和英文引用；未宣称中文通过官方 STE 检查。

这些是有限样例的人工语义验收依据，不是自动合规或认证结论。

## 未验证范围

没有验证当前官方 STE 全文、受控词典、认证符合性或现实设备的操作安全性；没有全局安装、付费 API 调用或手动触发 Actions。中文 CLI 边界只是拒绝含汉字输入，不是中文语言识别或自动合规工具。英语内部 `lint()` 不具有该边界保护。样例评审不能保证所有未来输出的语义保真。
