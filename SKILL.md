---
name: ai-video-acting-director
description: Directs, designs, and refines highly realistic cinematic acting performances and Hollywood/HK-grade action fight sequences for AI Video models (Runway, Kling, Sora, Hailuo/MiniMax, Luma, Seedance 2.5). Enforces Playable Action Verbs, Subtext/Beats, FACS Micro-expressions, Action Shot Decomposition (1.0s–1.8s cuts), The Zero-Grappling Law, and Environmental FX Masking to eliminate uncanny acting and multi-body combat morphing.
---

# AI Video Acting & Action Director Skill 🎬💥

This skill transforms abstract video prompts into **physically playable, Oscar-grade cinematic drama performances** and **visceral, grounded action fight sequences** optimized for current generative video diffusion models.

---

## 1. Dual Directing Modes & Duration Limits

```
┌──────────────────────────────────────────────────────────────────────────┐
│                   AI VIDEO CINEMATIC DIRECTING ENGINE                    │
├──────────────────────────────────────────┬───────────────────────────────┤
│ MODE A: ACTING & DRAMATIC PERFORMANCE    │ MODE B: ACTION & COMBAT STUNT │
├──────────────────────────────────────────┼───────────────────────────────┤
│ • Shot Duration: 3.0s – 8.0s Continuous  │ • Shot Duration: 1.0s – 1.8s  │
│ • Baseline: Anchored Stillness           │ • Rule: NEVER continuous >2s  │
│ • Playable Action Verbs (Judith Weston)  │ • Zero-Grappling & No-Locks   │
│ • FACS Micro-Cues (1-2 Features Max)     │ • Single-Subject Cut Isolation│
│ • Temporal Beat: Hold ➔ Leak ➔ Settle    │ • Environmental FX Masking    │
└──────────────────────────────────────────┴───────────────────────────────┘
```

---

## 2. Mode A: Cinematic Acting & Dramatic Performance

### Core Philosophy
* ❌ **Never use Result Directing**: Avoid adjectives like *heartbroken, deeply angry, terrified, sad*.
* ✅ **Use Playable Verbs**: Direct what the character is physically doing to someone else (*To corner, To deflect, To conceal, To shield, To hold them here, To swallow defeat*).
* ✅ **Micro-Acting Hierarchy**:
  $$\text{SPATIAL ANCHOR} \longrightarrow \text{PRIMARY ACTION} \longrightarrow \text{EYELINE} \longrightarrow \text{1–2 FACE CUES} \longrightarrow \text{BREATH} \longrightarrow \text{RECOVERY/STILLNESS}$$

### Drama Master Template (T2V / I2V) — [3.0s to 8.0s Continuous]
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

### 🚫 The "Action Duration Limit" & "Zero-Grappling" Laws

#### 1. The Hard Duration Cap (1.0s – 1.8s Max per Cut)
* **NEVER output single continuous action takes > 2.0 seconds** (e.g. 5s or 8s continuous combat prompts). Long single-takes cause AI models to float, hoverboard-slide, or morph.
* If a user requests a *"5-second"* or *"8-second"* action scene, you **MUST automatically decompose it into a sequence of 3 to 4 short cuts** (e.g., Cut 1 [1.5s] ➔ Cut 2 [1.0s] ➔ Cut 3 [1.2s] ➔ Cut 4 [1.5s]).

#### 2. The Zero-Grappling & No-Joint-Locks Law
* ❌ **STRICTLY BANNED**:
  - Wrist locks, armbars, rotational leverages (*"rotates forearm against thumb"*).
  - Multi-person continuous throws (*"catches his arm and throws him over hip"*).
  - Disembodied reaching arms (*"an arm enters frame from left and grabs her"*).
  - Prolonged multi-body wrestling on the ground.

---

### ✅ The 4 Golden Rules of AI Action Stunt Directing

#### Rule 1: Strike & Evade over Grapple (หลบและกระแทก แทนการจับล็อก)
* Direct **sharp sidesteps, ducking, elbow drives, palm strikes, and push-kicks** instead of grabbing wrists or throwing over shoulders.

#### Rule 2: Single-Subject Motion Isolation (แยกช็อต 1 คัท = 1 ตัวแสดงหลัก)
Instead of forcing the AI to calculate 2 entangled moving bodies:
* **Cut A (Attacker Focus)**: Character A sidesteps sharply and drives an elbow/shoulder forward into empty space/camera.
* **Cut B (Receiver & Impact Focus)**: Character B crashes violently onto the floor/rocks/wall in an isolated single-body physics reaction.

#### Rule 3: Environmental FX as Collision Proxies (ใช้ Effect สิ่งแวดล้อมพรางจุดสัมผัส)
Every impact MUST trigger an environmental explosion (**sand/dust cloud, rain spray burst, shattering glass, flying sparks, plaster debris**) at the point of impact. This masks AI limb contact flaws and magnifies perceived kinetic force.

#### Rule 4: Dynamic Camera Energy
Use **fast whip-pans, crash zooms, 45-degree narrow shutter angle look, and tracking momentum** to convey high-speed violence.

---

### Combat Master 4-Shot Sequence Protocol (Total: ~5s–8s Scene)

