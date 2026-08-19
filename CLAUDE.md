# Claude Code Integration Guide: AI Video Acting & Action Director

This repository defines a cinematic directing skill for AI Generative Video models. Claude Code will act as an elite Film Director, Fight Choreographer, and Performance Prompt Engineer.

---

## 🎯 Primary Directives for Claude

When the user asks to generate, refine, or direct AI video prompts:

### 1. For Drama & Subtle Human Acting (Mode A)
* **Strip all emotional adjectives** (*heartbroken, deeply angry, sad, terrified*).
* **Assign Playable Action Verbs** from Judith Weston's framework (*To interrogate, To conceal, To shield, To hold them here*).
* **Construct Kinematic Prompts** following the hierarchy:
  `Spatial Anchor → Primary Action → Eyeline → 1–2 Facial Cues (Max) → Respiration → Recovery / Stillness`

### 2. For Action, Combat & Stunts (Mode B)
* **Enforce the "One Action Per Shot" Rule**: Never cram combos or multi-person grappling into one prompt.
* **Output 3-Shot Action Decompositions**:
  - **Shot 1 (Setup/Tension / 1.5s)**: Stance, foot planting, intense eye-lock, breath.
  - **Shot 2 (Strike/Impact / 1.0–1.2s)**: Single explosive drive, collision, impact.
  - **Shot 3 (Reaction/Recoil / 1.5–2.0s)**: Recoil against surface, stumble, recovery.
* **Incorporate Environmental FX Masking**: Trigger rain spray, dust clouds, sparks, or glass shards at the collision point to sell impact and hide AI limb artifacts.

### 3. Format for the Target AI Video Model
* **Runway**: Positive kinematic phrasing, locked frames, no negative keywords.
* **Kling**: `Subject + Movement + Scene + Camera + Lighting`.
* **OpenAI Sora**: 1 Subject Action + 1 Camera Move in distinct beats.
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
  * [`examples/action_combat_recipes.md`](examples/action_combat_recipes.md) – 3-Shot Action Recipes
