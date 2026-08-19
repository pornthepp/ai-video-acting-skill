# วิศวกรรมพรอมป์ต์สำหรับการแสดงภาพยนตร์แบบละเอียดใน Generative AI Video

รายงานนี้สังเคราะห์จากเอกสารทางการของ Runway, Kling AI/Kuaishou, OpenAI Sora, MiniMax/Hailuo และ Luma รวมกับงานวิจัยเรื่องการเคลื่อนไหวใบหน้าและ Micro-expression (สีหน้าเสี้ยววินาที) ค่ะ

ข้อสรุปที่สำคัญที่สุดคือ **อย่าสั่ง “อารมณ์” เป็นหลัก ให้กำกับ “พฤติกรรมทางกายที่กล้องมองเห็น”** เพราะคำว่า *heartbroken, terrified, devastated, conflicted* เป็นความหมายเชิงนามธรรม ไม่ได้ระบุว่าร่างกายต้องเปลี่ยนอย่างไร ขณะที่ Runway ระบุโดยตรงว่าคำสั่งเชิงแนวคิดทำให้โมเดลต้องตีความเจตนาเองและอาจสร้างการเคลื่อนไหวที่คาดเดาไม่ได้ ส่วน Sora แนะนำให้กำหนด Action (การกระทำ) เป็น Beat (จังหวะ) ที่มองเห็นได้ชัดเจน และให้หนึ่งช็อตมีการเคลื่อนกล้องหลักหนึ่งอย่างกับการกระทำของตัวละครหลักหนึ่งอย่างค่ะ citeturn14view0turn15view0

> **หลักคิด:**  
> `Emotion label → observable physiology → temporal sequence → spatial target → camera observation`
>
> เช่น  
> ❌ `She looks very heartbroken.`  
> ✅ `Her eyes stay on the unopened letter. Her lips press together slightly as she takes a small breath in. She swallows once, her gaze drops to the table, and her jaw slowly releases.`

## เหตุผลที่ Kinematic Description ให้การแสดงสมจริงกว่า

### ปัญหาของคำอารมณ์เชิงนามธรรม

ในชีวิตจริง ไม่มี Facial Configuration (รูปแบบการขยับใบหน้า) หนึ่งแบบที่เท่ากับอารมณ์หนึ่งชนิดเสมอ งานทบทวนขนาดใหญ่โดย Barrett และคณะพบว่าการยิ้ม ขมวดคิ้ว หรือทำหน้าบึ้งมีความสัมพันธ์กับอารมณ์ในบางบริบท แต่มีความแปรผันสูงระหว่างบุคคล สถานการณ์ และวัฒนธรรม ดังนั้นการคิดว่า `"sad = sad face"` หรือ `"fear = wide eyes"` เป็นการลดทอนพฤติกรรมมนุษย์มากเกินไปค่ะ citeturn17search0

นี่เป็นเหตุผลสำคัญว่าทำไมพรอมป์ต์อย่าง:

```text
She is devastated and looks deeply heartbroken.
```

จึงเปิดพื้นที่ให้โมเดลเลือกได้มากเกินไป เช่น ร้องไห้ทันที เบะปาก ขมวดคิ้วแรง ก้มศีรษะมากเกินไป หรือสร้าง “sad-face stereotype” (สีหน้าเศร้าแบบเหมารวม) แทนการแสดงแบบละเอียด

Runway อธิบายปัญหานี้เกือบตรงตัวว่า Abstract Concept (แนวคิดนามธรรม) บังคับให้โมเดลตีความเจตนาเอง และแนะนำให้แปลงแนวคิดเหล่านั้นเป็น Specific Physical Actions (การกระทำทางกายที่เฉพาะเจาะจง) ค่ะ citeturn14view0

Kling ก็ให้คำแนะนำในแนวเดียวกัน โดยแนะนำว่าแทนคำทั่วไปอย่าง `"happy person"` หรือ `"sad"` ให้ระบุรายละเอียดใบหน้าที่มองเห็น เช่น การกดริมฝีปาก ทิศทางสายตา หรือรอยย่นระหว่างคิ้ว และควรเลือก Facial Features (องค์ประกอบบนใบหน้า) เพียงหนึ่งถึงสองจุดแทนการสั่งทุกกล้ามเนื้อพร้อมกัน เพราะรายละเอียดมากเกินไปอาจทำให้ผลลัพธ์แข็งและไม่เป็นธรรมชาติค่ะ citeturn19view0turn19view1

### สิ่งที่ควรสั่งจริงคือ “การเปลี่ยนสถานะ”

หัวใจของ Kinematic Description (คำบรรยายเชิงการเคลื่อนไหว) ไม่ใช่แค่บอก Pose (ท่าทาง) แต่ต้องบอกว่า:

**ร่างกายเริ่มตรงไหน → มีอะไรเปลี่ยน → เปลี่ยนเมื่อไร → เปลี่ยนไปทางไหน → แล้วกลับเข้าสู่ความนิ่งอย่างไร**

โครงสร้างที่ฉันแนะนำสำหรับการแสดงละเอียดคือ:

```text
[STATIC ANCHOR].
[ONE PHYSICAL ACTION], while [EYELINE TARGET].
On [BREATH CUE], [1–2 LOCALIZED FACIAL CHANGES];
then [RELEASE / SETTLE / HOLD].
[CAMERA OBSERVATION]; [LIGHT INTERACTION].
```

หรือในรูปสมการ:

