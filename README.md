# AI Video Acting Director Skill 🎬

[![Universal Agent Compatibility](https://img.shields.io/badge/Compatibility-Claude%20Code%20%7C%20OpenAI%20Codex%20%7C%20Hermes%20%7C%20Cursor%20%7C%20Antigravity-purple.svg)](#-universal-agent-setup--integration-guide)
[![AI Video Models](https://img.shields.io/badge/Supports-Runway%20%7C%20Kling%20%7C%20Sora%20%7C%20Hailuo%20%7C%20Luma%20%7C%20Seedance%202.5-orange.svg)](#-supported-video-ai-models)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An advanced agentic skill designed to direct, refine, and engineer **Oscar-grade, physically realistic cinematic acting performances** in Generative AI Video models (*Runway Gen-3 / Act-One, Kling AI, OpenAI Sora, MiniMax / Hailuo, Luma Dream Machine, and ByteDance Seedance 2.5 / Jimeng*).

Eliminates the "Uncanny Valley" and cartoonish overacting by replacing vague emotional adjectives with **Playable Action Verbs**, **FACS-based Micro-expressions**, **Gaze & Respiratory Dynamics**, and **Kinematic Prompt Grammar**.

---

## 🎯 The Core Problem: Why AI Video Acting Looks Uncanny

Most AI video prompts rely on **Result Directing**—commanding abstract emotional adjectives such as *"a heartbroken woman crying"* or *"a deeply angry detective"*. 

Generative video models struggle with abstract psychological labels, leading to:
* Exaggerated grimacing, rubbery facial morphing, and unnatural tears.
* Unblinking, static "frozen mask" stares.
* Overcrowded facial muscle movement without temporal breathing room (*"Micro-expression soup"*).

### The Solution: Kinematic & Playable Directing
In professional filmmaking, directors command **Playable Actions** (what the character is physically doing to someone else) and **Observable Kinematics** (subtle eyelines, jaw tension, breathing resets, and recovery).

```
┌──────────────────────────────────────────────────────────────────────────┐
│                      KINEMATIC DIRECTING PIPELINE                        │
│                                                                          │
│  [Abstract Emotion] ──► [Playable Action Verb] ──► [Moment Before]       │
│                                                          │               │
│  [Camera / Lighting] ◄── [Temporal Beats] ◄── [Physical Micro-Cues]      │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 📁 Repository Structure

```text
ai-video-acting-skill/
├── SKILL.md                                 # Core agent skill instructions (Antigravity & Agentic Skills)
├── SYSTEM_PROMPT.md                         # Universal System Prompt (Codex, ChatGPT, Hermes, LangChain)
├── CLAUDE.md                                # Dedicated configuration for Claude Code & Anthropic
├── .cursorrules                             # Directing rules for Cursor & Windsurf IDEs
├── README.md                                # English documentation & integration guide
├── references/
│   ├── cinematic_acting_frameworks.md       # Directing theory (Judith Weston, Stanislavski, Meisner, Beats 3s/5s/8s)
│   ├── micro_expressions_cinematic.md       # FACS Action Units, Gaze dynamics, Respiration, Laban analysis
│   └── cinematic_prompt_grammar.md          # Kinematic Prompt syntax, Anti-patterns, Benchmark prompts
└── examples/
    └── cinematic_prompt_recipes.md          # Ready-to-use prompt recipes across film genres
```

---

## 🌐 Universal Agent Setup & Integration Guide

This repository is built to work across **any AI assistant or agent framework**:

### 1. Claude Code (Anthropic)
Simply place this repository in your workspace or copy [`CLAUDE.md`](CLAUDE.md) to your project root. Claude Code will automatically reference the director instructions when crafting prompts.

### 2. OpenAI Codex / ChatGPT / Custom GPTs / Assistants API
Copy the full text inside [`SYSTEM_PROMPT.md`](SYSTEM_PROMPT.md) and paste it into:
* **Custom GPTs**: *Instructions* box.
* **ChatGPT**: *Custom Instructions* (`How would you like ChatGPT to respond?`).
* **OpenAI Assistants API / Codex**: `instructions` parameter.

### 3. Nous Hermes & Open-Source LLMs (Ollama / vLLM / LM Studio / Open-WebUI)
Load [`SYSTEM_PROMPT.md`](SYSTEM_PROMPT.md) as the System Message in your Ollama Modelfile or LM Studio system configuration:
```dockerfile
# Modelfile example for Ollama (Hermes 3 / Llama 3)
FROM hermes3:latest
SYSTEM """
<Paste contents of SYSTEM_PROMPT.md here>
"""
```

### 4. Cursor & Windsurf IDEs
The [`.cursorrules`](.cursorrules) file in the root directory is automatically recognized by Cursor and Windsurf, enabling immediate acting direction in chat and agent sessions.

### 5. Google Antigravity & Agent Skills Ecosystem
Antigravity automatically discovers and loads [`SKILL.md`](SKILL.md) as an active tool and skill in your agent workspace.

### 6. Python Agents (LangChain, CrewAI, AutoGen)
```python
from pathlib import Path

# Load Universal System Prompt
system_prompt = Path("ai-video-acting-skill/SYSTEM_PROMPT.md").read_text(encoding="utf-8")

# Pass to your agent (CrewAI / LangChain / AutoGen)
# e.g., Agent(role="Video Acting Director", backstory=system_prompt)
```

---

## 🧠 Theoretical Foundations

This skill synthesizes knowledge from 4 key disciplines:

1. **Screen Acting & Directing (Hollywood / Meisner / Weston)**:
   * **Playable Action Verbs**: Directing intentions like *To Interrogate*, *To Conceal*, *To Shield*, *To Disarm*.
   * **The Moment Before**: Incorporating residual physical tension and pre-shot history.
   * **Subtext & Oppositions**: Contradiction between internal pain and external suppression.

2. **Micro-Expressions & Facial Anatomy (Paul Ekman's FACS)**:
   * Translating emotion into localized **Action Units (AUs)** (e.g., lower eyelid tension, lip press, chin micro-tremor).
   * Restricting active facial zones to 1–2 features per beat to prevent model hallucination.

3. **Gaze & Respiratory Dynamics**:
   * Saccadic shifts vs. fixations, turn-taking eyelines, and defocused stares.
   * Inhale catches, held breath, and slow nasal exhales as internal shot pacing engines.

4. **Laban Movement Analysis (LMA)**:
   * Micro-movement effort qualities (*Weight, Space, Time, Flow*) applied to subtle cinematic gestures.

---

## ⚡ The Kinematic Prompting Formula

### Micro-Acting Hierarchy Equation
$$\text{SPATIAL ANCHOR} \longrightarrow \text{PRIMARY ACTION} \longrightarrow \text{EYELINE} \longrightarrow \text{1–2 FACE CUES} \longrightarrow \text{BREATH} \longrightarrow \text{RECOVERY/STILLNESS}$$

### Master Text-to-Video (T2V) Template
```text
[Character archetype], already [visible physical precondition] at/in [spatial anchor].

[Primary physical action in one or two beats].
Their eyes remain [precise eyeline target], then [one deliberate eyeline change].

On [small inhale / held breath / slow exhale], [1–2 localized facial movements: e.g., lower eyelids tighten slightly, lips press together].
They [swallow / blink once / release the jaw / settle the shoulders], then become still again.

[Shot size, e.g., Tight 85mm close-up], [depth of field], [locked camera OR one subtle camera move].
[Specific lighting source and interaction on face/eyes].

Restrained natural performance, low-amplitude real-time movement.
```

### Master Image-to-Video (I2V) Template
```text
The subject [single primary physical action].
Eyes remain [initial eyeline], then [one subtle gaze shift].
On [breath cue], [1–2 localized facial movements], followed by [jaw release / settling into stillness].
[One camera instruction, e.g., locked camera or slow push-in].
[Lighting interaction with micro-movement].
```

### Temporal Beat Structure (3s – 8s Shots)
$$\text{ESTABLISH / HOLD (0–1.5s)} \longrightarrow \text{MICRO-LEAK (1.5–3.5s)} \longrightarrow \text{RECOVER / SETTLE (3.5–5s)}$$

---

## 🛠 Supported Video AI Models

| Model | Syntax Strategy | Key Directing Rules |
| :--- | :--- | :--- |
| **Kling AI (1.5 / 2.0)** | `Subject + Movement + Scene + Camera + Lighting` | Direct physical actions achievable in 5 seconds; 1–2 facial cues max. |
| **Runway (Gen-3 / Act-One)** | Positive kinematic phrasing | **NEVER** use negative commands (`no`, `don't`). Use positive anchors (`locked camera`, `mouth remains neutral`). |
| **OpenAI Sora** | Storyboard beats + Count | `1 Subject Action + 1 Camera Move` in sequential count beats. |
| **MiniMax / Hailuo** | Explicit bracket tags | Use `[Static shot]` / `[Push in]` with literal kinematic phrasing. |
| **Luma Dream Machine** | Separate subject kinematics from camera language | Natural subject movement descriptions; explicit camera movement commands. |
| **Seedance 2.5 (ByteDance / Jimeng)** | `Subject Anchor + Kinematic Beats + Camera Trajectory + Lighting Details` | Excels at high aesthetic facial continuity and prompt adhesion. Use explicit spatial anchors, subtle gaze tracking cues, and clear camera trajectory (`slow push-in 85mm`, `locked close-up`). |

---

## 🎭 Before & After Comparison

| ❌ Uncanny / Vague Prompt | ✅ Oscar-Grade Kinematic Rewrite |
| :--- | :--- |
| `A heartbroken woman crying sadly in the rain, looking devastated.` | `She holds the letter steady with both hands. Her eyes remain fixed off-camera right. On a shallow inhale, her lower eyelids tighten slightly while her lips press together. She swallows once, lets her gaze fall to the floor, and releases her jaw on the slow exhale. Locked 85mm close-up, soft window light catching lower eyelid moisture.` |
| `A detective stares intensely with a deeply suspicious face.` | `He sits motionless behind the scarred desk. His eyes remain locked on the unseen witness off-camera left without turning his head. On a slow nasal exhale, his jaw shifts once before settling back into stillness. Tight telephoto close-up, slanted Venetian-blind lighting.` |

---

## 📚 Deep Documentation

* 📖 [Cinematic Acting Directing Frameworks](references/cinematic_acting_frameworks.md)
* 🧬 [Micro-Expressions & FACS Guide](references/micro_expressions_cinematic.md)
* 📐 [Kinematic Prompt Grammar & Benchmarks](references/cinematic_prompt_grammar.md)
* 🎬 [Curated Cinematic Prompt Recipes](examples/cinematic_prompt_recipes.md)

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
