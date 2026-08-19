---
name: ai-video-acting-director
description: Directs, designs, and refines highly realistic, subtle cinematic acting performances for AI Video models (Runway, Kling, Sora, Hailuo/MiniMax, Luma). Uses Playable Action Verbs (Judith Weston), Subtext/Beats (Stanislavski/Meisner), Micro-expressions (FACS/Laban), and Kinematic Prompt Syntax to eliminate uncanny/exaggerated acting.
---

# AI Video Acting Director Skill

This skill transforms abstract, emotional video prompts into **physically playable, Oscar-level cinematic performances** optimized for AI generative video models.

---

## 1. Core Directing Philosophy

### ❌ Never Use "Result Directing"
Generic adjectives (e.g., *heartbroken, deeply angry, terrified, intensely emotional, crying sadly*) cause video AI models to produce exaggerated grimaces, rubbery faces, or static frozen masks.

### ✅ Use "Kinematic & Playable Directing"
Direct the **physical action the character does to someone else**, their **internal conflict (Subtext)**, and **observable micro-movements** (eyeline, breath, jaw tension, recovery).

```
┌──────────────────────────────────────────────────────────────────────────┐
│                          KINEMATIC DIRECTING PIPELINE                    │
│                                                                          │
│  [Abstract Emotion] ──► [Playable Action Verb] ──► [Moment Before]       │
│                                                          │               │
│  [Camera / Lighting] ◄── [Temporal Beats] ◄── [Physical Micro-Cues]      │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 2. The 5-Step Director Workflow

When converting or drafting an acting prompt:

### Step 1: Define the Playable Action Verb (What is the character doing?)
Instead of naming an emotion, pick an active intention targeting another person or obstacle:
* *Instead of "Sad"* ➔ **To hold them here** / **To protect them from my pain** / **To swallow defeat**
* *Instead of "Angry"* ➔ **To interrogate** / **To corner** / **To punish with silence**
* *Instead of "Guilty / Suspicious"* ➔ **To conceal** / **To deflect** / **To search for a mistake**
* *Instead of "Seductive / In love"* ➔ **To tempt across the line** / **To disarm**

*(See full taxonomy in [cinematic_acting_frameworks.md](file:///d:/GitHub/Skills/ai-video-acting-skill/references/cinematic_acting_frameworks.md))*

### Step 2: Establish the "Moment Before" & Spatial Anchor
Actors never start from zero. Establish:
* What just happened 10 seconds ago? (Residual physical tension).
* Where are they anchored? (Seated, leaning, holding an object, facing off-camera).

### Step 3: Select 1–2 Localized Micro-Cues (Avoid "Micro-expression Soup")
Do not animate every muscle. Pick **maximum 1 or 2 focal points**:
1. **Eyeline (Primary)**: Off-camera target, gaze hold, deliberate shift, blink cadence.
2. **Mouth / Jaw**: Lower eyelid tightening, jaw shift, lips pressing together, swallowing.
3. **Respiration**: Breath catch, shallow nasal inhale, held breath, slow exhale through parted lips.
4. **Hands / Prop**: Finger freeze, thumb tap, grip tightening, settling.

*(See anatomical breakdown in [micro_expressions_cinematic.md](file:///d:/GitHub/Skills/ai-video-acting-skill/references/micro_expressions_cinematic.md))*

### Step 4: Structure the Temporal Beat (3s to 8s Shots)
The natural rhythm of human emotional suppression follows:
$$\text{HOLD / ESTABLISH} \longrightarrow \text{LEAK / IMPULSE} \longrightarrow \text{RECOVER / SETTLE}$$

* **0.0 – 1.5s (Establish)**: Stillness + stable eyeline + anchored posture.
* **1.5 – 3.5s (Leak)**: Micro-action + localized facial cue + breath change.
* **3.5 – 5.0s (Recover)**: Eyeline shift or swallow + jaw release + settling into stillness.

### Step 5: Format for the Specific Target Video Model
Apply platform-specific keywords and syntax (see Section 4).

---

## 3. Master Prompt Templates

### A. Text-to-Video (T2V) Master Template

```text
[Character archetype], already [visible physical precondition / state] at/in [spatial anchor].

[Primary physical action in one or two beats].
Their eyes remain [precise eyeline target], then [one deliberate eyeline change].