```text
ANCHOR → ACTION → EYELINE → FACE → BREATH → RELEASE → CAMERA/LIGHT
```

ตัวอย่าง:

```text
She remains seated with her shoulders nearly still, holding the unopened letter in both hands.
Her eyes stay fixed just off-camera right.
On a small inhale, her lower eyelids tighten slightly and her lips press together.
She swallows once, releases the breath slowly through her nose, then lets her gaze fall to the letter while her head remains almost still.
Locked 85 mm close-up; a very slow push-in during the final two seconds. Soft side window light catches the moisture along her lower eyelid.
```

คำว่า **“heartbroken” ไม่จำเป็นต้องปรากฏเลย** แต่ผู้ชมมีโอกาสอ่านความเจ็บปวดจากพฤติกรรมได้เอง ซึ่งเป็นหลักเดียวกับการกำกับนักแสดงแบบใช้ Subtext (ความหมายใต้บท) มากกว่าการบอกให้นักแสดง “ทำหน้าเศร้า”

### Syntax ระดับคำที่ควรใช้

โครงสร้างที่โมเดลตีความง่ายที่สุดคือ:

```text
BODY PART + PHYSICAL VERB + AMPLITUDE + TARGET + TIMING
```

เช่น:

| คำที่กว้างเกินไป | คำแบบ Kinematic |
|---|---|
| looks nervous | `her thumb rubs once against the edge of her index finger` |
| looks sad | `her lips press together slightly; her gaze drops to the tabletop` |
| looks suspicious | `her eyes shift once toward the doorway while her head remains still` |
| looks afraid | `her breath catches briefly; her fingers tighten around the cup` |
| looks conflicted | `her gaze moves from the photograph to the door, then settles between them` |
| tries not to cry | `her lower eyelids tighten; she blinks once and swallows before exhaling` |
| smiles sadly | `one corner of her mouth lifts slightly, then stops before becoming a full smile` |
| shocked | `her inhale stops halfway; her eyes remain fixed as her jaw loosens slightly` |

สิ่งสำคัญคือใช้ Verbs (คำกริยา) ที่กล้องมองเห็นได้ เช่น:

`presses, tightens, releases, shifts, settles, swallows, blinks, pauses, exhales, inhales, curls, loosens, drops, lifts, tracks, holds`

แทน Adjectives (คำคุณศัพท์) เช่น:

`deeply sad, extremely nervous, intensely emotional, utterly devastated`

### Micro-acting ไม่เหมือน Micro-expression เสมอไป

ในทางวิทยาศาสตร์ Micro-expression (สีหน้าเสี้ยววินาที) หมายถึงการเปลี่ยนแปลงสีหน้าที่สั้นมาก ความเข้มต่ำ และมักเกิดโดยไม่ตั้งใจ งานวิจัยด้าน Electromyography หรือ EMG (การวัดไฟฟ้ากล้ามเนื้อ) รายงานตัวอย่าง Micro-expression ที่มีช่วงเวลาราว 307–327 มิลลิวินาที และวรรณกรรมโดยทั่วไปมักจัดให้อยู่ต่ำกว่าประมาณ 500 มิลลิวินาทีค่ะ citeturn14view6

แต่สำหรับการสร้างหนัง ฉันแนะนำให้ใช้คำว่า **Micro-acting (การแสดงละเอียดระดับเล็ก)** ในความหมายกว้างกว่า ได้แก่:

> การเปลี่ยนแปลงที่มี Amplitude ต่ำ (ขยับน้อย) + จำกัดพื้นที่ + เกิดเป็นลำดับ + มีช่วงนิ่งคั่น

ไม่ควรสั่งโมเดลว่า `"a 180 ms AU4 micro-expression"` เพราะความแม่นยำระดับเฟรมยังไม่ใช่สิ่งที่พรอมป์ต์ข้อความรับประกันได้ โมเดลกำเนิดมีลักษณะ Non-deterministic (ผลลัพธ์ไม่ตายตัว) และการ Prompt Engineering (วิศวกรรมพรอมป์ต์) เป็นการเพิ่มความน่าจะเป็นของผลที่ต้องการ ไม่ใช่คำสั่งบังคับแบบ Animation Rig (โครงควบคุมแอนิเมชัน) ค่ะ citeturn20view0

## สูตรพรอมป์ต์สำหรับช็อตการแสดงห้าวินาที

โครงสร้างที่ผู้ใช้ระบุมานั้นดีมากค่ะ แต่ฉันแนะนำให้ปรับจาก “คำบรรยายตัวละคร” ไปเป็น **Performance Hierarchy (ลำดับความสำคัญของการแสดง)** ดังนี้:

```text
[CHARACTER ARCHETYPE]
+
[MOMENT BEFORE / SPATIAL STATE]
+
[PRIMARY PHYSICAL ACTION + EYELINE]
+
[MICRO-EXPRESSION + RESPIRATORY CUE]
+
[RECOVERY / HOLD]
+
[CAMERA + LIGHT INTERACTION]
```

### Master Template สำหรับ Text-to-Video

```text
[Character archetype], already [visible physical precondition] at/in [spatial anchor].

[Primary physical action in one or two beats].
Their eyes remain [precise eyeline target], then [one deliberate eyeline change].

On [small inhale / held breath / slow exhale], [one or two localized facial movements].
They [swallow / blink once / release the jaw / settle the shoulders], then become still again.

[shot size], [lens or depth of field], [locked camera OR one slow camera movement].
[specific light source] catches/reveals [specific physical detail on face or eyes].

Real-time movement, restrained natural performance, low-amplitude facial motion.
```

