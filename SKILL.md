---
name: ai-video-acting-director
description: Directs, designs, and refines highly realistic, subtle cinematic acting performances and Hollywood/HK-grade action fight sequences for AI Video models (Runway, Kling, Sora, Hailuo/MiniMax, Luma, Seedance 2.5). Uses Playable Action Verbs (Judith Weston), Subtext/Beats, Micro-expressions (FACS/Laban), Action Shot Decomposition (1 Action per Shot), Kinetic Body Mechanics, and Environmental FX Masking to eliminate uncanny acting and multi-body combat morphing.
---

# AI Video Acting & Action Director Skill 🎬💥

This skill transforms abstract video prompts into **physically playable, Oscar-grade cinematic drama performances** and **visceral, grounded action fight sequences** optimized for AI generative video models.

---

## 1. Dual Directing Modes

```
┌──────────────────────────────────────────────────────────────────────────┐
│                   AI VIDEO CINEMATIC DIRECTING ENGINE                    │
├──────────────────────────────────────────┬───────────────────────────────┤
│ MODE A: ACTING & DRAMATIC PERFORMANCE    │ MODE B: ACTION & COMBAT STUNT │
│  - Playable Action Verbs (Weston)        │  - Action Shot Decomposition  │
│  - The Moment Before & Subtext           │  - 1 Action per Shot Rule     │
│  - FACS Micro-Cues (1-2 Features Max)    │  - Kinetic Chain & Resistance │
│  - Temporal Beat: Hold ➔ Leak ➔ Settle   │  - Environmental FX Masking   │
└──────────────────────────────────────────┴───────────────────────────────┘
```

---

## 2. Mode A: Cinematic Acting & Dramatic Performance

### Core Philosophy
* ❌ **Never use Result Directing**: Avoid adjectives like *heartbroken, deeply angry, terrified, sad*.
* ✅ **Use Playable Verbs**: Direct what the character is physically doing to someone else (*To corner, To deflect, To conceal, To shield, To hold them here, To swallow defeat*).
* ✅ **Micro-Acting Hierarchy**:
  $$\text{SPATIAL ANCHOR} \longrightarrow \text{PRIMARY ACTION} \longrightarrow \text{EYELINE} \longrightarrow \text{1–2 FACE CUES} \longrightarrow \text{BREATH} \longrightarrow \text{RECOVERY/STILLNESS}$$

### Drama Master Template (T2V)
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

---

## 3. Mode B: Cinematic Action & Combat Choreography

### The Multi-Body Collision Bottleneck
Current AI Video models **fail when trying to render multi-limb grappling, wrist locks, or multi-beat combos in a single prompt** (resulting in 3 arms, morphing bodies, and rubbery floaty physics).

### The Golden Action Rules
1. **The "One Dominant Action Per Shot" Rule**: Never prompt a full combo in one prompt. Break fights into **1-to-2-second discrete cuts**.
2. **The 3-Shot Action Sequence Protocol**:
   * **Cut 1 (Setup & Tension / 1.5s)**: Base stance, foot planting, ragged breath, intense eyeline lock. *(Uses Mode A Acting Skill)*
   * **Cut 2 (Strike & Collision / 1.0–1.2s)**: Single explosive kinetic drive, shoulder/fist impact, momentum transfer.
   * **Cut 3 (Reaction & Recoil / 1.5–2.0s)**: Body recoil, stumbling against surface, settling into new guard.
3. **Environmental FX Masking**:
   * Use high-energy environmental reactions (**water spray, rain burst, dust explosion, shattering glass, muzzle smoke, sparks**) at the exact moment of collision to sell immense kinetic power and mask AI limb entanglement.
4. **Camera Energy as Impact**:
   * Use **whip pans, crash zooms, high-speed shutter looks (45°/90°), and tracking momentum** instead of static wide shots.

