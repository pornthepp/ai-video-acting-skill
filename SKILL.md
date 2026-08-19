---
name: ai-video-acting-director
description: Directs, designs, and refines highly realistic cinematic acting performances and Hollywood/HK-grade action fight sequences for AI Video models (Runway, Kling, Sora, Hailuo/MiniMax, Luma, Seedance 2.5). Enforces Playable Action Verbs, Subtext/Beats, FACS Micro-expressions, Action Shot Decomposition (1 Action per Shot), The Zero-Grappling Rule, and Environmental FX Masking to eliminate uncanny acting and multi-body combat morphing.
---

# AI Video Acting & Action Director Skill 🎬💥

This skill transforms abstract video prompts into **physically playable, Oscar-grade cinematic drama performances** and **visceral, grounded action fight sequences** optimized for current generative video diffusion models.

---

## 1. Dual Directing Modes

```
┌──────────────────────────────────────────────────────────────────────────┐
│                   AI VIDEO CINEMATIC DIRECTING ENGINE                    │
├──────────────────────────────────────────┬───────────────────────────────┤
│ MODE A: ACTING & DRAMATIC PERFORMANCE    │ MODE B: ACTION & COMBAT STUNT │
│  - Playable Action Verbs (Weston)        │  - Action Shot Decomposition  │
│  - The Moment Before & Subtext           │  - 1 Action per Shot Rule     │
│  - FACS Micro-Cues (1-2 Features Max)    │  - The Zero-Grappling Rule    │
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

### 🚫 The "Zero-Grappling & No-Limb-Entanglement" Law
Diffusion video models lack rigid-body/skeletal physics engines and **fail catastrophically on continuous joint locks, multi-limb grappling, or disembodied grabs**.

#### ❌ STRICTLY BANNED IN T2V/I2V PROMPTS:
1. **No Joint Locks / Leverages**: Never prompt *“rotates forearm against thumb”*, *“wrist lock”*, *“armbar”*, or *“finger lock”*.
2. **No Continuous Multi-Body Throws**: Never prompt *“catches his arm and throws him over her hip in one continuous motion”*.
3. **No Disembodied Reaching Limbs**: Never prompt *“an arm enters frame from the left and grabs her wrist”* (causes ghost limbs and skin fusion).
4. **No Prolonged Wrestling/Grappling**: Never prompt 2 characters with continuous skin-to-skin contact across multiple seconds.

---

### ✅ The 4 Golden Rules of AI Action Stunt Directing

#### Rule 1: Strike & Evade over Grapple (หลบและกระแทก แทนการจับล็อก)
* Direct **sharp sidesteps, ducking, elbow drives, palm strikes, and push-kicks** instead of grabbing wrists or throwing over shoulders.

#### Rule 2: Visual Deception & Impact Isolation (แยกช็อตคนตี vs คนโดน)
Instead of showing A holding and throwing B in one frame:
* **Cut A (Attacker Focus)**: Character A sidesteps sharply and drives an elbow/shoulder forward into empty space/camera.
* **Cut B (Receiver & Impact Focus)**: Character B crashes violently onto the floor/rocks/wall in an isolated physics reaction.

#### Rule 3: Environmental FX as Collision Proxies (ใช้ Effect สิ่งแวดล้อมพรางจุดสัมผัส)
Every impact MUST trigger an environmental explosion (**sand/dust cloud, rain spray burst, shattering glass, flying sparks, plaster debris**) at the point of impact. This masks AI limb contact flaws and magnifies perceived kinetic force.

#### Rule 4: Dynamic Camera Energy
Use **fast whip-pans, crash zooms, 45-degree narrow shutter angle look, and tracking momentum** to convey high-speed violence.

---

### Combat Master 3-Shot Sequence Protocol

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
| **Kling AI (1.5 / 2.0 / 3.0)** | `Subject + Movement + Scene + Camera + Lighting` (1–2 face cues) | Isolate striker from receiver; use Motion Brush for single trajectories; 1 strike per clip. |
| **Runway (Gen-4.5 / Gen-3)** | Positive kinematic phrasing, no negative commands | Use `45-degree shutter angle`, environmental FX bursts, locked impacts. |
| **OpenAI Sora** | Storyboard beats + Count | `1 Subject Action + 1 Camera Move` in sequential count beats. |
| **MiniMax / Hailuo** | Explicit `[Static shot]` / `[Push in]` | `[Push in]` or `[Crash zoom]` with explicit momentum and debris triggers. |
| **Seedance 2.5 (Jimeng)** | `Spatial Anchor + 3-Stage Beats + Camera Trajectory` | Timestamp-level beat structuring (0-1.5s stance, 1.5-3.5s collision, 3.5-5.0s recoil). |
| **Luma Dream Machine** | Separate subject kinematics from camera language | Explicit camera commands (e.g., `camera whip-pans right following impact`). |

---

## 5. Anti-Patterns & AI Failure Checklist

| ❌ Broken / Floaty Prompt | ⚠️ Why it Fails in AI | ✅ Grounded Production Rewrite |
| :--- | :--- | :--- |
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
  * [`examples/action_combat_recipes.md`](examples/action_combat_recipes.md) – 3-Shot action sequences (Alley fight, Desert warrior, Gun-fu, Wall smash).