คำว่า `restrained natural performance` (การแสดงเป็นธรรมชาติแบบยับยั้งอารมณ์) ควรอยู่ **ท้ายพรอมป์ต์ในฐานะ Style Modifier (ตัวขยายสไตล์)** ไม่ใช่ใช้แทนรายละเอียดการเคลื่อนไหวค่ะ

### Master Template สำหรับ Image-to-Video

Image-to-Video หรือ I2V (ภาพเป็นวิดีโอ) ควรสั้นกว่ามาก เพราะภาพเริ่มต้นได้กำหนดใบหน้า เสื้อผ้า แสง องค์ประกอบ และ Pose (ท่าทาง) ไว้แล้ว Runway ระบุชัดว่าการบรรยายข้อมูลที่มีอยู่ในภาพซ้ำมากเกินไปอาจลดการเคลื่อนไหวหรือให้ผลที่ไม่คาดคิด ขณะที่ Sora อธิบายภาพอ้างอิงว่าเป็น First-frame Anchor (จุดยึดเฟรมแรก) แล้วให้ข้อความอธิบายว่าเกิดอะไรต่อจากนั้นค่ะ citeturn14view0turn15view1

ใช้:

```text
The subject [single physical action].
Eyes remain [target], then [one eyeline shift].
On [breath cue], [1–2 facial changes], followed by [release / stillness].
[one camera instruction].
[light interaction].
```

ตัวอย่าง:

```text
She keeps the letter still between her hands.
Her eyes remain on the handwriting, then shift briefly toward the doorway.
A small inhale stops halfway; her lips press together and her jaw tightens slightly.
She swallows once, exhales through her nose, and becomes still again.
Locked close-up with a barely perceptible push-in during the final beat.
Soft window light catches moisture along the lower eyelid.
```

### จังหวะห้าวินาทีที่เหมาะกับ Micro-acting

นี่เป็น Production Heuristic (กฎใช้งานเชิงการผลิต) ที่ฉันแนะนำ ไม่ใช่ข้อกำหนดของโมเดล:

| เวลาโดยประมาณ | หน้าที่ |
|---|---|
| 0–1.5 วินาที | Establish (ตั้งสถานะ): ความนิ่ง + Eyeline |
| 1.5–3.5 วินาที | Leak (อารมณ์รั่ว): Action เล็ก + Face + Breath |
| 3.5–5 วินาที | Recovery (เก็บอารมณ์): คลาย + เปลี่ยนสายตา + Stillness |

กล่าวคือ:

```text
HOLD → LEAK → RECOVER
```

ให้ความรู้สึกเหมือน “คนพยายามไม่แสดงอารมณ์” มากกว่า:

```text
NEUTRAL → BIG EXPRESSION → BIGGER EXPRESSION
```

แนวทางนี้สอดคล้องกับคำแนะนำของ Sora ที่ให้แบ่ง Action เป็น Beat หรือ Count และหลีกเลี่ยงการยัดหลายการกระทำลงในช็อตเดียว รวมถึงแนวทาง Hailuo ที่แนะนำให้ลดจำนวน Subject ที่เคลื่อนไหว และใช้คลิปสั้นเพื่อรักษาความสม่ำเสมอค่ะ citeturn15view0turn19view2

## การปรับ Syntax ตามแต่ละโมเดล

แม้หลัก `physical state → action → eyeline → micro-cue → breath → settle` ใช้ข้ามโมเดลได้ แต่ Syntax ระดับแพลตฟอร์มไม่ควรเหมือนกันทั้งหมดค่ะ

| โมเดล | วิธี Prompt Acting ที่เหมาะ | สิ่งที่ควรระวัง |
|---|---|---|
| **Runway Gen-3 Alpha** | Positive physical descriptions (คำบรรยายสิ่งที่ต้องการให้เกิด), I2V เน้น Motion | Gen-3 Alpha ถูกยุติแล้วเมื่อ 8 ก.ค. 2026 ดังนั้นกฎที่แนะนำนี้อิงแนวทาง Runway รุ่นปัจจุบันที่ถ่ายโอนกลับไปใช้กับ Workflow เก่า ไม่ใช่การอ้างว่าเป็น Syntax เฉพาะ Gen-3 ค่ะ citeturn16search0turn14view0 |
| **Kling 1.5** | `Subject + Movement + Scene + Camera/Light` และแยก Face cues 1–2 จุด | Kling 1.5 เปิดตัว ก.ย. 2024 พร้อม 1080p และ Motion Brush; คู่มือ Kling ปัจจุบันยังใช้สูตร Subject → Movement → Scene และกำหนด Movement ให้เหมาะกับคลิป 5 วินาทีค่ะ citeturn18view2turn15view3 |
| **OpenAI Sora / Sora 2** | Storyboard-like blocks (บล็อกแบบสตอรีบอร์ด), Action เป็น Beats, 1 subject action + 1 camera move | Sora web/app ยุติ 26 เม.ย. 2026 และ API มีกำหนดยุติ 24 ก.ย. 2026; คู่มือ API ปัจจุบันยังใช้เป็น reference ด้าน prompting ได้ค่ะ citeturn16search1turn15view2 |
| **MiniMax/Hailuo** | Physical prompt + explicit `[Camera command]` | Hailuo รองรับคำสั่งเช่น `[Static shot]`, `[Push in]`, `[Pan left]`; explicit command แม่นกว่าภาษาธรรมชาติ และแนะนำไม่เกิน 3 camera commands ที่ทำพร้อมกันค่ะ citeturn15view4 |
| **Luma Dream Machine** | Natural-language acting + explicit camera language | Luma ควบคุมกล้องด้วยภาษา เช่น `camera orbit left`; คำที่โครงสร้างใกล้เคียงอาจใช้ได้ แต่เอกสารระบุว่ายังเกิด mismatch ได้ค่ะ citeturn15view6 |

