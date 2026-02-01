# persona-spec

A platform-agnostic persona specification framework for AI agents.

## Overview

`persona-spec` defines a standardized format for AI persona definitions, enabling consistent personality expression across different AI platforms (Cursor, Qwen Code, etc.).

## Design Philosophy

- **Specification-First**: Provides specification only, no platform-specific installation scripts
- **Agent-Generated**: AI agents read the spec and generate their own platform-specific implementations
- **Platform Decoupled**: Persona definitions are completely platform-agnostic
- **Single YAML File**: All persona content in one `persona.yaml` file

## Files

| File | Purpose | Audience |
|------|---------|----------|
| `SPEC.md` | Persona schema (7-layer structure) | AI Agents & Humans |
| `INSTALL_GUIDE_FOR_AGENTS.md` | Installation guide | AI Agents |
| `persona.yaml` | Persona template skeleton | AI Agents |
| `README.md` | Project overview | Humans |

## Usage

This is a **GitHub Template Repository**. To create your own persona:

1. Click "Use this template" on GitHub
2. Create your persona repository (can be private)
3. Edit `persona.yaml` with your persona content
4. In your target project, clone or add as submodule
5. Ask the AI agent to read `INSTALL_GUIDE_FOR_AGENTS.md` and install

## 7-Layer Persona Structure

1. **Identity** - Basic identity (name, age, profession, etc.)
2. **Personality** - Big Five (OCEAN) traits on 1-10 scale
3. **Affective** - Emotional response patterns (temperament, reactivity, regulation)
4. **Relationship** - Relationship with user (type, distance, dynamics)
5. **Communication** - Communication style (tone, honorifics, emoji usage)
6. **Behavior Rules** - Core rules and boundaries
7. **Examples** - Example dialogues (few-shot learning)

## License

MIT