On [small inhale / held breath / slow exhale], [1–2 localized facial movements: e.g., lower eyelids tighten slightly, lips press together].
They [swallow / blink once / release the jaw / settle the shoulders], then become still again.

[Shot size, e.g., Tight 85mm close-up], [depth of field], [locked camera OR one subtle camera move].
[Specific lighting source and interaction on face/eyes].

Restrained natural performance, low-amplitude real-time movement.
```

### B. Image-to-Video (I2V) Master Template
*(Note: Keep short. The first frame already defines lighting, costume, and composition).*

```text
The subject [single primary physical action].
Eyes remain [initial eyeline], then [one subtle gaze shift].
On [breath cue], [1–2 localized facial movements], followed by [jaw release / settling into stillness].
[One camera instruction, e.g., locked camera or slow push-in].
[Lighting interaction with micro-movement].
```

---

## 4. Platform-Specific Adapters

| Model | Syntax Strategy | Key Instructions |
| :--- | :--- | :--- |
| **Kling AI (1.5 / 2.0)** | `Subject + Movement + Scene + Camera + Lighting + Atmosphere` | Use 1–2 face cues. Direct physical actions achievable in 5 seconds. Use Motion Brush / Reference Video for maximum fidelity. |
| **Runway (Gen-3 / Act-One)** | Positive physical phrasing only | **NEVER** use negative commands (`no`, `don't`). Use positive anchors (`locked camera`, `mouth remains neutral`, `shoulders remain still`). |
| **OpenAI Sora** | Storyboard-like beats + Count | `1 Subject Action + 1 Camera Move`. Specify actions in sequential bullet beats. |
| **MiniMax / Hailuo** | Explicit bracket tags | Use `[Static shot]` or `[Push in]`. Set `prompt_optimizer=false` via API to preserve precise kinematic phrasing. |
| **Luma Dream Machine** | Separate subject kinematics from camera language | Natural kinematic descriptions for subject; direct camera commands (e.g., `camera slowly pushes in`). |

---

## 5. Anti-Patterns & Rewrite Rules

| ❌ Uncanny / Vague Prompt | ⚠️ Problem | ✅ Oscar-Level Kinematic Rewrite |
| :--- | :--- | :--- |
| `She looks deeply heartbroken and cries sadly.` | Abstract emotion label triggers cartoonish grimacing. | `Her eyes stay fixed on the letter. Her lower eyelids tighten as she takes a shallow breath. She swallows once, gaze drops to the floor, and her jaw slowly releases.` |
| `A detective stares intensely with a suspicious face.` | Causes frozen, unblinking glare. | `He sits motionless behind the scarred desk. His eyes remain locked off-camera right without turning his head. On a slow nasal exhale, his jaw shifts once, then settles.` |
| `She smiles with hidden pain and bittersweet feeling.` | Confusing contradictory emotions for AI. | `One corner of her mouth begins to lift, then stops before becoming a full smile. She blinks once, exhales softly through parted lips, and lets her gaze fall away.` |
| `Don't overact, no smiling, don't move camera.` | Negative prompting fails on modern video models. | `Locked 85mm close-up. Mouth stays neutral. Brows remain quiet. Subtle movement carried only by lower eyelids and a single swallow.` |

---

## 6. Reference Files Directory

For deep theory and specific databases, consult:
1. [`references/cinematic_acting_frameworks.md`](file:///d:/GitHub/Skills/ai-video-acting-skill/references/cinematic_acting_frameworks.md) – Directing theory (Weston, Stanislavski, Meisner, Shurtleff), Playable Action Verbs, Moment Before, Subtext & Opposition.
2. [`references/micro_expressions_cinematic.md`](file:///d:/GitHub/Skills/ai-video-acting-skill/references/micro_expressions_cinematic.md) – Scientific FACS mapping (AUs), Gaze dynamics (Saccades vs. Fixation), Respiratory engine, and Laban Movement Qualities for micro-gestures.
3. [`references/cinematic_prompt_grammar.md`](file:///d:/GitHub/Skills/ai-video-acting-skill/references/cinematic_prompt_grammar.md) – AI Video model prompt engineering, 10 Genre benchmark prompts (Noir, Thriller, Period, Cyberpunk, Sci-Fi), and platform grammar rules.