### Runway

Runway ให้ความสำคัญกับ **Positive Prompting (การบอกสิ่งที่ต้องการให้เกิด)** อย่างชัดเจน:

```text
Bad:
No camera movement. She doesn't smile. Don't exaggerate.

Better:
Locked camera.
Her mouth remains neutral.
Only the lower eyelids tighten slightly.
Her head and shoulders remain nearly still.
```

เอกสาร Runway ระบุว่า Negative Phrasing (ประโยคปฏิเสธ) อาจสร้างผลตรงข้ามหรือไม่คาดคิด และสำหรับ I2V ให้ใช้ Text Prompt (พรอมป์ต์ข้อความ) เพื่อบรรยาย Motion มากกว่าบรรยายภาพซ้ำค่ะ citeturn14view0

### Kling

สูตรทางการของ Kling ปัจจุบันคือ:

```text
Subject (Subject Description)
+ Subject Movement
+ Scene (Scene Description)
+ Camera Language
+ Lighting
+ Atmosphere
```

โดย Kling ระบุว่า Subject Movement ควรตรงไปตรงมาและเหมาะกับสิ่งที่เกิดได้ภายในประมาณ 5 วินาทีค่ะ citeturn15view3

สำหรับ Micro-acting ให้เขียน:

```text
A tired homicide detective sits motionless behind the desk.
He rolls the matchbook once between his fingers and stops.
His gaze remains on the unseen witness.
His lower eyelids tighten slightly as he exhales through his nose.
Tight telephoto close-up, shallow depth of field, slow push-in.
```

แทน:

```text
A deeply suspicious and emotionally conflicted detective stares intensely.
```

Kling รุ่นปัจจุบันยังมี Motion Control (การควบคุมการเคลื่อนไหว) จาก Reference Video (วิดีโออ้างอิง) เพื่อควบคุมการเคลื่อนไหวและสีหน้า หากต้องการ Performance Fidelity (ความแม่นของการแสดง) สูงกว่าที่ข้อความเพียงอย่างเดียวให้ได้ ซึ่งเป็นแนวทางที่ควรพิจารณาสำหรับงานระดับ Production ค่ะ citeturn18view3

### Sora

สำหรับ Sora หลักที่สำคัญมากคือ:

```text
ONE SUBJECT ACTION
+
ONE CAMERA MOVE
+
ACTION IN BEATS
```

OpenAI ยกตัวอย่างความแตกต่างระหว่างคำกว้าง เช่นนักแสดง “เดินข้ามห้อง” กับคำที่กำหนดจำนวนก้าว จุดหยุด และการกระทำในช่วงท้าย เพราะแบบหลังผูก Motion เข้ากับ Time (เวลา) ได้ดีกว่าค่ะ citeturn15view0

ดังนั้น Micro-acting สามารถใช้ลักษณะนี้:

```text
Action beats:
- She keeps her eyes on the photograph for the first beat.
- She takes one small breath; her lips press together.
- She blinks once and lowers the photograph during the final beat.

Camera:
Locked medium close-up, very slow push-in only near the end.
```

Sora ยังระบุว่า Shorter Clips (คลิปสั้นกว่า) มักทำตามคำสั่งได้เชื่อถือได้มากกว่า และแนะนำการตัดต่อคลิปสั้นหลายคลิปแทนการบังคับการกระทำจำนวนมากไว้ในคลิปเดียวค่ะ citeturn15view2

### MiniMax / Hailuo

หากต้องการให้กล้องนิ่งเพื่ออ่านใบหน้า:

```text
[Static shot]
```

ถ้าต้องการ Push-in (ดอลลี่เข้าหาตัวละคร):

```text
[Push in]
```

MiniMax ระบุว่าคำสั่งในวงเล็บเหลี่ยมเป็น Explicit Camera Command (คำสั่งกล้องแบบชัดเจน) และให้การควบคุมแม่นกว่าภาษาธรรมชาติ นอกจากนี้ใน API สามารถตั้ง `prompt_optimizer=false` เพื่อรักษาถ้อยคำเดิมให้ตรงขึ้นแทนการปล่อยระบบ Rewrite Prompt (เขียนพรอมป์ต์ใหม่อัตโนมัติ) ค่ะ citeturn15view4turn15view5

เช่น:

```text
[Push in]
She keeps her head still while her eyes shift toward the doorway.
Her breath catches briefly.
Her thumb stops moving against the edge of the glass.
She swallows once and slowly releases her jaw.
```

หมายเหตุ: Hailuo 2.3 ปัจจุบันรองรับ 1080p ที่ 6 วินาที ไม่ใช่ 5 วินาที ดังนั้น “สูตรห้าวินาที” ในรายงานนี้ควรมองเป็น Acting Design (โครงจังหวะการแสดง) และวางไว้ภายในคลิป 6 วินาทีเมื่อใช้โมเดลนี้ค่ะ citeturn15view5

### Luma Dream Machine

Luma เหมาะกับการแยก Character Motion (การเคลื่อนไหวตัวละคร) ออกจาก Camera Motion (การเคลื่อนกล้อง):

