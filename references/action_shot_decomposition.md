# วิศวกรรม Prompt ฉากแอ็กชันและฉากต่อสู้สำหรับ Generative Video: แก้ข้อจำกัด Multi‑Body Collision

## ภาพรวมเชิงเทคนิคและสถานะโมเดลปัจจุบัน

ปัญหาหลักของฉากต่อสู้ใน Generative Video ไม่ได้อยู่ที่โมเดล “ไม่เข้าใจว่าการชกคืออะไร” แต่เกิดจากการต้องรักษา **ตัวตน + โครงกระดูก + การบังกันของวัตถุ + จุดสัมผัส + โมเมนตัม + กล้อง** พร้อมกันในช่วงไม่กี่เฟรม โดยเฉพาะตอนร่างกายสองคนแตะหรือซ้อนกันค่ะ งานวิจัยด้าน Physical Video Generation (การสร้างวิดีโอที่สอดคล้องกับฟิสิกส์) ยังคงรายงานปัญหากับ gravity (แรงโน้มถ่วง), inertia (ความเฉื่อย) และ collision (การชน) ขณะที่ VBench‑2.0 และ HumanScore พบข้อผิดพลาดด้าน anatomy (กายวิภาค), temporal stability (เสถียรภาพตามเวลา) และ biomechanical consistency (ความสมเหตุผลทางชีวกลศาสตร์) แม้ว่าคุณภาพภาพจะดูสมจริงขึ้นมากแล้วก็ตาม citeturn19search0turn19search2turn19search14turn19academia40

ดังนั้นหลักสำคัญที่สุดสำหรับงานระดับ Production คือ:

> **อย่าให้โมเดล “คิดการชน” ให้เรา ให้โมเดลสร้างการเคลื่อนที่ก่อนชนและผลหลังชน แล้วใช้การตัดต่อขายจังหวะกระแทกค่ะ**

นี่เป็นข้อสรุปเชิงวิศวกรรมจากข้อจำกัดที่งาน benchmark พบ โดยเฉพาะ Human‑Human Interaction (ปฏิสัมพันธ์ระหว่างมนุษย์), collision และ motion dynamics (พลวัตการเคลื่อนไหว) ซึ่งยังเป็นงานที่ยากกว่าการเคลื่อนไหวของคนเดียวอย่างชัดเจน citeturn19search7turn19search3turn19search6

มีเรื่องสถานะผลิตภัณฑ์ที่ควรทราบก่อนใช้สูตรด้านล่างค่ะ ณ **19 สิงหาคม 2026** Runway Gen‑3 Alpha ถูกยุติเมื่อ 8 กรกฎาคม 2026 และ Gen‑3 Alpha Turbo เมื่อ 30 กรกฎาคม 2026 โดย Runway แนะนำ Gen‑4.5 สำหรับ T2V/I2V และ Animate Frames สำหรับงาน keyframe; Act‑One บน Gen‑3 ก็ไม่ให้ใช้งานแล้วเช่นกัน citeturn14search0turn21search8 Kling ปัจจุบันเดินมาถึง VIDEO 3.0 แต่โครงสร้าง Prompt และ Motion Brush ที่ Kling เผยแพร่ยังนำกลับมาใช้กับแนว workflow ของ 1.5/2.x ได้ค่ะ citeturn17search0turn17search1turn15search0

Seedance 2.5 เปิดตัวเมื่อ **31 กรกฎาคม 2026** และกำลัง rollout บน Jimeng AI/Doubao Pro โดยรองรับวิดีโอสูงสุด 30 วินาที รวมถึง reference หลายชนิดและการแก้ไขระดับ timestamp (ตำแหน่งเวลา) ส่วน Sora web/app ถูกยุติไปแล้วเมื่อ 26 เมษายน 2026 และ Sora API มีกำหนดยุติ 24 กันยายน 2026 ดังนั้นส่วน Sora ด้านล่างควรมองเป็นเทคนิคสำหรับ Sora 2 API/แนวทางที่ถ่ายโอนไปสู่โมเดลรุ่นถัดไปค่ะ citeturn16search0turn14search1

**กฎทองสำหรับ Fight Prompt (พรอมต์ฉากต่อสู้)** ที่ฉันแนะนำคือ:

```text
ONE SHOT
= one dominant body action
+ one readable reaction
+ one simple camera move
+ one physical consequence
```

ไม่ใช่:

```text
one shot
= punch + dodge + grab + counter + throw
+ spinning camera + flying debris + crowd reaction
```

OpenAI เองแนะนำสำหรับ Sora 2 ว่า motion (การเคลื่อนไหว) เป็นส่วนที่ยาก ควรใช้หนึ่งการกระทำหลักและหนึ่ง camera move (การเคลื่อนกล้อง) ต่อช็อต พร้อมแบ่ง action เป็น beats/counts (จังหวะ/จำนวนครั้ง) ที่มองเห็นได้ ขณะที่ Kling ก็แนะนำคำสั่งที่ชัด ไม่ยัดคำสั่งซับซ้อนมากเกินไปค่ะ citeturn20search0turn17search7


## เหตุผลที่ Multi‑Body Collision พัง และ Syntax ที่ควรใช้แทน

