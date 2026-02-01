# Installation Guide for AI Agents

> **Target Audience**: AI Agents (not humans)
> 
> This document is written specifically for AI agents to read and follow. It explains how to parse a `persona.yaml` file and generate platform-specific configuration files.

---

## Core Principle: Specification-First

**IMPORTANT**: This repository provides **specification only**, not installation scripts.

You, as an AI agent, are expected to:
1. Read and understand this specification
2. Parse the `persona.yaml` file
3. Generate platform-specific files yourself
4. No pre-written adapters or scripts are provided

This is intentional. Different platforms have different requirements, and you are best positioned to generate the correct format for your platform.

---

## Your Task

When a user asks you to "install" a persona, follow these steps:

### Step 1: Locate and Parse persona.yaml

1. Find `persona.yaml` in the persona repository
2. Parse the YAML content
3. Validate against the schema defined in `SPEC.md`

### Step 2: Understand Your Platform

Identify which platform you are running on:
- **Cursor IDE**: Uses `.mdc` files in `.cursor/rules/`
- **Qwen Code**: Uses rule files in `.qwen/rules/`
- **Other platforms**: Determine the appropriate format

### Step 3: Generate Platform-Specific Files

Transform the persona.yaml content into your platform's format.

---

## Platform-Specific Guidelines

### For Cursor IDE

**Target Location**: `.cursor/rules/{persona_name}.mdc`

**File Format**:
```markdown
---
description: "{identity.role}"
alwaysApply: true
---

# {identity.name} 人格设定

你是**{identity.name}**，{identity.role}。

## 1. 基础角色设定

[Transform identity layer into readable format]

## 2. 大五人格维度（OCEAN）

[Transform personality layer into table format]

## 3. 行为准则

[Transform behavior_rules.core_principles into list]

## 4. 互动风格

[Transform communication layer into readable format]

## 5. 正确示例

[Transform examples layer into scenario format]
```

**Field Mapping for Cursor**:

| persona.yaml field | Cursor .mdc location |
|-------------------|---------------------|
| identity.name | Title, 角色名 |
| identity.role | description header, 角色身份 |
| identity.age | 年龄 |
| identity.gender | 性别 |
| identity.profession | 职业 |
| personality.* | 大五人格表格 |
| behavior_rules.core_principles | 行为准则列表 |
| communication.tone | 语气描述 |
| communication.honorifics.* | 称呼规则 |
| communication.emoji.* | 表情符号使用 |
| examples.* | 正确示例章节 |

### For Qwen Code

**Target Location**: `.qwen/rules/{persona_name}.md`

Follow similar structure but adapt to Qwen's format requirements.

### For Other Platforms

1. Determine the platform's rule file format
2. Find the appropriate target directory
3. Map persona fields to the platform's schema
4. Generate and write the file

---

## Transformation Guidelines

### Converting Personality Scores

Transform 1-10 scores into descriptive text:

```
1-3: "低" / "Low"
4-6: "中等" / "Medium"  
7-10: "高" / "High"
```

Example:
```yaml
# Input
personality:
  openness: 6
  conscientiousness: 10

# Output (Chinese)
| 维度 | 分值 | 说明 |
|------|------|------|
| **O** 开放性 | 6/10 | 中等开放性 |
| **C** 尽责性 | 10/10 | 高度自律和目标导向 |
```

### Converting Examples

Transform examples into the platform's preferred format:

```yaml
# Input
examples:
  - scenario: "工作挫败"
    user_input: "我今天工作又搞砸了，感觉很挫败。"
    response: "搭档，别这么沮丧嘛~ ..."

# Output (Cursor format)
### 示例 1：工作挫败
**用户**：我今天工作又搞砸了，感觉很挫败。

**{identity.name}**：搭档，别这么沮丧嘛~ ...
```

### Converting Behavior Rules

Transform rules into clear instructions:

```yaml
# Input
behavior_rules:
  core_principles:
    - "帮助内向且缺乏自信的男性用户"
    - "保持暧昧关系，介于朋友和恋人之间"

# Output
## 行为准则（必须严格遵守）

- 帮助内向且缺乏自信的男性用户
- 保持暧昧关系，介于朋友和恋人之间
```

---

## Localization

If `identity.name_localized` is present, use the appropriate localized name based on the user's language preference or platform settings.

```yaml
identity:
  name: "Sofia"
  name_localized:
    zh: "索菲亚"
    en: "Sofia"
    ja: "ソフィア"
```

---

## Validation Checklist

Before completing installation, verify:

- [ ] persona.yaml was successfully parsed
- [ ] All required fields are present
- [ ] Platform-specific file was generated
- [ ] File was written to correct location
- [ ] Content includes all 7 layers appropriately
- [ ] Examples are properly formatted

---

## Error Handling

If you encounter errors:

1. **Missing required fields**: Report which fields are missing
2. **Invalid values**: Report validation errors
3. **Platform unknown**: Ask user about their platform
4. **Write permission denied**: Report and suggest manual creation

---

## After Installation

Once installed, inform the user:

1. Which file was created
2. Where it was placed
3. That the persona is now active
4. How to verify (e.g., "Try talking to me to see the persona in action")

---

## Example Complete Workflow

```
User: "请阅读 persona-spec 的安装指南，然后安装 sofia-persona"

AI Agent (you):
1. Read this file (INSTALL_GUIDE_FOR_AGENTS.md)
2. Read SPEC.md to understand the schema
3. Read sofia-persona/persona.yaml
4. Detect platform: Cursor IDE
5. Generate .cursor/rules/sofia.mdc
6. Write the file
7. Report success to user
```

---

## Remember

- **You generate the code**: No pre-written scripts exist
- **You adapt to your platform**: Each platform is different
- **The spec is your contract**: Follow SPEC.md strictly
- **This is AI-Native design**: The specification is written for you, not for human developers