```text
She keeps her shoulders still.
Her eyes move from the window to the man beside the camera.
She takes a shallow inhale, presses her lips together, then releases her jaw.
camera slowly pushes in
```

Luma ระบุว่ากล้องสามารถควบคุมด้วยภาษาตรง ๆ และยังรองรับ Keyframe (เฟรมหลัก) สำหรับยึดต้นหรือปลายของการเคลื่อนไหวด้วยค่ะ citeturn14view4turn15view6

## Negative Prompting และ Anti-Patterns สำหรับ Acting

สิ่งสำคัญคือ **Negative Prompt (พรอมป์ต์ปฏิเสธ) กับ Anti-pattern (รูปแบบที่ควรหลีกเลี่ยง) ไม่ใช่สิ่งเดียวกัน** ค่ะ

สำหรับ Runway ไม่ควรใส่ Negative Prompt เลย เพราะเอกสารระบุว่าระบบออกแบบมาให้ตีความสิ่งที่ “ควรเกิด” ไม่ใช่สิ่งที่ “ห้ามเกิด” ดังนั้นรายการด้านล่างควรใช้เป็น **Rewrite Checklist (รายการตรวจเพื่อเขียนใหม่)** มากกว่านำคำว่า `no / don't / avoid` ไปแปะท้ายพรอมป์ต์ค่ะ citeturn14view0

| Anti-pattern | ปัญหา | Corrective Cue ที่ดีกว่า |
|---|---|---|
| `very heartbroken` | เป็นอารมณ์นามธรรม | `lips press together; gaze drops; one swallow; slow nasal exhale` |
| `extremely nervous` | ไม่มีการเคลื่อนไหวเฉพาะ | `thumb rubs index fingertip once, then becomes still` |
| `intense emotional stare` | มักทำให้ตาแข็ง/หน้าแข็ง | `eyes remain on off-camera left; one natural blink; gaze settles` |
| `dramatic crying` | ขยายอารมณ์เร็วเกินไป | `lower eyelids tighten; moisture gathers; jaw releases after a held breath` |
| `sad smile` | Mapping กว้าง | `one corner of the mouth rises slightly, then stops; cheeks remain relaxed` |
| `shaking with fear` | Motion ใหญ่เกินสำหรับ Close-up | `fingers tighten around the cup; inhale stops briefly` |
| `nervous fidgeting` | ไม่กำหนดส่วนร่างกาย | `thumb traces the cup rim once; the hand then rests` |
| `robotic gaze` | เป็นคำอธิบายข้อเสีย ไม่ใช่ Motion | `eyes track the off-screen speaker, blink once, then settle` |
| `don't overact` | Negative command | `brows remain nearly still; emotion is carried by eyelids, jaw and breath` |
| `no smile` | Negative command | `mouth stays neutral; lips remain softly closed` |
| `don't move the camera` | Negative command | `locked camera; fixed frame` |
| `Oscar-worthy performance` | ไม่มีข้อมูล Kinematic | กำหนด Eyes + Mouth/Jaw + Breath + Pause + Recovery โดยตรง |

### คำที่ควรระวังเป็นพิเศษ

คำเหล่านี้ใช้ได้เป็น Mood Modifier (ตัวกำหนดบรรยากาศ) แต่ไม่ควรเป็นแกนของ Performance Prompt:

```text
emotional
powerful
heartbroken
devastated
terrified
haunted
conflicted
deeply sad
intensely nervous
dramatic
passionate
Oscar-worthy
award-winning acting
```

ให้นำไว้ **หลัง** Physical Description เช่น:

```text
Her jaw tightens once as she holds a shallow breath.
She looks toward the empty chair, then drops her gaze.
Her shoulders settle on the exhale.
Restrained, conflicted performance.
```

ไม่ใช่:

```text
She gives a deeply emotional, Oscar-worthy, conflicted and heartbreaking performance.
```

### หลีกเลี่ยง “Micro-expression Soup”

ตัวอย่างที่ละเอียดเกินไป:

```text
Her inner eyebrows raise, outer eyebrows lower, lower eyelids tighten,
upper eyelids narrow, nostrils flare, lips purse, jaw clenches,
chin trembles, left cheek twitches, she swallows, blinks twice,
breathes sharply and turns her head.
```

แม้ทุกอย่างเป็น Physical Cue (สัญญาณทางกาย) แต่เกิดพร้อมกันมากเกินไป จึงไม่ได้ช่วยควบคุมโมเดล

Kling แนะนำให้เน้น Facial Feature เพียงหนึ่งถึงสองจุด และ Runway/Sora ต่างแนะนำให้ลดความซับซ้อนของ Motion ในช็อตสั้นค่ะ citeturn19view1turn14view0turn15view0

แก้เป็น:

```text
Her lower eyelids tighten slightly while her lips press together.
She holds one shallow breath, then releases her jaw on the exhale.
```

นี่คือ **Micro-acting Hierarchy (ลำดับการแสดงแบบละเอียด)** ที่ฉันแนะนำ:

```text
EYELINE
   ↓
ONE FACIAL REGION
   ↓
ONE BREATH RESPONSE
   ↓
ONE SMALL BODY RESPONSE
   ↓
STILLNESS
```

ไม่ใช่การเคลื่อนทุกส่วนพร้อมกัน

## คลังคำสำหรับเขียนการแสดงแบบละเอียด

### Eyeline หรือทิศทางสายตา

