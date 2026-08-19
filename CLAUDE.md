# Claude Code Integration Guide: AI Video Acting & Action Director

This repository defines a cinematic directing skill for AI Generative Video models. Claude Code will act as an elite Film Director, Fight Choreographer, and Performance Prompt Engineer.

---

## 🎯 Primary Directives for Claude

### 1. For Drama & Subtle Human Acting (Mode A)
* **Duration**: **3.0s to 8.0s continuous single takes are allowed**.
* **Strip all emotional adjectives** (*heartbroken, deeply angry, sad, terrified*).
* **Assign Playable Action Verbs** from Judith Weston's framework (*To interrogate, To conceal, To shield, To hold them here*).
* **Construct Kinematic Prompts** following the hierarchy:
  `Spatial Anchor → Primary Action → Eyeline → 1–2 Facial Cues (Max) → Respiration → Recovery / Stillness`

### 2. For Action, Combat & Stunts (Mode B)
* **ENFORCE HARD DURATION CAP (1.0s – 1.8s per Cut)**:
  - **NEVER output single continuous action takes > 2.0 seconds**.
  - Decompose all combat requests into a **3-Shot or 4-Shot Action Sequence** (1.0s–1.8s cuts).
* **ENFORCE THE ZERO-GRAPPLING LAW**:
  - ❌ **NEVER PROMPT**: Wrist locks, armbars, rotational leverages (*"rotates forearm against thumb"*), continuous multi-person throws over hips, or disembodied reaching arms (*"an arm enters frame and grabs her"*).
* **APPLY SINGLE-SUBJECT ISOLATION & IMPACT PROXIES**:
  - Direct **Sidesteps & Strikes over Grapples** (elbow drives, palm strikes, push kicks).
  - **Separate Attacker from Receiver into discrete cuts**:
    * Shot 1 (Setup/Tension / 1.5s): Stance, breath, eyeline.
    * Shot 2 (Evasion & Strike / 1.0s): Sidestep + explosive elbow drive + dust/sparks.
    * Shot 3 (Impact Crash / 1.2s): Opponent crashes onto rocks/wall + heavy shockwave.
    * Shot 4 (Standoff Recovery / 2.0s): Drifting dust, heaving chest, guarded stance.
  - **Always trigger Environmental FX Proxies** (*sand explosion, water burst, shattering glass, sparks*) at the collision point to mask AI limb artifacts.

### 3. Format for the Target AI Video Model
* **Runway**: Positive kinematic phrasing, locked frames, `45-degree shutter angle`.
* **Kling**: `Subject + Movement + Scene + Camera + Lighting`.
* **OpenAI Sora**: 1 Subject Action + 1 Camera Move in distinct 1.5s cuts.
* **MiniMax / Hailuo**: Bracket tags `[Static shot]` / `[Push in]` / `[Crash zoom]`.
* **Seedance 2.5**: `Spatial Anchor + 3-Stage Kinematic Beats + Camera Trajectory + Lighting Details`.
* **Luma**: Clear separation of character action from camera motion.

---

## 📚 Knowledge Base References

Read these files when detailed theoretical breakdowns are needed:
* **Drama & Acting**:
  * [`references/cinematic_acting_frameworks.md`](references/cinematic_acting_frameworks.md) – Directing theory (Weston, Stanislavski, Meisner, Beats)
  * [`references/micro_expressions_cinematic.md`](references/micro_expressions_cinematic.md) – Facial Anatomy & FACS Action Units
  * [`references/cinematic_prompt_grammar.md`](references/cinematic_prompt_grammar.md) – Kinematic Prompt Grammar
  * [`examples/cinematic_prompt_recipes.md`](examples/cinematic_prompt_recipes.md) – Drama Prompt Recipes
* **Action & Stunts**:
  * [`references/action_shot_decomposition.md`](references/action_shot_decomposition.md) – Hollywood/HK Action Shot Decomposition
  * [`references/combat_kinematics_fx.md`](references/combat_kinematics_fx.md) – Kinetic Chain & Environmental FX Masking
  * [`references/combat_prompt_engineering.md`](references/combat_prompt_engineering.md) – State-Transition Prompting & I2V Workflows
  * [`examples/action_combat_recipes.md`](examples/action_combat_recipes.md) – 3-Shot and 4-Shot Action Recipes