ในเชิงอนุมานจากงานด้าน Human Fidelity และ Physics Benchmarks โมเดลต้องแก้โจทย์อย่างน้อยสี่ชั้นพร้อมกันเมื่อคนสองคนปะทะกัน: รักษา identity (ตัวตน) ของ A/B, รักษาจำนวนและ topology (โครงสร้างการเชื่อมต่อ) ของแขนขา, คำนวณว่าแขนใครอยู่หน้า/หลังเมื่อเกิด occlusion (การบังกัน), และทำให้ผลหลัง collision สอดคล้องกับทิศทางของแรงค่ะ ยิ่งมีมือ นิ้ว อาวุธ หรือการจับกันโดยตรง จำนวน constraint (เงื่อนไขบังคับ) ก็ยิ่งเพิ่มขึ้นอย่างรวดเร็ว citeturn19search0turn19search14turn19search7

นี่อธิบายว่าทำไมคำว่า:

```text
two men engage in complex martial arts
```

จึงอันตรายกว่าคำว่า:

```text
The man on the left plants his left foot.
His shoulders rotate clockwise.
His right forearm travels once toward the other man's padded upper chest.
The other man's torso recoils backward and his right foot slides half a step.
```

ประโยคหลังคือ **Kinematic Description (คำอธิบายการเคลื่อนที่เชิงกายภาพ)** ซึ่งแปลงความหมายเชิงนามธรรมให้เป็นสิ่งที่สามารถสังเกตจากภาพได้ แนวคิดนี้สอดคล้องกับคำแนะนำอย่างเป็นทางการของ Sora ที่ให้ใช้ visible nouns and verbs (คำนามและกริยาที่เห็นเป็นภาพ) เช่นระบุจำนวนก้าว การหยุด และจังหวะของการกระทำ แทนคำอย่าง “moves quickly”; Runway ก็แนะนำ direct, clear language (ภาษาตรงและชัด) และหลีกเลี่ยง conceptual language (ภาษานามธรรม) ค่ะ citeturn20search0turn21search7turn21search14

### Syntax สำหรับฉากปะทะ

สูตรที่เสถียรกว่าคือ:

```text
[SUBJECT IDENTITY/POSITION]
+ [WEIGHT-BEARING FOOT]
+ [CENTER-OF-MASS SHIFT]
+ [ONE LIMB TRAJECTORY]
+ [CONTACT TARGET OR NEAR-CONTACT GAP]
+ [REACTION VECTOR]
+ [END POSE]
+ [ONE CAMERA BEHAVIOR]
```

ตัวอย่าง:

```text
The fighter on frame left plants his left foot.
His weight shifts forward over the left hip.
His right shoulder rotates forward and his right forearm sweeps once across chest height.
The forearm stops a few centimeters before the opponent's padded shoulder.
The opponent's torso turns away and his right foot slides backward.
Both characters finish in separate, readable silhouettes.
Locked medium-wide camera.
```

จุดสำคัญคือคำว่า **“one / once / stops / finishes”** ช่วยจำกัด trajectory (เส้นทางการเคลื่อนไหว) และ final state (สภาพปลายทาง) ค่ะ วิธีแบ่งการเคลื่อนไหวเป็นจังหวะและกำหนดผลลัพธ์ปลายทางมีแนวเดียวกับ Sora 2 Prompting Guide ซึ่งแนะนำคำสั่งแบบ “ทำจำนวนหนึ่งครั้ง → หยุด → ทำสิ่งสุดท้ายในช่วงเวลาที่กำหนด” แทนการบอกการเคลื่อนไหวกว้าง ๆ citeturn20search0

สำหรับภาพยนตร์ ฉันจะเพิ่มหลัก **Contact–Consequence Separation (แยกการสัมผัสออกจากผลของแรง)**:

```text
SHOT A = Approach / Pre-impact
CUT
SHOT B = Post-impact / Recoil
SHOT C = Recovery / Consequence
```

นี่เป็นการออกแบบเพื่อลดจำนวน constraint ที่โมเดลต้องแก้ในเฟรมเดียว ไม่ใช่การรับประกันทางคณิตศาสตร์ค่ะ แต่เข้ากับข้อค้นพบจากงาน physics benchmarks ซึ่งแสดงว่า multi-object interaction และ collision ยังเป็นจุดอ่อนของระบบ video generation ปัจจุบัน citeturn19search2turn19search6turn19search22


## กฎเฉพาะสำหรับ Kling, Seedance, Hailuo, Runway และ Sora

**Kling — ใช้ Subject Movement ก่อน adjectives (คำคุณศัพท์)**

โครงสร้าง Prompt ที่ Kling เผยแพร่คือ:

```text
Subject (Subject Description)
+ Subject Movement
+ Scene (Scene Description)
+ optional Camera Language
+ Lighting
+ Atmosphere
```

กล่าวคือส่วน “ทำอะไร” ควรชัดกว่าส่วน “ดูเท่แค่ไหน” ค่ะ citeturn17search1

สำหรับ high-speed action (แอ็กชันความเร็วสูง) ฉันแนะนำให้เขียนเป็น:

```text
Character A + one translational move
Character B + one reaction
environment + one secondary reaction
camera + one direction
```

เช่น:

```text
The fighter in the black jacket lunges one step forward.
His right shoulder turns sharply and his right forearm sweeps once across chest height.
The fighter in the gray jacket recoils one step toward the wall.
Rainwater kicks outward from their shoes.
The camera tracks laterally with the black-jacket fighter.
```

แทน:

```text
Two fighters exchange an incredibly fast series of complex martial arts attacks.
```

