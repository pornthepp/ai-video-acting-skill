# AI Video Acting Director Skill 🎬

[![Antigravity Skill](https://img.shields.io/badge/Antigravity-Skill-blue.svg)](https://github.com/pornthepp/ai-video-acting-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![AI Video Models](https://img.shields.io/badge/Supports-Runway%20%7C%20Kling%20%7C%20Sora%20%7C%20Hailuo%20%7C%20Luma%20%7C%20Seedance%202.5-orange.svg)](#supported-video-ai-models)

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
├── SKILL.md                                 # Main agent skill instructions, templates & model adapters
├── README.md                                # English documentation & overview
├── references/
│   ├── cinematic_acting_frameworks.md       # Directing theory (Judith Weston, Stanislavski, Meisner, Beats 3s/5s/8s)
│   ├── micro_expressions_cinematic.md       # FACS Action Units, Gaze dynamics, Respiration, Laban analysis
│   └── cinematic_prompt_grammar.md          # Kinematic Prompt syntax, Anti-patterns, Benchmark prompts
└── examples/
    └── cinematic_prompt_recipes.md          # Ready-to-use prompt recipes across film genres
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

## 🚀 Getting Started

### Using in Antigravity or AI Agents
1. Clone or copy this repository into your skills directory:
   ```bash
   git clone https://github.com/pornthepp/ai-video-acting-skill.git
   ```
2. The agent will read [`SKILL.md`](SKILL.md) and automatically apply the 5-step directing workflow, reference databases, and model adapters whenever video prompts or scene directions are requested.

---

## 📚 Deep Documentation

* 📖 [Cinematic Acting Directing Frameworks](references/cinematic_acting_frameworks.md)
* 🧬 [Micro-Expressions & FACS Guide](references/micro_expressions_cinematic.md)
* 📐 [Kinematic Prompt Grammar & Benchmarks](references/cinematic_prompt_grammar.md)
* 🎬 [Curated Cinematic Prompt Recipes](examples/cinematic_prompt_recipes.md)

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