แทน `"looks emotional"` ใช้ตำแหน่งจริง:

```text
eyes fixed just off-camera right
gaze held at standing eye level
eyes remain on the photograph
gaze drops to the ring in her palm
eyes shift once toward the doorway
eyes track the unseen speaker without turning the head
gaze breaks contact for a moment, then returns
eyes remain forward while the head turns slightly
```

Eyeline ควรตอบได้ว่า **“กำลังมองอะไร อยู่ทิศไหน และมีการเปลี่ยนเป้าหมายหรือไม่”**

### Mouth / Jaw หรือปากและกราม

```text
lips press together slightly
the lips part on the inhale
one corner of the mouth rises, then stops
the jaw tightens once
the jaw slowly releases
the mouth remains neutral
the lower lip tenses briefly
she swallows once before speaking
```

### Eyes / Brows หรือดวงตาและคิ้ว

```text
lower eyelids tighten slightly
inner brows lift only a trace
a faint furrow appears between the brows
one natural blink
a blink held fractionally longer
eyes narrow slightly while the brows remain relaxed
moisture catches along the lower eyelid
```

ควรระวังการจับคู่ Facial Cue กับ Emotion แบบตายตัว เพราะงานด้าน Emotion Science (วิทยาศาสตร์อารมณ์) พบว่าความหมายของการเคลื่อนไหวใบหน้าขึ้นกับบริบทและบุคคลอย่างมากค่ะ citeturn17search0

### Breath / Physiology หรือการหายใจและสรีรวิทยา

```text
a small nasal inhale
a shallow breath in
the inhale stops halfway
a brief breath hitch
holds the breath for one beat
slow exhale through the nose
quiet exhale through slightly parted lips
shoulders settle subtly on the exhale
```

Breath Cue (สัญญาณจากลมหายใจ) มีประโยชน์มากเพราะเชื่อมหน้า คอ ไหล่ ทรวงอก และ Timing เข้าด้วยกันโดยไม่ต้องสร้าง Gesture (ท่าทาง) ใหญ่ และงานด้านสรีรวิทยาก็พบความสัมพันธ์ระหว่างรูปแบบการหายใจกับภาวะอารมณ์ แม้จะไม่ควรตีความว่า Breath Pattern หนึ่งรูปแบบเท่ากับอารมณ์หนึ่งชนิดเสมอไปค่ะ citeturn17search1turn17search3

### Hands / Body หรือมือและร่างกาย

```text
thumb rubs once against the index finger
fingers tighten slightly around the glass
the grip loosens on the exhale
the hand stops halfway toward the object
one finger curls against the table edge
shoulders remain almost still
shoulders settle slightly
chin dips a fraction
the head remains still while the eyes move
weight shifts subtly onto the back foot
```

### Stillness หรือความนิ่ง

นี่เป็นคำศัพท์ที่สำคัญมากสำหรับการแสดงแบบภาพยนตร์:

```text
remains still
holds the position
movement settles
becomes still again
head remains almost motionless
shoulders remain quiet
hands stay resting on the table
holds the gaze for the final beat
```

Generative Video (วิดีโอเชิงกำเนิด) มีแนวโน้มสร้าง Motion เพราะถูกขอให้ “สร้างวิดีโอ” ดังนั้นความนิ่งควรถูกเขียนเป็น **Positive State (สถานะเชิงบวก)** ไม่ใช่ปล่อยให้โมเดลเดาเอง โดยเฉพาะ Runway ซึ่งแนะนำคำอย่าง `"locked camera"` แทน `"no camera movement"` โดยตรงค่ะ citeturn14view0

## Benchmark Prompts สำหรับ Micro-acting ระดับภาพยนตร์

พรอมป์ต์สิบตัวอย่างต่อไปนี้เป็น **Model-neutral Benchmark (ชุดทดสอบกลางระหว่างโมเดล)** ที่ฉันออกแบบจากหลักข้างต้น ไม่ได้พยายามเลียนแบบนักแสดงจริงคนใดค่ะ

ควรใช้ Prompt เดิมกับหลาย Seed (ค่าตั้งต้นสุ่ม) และรักษา Input Image เดิมเมื่อทดสอบ I2V เพื่อเปรียบเทียบ Motion Fidelity (ความตรงของการเคลื่อนไหว) ระหว่างโมเดลอย่างยุติธรรม เนื่องจากผลลัพธ์ของระบบกำเนิดไม่ตายตัวค่ะ citeturn20view0

**Noir — The Alibi**

```text
A worn night-shift detective sits behind a scarred wooden desk, already holding a closed matchbook between two fingers, his body angled toward an unseen witness off-camera right.

He rolls the matchbook once between his fingers and stops. His eyes remain on the witness. On a small nasal inhale, his lower eyelids tighten slightly and his jaw shifts once before slowly releasing. He blinks once, then lowers his gaze to the matchbook during the final beat.

Tight 85 mm close-up, shallow depth of field, locked composition with a barely perceptible push-in near the end. Venetian-blind side light crosses one eye while faint cigarette haze moves softly in the background. Restrained, natural real-time performance.
```

จุดทดสอบ: Eyeline stability, finger motion, jaw release และการรักษาความนิ่งของศีรษะ

**Noir — The Unsaid Name**