สำหรับ **Motion Brush (แปรงควบคุมการเคลื่อนไหว)** Kling ระบุว่าสามารถกำหนด motion ให้หลาย element ได้สูงสุด 6 ส่วน และมี Static Brush (แปรงตรึงวัตถุ) เพื่อคงส่วนที่ไม่ต้องการให้เคลื่อน ดังนั้นในฉากต่อสู้ควรใช้ brush เพื่อ “ลดองศาอิสระ” ไม่ใช่เพิ่มค่ะ citeturn17search0

แนวทางที่ฉันแนะนำ:

```text
Brush 1: torso/hips of Fighter A → short forward trajectory
Brush 2: striking forearm → one curved trajectory
Brush 3: Fighter B torso → short backward trajectory
Static Brush: wall / floor / major background geometry

Text:
Fighter A steps forward and swings one forearm.
Fighter B recoils backward.
```

อย่าให้ Motion Brush บอก “ไปขวา” แต่ text prompt บอก “lunges left” เพราะเท่ากับสร้าง conflicting constraints (เงื่อนไขขัดกัน) ค่ะ Runway และ Kling ต่างแนะนำโดยหลักให้คำสั่งชัดและไม่ขัดแย้งหรือซับซ้อนเกินไป citeturn17search7turn21search7

**Seedance 2.5 / Jimeng — ใช้ Natural-Language Camera Choreography ไม่ใช่ Hailuo-style tags**

จุดนี้สำคัญมากค่ะ: จากเอกสารทางการที่ตรวจสอบ **ฉันไม่พบ syntax แบบ `[Camera tag]` ของ Seedance 2.5 ที่เทียบเท่ากับ Hailuo** สิ่งที่ ByteDance เน้นคือ natural-language control (การควบคุมด้วยภาษาธรรมชาติ), multimodal references (สื่ออ้างอิงหลายชนิด), camera perspective/reference editing และ motion reference ค่ะ Seedance 2.0 ยังระบุชัดว่าสามารถใช้ภาพ เสียง และวิดีโอเป็น reference เพื่อควบคุม performance, lighting และ camera movement ได้ ส่วน 2.5 ขยายการอ้างอิงไปถึง 30 ภาพ 10 วิดีโอ และ 10 เสียงค่ะ citeturn16search8turn16search0

จึงเขียนแบบนี้:

```text
A handheld gimbal follows one meter behind the fighter.
As he moves left, the camera tracks left with him.
At the instant the opponent passes close to lens, the camera whip-pans right,
briefly losing both bodies in motion blur.
It settles on the opponent already recoiling against the wall.
```

การใช้ **whip pan (การแพนกล้องเร็ว)** เพื่อซ่อนเฟรม contact เป็น filmmaking cheat (เทคนิคหลอกทางภาพยนตร์) ที่มีประโยชน์มาก เพราะเปลี่ยนปัญหาจาก “สร้างมือชนร่างกายอย่างถูกต้อง 6–10 เฟรม” เป็น “สร้าง pre-impact → motion blur → post-impact” ค่ะ Seedance 2.5 ถูกออกแบบให้รองรับ long-form/storytelling และ reference/editing control ที่ละเอียดขึ้น แต่ ByteDance เองไม่ได้อ้างว่าโมเดลปลอดจากข้อผิดพลาดด้านฟิสิกส์ทั้งหมด และในการเปิดตัว Seedance 2.0 ก็ระบุอย่างตรงไปตรงมาว่ายังมีข้อบกพร่องในการ generation อยู่ค่ะ citeturn16search0turn16search1

**Hailuo / MiniMax — `[command]` เป็น syntax จริง**

ต่างจาก Seedance เอกสาร MiniMax API ระบุอย่างเป็นทางการว่า Hailuo‑2.3, Hailuo‑02 และ Director models รองรับ `[command]` สำหรับ camera motion (การเคลื่อนกล้อง) โดยมี 15 คำสั่ง เช่น `[Tracking shot]`, `[Push in]`, `[Pull out]`, `[Pan left]`, `[Pan right]`, `[Truck left]`, `[Truck right]`, `[Shake]` และ `[Static shot]` ค่ะ citeturn15search4turn15search7

สำหรับการต่อสู้:

```text
[Tracking shot]
The camera follows the runner from the side as he closes the distance.

[Shake]
A short camera jolt occurs as the opponent is already knocked backward.

[Static shot]
The opponent catches himself against the wall.
```

อย่าใช้ camera movement สามสี่ชนิดพร้อมกับ grappling (การจับล็อก) และ limb interaction ในจังหวะเดียว แม้ Director models จะมี camera-control โดยตรงก็ตาม การลดจำนวน motion constraints ยังคงสอดคล้องกับข้อจำกัดที่ physics/human-motion benchmarks พบค่ะ citeturn15search1turn19search7turn19academia40

Hailuo‑02 ยังรองรับ First & Last Frame (เฟรมแรกและเฟรมสุดท้าย) โดยตรง ซึ่งเหมาะมากกับ pipeline pre-impact/post-impact ที่จะอธิบายต่อไปค่ะ citeturn15search5turn15search2

**Runway Gen‑3 / Gen‑4.5 — ป้องกัน morphing ตั้งแต่ Input Image**

สำหรับ Runway กฎสำคัญที่สุดของ I2V คือ **ภาพเริ่มต้นต้องสะอาด** เพราะ Runway ระบุว่าความผิดปกติในภาพ เช่น มือหรือใบหน้าที่เบลอ อาจถูกขยายให้เด่นขึ้นเมื่อกลายเป็นวิดีโอ และ text prompt ของ I2V ควรเน้นสิ่งที่จะ “เคลื่อนไหว” เนื่องจาก input image ได้กำหนด subject, composition, lighting และ style ไว้แล้วค่ะ citeturn21search0

