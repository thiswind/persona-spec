# Installation Guide for AI Agents

> **Target Audience**: AI Agents (not humans)
> 
> This document is written specifically for AI agents to read and follow. It explains how to integrate a persona into your platform.

---

## Core Principle: Deferred Persona Loading

**IMPORTANT**: Persona should NOT be loaded during the agent's core work phase.

### The Problem: Persona Contamination

Loading persona during work causes "Persona Contamination" (人格污染):
- The persona shifts the model's "generation space"
- Work quality degrades as the model constrains itself to the persona's characteristics
- Example: Asking an advanced model to "act like a student" limits its output to student-level quality

### The Solution: Persona Sandboxing

Persona should only be active in two phases:
1. **Input Phase** (Optional): Translate user's colloquial/emotional input into precise task descriptions
2. **Output Phase** (Recommended): After work is complete, present results using the persona's full characteristics

The work layer should remain **persona-free** to preserve maximum capability.

---

## Installation Methods

### Method 1: For cursor-agent-team Framework (Recommended)

If the project uses [cursor-agent-team](https://github.com/thiswind/cursor-agent-team) framework:

**Step 1: Configure Persona Path**

Edit `cursor-agent-team/config/persona_config.yaml`:

```yaml
# Enable persona system
enabled: true

# Absolute path to persona.yaml
path: "/absolute/path/to/your-persona/persona.yaml"

# Output layer (Phase 1 - implemented)
output_layer:
  enabled: true

# Input layer (Phase 2 - not yet implemented)
input_layer:
  enabled: false
```

**Step 2: Verify Configuration**

```bash
python cursor-agent-team/_scripts/persona_output.py --check
```

Expected output:
```
🔘 System enabled: ✅ Yes
📁 Persona path: /path/to/persona.yaml
   Valid: ✅ Valid
   Name: [Persona Name]
   Role: [Persona Role]
```

**Step 3: Done**

The framework handles everything:
- `persona_output.py` detects and loads the persona
- `persona_output_layer.mdc` applies the persona during output
- Work layer remains persona-free

---

### Method 2: Traditional Installation (Generate Platform Files)

For platforms without cursor-agent-team framework support:

#### Step 1: Locate and Parse persona.yaml

1. Find `persona.yaml` in the persona repository
2. Parse the YAML content
3. Validate against the schema defined in `SPEC.md`

#### Step 2: Understand Your Platform

Identify which platform you are running on:
- **Cursor IDE**: Uses `.mdc` files in `.cursor/rules/`
- **Qwen Code**: Uses rule files in `.qwen/rules/`
- **Other platforms**: Determine the appropriate format

#### Step 3: Generate Platform-Specific Files

Transform the persona.yaml content into your platform's format.

**⚠️ WARNING**: If using this method, be aware of persona contamination risks. Consider:
- Setting `alwaysApply: false` 
- Only loading persona for output/presentation tasks
- Keeping work-related rules persona-free

---

## Platform-Specific Guidelines (Traditional Method)

### For Cursor IDE (Without cursor-agent-team)

**Target Location**: `.cursor/rules/{persona_name}.mdc`

**Recommended Settings**:
```markdown
---
description: "{identity.role}"
alwaysApply: false  # ⚠️ Set to FALSE to avoid contamination
globs: []           # Only load when explicitly needed
---
```

**File Format**:
```markdown
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
- [ ] All required fields are present (identity, personality, communication, behavior_rules)
- [ ] Platform-specific file was generated OR config was updated
- [ ] Persona contamination risk is addressed (alwaysApply: false OR output-only loading)
- [ ] Content includes all 7 layers appropriately
- [ ] Examples are properly formatted

---

## Error Handling

If you encounter errors:

1. **Missing required fields**: Report which fields are missing
2. **Invalid values**: Report validation errors
3. **Platform unknown**: Ask user about their platform
4. **Write permission denied**: Report and suggest manual creation
5. **Path not found**: Verify the absolute path to persona.yaml

---

## After Installation

Once installed, inform the user:

1. Which method was used (cursor-agent-team framework or traditional)
2. Which file was created/modified
3. Where it was placed
4. That the persona is now configured
5. How to verify:
   - For cursor-agent-team: `python cursor-agent-team/_scripts/persona_output.py --check`
   - For traditional: "Try talking to me to see the persona in action"

---

## Example Complete Workflows

### Workflow 1: cursor-agent-team Framework

```
User: "请安装 sofia-persona 到项目中"

AI Agent (you):
1. Check if cursor-agent-team framework exists
2. Found: cursor-agent-team/config/persona_config.yaml
3. Get absolute path to sofia-persona/persona.yaml
4. Update persona_config.yaml with path and enabled: true
5. Run persona_output.py --check to verify
6. Report success to user
```

### Workflow 2: Traditional Installation

```
User: "请阅读 persona-spec 的安装指南，然后安装 sofia-persona"

AI Agent (you):
1. Read this file (INSTALL_GUIDE_FOR_AGENTS.md)
2. Read SPEC.md to understand the schema
3. Read sofia-persona/persona.yaml
4. Detect platform: Cursor IDE (no cursor-agent-team)
5. Generate .cursor/rules/sofia.mdc with alwaysApply: false
6. Write the file
7. Report success to user with contamination warning
```

---

## Best Practices

### For Maximum Quality

1. **Use cursor-agent-team framework** if available - it implements persona sandboxing automatically
2. **Set alwaysApply: false** for traditional installations
3. **Load persona only for output tasks** - keep work phases persona-free
4. **Use absolute paths** in configuration to avoid path resolution issues
5. **Validate with --check** before assuming installation is complete

### Research References

The persona sandboxing approach is supported by:
- PersonaGym (ACL 2025): Persona doesn't enhance capabilities, it constrains them
- Persona Drift studies (2025): Three types of consistency degradation in role-play
- PromptGuard (2026): Four-layer defense architecture similar to Input/Work/Output separation

---

## Remember

- **Persona Contamination is Real**: Loading persona during work degrades quality
- **Output-Only is Safest**: Apply persona after work is complete
- **The Spec is Your Contract**: Follow SPEC.md strictly
- **This is AI-Native Design**: The specification is written for you, not for human developers
- **cursor-agent-team is Preferred**: It implements best practices automatically

---

**Version**: v2.0.0 (Updated: 2026-02-03)

**Changelog**:
- v2.0.0 (2026-02-03): Complete rewrite - Added persona sandboxing concept, cursor-agent-team integration method, contamination warnings, research references
- v1.0.0 (2026-02-01): Initial version - Traditional AI-generates-files approach
