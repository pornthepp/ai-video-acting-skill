# AI Video Acting & Action Director Skill 🎬💥

[![Universal Agent Compatibility](https://img.shields.io/badge/Compatibility-Claude%20Code%20%7C%20OpenAI%20Codex%20%7C%20Hermes%20%7C%20Cursor%20%7C%20Antigravity-purple.svg)](#-universal-agent-setup--integration-guide)
[![AI Video Models](https://img.shields.io/badge/Supports-Runway%20%7C%20Kling%20%7C%20Sora%20%7C%20Hailuo%20%7C%20Luma%20%7C%20Seedance%202.5-orange.svg)](#-supported-video-ai-models)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An advanced agentic skill designed to direct, refine, and engineer **Oscar-grade cinematic acting performances** and **visceral, grounded action fight sequences** in Generative AI Video models (*Runway Gen-4.5 / Gen-3, Kling AI 1.5/2.0/3.0, OpenAI Sora, MiniMax / Hailuo, Luma Dream Machine, and ByteDance Seedance 2.5 / Jimeng*).

Eliminates the "Uncanny Valley", rubbery facial overacting, and multi-body combat morphing through **Playable Action Verbs**, **FACS-based Micro-expressions**, **Action Shot Decomposition (1 Action per Shot)**, and **Environmental FX Masking**.

---

## 🎯 The Core Problems in AI Video & How We Solve Them

```
┌──────────────────────────────────────────────────────────────────────────┐
│                   AI VIDEO CINEMATIC DIRECTING ENGINE                    │
├──────────────────────────────────────────┬───────────────────────────────┤
│ MODE A: ACTING & DRAMATIC PERFORMANCE    │ MODE B: ACTION & COMBAT STUNT │
│  - Replaces abstract emotion adjectives  │  - Solves multi-body collision│
│  - Playable Action Verbs (Weston)        │  - Action Shot Decomposition  │
│  - FACS Micro-Cues (1-2 Features Max)    │  - 1 Dominant Action per Cut  │
│  - Temporal Beat: Hold ➔ Leak ➔ Settle   │  - Environmental FX Masking   │
└──────────────────────────────────────────┴───────────────────────────────┘
```

### 1. Problem 1: Uncanny Facial Overacting (Drama)
* **Root Cause**: Generative models fail when prompted with abstract psychological labels (*"heartbroken"*, *"deeply sad"*, *"intensely angry"*).
* **Solution**: Direct **Playable Action Verbs** (*To corner*, *To deflect*, *To conceal*, *To shield*) and **Kinematic Micro-acting** (eyeline shifts, breath catches, and jaw releases).

### 2. Problem 2: Multi-Body Collision & Limb Morphing (Action)
* **Root Cause**: AI Video models cannot render multi-limb grappling, wrist locks, or multi-beat combos in a single prompt (resulting in 3 arms, morphing bodies, and rubbery floaty physics).
* **Solution**: The **3-Shot Action Sequence Protocol** (1 Dominant Action per Cut) + **Environmental FX Masking** (rain spray, debris explosion, sparks) to sell kinetic impact while masking contact points.

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
│   ├── cinematic_prompt_grammar.md          # Kinematic Prompt syntax, Anti-patterns, Benchmark prompts
│   ├── action_shot_decomposition.md         # Hollywood/HK fight decomposition, 1-to-2-second cuts
│   ├── combat_kinematics_fx.md              # Kinetic chain, Weight transfer, Environmental FX masking
│   └── combat_prompt_engineering.md         # State-transition prompting, collision workarounds, I2V staging
└── examples/
    ├── cinematic_prompt_recipes.md          # Ready-to-use drama prompt recipes across genres
    └── action_combat_recipes.md             # Production-ready 3-shot action prompt sequences
```

---

## 🌐 Universal Agent Setup & Integration Guide

This repository is built to work across **any AI assistant or agent framework**:

### 1. Claude Code (Anthropic)
Place this repository in your workspace or copy [`CLAUDE.md`](CLAUDE.md) to your project root. Claude Code will automatically reference both Drama and Action directives.

### 2. OpenAI Codex / ChatGPT / Custom GPTs / Assistants API
Copy the full text inside [`SYSTEM_PROMPT.md`](SYSTEM_PROMPT.md) and paste it into Custom Instructions or System Prompt.

### 3. Nous Hermes & Open-Source LLMs (Ollama / vLLM / LM Studio / Open-WebUI)
Load [`SYSTEM_PROMPT.md`](SYSTEM_PROMPT.md) as the System Message in your Ollama Modelfile or LM Studio system configuration.

### 4. Cursor & Windsurf IDEs
The [`.cursorrules`](.cursorrules) file in the root directory is automatically recognized by Cursor and Windsurf for instant prompt engineering.

### 5. Google Antigravity & Agent Skills Ecosystem
Antigravity automatically discovers and loads [`SKILL.md`](SKILL.md) as an active tool and skill in your workspace.

### 6. Python Agents (LangChain, CrewAI, AutoGen)
```python
from pathlib import Path

# Load Universal System Prompt
system_prompt = Path("ai-video-acting-skill/SYSTEM_PROMPT.md").read_text(encoding="utf-8")

# Pass to your agent (CrewAI / LangChain / AutoGen)
# e.g., Agent(role="Cinematic Action & Acting Director", backstory=system_prompt)
```

---

## ⚡ Master Directing Templates

### Mode A: Drama Master Template (T2V)
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

### Mode B: Combat Master 3-Shot Protocol (T2V / I2V)
```text
[SHOT 1 - TENSION/STANCE - 1.5s]
[Framing: Tight medium 35mm], [Environment]. [Fighter A], weight planted firmly on rear foot, fists in tight high guard. Breathing heavily through clenched teeth. Eyes locked aggressively on opponent off-camera. Locked dynamic camera.

[SHOT 2 - IMPACT/STRIKE - 1.2s]
[Camera: Fast whip-pan / crash push-in]. Fighter A explosively drives forward, slamming [shoulder / strike] into Fighter B's [chest / guard], forcing Fighter B violently backward into [wall / floor]. [Environmental FX: Rain spray / dust / debris bursts outward on impact]. High kinetic energy, physical resistance, motion blur, 45-degree shutter.

[SHOT 3 - RECOIL/AFTERMATH - 1.5s]
[Framing: Medium close-up]. Fighter B recoils and stumbles back against [surface]. Fighter A takes half a step back, fists still raised, chest heaving with rapid breaths. Neon rim lighting, grounded realism.
```

---

## 🛠 Supported Video AI Models

| Model | Drama / Acting Strategy | Action / Combat Strategy |
| :--- | :--- | :--- |
| **Kling AI (1.5 / 2.0 / 3.0)** | `Subject + Movement + Scene + Camera + Lighting` (1–2 face cues) | Separate striker from receiver; use Motion Brush for trajectory; 1 strike per 5s clip. |
| **Runway (Gen-4.5 / Gen-3)** | Positive kinematic phrasing, no negative commands | High-speed shutter phrasing (`45-degree shutter`), environmental FX bursts, locked impacts. |
| **OpenAI Sora** | Storyboard beats + Count | `1 Subject Action + 1 Camera Move` in sequential count beats. |
| **MiniMax / Hailuo** | Explicit `[Static shot]` / `[Push in]` | `[Push in]` or `[Crash zoom]` with explicit momentum and debris triggers. |
| **Seedance 2.5 (Jimeng)** | `Spatial Anchor + 3-Stage Beats + Camera Trajectory` | Timestamp-level beat structuring (0-1.5s stance, 1.5-3.5s collision, 3.5-5.0s recoil). |
| **Luma Dream Machine** | Separate subject kinematics from camera language | Explicit camera commands (e.g., `camera whip-pans right following impact`). |

---

## 🎭 Before & After Comparisons

### Drama / Acting
| ❌ Uncanny / Vague Prompt | ✅ Oscar-Grade Kinematic Rewrite |
| :--- | :--- |
| `A heartbroken woman crying sadly in the rain, looking devastated.` | `She holds the letter steady with both hands. Her eyes remain fixed off-camera right. On a shallow inhale, her lower eyelids tighten slightly while her lips press together. She swallows once, lets her gaze fall to the floor, and releases her jaw on the slow exhale. Locked 85mm close-up, soft window light catching lower eyelid moisture.` |

### Action / Martial Arts
| ❌ Broken / Floaty Combat Prompt | ✅ Grounded 3-Shot Action Sequence |
| :--- | :--- |
| `An athletic girl gets cornered by a thug, blocks his punch, grapples his wrist, and slams him into a brick wall with fast martial arts.` | **Shot 1 (1.5s)**: *Tight medium, rainy alley. Athletic woman backed against wet brick wall, breathing heavily through clenched teeth. Weight loaded on rear foot, fists in tight guard. Eyes lock aggressively off-camera right.*<br>**Shot 2 (1.2s)**: *Fast whip-pan. Woman drives forward explosively, slamming her shoulder into the man's chest and ramming him backward into the brick wall. Heavy rain spray bursts outward upon impact. Motion blur, 45° shutter.*<br>**Shot 3 (1.5s)**: *Medium close-up. Man recoils and slumps down the wall. Woman steps back, chest heaving with rapid breaths, fists still raised.* |

---

## 📚 Deep Documentation & Knowledge Base

* 🎭 **Drama & Acting References**:
  * 📖 [Cinematic Acting Directing Frameworks](references/cinematic_acting_frameworks.md)
  * 🧬 [Micro-Expressions & FACS Guide](references/micro_expressions_cinematic.md)
  * 📐 [Kinematic Prompt Grammar & Benchmarks](references/cinematic_prompt_grammar.md)
  * 🎬 [Curated Drama Prompt Recipes](examples/cinematic_prompt_recipes.md)
* 💥 **Action & Combat References**:
  * 🥋 [Action Shot Decomposition Framework](references/action_shot_decomposition.md)
  * ⚡ [Combat Kinematics & Environmental FX Masking](references/combat_kinematics_fx.md)
  * 🛠 [Combat Prompt Engineering & I2V Pipelines](references/combat_prompt_engineering.md)
  * 🥊 [Curated 3-Shot Action Combat Recipes](examples/action_combat_recipes.md)

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