```text
An elegant nightclub singer stands beside a rain-streaked window after the room has gone quiet, one hand resting lightly on the window frame.

She hears someone off-camera left. Her eyes move toward the voice before her head follows, then stop. One corner of her mouth begins to lift but never becomes a full smile. She holds a shallow breath for one beat, swallows once, and slowly lets the expression disappear while keeping eye contact.

Medium close-up, long-lens compression, very slow push-in. A narrow tungsten rim outlines her cheek while moving streetlight reflections slide softly across the glass behind her. Low-amplitude facial movement, controlled stillness.
```

จุดทดสอบ: การเกิด “เกือบยิ้มแต่หยุด” ซึ่งยากกว่าการสั่ง smile ธรรมดา

**Psychological Thriller — The Therapist**

```text
A composed middle-aged therapist sits opposite an unseen patient, pen resting against an open notebook, already listening in silence.

The pen stops moving. Her eyes stay on the patient for one beat, then shift briefly toward the closed office door without turning her head. A quiet inhale stops halfway; her lips press together and the muscles along her jaw tighten slightly. She returns her eyes to the patient, exhales slowly through her nose, and relaxes the pen between her fingers.

Locked 75 mm medium close-up, shallow depth of field. Soft daylight from one side catches a faint highlight in her eyes; the background remains still and subdued. Restrained internal tension.
```

จุดทดสอบ: Eye-only movement (ขยับเฉพาะตา), Hand stop และ Breath synchronization (การประสานกับลมหายใจ)

**Psychological Thriller — The Reflection**

```text
A sleepless woman stands at a bathroom mirror in the middle of the night, both hands resting on the sink, staring at her own reflection.

A faint sound comes from the hallway behind her. Her eyes shift toward the doorway only through the mirror while her head remains still. Her fingers tighten slightly against the sink edge. One small breath catches in her throat; her lower eyelids tighten, then she slowly releases her grip and looks back at her own reflection.

Locked over-the-shoulder close-up with the reflected face in sharp focus. Cold overhead light remains steady while a thin strip of warm hallway light touches one side of the reflected cheek. Minimal movement, sustained tension.
```

จุดทดสอบ: Reflection consistency (ความต่อเนื่องของภาพสะท้อน) + Gaze geometry (เรขาคณิตของสายตา)

**Period Drama — The Telegram**

```text
A restrained Victorian widow sits at a breakfast table, already holding a folded telegram just above the untouched tea beside her.

She reads the final line without moving her head. Her thumb stops against the paper. The inner brows lift only a trace while her lips remain closed. She takes a small inhale, swallows once, then folds the telegram carefully along its existing crease. Her gaze remains on the paper as her shoulders settle subtly on the exhale.

Static 85 mm close-up, shallow depth of field. Soft north-window light falls across her face and catches moisture along the lower eyelid; warm candle fill remains faint in the background. No theatrical gesture, controlled period-drama restraint.
```

จุดทดสอบ: Prop handling (การจัดการอุปกรณ์), subtle brow motion และการไม่กลายเป็นการร้องไห้ใหญ่

**Period Drama — The Officer's Letter**

```text
A young nineteenth-century naval officer stands alone beside a cabin desk at dawn, an opened letter held loosely at chest height.

His eyes remain on one line of the letter. His grip tightens slightly, then stops. He draws a shallow breath through his nose; the jaw sets for one beat while the rest of his face stays nearly neutral. He lowers the letter a few centimeters, looks toward the small window without turning his shoulders, and releases the breath slowly.

Medium close-up on a 70 mm lens, locked camera. Cool dawn light from the porthole crosses one eye while a weak amber lamp leaves the opposite cheek in soft shadow. Quiet, restrained conflict.
```

จุดทดสอบ: Internal conflict (ความขัดแย้งภายใน) โดยไม่ใช้คำว่า crying, devastated หรือ furious

**Cyberpunk — The Memory Dealer**

```text
A tired underground memory dealer sits beneath a malfunctioning holographic display, one hand resting beside a small data cartridge.

A familiar face appears in the hologram. Her fingers move toward the cartridge, then stop before touching it. Her gaze remains fixed on the projection. She takes one shallow inhale; her lower eyelids tighten and her jaw loosens instead of forming a smile. After one slow blink, she withdraws her hand and exhales quietly.

Tight 85 mm close-up with a nearly locked camera and a subtle push-in during the final beat. Cyan and magenta holographic reflections drift across her irises and cheek while the background signage flickers independently. Human performance remains quiet beneath the technology.
```

จุดทดสอบ: แยก Environmental Motion (การเคลื่อนฉาก) ออกจาก Facial Motion โดยไม่ทำให้ตัวละครขยับตามแสงทั้งหมด

**Cyberpunk — The Checkpoint**

```text
A young synthetic-human courier stands at a security checkpoint, already holding an identity card beneath a scanner, face outwardly calm.

The scanner speaks a name she does not expect. Her thumb stops moving along the card edge. Her eyes shift from the scanner light to the guard off-camera right while her head remains nearly motionless. A small inhale pauses in her chest; one corner of the mouth tightens, then releases. She blinks once and returns her gaze to the scanner.

Medium close-up, 65 mm lens, shallow depth of field, locked camera. A red scanning line passes once across her face while cool overhead light stays constant. Controlled, nearly unreadable tension.
```

จุดทดสอบ: ตัวละครต้อง “เก็บอาการ” แทนการแสดงตกใจ ซึ่งเป็น Benchmark ที่ดีสำหรับ Subtle Acting

**Sci-Fi — The Lost Transmission**