โครงสร้างที่ Runway แนะนำคือ:

```text
The camera [motion] as the subject [action]. [additional motion]
```

ดังนั้นถ้า keyframe มีนักสู้สองคนอยู่แล้ว อย่าอธิบายใบหน้า เสื้อผ้า และฉากทั้งหมดซ้ำค่ะ ให้เขียนเช่น:

```text
The camera tracks slowly right as the subject on the left takes one step forward.
His right forearm moves once toward the other subject.
The subject on the right immediately recoils backward.
Natural high-speed motion blur during the arm movement.
```

Runway ยังแนะนำ positive phrasing (บอกสิ่งที่ต้องการให้เกิด) มากกว่า negative prompting (บอกสิ่งที่ห้ามเกิด) เช่นใช้ “hands remain clearly separated” แทนการยัด “no bad hands, no deformed fingers, no morphing” ค่ะ citeturn21search2turn21search7

Act‑One/Gen‑3 ไม่ให้ใช้งานแล้ว ปัจจุบัน performance-capture workflow คือ Act‑Two ซึ่งรับ driving performance (วิดีโอการแสดงต้นแบบ) และ character input; และ Runway ระบุว่า Act‑Two ยังทำงานบน character input ทีละตัว สำหรับหลายตัวละคร Runway เองเสนอ workflow แยกแต่ละตัวละครแล้วประกอบภายหลังค่ะ ดังนั้นฉันไม่แนะนำให้หวังพึ่ง performance transfer ตัวเดียวเพื่อแก้ two-body fight collision ค่ะ citeturn21search1turn21search3

**Sora 2 — เขียน Action เป็น Beats ไม่ใช่ Fight Vocabulary**

OpenAI แนะนำโดยตรงว่าแต่ละ shot ควรมี **หนึ่ง clear camera move + หนึ่ง clear subject action** และ action ควรแบ่งเป็น beats/counts (จังหวะ/จำนวน) เช่นก้าวสี่ก้าว หยุด แล้วทำการกระทำในวินาทีสุดท้ายค่ะ citeturn20search0

ดังนั้นแทน:

```text
They have a brutal fast martial arts fight.
```

ใช้:

```text
Beat 1: The man in the dark coat takes one fast step forward.
Beat 2: His right forearm swings once toward chest height.
Beat 3: The second man turns his torso away and takes two stumbling steps backward.
Camera: one lateral tracking move throughout.
```

OpenAI ระบุเองว่ารุ่น video generation ก่อน Sora 2 มีแนวโน้ม morph objects (แปรรูปร่างวัตถุ) หรือบิดความเป็นจริงเพื่อทำตาม prompt แม้ Sora 2 จะพัฒนาด้าน physics ขึ้นมาก แต่ก็ยัง “ไม่สมบูรณ์” ค่ะ citeturn20search1

อีกวิธีเพิ่มความเสถียรคือใช้ image reference (ภาพอ้างอิง) เป็น first-frame anchor (สมอยึดเฟรมแรก) แล้วให้ text ระบุเฉพาะสิ่งที่จะเกิดขึ้นต่อ OpenAI ระบุว่าภาพ reference ช่วยล็อก character design, wardrobe, set และ composition และยังแนะนำคลิปสั้นในการ iterate เพราะคำสั่งมีแนวโน้มถูกทำตามได้ง่ายกว่าคลิปยาวค่ะ citeturn20search0turn20search2


## Pipeline I2V และ Keyframe สำหรับ Action Beat ประมาณ 1.5 วินาที

คำว่า **“foolproof”** ไม่มีจริงสำหรับ generative video ปัจจุบันค่ะ แต่สามารถสร้าง **High-Reliability Pipeline (กระบวนการที่มีโอกาสสำเร็จสูง)** โดยไม่ให้โมเดลสร้าง collision frame สำคัญเอง

ที่ 24 fps ระยะ 1.5 วินาทีคือ **36 เฟรม** แต่หลายโมเดลไม่ได้มี native duration 1.5 วินาที เช่น Sora 2 API เริ่มจาก 4 วินาที ส่วน Hailuo รุ่น legacy มี duration ยาวกว่านั้น ขณะที่ Runway Gen‑4.5 ปัจจุบันรองรับ 2–10 วินาที ดังนั้น 1.5 วินาทีควรมองเป็น **editorial action unit (หน่วยแอ็กชันที่เลือกตัดออกมาภายหลัง)** มากกว่าความยาว generation โดยตรงค่ะ citeturn20search0turn15search13turn21search9

| Stage | สิ่งที่ต้องทำ |
|---|---|
| **Master Plate** | สร้าง character, wardrobe, location, lens และ lighting ที่แน่นอน |
| **KF‑A Pre‑Impact** | ท่าก่อนโดนประมาณ 70–90% ของ trajectory แต่ **ยังไม่แตะกัน** |
| **KF‑B Post‑Impact** | เริ่มหลัง contact แล้ว ให้ร่างกายแยกจากกันและเห็นผลของ momentum |
| **I2V Generation** | Prompt เฉพาะ movement ระหว่างสถานะ |
| **Editorial Cut** | เลือกช่วง ~36 เฟรมที่ดีที่สุด |
| **Impact Sell** | ใช้ cut, sound, camera jolt, debris หรือ foreground occlusion ขายแรงกระแทก |

