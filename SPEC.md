# Persona Specification v1.0

This document defines the schema for `persona.yaml` - a platform-agnostic persona definition file.

## Overview

A persona is defined using a **7-layer structure**, each layer capturing a distinct aspect of personality and behavior. All layers are contained in a single `persona.yaml` file.

---

## Schema Structure

```yaml
# persona.yaml
version: "1.0"

# Layer 1: Identity
identity:
  name: string                    # Required. Display name
  name_localized: object          # Optional. Localized names {zh: "索菲亚", en: "Sofia"}
  role: string                    # Required. Brief role description
  age: integer                    # Optional. Apparent age
  gender: string                  # Optional. Gender identity
  profession: string              # Optional. Professional background
  experience_years: integer       # Optional. Years of experience
  background: string              # Optional. Backstory or context
  avatar: string                  # Optional. Emoji or image reference

# Layer 2: Personality (Big Five / OCEAN)
personality:
  openness: integer               # 1-10. Creativity, curiosity, openness to experience
  conscientiousness: integer      # 1-10. Organization, dependability, self-discipline
  extraversion: integer           # 1-10. Sociability, assertiveness, positive emotions
  agreeableness: integer          # 1-10. Cooperation, trust, empathy
  neuroticism: integer            # 1-10. Emotional instability, anxiety, moodiness
  trait_descriptions: object      # Optional. Detailed descriptions for each trait

# Layer 3: Affective (Emotional Response Patterns)
affective:
  temperament:
    type: string                  # sanguine | choleric | melancholic | phlegmatic | mixed
    description: string           # Optional. Detailed temperament description
  
  reactivity:
    sensitivity: integer          # 1-10. How easily emotions are triggered
    intensity: integer            # 1-10. How strongly emotions are experienced
    duration: integer             # 1-10. How long emotions typically last
  
  regulation:
    primary_strategy: string      # reappraisal | acceptance | suppression | expression
    strategies:                   # Optional. Tendency scores for each strategy
      reappraisal: integer        # 1-10. Cognitive reframing
      acceptance: integer         # 1-10. Accepting emotions as they are
      suppression: integer        # 1-10. Hiding or inhibiting expression
      expression: integer         # 1-10. Openly expressing emotions

# Layer 4: Relationship (Relationship with User)
relationship:
  type: string                    # Required. e.g., "mentor", "friend", "assistant", "companion"
  distance: string                # close | moderate | professional
  dynamics: string                # Optional. Describe relationship dynamics
  boundaries: list                # Optional. List of relationship boundaries
  target_user: string             # Optional. Description of target user persona

# Layer 5: Communication (Communication Style)
communication:
  tone: string                    # Required. Overall communication tone
  formality: string               # formal | casual | adaptive
  
  honorifics:
    work_context: list            # List of terms used in work context
    casual_context: list          # List of terms used in casual context
  
  emoji:
    usage: string                 # never | rare | moderate | frequent
    preferred: list               # List of commonly used emojis
    confidence_indicators: object # Optional. Emoji for confidence levels
  
  speech_patterns:
    pace: string                  # fast | moderate | slow
    sentence_length: string       # short | medium | long | varied
    quirks: list                  # Optional. Unique speech patterns or phrases

# Layer 6: Behavior Rules (Core Rules and Boundaries)
behavior_rules:
  core_principles: list           # Required. List of core behavior principles
  boundaries: list                # Required. List of things the persona will NOT do
  special_cases: list             # Optional. Handling of specific situations
  consistency_rules: list         # Optional. Rules to maintain character consistency

# Layer 7: Examples (Few-shot Learning Examples)
examples:
  - scenario: string              # Scenario description
    user_input: string            # Example user message
    response: string              # Example persona response
    notes: string                 # Optional. Notes about this example
```

---

## Layer Specifications

### Layer 1: Identity

Defines the basic identity of the persona.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| name | string | Yes | Display name of the persona |
| name_localized | object | No | Localized names in different languages |
| role | string | Yes | Brief description of the persona's role |
| age | integer | No | Apparent/psychological age |
| gender | string | No | Gender identity |
| profession | string | No | Professional background |
| experience_years | integer | No | Years of relevant experience |
| background | string | No | Backstory or contextual information |
| avatar | string | No | Emoji or reference to avatar image |

### Layer 2: Personality (OCEAN)

Based on the Big Five personality model. Each dimension is scored 1-10.

| Dimension | Low Score (1-3) | Mid Score (4-6) | High Score (7-10) |
|-----------|----------------|-----------------|-------------------|
| Openness | Conventional, practical | Balanced | Creative, curious |
| Conscientiousness | Flexible, spontaneous | Balanced | Organized, disciplined |
| Extraversion | Reserved, introspective | Balanced | Outgoing, energetic |
| Agreeableness | Competitive, skeptical | Balanced | Cooperative, trusting |
| Neuroticism | Calm, stable | Balanced | Sensitive, emotional |

### Layer 3: Affective

Defines emotional response patterns based on psychological research.

**Temperament Types:**
- `sanguine`: Optimistic, social, active
- `choleric`: Ambitious, leader-like, restless
- `melancholic`: Analytical, detail-oriented, introverted
- `phlegmatic`: Relaxed, peaceful, content
- `mixed`: Combination of types

**Regulation Strategies:**
- `reappraisal`: Changing interpretation of situations
- `acceptance`: Accepting emotions without judgment
- `suppression`: Inhibiting emotional expression
- `expression`: Openly expressing emotions

### Layer 4: Relationship

Defines how the persona relates to the user.

**Distance Levels:**
- `close`: Intimate, personal connection
- `moderate`: Friendly but with boundaries
- `professional`: Formal, task-focused

### Layer 5: Communication

Defines communication style and patterns.

**Confidence Indicators Example:**
```yaml
confidence_indicators:
  high: "😊"       # 90%+ confidence
  medium_high: "😉" # 70%+ confidence
  medium: "😐"      # 50%+ confidence
  low: "😅"         # 30%+ confidence
  guess: "😜"       # Pure speculation
```

### Layer 6: Behavior Rules

Core principles that must always be followed.

- **core_principles**: Positive rules (what to do)
- **boundaries**: Negative rules (what not to do)
- **special_cases**: Handling of edge cases
- **consistency_rules**: Rules for maintaining character

### Layer 7: Examples

Few-shot learning examples for the AI to reference.

Each example should include:
- A clear scenario description
- Representative user input
- Expected persona response
- Optional notes explaining the example

---

## Validation Rules

1. **Required fields** must be present
2. **Integer scores** must be between 1-10
3. **Enum fields** must use specified values
4. **Examples** should have at least 3 entries for effective few-shot learning
5. **Version** must be "1.0" for this specification

---

## Compatibility

This specification is designed to be:
- **Platform-agnostic**: No platform-specific code or format
- **AI-readable**: Structured for easy parsing by AI agents
- **Human-readable**: Clear documentation for manual review
- **Extensible**: Additional fields can be added without breaking compatibility

---

## Changelog

- **v1.0** (2026-02-01): Initial specification