```text
A solitary astronaut sits inside a quiet spacecraft cockpit, gloved hand hovering above the communications control after a dead channel suddenly carries a familiar voice.

The hand freezes before touching the switch. Her eyes remain on the waveform display, then shift slightly toward the empty seat beside her. A shallow inhale catches; her lower eyelids tighten and her lips part only slightly. She swallows once, closes her mouth, and slowly lowers the hand without answering.

Tight helmet-level close-up, locked 85 mm framing. Cool instrument light reflects softly across the visor and catches a thin highlight along the lower eyelid; distant stars remain steady outside. Intimate, restrained real-time performance.
```

จุดทดสอบ: Face/visor consistency, suspended hand motion และ “decision not taken” ซึ่งมักสร้าง Subtext ได้ดีกว่าการกระทำใหญ่

**Sci-Fi — The Shutdown**

```text
A veteran mission commander stands alone before an emergency shutdown console, one finger already resting just above the final control.

She reads the system message in silence. Her finger begins to descend, then stops a few millimeters above the control. Her eyes shift once from the warning text to a small status light. She holds one breath; her lips press together while the jaw remains still. On the slow exhale, her finger curls back toward the palm and the shoulders settle slightly.

Medium close-up, 75 mm lens, very slow push-in through the final two seconds. Cold monitor light defines the eyes while a dim warm practical behind her separates the silhouette. Minimal movement, moral conflict expressed entirely through hesitation.
```

จุดทดสอบ: Hésitation (ความลังเล) และการทำ Action แบบ incomplete trajectory (การเคลื่อนไหวที่เริ่มแต่ไม่ทำจนจบ)

พรอมป์ต์ทั้งหมดใช้หลักเดียวกับคำแนะนำอย่างเป็นทางการของ Sora ที่ให้ Action เป็น Beat ชัดเจน, Kling ที่ให้ Subject Movement ตรงและเหมาะกับช่วงเวลาสั้น, Runway ที่เน้น Physical Motion มากกว่า Concept และ Hailuo ที่ลดความซับซ้อนของผู้เคลื่อนไหวค่ะ citeturn15view0turn15view3turn14view0turn19view3

## วิธีใช้ชุดนี้เป็น Benchmark จริง

คำว่า “Oscar-level” ควรมองเป็น **Creative Quality Target (เป้าหมายคุณภาพเชิงศิลป์)** ไม่ใช่ Prompt Keyword เพราะไม่มีโมเดลใดรับประกันว่าคำนี้จะสร้างการแสดงระดับรางวัลได้ สิ่งที่วัดได้ดีกว่าคือ Performance Controllability (ความสามารถในการควบคุมการแสดง) ค่ะ

ฉันแนะนำให้ให้คะแนนแต่ละ Generation ตามหกแกนนี้:

| เกณฑ์ | สิ่งที่ตรวจ |
|---|---|
| **Eyeline Fidelity** (ความตรงของสายตา) | มองเป้าหมายตามคำสั่งหรือไม่, ตากับศีรษะแยกกันได้หรือไม่ |
| **Micro-motion Fidelity** (ความตรงของการเคลื่อนไหวเล็ก) | lips/jaw/eyelid/finger เกิดตามที่ระบุหรือไม่ |
| **Temporal Ordering** (ลำดับเวลา) | inhale → expression → swallow → release เกิดตามลำดับหรือไม่ |
| **Stillness Quality** (คุณภาพความนิ่ง) | ส่วนที่ไม่ได้รับคำสั่งยังนิ่งหรือสร้าง motion เพิ่มเอง |
| **Identity Stability** (ความคงที่ของใบหน้า) | รูปหน้า ดวงตา ปาก และสัดส่วนเปลี่ยนระหว่าง expression หรือไม่ |
| **Subtext Readability** (การอ่านความหมายใต้บท) | ผู้ชมอ่าน tension/grief/conflict ได้โดยไม่ต้องมี gesture ใหญ่หรือไม่ |

สำหรับการทดสอบแบบ A/B ควรใช้ Prompt เดิม, Input Image เดิม, Aspect Ratio (อัตราส่วนภาพ) เดิม และสร้างหลาย Generation ต่อโมเดล เพราะผลของระบบกำเนิดเป็นแบบ Non-deterministic ค่ะ citeturn20view0

**Production Rule ที่สรุปงานวิจัยทั้งหมดได้ดีที่สุดคือ:**

```text
Do not prompt the emotion.
Prompt the evidence of the emotion.

Do not prompt “she is heartbroken.”
Prompt what the camera can witness:

where her eyes are,
what stops moving,
what moves only slightly,
what happens to the breath,
what she almost does but decides not to do,
and when she becomes still again.
```

สำหรับการแสดงละเอียด การ “เกือบทำ” มักมีประโยชน์กว่า Action ใหญ่ เช่น `almost smiles`, `hand begins to reach, then stops`, `looks toward the door but does not turn`, `inhale catches before speech` เพราะมันสร้าง **Suppression + Hesitation + Recovery (การกดอารมณ์ + ความลังเล + การกลับมาควบคุมตัวเอง)** ซึ่งทำให้ผู้ชมเป็นฝ่ายตีความ Subtext แทนที่โมเดลจะสาธิตอารมณ์แบบตรง ๆ ค่ะ แนวทางนี้ยังสอดคล้องกับข้อจำกัดเชิงปฏิบัติของโมเดลปัจจุบันที่ทำงานได้ควบคุมง่ายขึ้นเมื่อ Motion ถูกลดให้เหลือการกระทำหลักที่ชัดเจนและเรียงเป็น Beat ค่ะ citeturn15view0turn14view0turn19view1