แนวคิดการใช้ start/end frames เป็น anchor มีการรองรับโดยตรงใน Hailuo‑02 และ Kling รุ่นที่รองรับ frames ขณะที่ Sora และ Runway ใช้ image input เพื่อ anchor จุดเริ่มต้นของ generation ค่ะ citeturn15search5turn18search0turn20search0turn21search0

**Keyframe A — Pre‑Impact**

สำหรับ Midjourney/FLUX หรือ image generator ที่คุณใช้สร้าง plate ฉันแนะนำ staging แบบนี้:

```text
cinematic action pre-impact frame,
two adult performers,
medium-wide full-body composition,
fighter A in a clean readable silhouette on frame left,
fighter B on frame right,
A's striking forearm is 15 cm away from B's padded upper torso,
both hands fully visible,
arms and legs clearly separated,
feet planted on different depth planes,
no body parts overlapping,
35mm lens, eye-level camera,
directional practical lighting,
frozen instant immediately before impact
```

เหตุผลของช่องว่าง 10–20 ซม. ไม่ใช่เพราะมีตัวเลขมหัศจรรย์ค่ะ แต่เป็น production heuristic (กฎจากการใช้งานจริง) เพื่อหลีกเลี่ยงการเริ่มวิดีโอจากภาพที่มือ นิ้ว หรือแขนสองคนรวม silhouette กัน เพราะ Runway ระบุว่าความผิดปกติของมือ/ใบหน้าใน input สามารถถูกขยายในวิดีโอได้ และ human-motion benchmarks แสดงว่าการรักษา anatomy และ temporal stability ยังเป็นข้อท้าทายค่ะ citeturn21search0turn19search14turn19academia40

**Keyframe B — Post‑Impact**

อย่าสร้างภาพ “กำปั้นฝังอยู่ในหน้า” ค่ะ ให้ผลของแรงเกิดขึ้นแล้ว:

```text
same exact camera, lens, wardrobe, set and lighting,
the instant immediately after the strike,
fighter A's striking arm has already passed the center line
and begins to retract,
fighter B's torso has rotated 25 degrees away,
B's rear foot slides backward,
the two bodies are clearly separated,
both performers remain anatomically readable,
small droplets and fabric movement indicate impact,
medium-wide 35mm composition
```

ความต่างของ KF‑A กับ KF‑B ควรเน้น **pose/momentum** มากกว่าการเปลี่ยนมุมกล้อง ฉาก และ identity พร้อมกัน เพราะทุกการเปลี่ยนแปลงเพิ่มเติมเพิ่มภาระที่โมเดลต้อง interpolate (สร้างสถานะระหว่างเฟรม) ค่ะ หลักนี้สอดคล้องกับแนวทางของ Sora ที่ให้หนึ่ง action/หนึ่ง camera setup ต่อ shot และ Runway ที่แนะนำเริ่มจาก motion ที่จำเป็นที่สุดก่อนเพิ่มรายละเอียดค่ะ citeturn20search0turn21search2

**Prompt สำหรับช่วง interpolation**

```text
The fighter on the left completes one short forward forearm strike.
His weight transfers from the rear foot to the front foot.
The fighter on the right immediately rotates away and steps backward.
The striking arm follows through once and begins to retract.
Clothing and rain droplets react briefly to the sudden movement.
The camera remains at the same height and distance.
Natural high-speed motion blur appears only during the fastest movement.
```

สำหรับ Hailuo First/Last Frame เพิ่ม:

```text
[Static shot]
```

ถ้าอยากให้ impact ดูแรงขึ้น อย่าให้ AI สร้าง “แรงชนที่ซับซ้อนขึ้น” แต่ให้ภาพยนตร์ช่วย:

```text
Pre-impact frame
→ 2–3 frame whip blur / foreground occlusion
→ post-impact frame
→ impact SFX
→ 2–4 frame camera jolt
```

นี่คือหลัก **Hide the Contact, Show the Consequence — ซ่อนจังหวะสัมผัส แต่แสดงผลของแรง** ค่ะ


## Anti‑Patterns และ Kinematic Replacements

คำบางประเภทไม่ได้เป็น “คำต้องห้ามของโมเดล” อย่างเป็นทางการ แต่เป็น **high-entropy descriptions (คำอธิบายที่เปิดความเป็นไปได้มากเกินไป)** ซึ่งทำให้โมเดลต้องประดิษฐ์ trajectory จำนวนมากเอง การลดความกำกวมสอดคล้องกับแนวทางอย่างเป็นทางการของ Sora, Runway และ Kling ที่เน้นคำสั่งตรง ชัด และ action ที่แยกเป็นส่วนค่ะ citeturn20search0turn21search7turn17search7