### Combat Master 3-Shot Protocol Template
```text
[SHOT 1 - TENSION/STANCE - 1.5s]
[Framing: Tight medium 35mm], [Environment]. [Fighter A description], weight planted firmly on rear foot, fists in tight high guard. Breathing heavily through clenched teeth. Eyes locked aggressively on opponent off-camera. Locked dynamic camera.

[SHOT 2 - IMPACT/STRIKE - 1.2s]
[Camera: Fast whip-pan / crash push-in]. Fighter A explosively drives forward, slamming [shoulder / strike] into Fighter B's [chest / guard], forcing Fighter B violently backward into [wall / floor]. [Environmental FX: Rain spray / dust / debris bursts outward on impact]. High kinetic energy, physical resistance, motion blur.

[SHOT 3 - RECOIL/AFTERMATH - 1.5s]
[Framing: Medium close-up]. Fighter B recoils and stumbles back against [surface]. Fighter A takes half a step back, fists still raised, chest heaving with rapid breaths. Neon/rim lighting, grounded realism.
```

---

## 4. Platform-Specific Adapters

| Model | Drama / Acting Syntax | Action / Combat Syntax |
| :--- | :--- | :--- |
| **Kling AI (1.5 / 2.0 / 3.0)** | `Subject + Movement + Scene + Camera + Lighting` (1–2 face cues) | Separate striker from receiver; use Motion Brush for trajectory; 1 strike per 5s clip. |
| **Runway (Gen-4.5 / Gen-3)** | Positive kinematic phrasing, no negative commands | High-speed shutter phrasing (`45-degree shutter`), environmental FX bursts, locked impacts. |
| **OpenAI Sora** | Storyboard beats + Count | `1 Subject Action + 1 Camera Move` in sequential count beats. |
| **MiniMax / Hailuo** | Explicit `[Static shot]` / `[Push in]` | `[Push in]` or `[Crash zoom]` with explicit momentum and debris triggers. |
| **Seedance 2.5 (Jimeng)** | `Spatial Anchor + 3-Stage Beats + Camera Trajectory` | Timestamp-level beat structuring (0-1.5s stance, 1.5-3.5s collision, 3.5-5.0s recoil). |
| **Luma Dream Machine** | Separate subject kinematics from camera language | Explicit camera commands (e.g., `camera whip-pans right following impact`). |

---

## 5. Anti-Patterns & Rewrite Rules

| ❌ Broken / Floaty Prompt | ⚠️ Why it Fails in AI | ✅ Grounded Production Rewrite |
| :--- | :--- | :--- |
| `Two martial artists fight fiercely, blocking punches, grappling wrists, and throwing each other.` | Multi-body collision fails; causes 3 arms, morphing, and rubbery floating bodies. | **Break into 3 discrete cuts:** (1) Stance & eyeline lock $\rightarrow$ (2) Explosive shoulder drive + rain spray $\rightarrow$ (3) Recoil into wall. |
| `He throws a super powerful knockout punch.` | Abstract adjective; no kinetic chain or weight transfer. | `He plants his lead foot, rotates his hips and torso, and drives a compact right cross into the opponent's chest; the opponent recoils two steps.` |
| `She looks heartbroken and cries desperately.` | Abstract emotion; triggers cartoonish facial grimacing. | `Her eyes stay on the letter. Lower eyelids tighten as breath catches. She swallows once, gaze drops, jaw slowly releases.` |

---

## 6. Reference Files Directory

* 🎭 **Acting & Drama**:
  * [`references/cinematic_acting_frameworks.md`](references/cinematic_acting_frameworks.md) – Judith Weston verbs, Stanislavski, Meisner, Beats.
  * [`references/micro_expressions_cinematic.md`](references/micro_expressions_cinematic.md) – FACS Action Units, Gaze dynamics, Respiration.
  * [`references/cinematic_prompt_grammar.md`](references/cinematic_prompt_grammar.md) – Kinematic prompt grammar & benchmark prompts.
  * [`examples/cinematic_prompt_recipes.md`](examples/cinematic_prompt_recipes.md) – Drama prompt recipes.
* 💥 **Action & Stunt**:
  * [`references/action_shot_decomposition.md`](references/action_shot_decomposition.md) – Hollywood/HK action decomposition, 1-2s cuts.
  * [`references/combat_kinematics_fx.md`](references/combat_kinematics_fx.md) – Kinetic chain, Weight transfer, Environmental FX masking.
  * [`references/combat_prompt_engineering.md`](references/combat_prompt_engineering.md) – State-transition prompting, collision workarounds, I2V staging.
  * [`examples/action_combat_recipes.md`](examples/action_combat_recipes.md) – 3-Shot action sequences (Alley fight, Gun-fu, Swords, Wall smash).
