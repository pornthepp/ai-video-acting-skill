# AI Video Acting Director Skill 🎬

An advanced agentic skill designed to direct, refine, and prompt **Oscar-level, realistic cinematic acting performances** in Generative AI Video models (Runway Gen-3 / Act-One, Kling AI, OpenAI Sora, MiniMax / Hailuo, Luma Dream Machine).

Eliminates the "Uncanny Valley" and cartoonish overacting by replacing abstract emotional adjectives with **Playable Action Verbs**, **FACS-based Micro-expressions**, **Gaze/Respiratory Dynamics**, and **Kinematic Prompt Grammar**.

---

## 📁 Repository Structure

```text
ai-video-acting-skill/
├── SKILL.md                                 # Core agent skill instructions, templates & model adapters
├── README.md                                # Project documentation & overview
├── references/
│   ├── cinematic_acting_frameworks.md       # Directing theory (Judith Weston, Stanislavski, Meisner, Beats)
│   ├── micro_expressions_cinematic.md       # FACS Action Units, Gaze dynamics, Respiration, Laban analysis
│   └── cinematic_prompt_grammar.md          # Kinematic Prompt syntax, Anti-patterns, Benchmark prompts
└── examples/
    └── cinematic_prompt_recipes.md          # Ready-to-use prompt recipes across film genres
```

---

## 🧠 Core Principles

### 1. "Playable Actions" over "Result Directing"
* ❌ **Result Directing (Fails in AI)**: `"She looks deeply heartbroken and cries sadly."`
* ✅ **Playable Directing (Realistic)**: Direct what the character is doing to someone else (e.g., *To swallow defeat*, *To protect them from seeing my pain*).

### 2. Kinematic Micro-Acting Hierarchy
$$\text{SPATIAL ANCHOR} \longrightarrow \text{PRIMARY ACTION} \longrightarrow \text{EYELINE} \longrightarrow \text{1–2 FACE CUES} \longrightarrow \text{BREATH} \longrightarrow \text{RECOVERY/STILLNESS}$$

### 3. Temporal Beat Structure (3s – 8s Shots)
$$\text{ESTABLISH / HOLD} \longrightarrow \text{MICRO-LEAK / IMPULSE} \longrightarrow \text{RECOVER / SETTLE}$$

---

## 🛠 Supported Video AI Models

* **Kling AI (1.5 / 2.0)**: Structured `Subject + Movement + Scene + Camera + Lighting` formatting.
* **Runway (Gen-3 Alpha / Act-One)**: Positive kinematic descriptions and locked compositions without negative phrasing.
* **OpenAI Sora**: 1 Character Action + 1 Camera Move structured in sequential storyboard beats.
* **MiniMax / Hailuo**: Bracketed camera syntax `[Static shot]` / `[Push in]` with literal kinematic phrasing.
* **Luma Dream Machine**: Explicit separation of character kinematics and camera motion.

---

## 📖 Usage

Import this skill into your Antigravity or AI agent workflow. When generating video prompts or directing scenes, the agent will automatically apply the acting frameworks, FACS cues, and model-specific kinematic grammar defined in [`SKILL.md`](SKILL.md).

---

## 📄 License
MIT License