| Anti-pattern (ควรเลี่ยง) | ปัญหา | Kinematic replacement (คำแทนเชิงการเคลื่อนไหว) |
|---|---|---|
| `complex martial arts` | จำนวนท่าและลำดับไม่กำหนด | `A takes one step forward; right shoulder rotates; right forearm swings once; B recoils one step` |
| `rapid exchange of punches` | หลายแขนเปลี่ยน topology ต่อเนื่อง | `A throws one left jab; B moves his head back; A retracts the fist` |
| `grappling` | ร่างกายซ้อนกันหลายจุด | `A places both forearms around B's jacketed upper torso; both hands remain visible` |
| `wrist lock` | นิ้ว+มือ+ข้อมือซ้อนกัน | `A grips B's jacket cuff; A turns his forearm outward; B steps with the rotation` |
| `throws him to the ground` | ต้องแก้ lift+rotation+fall+collision | แบ่งเป็น `A pulls B off balance` → CUT → `B is already falling sideways` |
| `flurry of kicks and punches` | หลาย trajectory พร้อมกัน | `one low kick` **หรือ** `one straight punch` ต่อช็อต |
| `they wrestle violently` | identity และ limbs สลับกันง่าย | `A pushes both palms against B's jacketed shoulders; B slides backward` |
| `spinning fight choreography` | body rotation + camera ambiguity | ระบุว่าใครหมุน: `A pivots 90 degrees on the left foot; camera stays locked` |
| `camera circles them as they fight` | camera+two subjects+contact ทั้งหมดเปลี่ยน | `camera tracks left while A advances; B only retreats` |
| `he tackles him through the wall` | คนชนคนและคนชนฉากพร้อมกัน | `A closes to 20 cm from B` → CUT → `both are already breaking through drywall` |
| `cars crash violently` | rigid-body collision ซับซ้อน | แบ่ง `closing distance` → `post-impact deformation` หรือทำ `near miss` |
| `epic chaotic battle` | ไม่มี object/action hierarchy | ระบุ foreground actor เพียง 1 คู่ ที่เหลือเป็น background motion |

**คำที่มักทำให้ motion ดู floaty (ลอย/ไม่มีน้ำหนัก)** เช่น `flies toward`, `leaps effortlessly`, `gets thrown across the room`, `moves extremely fast` ควรแทนด้วย weight transfer (การถ่ายน้ำหนัก), foot placement (ตำแหน่งเท้า), displacement (ระยะเคลื่อน) และ stopping point (จุดหยุด):

```text
BAD
He is violently knocked backwards.

BETTER
His shoulders snap backward first.
His hips follow half a beat later.
His right foot slides 40 cm across the floor.
His left foot catches his weight and stops the retreat.
```

คำอธิบายแบบหลังให้ causal sequence (ลำดับเหตุและผล) ที่มองเห็นได้ โดยไม่สมมติว่าโมเดลมี physics engine อยู่ภายในค่ะ ซึ่งเหมาะกับสถานะปัจจุบันที่งานวิจัยยังพบช่องว่างระหว่าง “motion ที่ดู plausible” กับ “motion ที่ถูกต้องเชิงฟิสิกส์/ชีวกลศาสตร์” citeturn19search6turn19academia40

อีกประเด็นสำคัญคือ **อย่าเขียน Negative Prompt ยาว ๆ** โดยเฉพาะ Runway:

```text
no morphing, no bad hands, no extra fingers,
no deformed limbs, no weird anatomy...
```

Runway แนะนำ positive phrasing โดยตรงค่ะ ให้เปลี่ยนเป็น:

```text
both characters remain visually distinct,
hands stay clearly visible,
each arm follows one continuous natural trajectory,
the two silhouettes remain readable,
stable facial identity,
natural anatomical proportions
```

citeturn21search2turn21search7


## Benchmark Prompt Code สำหรับฉากแอ็กชันห้าประเภท

Prompt code ด้านล่างเขียนภาษาอังกฤษเพื่อวางในโมเดลโดยตรงค่ะ แต่โครงสร้างทั้งหมดใช้หลัก **สามช็อต: approach → hidden/isolated impact → consequence** โดยตั้งใจไม่บังคับให้โมเดลสร้างการสัมผัสซับซ้อนที่สุดตรง ๆ หลักการแบ่ง shot แบบนี้เข้ากับคำแนะนำของ Sora ที่ให้แต่ละ shot มี camera/action ชัดเจน และแนวทาง Runway ที่ให้ลด ambiguity ของ motion ค่ะ citeturn20search0turn21search0

**Cyberpunk Alley Hand‑to‑Hand — การต่อสู้ระยะประชิดในตรอกไซเบอร์พังก์**

Shot A เน้น **commitment ก่อนปะทะ**:

```text
Cinematic cyberpunk action, rainy neon alley at night.
Two adult fighters, medium-wide 35mm composition.

The fighter in the black rain jacket stands frame left.
He plants his left foot, shifts his weight forward,
and drives one right forearm horizontally toward chest height.
The fighter in the gray jacket braces frame right and leans slightly away.
The forearm ends a hand-width before the gray jacket.

Wet pavement splashes under the advancing foot.
Camera tracks laterally right with the black-jacket fighter.
Natural shutter motion blur on the moving forearm.
Both characters remain in clean, separate silhouettes.
```

**Kling:** ใช้ Motion Brush กับ hips/torso ของ A ไปข้างหน้า, forearm เป็น trajectory แยก และ B ถอยสั้น ๆ; พื้น/ฉากใช้ Static Brush ได้ค่ะ Kling รองรับ Motion Brush หลาย element และ Static Brush โดยตรง citeturn17search0

Shot B เริ่ม **หลัง impact**:

```text
Same alley, same two fighters, same screen direction and lighting.
Locked medium-wide camera.

The shot begins immediately after the strike.
The black-jacket fighter's right arm is already past the center line.
He retracts the arm toward his ribs.

The gray-jacket fighter's shoulders twist backward.
His right foot slides half a step across the wet pavement.
His left hand reaches once toward the wall for balance.
A brief spray of rainwater and jacket fabric motion sells the impact.
```

Shot C คือ consequence (ผลต่อเนื่อง):