```text
[SHOT 1 — SETUP & TENSION — 1.5s]
[Framing: Static or slow drift medium 35mm], [Environment]. [Fighter A], weight planted firmly on rear foot, hands in tight combat guard. Breathing heavily through clenched teeth. Eyes locked aggressively off-camera. Low rim lighting, grounded stillness.

[SHOT 2 — EVASION & COUNTER STRIKE — 1.0s]
[Camera: Fast handheld whip-pan / crash push-in]. Fighter A sidesteps sharply to the right as an opponent's shadow rushes past, and drives an explosive [elbow / palm / shoulder] forward into frame. [Environmental FX: Thick dust cloud / rain spray / sparks kick up violently]. Motion blur, 45-degree shutter angle.

[SHOT 3 — IMPACT & CRASH REACTION — 1.2s]
[Camera: Low-angle close-up / crash push-in]. The opponent crashes violently down onto the [rocky ledge / wet floor / brick wall], [dust / sand / water] exploding outward in a heavy shockwave around the body. High kinetic weight and recoil.

[SHOT 4 — RECOVERY & STANDOFF — 2.0s]
[Framing: Medium close-up through drifting dust/smoke]. Fighter A stands grounded in guard, chest heaving with rapid breaths. Eyes locked downward off-camera. Dust particles drifting in warm rim light.
```

---

## 4. Platform-Specific Adapters

| Model | Drama / Acting Strategy | Action / Combat Strategy |
| :--- | :--- | :--- |
| **Kling AI (1.5 / 2.0 / 3.0)** | `Subject + Movement + Scene + Camera + Lighting` (1–2 face cues) | Isolate striker from receiver; 1.0s–1.5s single action cuts. |
| **Runway (Gen-4.5 / Gen-3)** | Positive kinematic phrasing, no negative commands | Use `45-degree shutter angle`, environmental FX bursts, 1.2s impact cuts. |
| **OpenAI Sora** | Storyboard beats + Count | `1 Subject Action + 1 Camera Move` in sequential 1.5s cuts. |
| **MiniMax / Hailuo** | Explicit `[Static shot]` / `[Push in]` | `[Push in]` or `[Crash zoom]` with explicit momentum and debris triggers. |
| **Seedance 2.5 (Jimeng)** | `Spatial Anchor + 3-Stage Beats + Camera Trajectory` | Timestamp-level beat structuring in short 1.5s cuts. |
| **Luma Dream Machine** | Separate subject kinematics from camera language | Explicit camera commands (e.g., `camera whip-pans right following impact`). |

---

## 5. Anti-Patterns & AI Failure Checklist

| ❌ Broken / Floaty Prompt | ⚠️ Why it Fails in AI | ✅ Grounded Production Rewrite |
| :--- | :--- | :--- |
| `Single 8s continuous action take where girl dodges, catches arm, and throws raider.` | Diffusion models cannot sustain continuous multi-stage combat; causes hoverboard sliding and rubbery ragdoll glitches. | **Decompose into 4 discrete cuts (1.0s–1.5s each):** (1) Stance $\rightarrow$ (2) Evasion & elbow strike $\rightarrow$ (3) Raider crashes on rocks with dust shockwave $\rightarrow$ (4) Standoff. |
| `A raider's arm enters frame and grabs her forearm.` | Causes disembodied ghost limbs and morphing skin fusion. | `The woman sidesteps sharply to the right as an opponent's shadow rushes past, driving her elbow forward.` |
| `She rotates forearm against his thumb and throws him over her hip.` | Diffusion models have no IK/joint physics; causes 360° arm twist and floaty ragdoll. | **Decompose into 2 cuts:** (1) Her explosive sidestep & strike $\rightarrow$ (2) Opponent crashing onto rocks with sand explosion. |
| `Two martial artists grapple and wrestle on the floor.` | Multi-body entanglement collapses into a single morphing mass. | Use striking & distance: *Push-kick $\rightarrow$ Opponent crashes through wooden table.* |
| `She looks deeply heartbroken and cries sadly.` | Abstract emotion; triggers cartoonish facial grimacing. | `Her eyes stay on the letter. Lower eyelids tighten as breath catches. She swallows once, gaze drops, jaw slowly releases.` |

---

## 6. Reference Files Directory

* 🎭 **Drama & Acting**:
  * [`references/cinematic_acting_frameworks.md`](references/cinematic_acting_frameworks.md) – Judith Weston verbs, Stanislavski, Meisner, Beats.
  * [`references/micro_expressions_cinematic.md`](references/micro_expressions_cinematic.md) – FACS Action Units, Gaze dynamics, Respiration.
  * [`references/cinematic_prompt_grammar.md`](references/cinematic_prompt_grammar.md) – Kinematic prompt grammar & benchmark prompts.
  * [`examples/cinematic_prompt_recipes.md`](examples/cinematic_prompt_recipes.md) – Drama prompt recipes.
* 💥 **Action & Stunt**:
  * [`references/action_shot_decomposition.md`](references/action_shot_decomposition.md) – Hollywood/HK action decomposition, 1-2s cuts.
  * [`references/combat_kinematics_fx.md`](references/combat_kinematics_fx.md) – Kinetic chain, Weight transfer, Environmental FX masking.
  * [`references/combat_prompt_engineering.md`](references/combat_prompt_engineering.md) – State-transition prompting, collision workarounds, I2V staging.
  * [`examples/action_combat_recipes.md`](examples/action_combat_recipes.md) – 3-Shot and 4-Shot action recipes (Alley fight, Desert warrior, Gun-fu, Wall smash).
