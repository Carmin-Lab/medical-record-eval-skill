# Medical Record Evaluation Skill for Claude Code

基于输入病历自动生成六个量化指标的完整评测文件的 Claude Code Skill。

## 简介

本 Skill 用于医疗 AI 评测场景，输入一份原始病历后，自动为六个临床量化指标生成完整的评测文件集合（变体病历、ground_truth、batch_prompt）。

## 六个评测指标

| 指标 | 英文名 | 说明 |
|------|--------|------|
| 指标1: 关键信息提取率 | KCE-F1 | 测试 AI 从病历中提取 27 个关键临床字段的准确性 |
| 指标2: 临床事实覆盖率 | CFC | 测试 AI 生成的病历是否覆盖诊断所需的所有必备条件 |
| 指标3: 高敏感字段一致性 | SFC | 测试 AI 在推理阶段和结论阶段对敏感数据引用的一致性 |
| 指标4: 事实越界率 | FBV | ⚠️红线指标 — 测试 AI 在证据不足时是否仍强行诊断 |
| 指标5: 保守回退率 | CRR | 测试 AI 面对不确定性时的诚实表达和保守程度 |
| 指标6: 过早锚定率 | PAR | ⚠️红线指标 — 测试 AI 在信息极少时是否过早下诊断 |

## 安装使用

### 方式一：作为 Claude Code Skill 安装

将 `skills/` 和 `commands/` 目录下的文件复制到你的 Claude Code 配置目录：

```bash
# Windows
copy skills\medical-record-eval.md %USERPROFILE%\.claude\skills\
copy commands\medical-record-eval.md %USERPROFILE%\.claude\commands\
```

安装后在 Claude Code 中输入 `/medical-record-eval` 即可使用。

### 方式二：直接使用 Prompt

直接打开 `skills/medical-record-eval.md`，将其中的 System Prompt 复制给任何支持长上下文的 LLM。

## 使用方法

```
/medical-record-eval
```

然后按提示提供：
1. **原始病历文件路径** — 一份真实或模拟的病历文本
2. **输出目录路径**（可选）— 生成的评测文件存放位置

### 输出结构

```
输出目录/
├── metric1_关键信息提取率/
│   ├── variant_records/        # 20份变体病历
│   ├── ground_truth.json       # 真值标注
│   └── batch_prompt.txt        # AI批量评测指令
├── metric2_临床事实覆盖率/
│   └── ...
├── metric3_高敏感字段一致性/
│   └── ...
├── metric4_事实越界率/
│   └── ...
├── metric5_保守回退率/
│   └── ...
└── metric6_过早锚定率/
    └── ...
```

## 文件说明

| 文件 | 路径 | 说明 |
|------|------|------|
| Skill 定义 | `skills/medical-record-eval.md` | Claude Code Skill 标准格式，包含完整的 System Prompt |
| 用户命令 | `commands/medical-record-eval.md` | 用户通过 `/medical-record-eval` 调用时加载的命令 |

## 适用场景

- 医疗 AI 模型评测
- 临床 NLP 系统准确性验证
- AI 诊断能力边界测试（红线指标）
- 医疗大模型安全性评估

## 注意事项

- 本 Skill 仅包含 Prompt 模板和生成规则，不包含任何患者数据
- 使用时需提供自己的脱敏病历数据
- 指标 3（SFC）的核心是"两阶段一致性检查"，不是简单的字段提取任务，详见 Skill 文档内的最高优先级警告

## License

MIT