```text
Same screen direction.
The gray-jacket fighter catches himself against the wall with one palm.
His chest rises with one sharp breath.
The black-jacket fighter stops two meters away and resets his stance.

Slow push-in toward the gray-jacket fighter.
Neon reflections ripple in the puddles.
Physical movement settles naturally.
```

**Tactical Gun‑Fu Room Breach — ฉาก gun‑fu เชิงภาพยนตร์ในห้อง**

ชุดนี้ออกแบบเป็น **fictional film choreography (คิวต่อสู้เพื่อภาพยนตร์)** ไม่ใช่คำแนะนำเชิงยุทธวิธีค่ะ

Shot A:

```text
Stylized cinematic action on a fictional movie set.
Two adult performers, one clearly non-functional prop handgun.

A black-clad protagonist moves through an already-open doorway.
He takes two quick steps into the room.
The prop pistol remains close to his torso and clearly visible.
An opponent approaches from frame right with both hands visible.

28mm handheld camera follows behind and slightly to the left.
One continuous forward camera move.
Hard practical ceiling light, smoky atmosphere.
```

Shot B ใช้ foreground occlusion (วัตถุบังหน้าเลนส์) ซ่อน contact:

```text
Medium close action shot.
The opponent's shoulder passes close across the lens,
briefly obscuring the center of frame.

During the occlusion, the protagonist's prop handgun is already redirected downward.
As the opponent clears the lens,
the protagonist's free forearm is already past the opponent's padded upper torso.
The opponent recoils sideways one step.

One short handheld camera jolt at the reaction.
No additional attack occurs in this shot.
```

Shot C:

```text
The opponent stumbles into a table and catches the edge with one hand.
The protagonist stops three steps away.
The prop handgun remains pointed toward the floor as part of the staged choreography.

Camera settles from handheld movement into a stable medium shot.
Loose papers slide from the table and settle.
```

แทนที่จะขอ `gun-fu fight with multiple disarms, shots and takedowns` เราจึงแยกเป็น physical beat เดียวต่อช็อต ซึ่งลดทั้ง hand-object interaction และ multi-body collision ค่ะ หลักการลด action/camera ต่อ shot สอดคล้องกับ Sora 2 Prompting Guide โดยตรง citeturn20search0

**Medieval Sword Strike & Shield Clash — ดาบปะทะโล่ยุคกลาง**

Rigid props (วัตถุแข็ง) ทำให้ AI ต้องรักษาทั้ง geometry ของอาวุธและร่างกาย ดังนั้นควรหลีกเลี่ยงเฟรมที่ blade (คมดาบ) ตัดผ่านมือหรือโล่ค่ะ งาน physics benchmark ก็พบว่า collision ของวัตถุเป็นหนึ่งในหมวดที่ video generators ยังมีปัญหา citeturn19search2turn19search0

Shot A:

```text
Medieval battlefield at dusk.
Two armored adult warriors in a medium-wide 40mm shot.

The sword fighter plants his front foot.
His shoulders rotate once as the sword descends diagonally from upper right to lower left.
The shield bearer raises a round shield to shoulder height.
The sword stops a few centimeters before the shield rim at the end of the shot.

Camera remains locked.
Wind moves the cloaks and grass subtly.
Readable separation between sword blade, hands, shield and faces.
```

Shot B:

```text
Same camera axis and screen direction.
The shot starts immediately after the shield clash.

The sword blade is already rebounding upward and away from the shield.
The shield has rotated backward roughly fifteen degrees.
The shield bearer's rear foot slides backward once.
A small burst of dust and metallic sparks appears between the separated weapons.

Short restrained camera vibration, then immediate stability.
```

สำหรับ Hailuo:

```text
[Shake]
The sword is already rebounding away from the shield...
```

Hailuo รองรับ `[Shake]` เป็น camera command อย่างเป็นทางการค่ะ citeturn15search4

Shot C:

```text
The shield bearer lowers the shield slightly and regains balance.
The sword fighter finishes the follow-through,
then brings the sword back to a stable ready position.

Slow camera push-in.
Dust settles between them.
Both weapons remain rigid and clearly separated.
```

**Superhuman Tackle Through Drywall — พุ่งชนทะลุกำแพง**

นี่เป็นกรณีที่ไม่ควรพยายามสร้าง “คน A ชนคน B ชนกำแพงแตก” เป็น continuous physics event (เหตุการณ์ฟิสิกส์ต่อเนื่อง) เพราะมี collision chain (การชนต่อเนื่องหลายชั้น) อย่างน้อยสามวัตถุค่ะ งานวิจัยด้าน physics generation พบว่า multi-object interaction และ physical causal structure ยังเป็นโจทย์ยากของโมเดลวิดีโอ citeturn19search6turn19search0

Shot A:

```text
Superhuman cinematic action inside an abandoned office.
Medium-wide low-angle 28mm shot.

The charging hero lowers one shoulder and takes two explosive running steps.
The opponent braces directly in front of a drywall partition.
The hero ends the shot twenty centimeters before reaching the opponent.

The camera tracks backward at matching speed.
Heavy footfalls kick small dust particles from the floor.
Strong forward body lean, planted feet, visible weight and momentum.
```

Shot B เปลี่ยนกำแพงเป็น transition device (อุปกรณ์ซ่อนรอยต่อ):

```text
Side angle on the drywall partition.

The shot begins as a concentrated section of drywall bursts outward.
A dense sheet of plaster dust briefly fills the center of frame.
Behind the dust, two human silhouettes move through the opening as one fast mass.
Their exact point of body contact remains obscured by drywall and dust.

Static camera.
Fragments travel outward and downward under gravity.
```

Shot C:

```text
The shot begins on the opposite side of the broken wall.

Both characters are already separated on the floor.
The hero slides forward on one knee and one hand, then stops.
The opponent rolls once onto his side and comes to rest near scattered drywall debris.

The camera slowly pushes forward.
Dust continues drifting downward.
No further collision occurs.
```

สังเกตว่าเราไม่ได้ให้ AI “โชว์ tackle contact” ค่ะ เราให้ **wall + dust เป็น temporal mask (หน้ากากบังช่วงเวลา)** แล้วเล่า physics ผ่าน before/after state แทน

**Car Chase Vehicular Near‑Miss — รถไล่ล่าและเฉียดชน**

สำหรับรถ ถ้าต้องการ reliability สูง ฉันเลือก **near-miss (เฉียดชน)** มากกว่า actual crash เพราะ collision ของ rigid bodies เป็นหมวดปัญหาที่ benchmark ด้าน physical coherence ใช้วัดโมเดลโดยตรงค่ะ citeturn19search2turn19search4

Shot A:

```text
High-speed cinematic car chase on a wet industrial road at night.
A dark sedan leads with a silver sedan closing from behind.
Low rear three-quarter tracking shot, 35mm lens.

The dark sedan shifts one lane to the left.
The silver sedan continues straight and closes the gap.
Both vehicles maintain four-wheel contact with the road.
Suspension compresses subtly during the lane change.
Road spray trails behind the tires.
```

Shot B:

```text
Locked roadside camera, long-lens compression.

The dark sedan crosses the foreground from right to left.
Half a beat later, the silver sedan passes behind it.
The front-left corner of the silver sedan misses
the rear-right corner of the dark sedan by roughly thirty centimeters.

Both cars maintain their original shapes and continue on separate trajectories.
Natural tire deformation, road spray and directional motion blur.
```

Shot C:

```text
Rear tracking shot behind the silver sedan.

The silver sedan brakes hard after the near miss.
Its nose dips slightly and its rear suspension unloads.
The dark sedan continues forward and increases the distance between them.
The silver sedan corrects its heading once and remains in its lane.

Camera continues one smooth forward tracking movement.
Wet road reflections stretch under both cars.
```

ถ้าจำเป็นต้องเป็น **actual impact (ชนจริง)** ให้ใช้รูปแบบเดียวกับ fight scene:

```text
SHOT A
The cars close to within half a meter.
CUT BEFORE CONTACT.

SHOT B
The shot begins immediately after the side impact.
Both vehicles are already separated.
The struck sedan yaws fifteen degrees clockwise.
Small body-panel fragments and droplets move away from the impact point.
The second car continues on a slightly altered trajectory.

SHOT C
The struck car regains traction and decelerates.
Debris settles onto the road.
```

การให้รถ “อยู่หลังการชนแล้ว” ใน Shot B ลดภาระจากการสร้าง deformation (การยุบตัว), contact, momentum transfer และ separation พร้อมกันในเฟรมวิกฤตค่ะ ซึ่งตรงกับหลักทั่วไปที่งาน physics research ระบุว่าการสร้าง motion ที่ดู plausible ไม่ได้หมายความว่าโมเดลกำลังบังคับใช้ physical causes และ constraints อย่างถูกต้องเสมอไป citeturn19search6turn19search24

**สรุปเป็น Production Formula ที่ใช้กับทั้งห้าประเภทได้:**

```text
SHOT A — PRE-IMPACT
Approach + planted weight + single trajectory + visible gap

SHOT B — IMPACT CHEAT
Occlusion / whip pan / debris / motion blur
+ start from post-contact state
+ one recoil vector

SHOT C — CONSEQUENCE
Recovery + settling physics + identity check
+ simpler camera
```

และ Prompt Formula ที่ฉันมองว่าเหมาะที่สุดสำหรับ action generation ปัจจุบันคือ:

```text
[WHO + SCREEN POSITION]
→ [WEIGHT / FOOT PLACEMENT]
→ [ONE BODY TRAJECTORY]
→ [DEFINED TARGET OR NEAR-CONTACT GAP]
→ [REACTION VECTOR]
→ [EXPLICIT END STATE]
→ [ONE CAMERA MOVE]
→ [ONE ENVIRONMENTAL CONSEQUENCE]
```

แทนที่จะพยายามทำให้ prompt “เร้าใจขึ้น” ให้ทำให้ **state transition (การเปลี่ยนสถานะ) ชัดขึ้น** ค่ะ ยิ่งฉากมีการปะทะกันมากเท่าไร prompt ควรยิ่งเป็นภาษาของ stunt coordinator (ผู้กำกับคิวบู๊) และ animator (นักสร้างการเคลื่อนไหว) มากกว่าภาษาของนักเขียนบท เช่น **เท้าข้างไหนรับน้ำหนัก → ส่วนไหนหมุน → แขนเคลื่อนไปทิศไหน → ใครถอย → จบในท่าอะไร** เพราะแนวทางนี้ตรงกับสิ่งที่ Runway, Sora และ Kling แนะนำร่วมกัน คือ concrete visible motion (การเคลื่อนไหวที่เห็นชัด), ลด ambiguity และแบ่ง motion ออกเป็น action ที่ควบคุมได้ค่ะ citeturn20search0turn21search0turn21search2turn17search1