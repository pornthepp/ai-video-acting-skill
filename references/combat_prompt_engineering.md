# Action & Fight Scene Prompt Engineering: การเอาชนะข้อจำกัด Multi-Body Collision ในวิดีโอ Generative AI

## บทสรุปเชิงเทคนิค

ปัญหาหลักของฉากต่อสู้ใน Generative Video ไม่ใช่แค่ “โมเดลวาดมือไม่เก่ง” แต่เป็นปัญหา **multi-body collision (การชนและปฏิสัมพันธ์ของหลายร่าง)** ซึ่งต้องรักษาพร้อมกันทั้งตัวตนของผู้แสดง ตำแหน่งแขนขา การบังกันของร่างกาย ทิศแรง โมเมนตัม และผลหลังการปะทะในหลายเฟรมต่อเนื่อง งานวิจัย VideoPhy-2 พบว่าแม้โมเดลที่ดีที่สุดในการทดสอบยังทำคะแนนร่วมด้านความตรงตามคำสั่งและความสมเหตุสมผลทางฟิสิกส์ได้เพียง 22% ในชุดยาก และมีปัญหาเด่นกับกฎอนุรักษ์มวลและโมเมนตัม ขณะที่ SocialDirector ระบุปัญหาเฉพาะของวิดีโอหลายคน เช่น “คนผิดคนทำแอ็กชัน”, เป้าหมายของแอ็กชันผิดคน และลำดับปฏิสัมพันธ์สับสนค่ะ citeturn17view2turn17view3

การสัมผัสตัวต่อเนื่องยิ่งยากขึ้น เพราะ **occlusion (การบังกัน)** ทำให้โมเดลต้องแยกแขน ขา มือ และลำตัวสองชุดออกจากรูปร่างที่ซ้อนกัน งาน Hi4D พบว่าการแยกมนุษย์ที่อยู่ชิดและสัมผัสกันเป็นเวลานานเป็นปัญหายากโดยธรรมชาติ เนื่องจากการบังกันและรูปร่างที่ซับซ้อน แม้แต่ระบบวิเคราะห์สามมิติก็อาจรวมคนสองคนเป็นผิวเดียวกันได้ค่ะ citeturn15view2

ดังนั้นหลักการที่ให้ผลเสถียรกว่าคือ:

> **อย่าสั่งให้โมเดล “ออกแบบการต่อสู้” ให้สั่งให้มัน “เชื่อมสถานะก่อนปะทะไปยังสถานะหลังปะทะ”**

กล่าวอีกแบบหนึ่ง คือเปลี่ยนโจทย์จาก

`สองคนต่อสู้กันอย่างรวดเร็วด้วยคอมโบศิลปะการต่อสู้ซับซ้อน`

เป็น

`นักสู้เสื้อแดงวางเท้าหน้าค้าง หมุนสะโพกและปล่อยหมัดขวาตรงหนึ่งครั้ง นักสู้เสื้อดำสะท้อนลำตัวถอยและก้าวถอยหนึ่งก้าว`

นี่คือ **state-transition prompting (การพรอมป์ต์แบบเปลี่ยนสถานะ)** แทน **choreography prompting (การพรอมป์ต์ท่าต่อสู้เป็นชุด)** ซึ่งสอดคล้องกับแนวทางของ Sora ที่ระบุว่า movement (การเคลื่อนไหว) เป็นส่วนที่ควบคุมยาก และแนะนำให้แต่ละช็อตมี camera move (การเคลื่อนกล้อง) หลักหนึ่งอย่างกับ subject action (การกระทำของตัวแบบ) หลักหนึ่งอย่างเท่านั้น รวมทั้งบรรยายการเคลื่อนไหวเป็นจังหวะหรือจำนวนครั้งที่ชัดเจนค่ะ citeturn17view1

สถานะของแพลตฟอร์ม ณ **19 สิงหาคม 2026** ต้องแยกจากคำแนะนำเก่าในอินเทอร์เน็ตด้วยค่ะ:

| Platform | สถานะที่เกี่ยวข้องกับงานนี้ |
|---|---|
| **Runway Gen-3 / Act-One** | Gen-3 Alpha ถูกยุติ 8 ก.ค. 2026 และ Turbo 30 ก.ค. 2026; Act-One ไม่ให้ใช้งานแล้ว ปัจจุบัน Runway ชี้ไปที่ Gen-4.5, Animate Frames และ Act-Two citeturn15view8turn16view6 |
| **Kling 1.5 / 2.0** | เป็นรุ่นย้อนหลังแล้ว; ปัจจุบัน Kling มีตระกูล VIDEO 3.0 แต่หลัก Motion Brush และโครงสร้าง Subject + Movement ยังมีประโยชน์โดยตรงกับ workflow แบบ I2V ค่ะ citeturn18search0turn18search2turn15view3 |
| **Seedance 2.5 / Jimeng** | Seedance 2.5 รองรับคลิปสูงสุด 30 วินาที, multimodal references (สื่ออ้างอิงหลายรูปแบบ) และ timestamp-level editing (แก้ไขระดับเวลา) และกำลังเปิดใช้บน Jimeng AI/Doubao ค่ะ citeturn15view5 |
| **MiniMax / Hailuo** | API ปัจจุบันรองรับ Hailuo 2.3/2.3 Fast/02 และมี `[Camera command]` อย่างเป็นทางการค่ะ citeturn17view0 |
| **OpenAI Sora 2** | Videos API และ Sora 2 ถูกประกาศ deprecated (เลิกสนับสนุน) และกำหนดปิดวันที่ **24 กันยายน 2026** ค่ะ citeturn16view1 |

## เหตุผลที่ฉากปะทะหลายร่างพัง และวิธีคิดแบบ Kinematic

การต่อสู้แบบใกล้ตัวสร้าง “constraint stack (ชุดข้อจำกัดซ้อนกัน)” หลายระดับพร้อมกัน ได้แก่ ตัวตนของ A และ B ต้องไม่สลับกัน, มือของ A ต้องยังติดกับแขน A, หมัดต้องเคลื่อนเข้าหา B, การสัมผัสต้องเกิดเวลาที่ถูกต้อง, B ต้องตอบสนองหลังสัมผัส ไม่ใช่ก่อนสัมผัส และโมเมนตัมต้องต่อเนื่องจากก่อนชนไปหลังชน งาน SocialDirector แสดงให้เห็นตรง ๆ ว่าโมเดลหลายคนยังขาด explicit control (การควบคุมแบบระบุชัด) ว่า “ใครทำอะไร เมื่อใด และทำกับใคร” และ directional wording (คำกำหนดทิศทาง) มีความสำคัญต่อการผูกการกระทำเข้ากับเป้าหมายค่ะ citeturn17view3

### กฎหนึ่งแรง หนึ่งเหตุการณ์ หนึ่งผลลัพธ์

สำหรับช็อตแอ็กชันระยะสั้น ให้คิดเป็นสมการเล่าเรื่องดังนี้:

**Setup → Drive → Contact Cue → Reaction → Separation**

ตัวอย่าง:

`plants left foot → rotates hips → drives right shoulder → rain spray bursts at contact → opponent's torso recoils → opponent steps back`

แทนที่จะเป็น:

`throws a powerful punch`

คำว่า “powerful (ทรงพลัง)” เป็นคำเชิงนามธรรม แต่ `plants`, `rotates`, `drives`, `recoils`, `steps back` เป็นสิ่งที่มีตำแหน่งและทิศทางให้โมเดลประมาณได้ แนวทางนี้ตรงกับ Runway ซึ่งแนะนำให้แปลแนวคิดนามธรรมเป็น physical actions (การกระทำทางกายภาพ) ที่ตรงและชัด รวมทั้งเริ่มจาก motion (การเคลื่อนไหว) หลักก่อนเพิ่มรายละเอียดค่ะ citeturn19search0turn15view9

### ระบุ Actor ด้วยภาพ ไม่ใช่สรรพนาม

โครงสร้างที่เสี่ยง:

```text
He punches him, then he falls backward.
```

โมเดลต้องแก้ ambiguity (ความกำกวม) ว่า `he` แต่ละตัวคือใคร

โครงสร้างที่เสถียรกว่า:

```text
The red-jacket fighter drives one right straight punch toward the black-armored guard.
The black-armored guard's torso recoils backward and he takes one recovery step.
```

สำหรับฉากสองคน ควรคงฉลากเดิมทุกช็อต เช่น:

```text
RED FIGHTER = red leather jacket, screen left
BLACK GUARD = black tactical jacket, screen right
```

แล้วใช้ `red-jacket fighter` และ `black-armored guard` ซ้ำโดยไม่เปลี่ยนคำเรียกค่ะ การคง character phrasing (ถ้อยคำบรรยายตัวละคร) เดิมข้ามช็อตเป็นแนวทางที่ OpenAI แนะนำโดยตรงสำหรับ Sora 2 และงาน SocialDirector ก็พบว่าการแยก token (โทเคน) ของผู้แสดงแต่ละคนช่วยลด actor-action mismatch (คนผิดคนทำแอ็กชัน) ค่ะ citeturn16view3turn17view3

### อย่าฝืนสร้าง “เฟรมสัมผัสสมบูรณ์”

ช็อตที่มีความเสี่ยงสูงสุดมักเป็นช่วง 2–4 เฟรมรอบจุด contact (สัมผัส) ดังนั้นในงานภาพยนตร์ AI ควรออกแบบให้สิ่งสำคัญคือ **ผู้ชมรับรู้แรง** มากกว่าต้องมองเห็นนิ้ว มือ หมัด หรือใบดาบสัมผัสกันชัดทุกพิกเซลค่ะ นี่เป็นข้อเสนอเชิง production inference (ข้อสรุปเพื่อการผลิต) จากข้อจำกัดด้าน occlusion, actor binding และ physics ที่งานวิจัยข้างต้นพบค่ะ citeturn15view2turn17view2turn17view3

สูตรที่มีประสิทธิภาพคือ:

**Visible approach → FX occlusion → visible recoil**

เช่น

```text
The fist closes the final gap.
A sharp burst of rain spray erupts between both jackets at impact.
The guard's shoulders recoil backward.
Both fighters separate immediately.
```

น้ำ ฝุ่น ควัน เศษปูน ประกายไฟ หรือเศษกระจกจึงไม่ได้เป็นแค่ decoration (ของตกแต่ง) แต่เป็น **collision mask (ตัวพรางช่วงปะทะ)** ที่ซ่อนเฟรมซึ่งโมเดลมีโอกาสรวมรูปร่างเข้าหากันมากที่สุดค่ะ

## กฎเฉพาะของ Kling, Seedance, Hailuo, Runway และ Sora

### Kling: Motion Brush ควรคุมมวลหลัก ไม่ใช่วาดเส้นทางทุกแขนขา

Kling อธิบาย prompt grammar (ไวยากรณ์พรอมป์ต์) ของตัวเองเป็น:

`Subject + Subject Movement + Scene + optional Camera Language + Lighting + Atmosphere`

และระบุว่า Subject Movement ควรตรงไปตรงมาและเหมาะกับระยะเวลาคลิป ไม่ใช่ choreography ยาว ๆ ค่ะ citeturn15view4

สำหรับ **Motion Brush (แปรงกำหนดการเคลื่อนไหว)** Kling รองรับการกำหนด trajectory (วิถี) ให้ element และรองรับสูงสุดหกองค์ประกอบ แต่เอกสารแนะนำให้ brush เฉพาะส่วนสำคัญเมื่อจำเป็นต้องการความแม่นยำ และ text prompt ต้องสอดคล้องกับพื้นที่ที่ brush ไว้ หากไม่ต้องการให้กล้องลอยตาม trajectory สามารถใช้ Static Brush (แปรงตรึงภาพ) ตรึงพื้นหลังได้ค่ะ citeturn15view3

สำหรับ fight scene ให้ใช้ Motion Brush แบบนี้:

| สิ่งที่ควรทำ | เหตุผล |
|---|---|
| Brush ลำตัว+ไหล่ของ attacker เป็นมวลหลักหนึ่งก้อน | ให้โมเดลเห็น trajectory ของ center mass (มวลกลาง) ก่อนปลายแขน |
| ใช้ trajectory สั้นและทิศเดียว | ลดการตีความเป็น choreography หลายขั้น |
| ถ้ากล้องต้องนิ่ง ให้ Static Brush พื้น/กำแพง | Kling ระบุว่า Static Brush สามารถตรึงพิกเซลและลด camera movement ที่ไม่ต้องการได้ citeturn15view3 |
| Prompt ให้ตรงกับ Brush | เอกสาร Kling ระบุให้ใช้รูปแบบ “element + motion” ที่สอดคล้องกับพื้นที่และวิถีค่ะ citeturn15view3 |

ตัวอย่างสำหรับ high-speed strike (การโจมตีความเร็วสูง):

```text
The red-jacket fighter plants his lead foot and drives his upper body forward once,
rotating his hips into one straight right punch.
The black-jacket fighter recoils backward one step.
Rain spray bursts outward at the instant of impact.
Medium shot, fast lateral tracking.
```

สำหรับ Kling รุ่นใหม่กว่า 2.0 การใช้ visual reference (ภาพ/วิดีโออ้างอิง) ยิ่งน่าสนใจ เพราะ Kling 2.0 เปิดแนวคิด Multi-modal Visual Language และ Kling 3.0 ปัจจุบันเพิ่ม multi-shot, references และ consistency controls มากขึ้น อย่างไรก็ตาม แม้เวอร์ชันใหม่จะดีขึ้น ก็ไม่ควรตีความว่าปัญหา multi-body collision หายไปค่ะ citeturn18search4turn18search2

### Seedance: ใช้ Reference และ Timestamp มากกว่าคิดค้น Camera Tag

จุดสำคัญคือ **Seedance/Jimeng ไม่ควรถูกพรอมป์ต์ด้วย syntax `[Camera tags]` แบบ Hailuo โดยอัตโนมัติ** ค่ะ เอกสาร ByteDance ที่พบใช้ natural-language camera description (คำบรรยายกล้องภาษาธรรมชาติ), reference assets (`@Image`, `@Video`) และคำสั่งตามช่วงเวลา ไม่ได้ระบุชุด bracket command 15 คำแบบ MiniMax ค่ะ citeturn15view5turn16view9

Seedance 2.0 สามารถอ้างอิง motion (การเคลื่อนไหว), camera movement (การเคลื่อนกล้อง) และ VFX (เอฟเฟ็กต์ภาพ) จากวิดีโออ้างอิง และตัวอย่างอย่างเป็นทางการใช้ช่วงเวลาเช่น `[0s-3s]`, `[3s-6s]`, `[6s-8s]` เพื่อกำหนดลำดับกล้องและการกระทำ ส่วน Seedance 2.5 เพิ่ม timestamp-level control และเพิ่มจำนวน reference อย่างมากค่ะ citeturn15view6turn16view9turn15view5

สำหรับ fight beat (จังหวะการต่อสู้) สามารถประยุกต์เป็น:

```text
Reference @Image1 for the red-jacket fighter.
Reference @Image2 for the black-armored guard.
Reference @Video1 only for the lateral camera movement and impact timing.

[0.0s-0.5s] Medium shot. The red-jacket fighter plants his lead foot and rotates his hips.
[0.5s-0.8s] He drives one right straight punch toward the guard's upper chest.
Rain spray bursts sharply between them at impact; brief handheld camera jolt.
[0.8s-1.5s] The guard's torso recoils backward and he takes one recovery step.
The two fighters clearly separate.
```

การใช้ช่วงเวลา 0.0–1.5 วินาทีด้านบนเป็น **production adaptation (การประยุกต์เพื่อการผลิต)** จากรูปแบบ timestamp ของ ByteDance ไม่ใช่การรับประกันว่า UI ทุกเวอร์ชันจะบังคับ timing ได้ระดับเฟรมค่ะ ความได้เปรียบของ Seedance 2.5 คือสามารถให้อ้างอิง motion/camera จากวิดีโอโดยตรงแทนการอธิบาย choreography ทั้งหมดด้วยภาษา ซึ่ง ByteDance ระบุเป็น capability หลักของสาย Seedance 2.x ค่ะ citeturn15view5turn16view9

### Hailuo: `[Camera command]` เป็น syntax จริง และควรใช้ไม่เกินที่จำเป็น

MiniMax API ระบุ camera commands (คำสั่งกล้อง) สำหรับ Hailuo 2.3, 2.3 Fast, Hailuo-02 และ I2V-01-Director โดยตรง เช่น `[Pan left]`, `[Push in]`, `[Shake]`, `[Tracking shot]`, `[Static shot]` เป็นต้น คำสั่งหลายตัวสามารถรวมในวงเล็บเดียวกันได้ โดย MiniMax แนะนำสูงสุดสามคำสั่ง และบอกว่า explicit command (คำสั่งระบุชัด) ให้ความแม่นยำกว่าภาษาธรรมชาติค่ะ citeturn17view0

สำหรับ combat (การต่อสู้) ไม่ควรใช้สามคำสั่งพร้อมกันเพียงเพราะระบบรองรับ ตัวอย่างที่คุมง่ายกว่า:

```text
[Tracking shot]
The armored fighter charges left to right and drives one shoulder into the opponent's upper torso.
Dust bursts outward at contact.
The opponent recoils backward one step.

[Shake]
A single sharp impact jolt, then the camera immediately settles.
```

หากใช้ API และต้องการให้ข้อความของเราคงรูปมากที่สุด MiniMax ระบุว่า `prompt_optimizer=false` สามารถใช้เพื่อ precision control (ควบคุมแม่นยำ) มากขึ้นได้ค่ะ citeturn17view0

Hailuo 2.3 ถูก MiniMax ระบุว่าปรับปรุง complex body movement (การเคลื่อนไหวร่างกายซับซ้อน), naturalness (ความเป็นธรรมชาติ) และ response to motion commands (การตอบสนองต่อคำสั่งการเคลื่อนไหว) แต่คำกล่าวนี้เป็นข้อมูลจากผู้ผลิต จึงควรถือว่าเป็น capability claim ไม่ใช่หลักฐานว่าการชนสองคนสมบูรณ์ทุกกรณีค่ะ citeturn17view6

### Runway: Gen-3/Act-One เป็น Legacy แล้ว และ Gen-4.5 ต้องเขียน “การเคลื่อนไหว” ไม่ใช่อธิบายภาพซ้ำ

ณ วันที่รายงานนี้ Gen-3 Alpha/Alpha Turbo และ Act-One ไม่ให้ใช้แล้ว โดย Runway ชี้ไปที่ Gen-4.5 สำหรับ T2V/I2V, Animate Frames สำหรับ keyframes และ Act-Two สำหรับ performance capture ค่ะ citeturn15view8

สำหรับ I2V ของ Gen-4.5 รูปอินพุตกำหนด subject, composition, lighting และ style ไปแล้ว จึงควรให้ prompt บรรยาย **สิ่งที่จะเคลื่อนไหว, กล้อง, timing, direction และ speed** แทนการเล่ารายละเอียดภาพเดิมซ้ำค่ะ citeturn15view9turn16view7

ดังนั้น:

ไม่ดี:

```text
A tall fighter with black hair wearing a red jacket in a rainy neon alley
with another man wearing black tactical clothing, cinematic alley...
```

ดีกว่าเมื่อภาพแรกมีรายละเอียดเหล่านั้นอยู่แล้ว:

```text
The left fighter plants his front foot, rotates his hips, and drives one straight punch.
The right fighter's torso recoils backward and he takes one recovery step.
Rain spray bursts at impact.
The handheld camera jolts once, then stabilizes.
```

Runway ยังระบุชัดว่า Gen-4 ไม่รองรับ negative phrasing (การสั่งเชิงปฏิเสธ) อย่างเหมาะสม และคำเช่น “no camera movement” อาจได้ผลตรงข้าม จึงควรเปลี่ยนจาก:

```text
no extra limbs, no morphing, no distorted hands, no camera movement
```

เป็น positive state (สถานะเชิงบวก):

```text
locked camera.
Both fighters remain visually distinct.
The left fighter keeps a clear silhouette.
The right fighter takes one backward recovery step.
```

ข้อแรกเรื่อง positive prompting เป็นคำแนะนำตรงจาก Runway ส่วนข้อความ “both fighters remain visually distinct” เป็นเทคนิคเชิง production เพื่อเพิ่ม spatial intent (เจตนาตำแหน่ง) ไม่ใช่ guarantee (การรับประกัน) ว่าจะห้าม hallucination ได้ค่ะ citeturn19search0turn19search9

Act-Two เหมาะกับการถ่ายทอด driving performance (วิดีโอการแสดงต้นแบบ) ไปยังตัวละคร โดย character image รองรับ gesture/body control ขณะที่ character video จะคง motion/environment/camera เดิมและควบคุมใบหน้าเป็นหลัก เพราะระบบถูกออกแบบรอบ performance input หนึ่งชุด จึงเหมาะอย่างยิ่งกับการทำ **solo attack plate (เพลตแอ็กชันตัวละครเดี่ยว)** หรือ **reaction plate (เพลตปฏิกิริยา)** แล้วตัดต่อรอบจุด collision มากกว่าการหวังให้มันเป็น two-person collision solver โดยตรงค่ะ ข้อเสนอช่วงท้ายนี้เป็น production inference จากรูปแบบอินพุตของ Act-Two ค่ะ citeturn16view6

### Sora: One Action + One Camera Move + Beat Count

OpenAI ระบุโดยตรงใน Sora 2 Prompting Guide ว่า movement เป็นส่วนที่ทำยาก และแนะนำให้แต่ละ shot มี **one clear camera move (การเคลื่อนกล้องหลักหนึ่งอย่าง)** และ **one clear subject action (การกระทำหลักหนึ่งอย่าง)** พร้อมอธิบาย timing เป็น beats/counts (จังหวะ/จำนวนครั้ง) ค่ะ citeturn17view1

ดังนั้นอย่าใช้:

```text
Two fighters exchange a rapid sequence of punches, blocks, kicks and throws
while the camera spins dynamically around them.
```

ให้แบ่ง:

```text
Medium shot, tracking left to right.
The red-jacket fighter takes one step forward and throws one straight right punch.
The black-jacket fighter recoils one step.
```

แล้วค่อย cut (ตัดภาพ) ไป:

```text
Low-angle medium shot.
The black-jacket fighter pivots and drives one front kick.
The red-jacket fighter slides backward across the wet pavement.
```

Sora รองรับ image reference ซึ่งทำหน้าที่เป็น first frame (เฟรมแรก) และ OpenAI แนะนำให้ใช้คำบรรยาย character เดิมซ้ำเพื่อ continuity (ความต่อเนื่อง) ค่ะ citeturn16view2turn16view3

อย่างไรก็ตาม ณ 19 สิงหาคม 2026 ควรระวังเรื่อง pipeline ระยะยาว เพราะ OpenAI ระบุว่า Sora 2 และ Videos API จะปิดวันที่ **24 กันยายน 2026** ค่ะ citeturn16view1

## I2V และ Keyframe Pipeline สำหรับแอ็กชันประมาณหนึ่งวินาทีครึ่ง

“Foolproof (พลาดไม่ได้)” ในความหมายตามตัวอักษรยังไม่มีค่ะ เพราะ benchmark ปัจจุบันยังแสดงข้อผิดพลาดด้านฟิสิกส์ชัดเจน แต่เราสามารถออกแบบ workflow ให้ **ลดจำนวนสิ่งที่โมเดลต้องเดา** จนเข้าใกล้ deterministic production (การผลิตที่คาดเดาผลได้) มากขึ้นได้ค่ะ citeturn17view2

### ใช้สองคีย์เฟรมเป็น “สถานะ” ไม่ใช่สร้างสองภาพใหม่แบบอิสระ

ให้มี:

**P0 — Pre-impact keyframe (คีย์เฟรมก่อนปะทะ)**  
**P1 — Post-impact keyframe (คีย์เฟรมหลังปะทะ)**

อย่าพยายามสร้าง P0 และ P1 จาก text-to-image แยกคนละ generation เพราะคุณกำลังเปิดโอกาสให้หน้า เสื้อผ้า กล้อง lens geometry และฉากเปลี่ยนโดยไม่จำเป็น

ทางที่เสถียรกว่าคือสร้าง P0 ให้สมบูรณ์ แล้วใช้ image editing (แก้ไขภาพ) สร้าง P1 จากภาพเดิม โดยเปลี่ยนเฉพาะ pose/reaction/FX ค่ะ FLUX.2 ปัจจุบันรองรับ image editing และ multi-reference สูงสุดแปดภาพผ่าน API หรือสิบภาพใน playground ส่วน Midjourney มี Editor, layers และ selective editing; Omni Reference ใช้อ้างอิงตัวละครหรือวัตถุใน V7 ได้ค่ะ citeturn17view5turn17view4turn15view12

### การจัด P0 ก่อนปะทะ

P0 ที่ดีไม่ควรเป็น “หมัดชนหน้าอยู่แล้ว” แต่ควรมี:

| ตัวแปร | ค่าแนะนำเชิง production |
|---|---|
| นักแสดง | สอง silhouette (เงาร่าง) แยกอ่านออกชัด |
| ด้านจอ | A อยู่ซ้าย, B อยู่ขวา และคงตลอดช็อต |
| มือ/อาวุธ | ยังมี visible gap (ช่องว่างที่มองเห็น) ก่อนสัมผัส |
| เท้า | อย่างน้อย support foot (เท้ารับน้ำหนัก) ของผู้โจมตีอ่านออก |
| ลำตัว | attacker อยู่ช่วง loaded pose (ท่าเตรียมแรง) ไม่ใช่ท่าเริ่มนิ่ง |
| กล้อง | medium/medium-wide ดีกว่า extreme close contact เมื่อยังทดสอบ topology |
| ฉาก | มีพื้นที่ให้ B ถอยในทิศที่ต้องการ |

ตัวอย่างคีย์เฟรม:

```text
Cinematic pre-impact action keyframe.
Red-jacket fighter on screen left, black-armored guard on screen right.
The red-jacket fighter has his lead foot planted and his right shoulder loaded backward.
His right fist is approaching but has not yet touched the guard.
A clear visible gap remains between both bodies.
Both silhouettes are readable.
Wet neon alley, rain, medium shot, eye level, 35mm cinematic framing.
```

ข้อความ `clear visible gap` เป็นการกำหนดองค์ประกอบเชิงบวก ไม่ใช่ negative prompt ค่ะ แนวคิดนี้ใช้ spatial separation เพื่อไม่เริ่ม I2V จากเฟรมที่มี occlusion รุนแรง ซึ่งเป็น production inference จากปัญหา close-contact segmentation และ multi-person control ที่งานวิจัยบันทึกไว้ค่ะ citeturn15view2turn17view3

### สร้าง P1 ด้วย State Delta

ใช้ P0 เป็น input แล้วแก้เฉพาะ “สิ่งที่แรงกระแทกเปลี่ยน”

```text
Keep the same characters, wardrobe, alley, camera position, lens, lighting and composition.
Change only the body positions after impact:

The red-jacket fighter has completed the forward follow-through of one straight right punch.
The black-armored guard is displaced backward, shoulders rotated away,
knees flexed, taking one recovery step.
Both fighters are separated again.
Rain spray hangs in the air between them.
```

นี่คือ **state delta (ผลต่างของสถานะ)** หลักการคือไม่ขอให้ image model “สร้างฉากใหม่” แต่ให้ “แก้ผลของแรง” เท่านั้น FLUX.2 ถูกออกแบบรองรับ reference-based editing ส่วน Midjourney Editor สามารถแก้เฉพาะพื้นที่ที่เลือกและคงพื้นที่ที่ไม่ได้เปิดเป็น transparency ไว้ได้ค่ะ citeturn17view5turn17view4

### อย่าวางคีย์เฟรมปลายไว้ตรง “เฟรมหมัดติดหน้า”

P1 ที่ดีควรเป็น **หลัง impact ไปแล้วเล็กน้อย**:

```text
A → follow-through
B → recoil
FX → expanding outward
Bodies → separated
```

แทน:

```text
A's fist pressed against B's face
```

เพราะ sustained overlap (การซ้อนตัวต่อเนื่อง) คือช่วงที่ต้องรักษา topology ของสองร่างพร้อมกันมากที่สุด และ close-contact occlusion เป็นโจทย์ที่ยากโดยตัวมันเองค่ะ citeturn15view2

### Motion prompt ระหว่าง P0 → P1 ต้องสั้นมาก

เมื่อ input image กำหนดภาพอยู่แล้ว คำสั่ง I2V ที่เหมาะคือ:

```text
The left fighter drives forward once into a straight punch.
At impact, rain spray bursts outward.
The right fighter's torso recoils and he takes one backward recovery step.
The camera jolts once and immediately stabilizes.
```

ไม่ต้องใส่ซ้ำ:

```text
cyberpunk alley, red leather jacket, wet pavement, blue neon...
```

ถ้าสิ่งเหล่านั้นล็อกอยู่ใน input image แล้ว Runway แนะนำตรง ๆ ว่า I2V prompt ควรเกือบทั้งหมดอธิบาย motion และ Sora ใช้ image input เป็น first-frame anchor เช่นกันค่ะ citeturn15view9turn16view2

### หนึ่งวินาทีครึ่งควรเป็น “Editorial Beat” ไม่ใช่ความยาว Generation

ความยาวประมาณ **1.0–1.5 วินาที** เหมาะมากในฐานะ editorial beat (ช่วงที่ใช้จริงในการตัดต่อ) แต่ไม่ควรสมมติว่าทุกแพลตฟอร์ม render 1.5 วินาที native ได้ค่ะ ตัวอย่างเช่น Runway Gen-4.5 ปัจจุบันรองรับ 2–10 วินาที ขณะที่ Hailuo 2.3 API ใช้ 6 หรือ 10 วินาทีตาม resolution/model และ Kling documentation รุ่น 2.x อธิบาย output 5 หรือ 10 วินาทีค่ะ citeturn16view7turn17view0turn15view4

ดังนั้น workflow ที่แม่นกว่าคือ:

**Generate shortest practical clip → หา 24–36 เฟรมที่ดีที่สุด → trim เหลือ 1–1.5 วินาที → cut before topology fails**

หลักนี้สำคัญมากค่ะ เพราะคุณไม่ได้ต้องการ “วิดีโอ AI สมบูรณ์หกวินาที” คุณต้องการ **36 เฟรมที่หลอกตาได้ดีพอสำหรับหนัง 24 fps** เท่านั้น

## Anti-Patterns และคำแทนแบบ Kinematic

คำที่อันตรายไม่ได้ “ถูกห้ามโดยโมเดล” แต่เป็นคำที่โยน degrees of freedom (องศาอิสระในการตีความ) จำนวนมากให้โมเดลค่ะ Runway แนะนำตรง ๆ ว่าคำ abstract (นามธรรม) และ prompt ที่ซับซ้อนเกินไปเพิ่มโอกาสเกิดพฤติกรรมที่ไม่คาดคิด ส่วน Sora แนะนำให้ลดแต่ละ shot เหลือ action หลักกับ camera movement หลักค่ะ citeturn19search0turn17view1

| Anti-pattern | ปัญหาที่โมเดลต้องแก้ | Kinematic replacement (คำแทนเชิงจลนศาสตร์) |
|---|---|---|
| `complex martial arts choreography` | หลาย action + หลาย contact + limb crossing | `plants lead foot, rotates hips, throws one right straight punch; opponent recoils one step` |
| `fast fight / intense fight` | ไม่มี trajectory หรือจำนวนเหตุการณ์ | `fighter advances one step and drives one shoulder strike; receiver steps backward once` |
| `rapid exchange of punches` | สลับผู้โจมตีหลายครั้ง | แยกเป็น 3 cuts; `one punch per shot` |
| `grappling` | sustained body overlap (ซ้อนตัวต่อเนื่อง) | `one shoulder shove`; หรือ cut ก่อนล็อก แล้วเริ่มช็อตใหม่ที่ post-control pose |
| `wrestling` | แขน/ขาไขว้และ torso overlap จำนวนมาก | `fighter drives forward at the torso; opponent pivots and loses balance` |
| `wrist lock` | ต้องแม่นระดับมือ นิ้ว ข้อมือ และข้อศอก | `fighter redirects the opponent's forearm outward; cut`; ช็อตถัดไปเริ่มจากแขนถูกพับแล้ว |
| `armbar` | limb topology ยากมาก | สร้าง pose ปลายเป็น keyframe แยก ไม่ generate transition ต่อเนื่อง |
| `disarms him` | มือสองคน + อาวุธหนึ่งชิ้นเปลี่ยน owner | `guard's weapon is knocked sideways`; cut; next shot starts with weapon on floor |
| `spin kick combo` | หมุน 3D + occlusion + contact + recovery | `rear leg is chambered; fighter extends one side kick; support foot stays planted` |
| `throws him across the room` | grip + lift + release + flight | `fighter drives one shoulder tackle`; cut; next shot starts with opponent already airborne/backward |
| `smashes him through the wall` | body collision + wall collision + debris simultaneously | Shot A tackle → Shot B debris wipe → Shot C bodies emerge through broken wall |
| `sword fight` | ไม่รู้ว่าใครฟัน ใครรับ และอาวุธไปทางใด | `swordsman delivers one downward diagonal cut; shield bearer braces; blade strikes shield rim` |
| `blades clash rapidly` | จุดชนเคลื่อนหลายครั้ง | `one blade meets the shield rim; sparks fan sideways; blade glances away` |
| `gun-fu` | มือ ปืน opponent และ trajectory หลายชุด | แตกเป็น `pivot`, `shoulder check`, `one controlled weapon movement` คนละช็อต |
| `car crash` | ไม่ระบุรถไหนชนตรงไหนและหมุนทิศใด | `black car's front-left bumper clips silver car's rear-right quarter; silver car yaws clockwise` |

### คลังคำกริยาที่โมเดลตีความเป็นการเคลื่อนไหวได้ตรงกว่า

คำเหล่านี้มีประโยชน์เพราะแต่ละคำสื่อ trajectory, joint action หรือ consequence (ผลลัพธ์) ที่สังเกตได้ ไม่ใช่อารมณ์เชิงนามธรรมค่ะ หลักการนี้สอดคล้องกับ Runway และ Sora ที่แนะนำ physical/direct movement descriptions ค่ะ citeturn19search0turn17view1

| หน้าที่ | Verbs |
|---|---|
| สร้างฐานแรง | `plants the lead foot` (วางเท้าหน้าแน่น), `braces` (ตั้งรับแรง), `lowers the stance` (ลดระดับท่า) |
| สร้างแรงหมุน | `pivots` (หมุนบนจุดยึด), `rotates the hips` (หมุนสะโพก), `turns the shoulders` (หมุนหัวไหล่) |
| ส่งแรง | `drives forward` (ส่งแรงไปข้างหน้า), `extends once` (เหยียดหนึ่งครั้ง), `thrusts` (แทง/ดันตรง), `shoves` (ผลัก) |
| เปลี่ยนทิศ | `deflects` (เบี่ยงออก), `redirects` (เปลี่ยนวิถี), `glances off` (เฉไถลออก), `ricochets` (กระเด้งเปลี่ยนทิศ) |
| รับแรง | `recoils` (สะท้อนถอย), `compresses` (ยุบรับแรง), `buckles` (ทรุด), `pivots backward` (หมุนถอย) |
| Recovery | `takes one recovery step` (ก้าวตั้งหลักหนึ่งก้าว), `regains footing` (กลับมาตั้งหลัก), `slides backward` (ไถลถอย) |
| ของแข็ง/อาวุธ | `scrapes` (ครูด), `strikes` (ปะทะ), `glances away` (เฉออก), `splinters` (แตกเป็นเสี้ยน) |
| รถ | `clips` (เฉี่ยวกระแทก), `yaws` (หมุนรอบแกนดิ่ง), `skids` (ไถล), `countersteers` (หักพวงมาลัยแก้อาการ), `swerves` (หักหลบ) |
| FX | `bursts outward` (ระเบิดพุ่งออก), `blooms` (พวยพุ่งขยาย), `fans sideways` (แผ่ออกด้านข้าง), `trails behind` (ลากตามหลัง) |

คำรับแรงที่ดีมากสำหรับมนุษย์คือ:

```text
torso recoils first,
shoulders rotate backward,
the head follows a fraction later,
one recovery step follows
```

แทน `his head violently snaps back` เพราะคำหลังมีโอกาสผลักโมเดลไปสู่ neck deformation (คอบิดผิดรูป) ได้มากกว่าในเชิง production ค่ะ

## กล้องและ FX สำหรับพราง Collision โดยไม่ทำให้ฉากยุ่งขึ้น

Motion blur (ภาพเบลอจากการเคลื่อน), camera shake (การสั่นกล้อง), spray/dust/debris (ละออง/ฝุ่น/เศษวัสดุ) ควรทำงานเป็น **impact punctuation (เครื่องหมายเน้นจังหวะกระแทก)** ไม่ใช่เกิดตลอดช็อตค่ะ หากทุกอย่างสั่นและฟุ้งตลอด ผู้ชมจะไม่มี baseline ให้รู้สึกถึง “แรงกระแทกที่เกิดขึ้นทันที”

โครงสร้างที่ดี:

```text
0.00–0.45  stable tracking
0.45–0.60  acceleration
0.60–0.70  impact + FX burst + one camera jolt
0.70–1.20  recoil
1.20–1.50  camera settles
```

Sora แนะนำให้ motion มี beat/count ชัดเจน ส่วน Seedance มีตัวอย่าง official ที่แบ่ง camera/action ตาม timestamp และ Hailuo รองรับ `[Shake]` กับ `[Tracking shot]` เป็นคำสั่งโดยตรงค่ะ citeturn17view1turn16view9turn17view0

### FX ที่มีประโยชน์ตามชนิดปะทะ

| Contact | FX mask ที่เหมาะ | Prompt phrase |
|---|---|---|
| หมัดในฝน | rain spray จากเสื้อ/ผิว | `a tight burst of rain spray erupts outward at impact` |
| รองเท้ากระแทกพื้น | ฝุ่น/น้ำ | `puddle water kicks outward beneath the planted foot` |
| ตัวชนกำแพง | plaster dust | `a dense burst of plaster dust blooms between both bodies and the wall` |
| ดาบชนโล่ | sparks + splinters | `a brief fan of sparks and wood splinters bursts from the shield rim` |
| รถเฉี่ยวกัน | tire smoke + fragments | `tire smoke snaps outward; small body-panel fragments trail behind` |
| ประตูถูกกระแทก | dust + wood chips | `wood chips and dust burst inward as the door swings open` |

คำอย่าง `dense`, `brief`, `tight burst`, `single burst` มีประโยชน์กว่า `huge cinematic explosion everywhere` เพราะเราต้องการ FX เฉพาะบริเวณ contact ไม่ใช่เพิ่มวัตถุเคลื่อนที่ทั่วเฟรมค่ะ

### Whip pan และ Crash Zoom ใช้เป็น “Edit Mask”

`whip pan (แพนกล้องสะบัดเร็ว)` มีประโยชน์เมื่อคุณไม่ต้องการแสดง contact เลย:

**Shot A:** attacker เริ่มพุ่ง → whip pan  
**Cut inside blur**  
**Shot B:** receiver อยู่ post-impact แล้ว

วิธีนี้ให้ NLE/editor (โปรแกรมตัดต่อ) เป็นผู้ทำ collision แทน video model ค่ะ

`crash zoom (ซูมพุ่งเร็ว)` เหมาะกว่าใน impact ที่ยังอ่าน silhouette ได้ เช่น ดาบกระแทกโล่หรือรถเฉี่ยวกัน แต่ไม่ควรใช้พร้อม orbit + whip + shake + zoom ใน shot เดียว เพราะคุณเพิ่ม motion constraints พร้อมกันมากเกินไป หลักหนึ่ง camera move ต่อ shot ของ Sora เป็นแนวทางอนุรักษนิยมที่เหมาะใช้ข้ามโมเดลค่ะ citeturn17view1

สำหรับ Hailuo เช่น:

```text
[Push in]
The sword descends toward the shield.

[Shake]
The blade strikes the shield rim. Sparks burst once.

The camera immediately settles.
```

สำหรับ Seedance ให้ใช้ภาษากล้องธรรมชาติ:

```text
[0.0s-0.6s] slow forward tracking toward both fighters.
[0.6s-0.7s] one brief handheld impact jolt as the blade meets the shield.
[0.7s-1.5s] camera stabilizes while the blade glances away.
```

อย่าแปลง Seedance เป็น `[Tracking shot] [Shake]` เพียงเพราะ Hailuo ใช้รูปแบบนั้นค่ะ ชุด bracket command ดังกล่าวมีหลักฐานอย่างเป็นทางการสำหรับ MiniMax/Hailuo โดยเฉพาะ ส่วน ByteDance แสดงการใช้ natural-language/timestamp/reference workflow ค่ะ citeturn17view0turn16view9

## Benchmark Prompt Suite: ห้าลำดับแอ็กชันแบบสามช็อต

พรอมป์ต์ต่อไปนี้ตั้งใจเป็น **benchmark (ชุดทดสอบเปรียบเทียบ)** มากกว่าจะเป็น prose สวย ๆ ค่ะ แต่ละ sequence จำกัด collision หลักไว้หนึ่งครั้งต่อ shot และคงชื่อ actor เดิม เพื่อลด actor swap (สลับตัวละคร) ตามข้อค้นพบของงาน multi-person video และแนวทาง one-action-per-shot ของ Sora ค่ะ citeturn17view3turn17view1

สำหรับ **Runway/Sora/Kling** ใช้ camera sentence เป็นภาษาธรรมชาติ ส่วน **Hailuo** สามารถแทนด้วยคำสั่ง `[Tracking shot]`, `[Push in]`, `[Shake]` ตามที่ระบุค่ะ citeturn17view0

### Cyberpunk Alley Hand-to-Hand Brawl

**Shot A — Load / Approach**

```text
Medium-wide shot, lateral tracking left to right.

RED FIGHTER, wearing a red leather jacket on screen left,
plants his left lead foot in the wet pavement and rotates his hips toward BLACK GUARD.

BLACK GUARD, wearing a matte black tactical jacket on screen right,
braces with both feet planted.

RED FIGHTER drives his right shoulder forward and begins one straight right punch.
The fist approaches but the fighters remain visibly separated.
Rain streaks through neon light.
```

สำหรับ Kling ให้ Motion Brush บริเวณ torso+right shoulder ของ RED FIGHTER ไปทางขวา และใช้ Static Brush กับพื้น/กำแพงถ้าต้องการให้ camera movement มาจาก camera control ไม่ใช่ trajectory ค่ะ citeturn15view3

**Shot B — Impact Mask**

```text
Medium shot.

RED FIGHTER completes one straight right punch toward BLACK GUARD's upper chest.
At the instant of impact, a tight burst of rain spray erupts between both jackets.

BLACK GUARD's torso compresses and rotates backward.
His shoulders move first; his head follows a fraction later.

One brief handheld camera jolt at impact, then immediate stabilization.
No additional exchange; this is one impact beat.
```

สำหรับ Runway ตัดบรรทัดสุดท้าย `No additional exchange` ออก แล้วเขียนเชิงบวกแทน:

```text
The shot ends on this single impact beat.
```

เพราะ Runway แนะนำ positive phrasing และไม่สนับสนุน negative prompting อย่างเป็นทางการค่ะ citeturn19search0

**Shot C — Recoil / Separation**

```text
Medium-wide trailing shot.

RED FIGHTER finishes the forward follow-through and returns to a balanced stance.

BLACK GUARD is already separated from him,
stumbles backward exactly one step into a shallow neon-lit puddle,
and bends both knees to recover balance.

Water splashes outward from the recovery step.
The camera tracks backward with BLACK GUARD and settles.
```

จุดสำคัญคือ contact ถูกจำกัดไว้ Shot B เท่านั้น ส่วน Shot C เริ่มด้วย separation แล้ว จึงลดเวลาที่โมเดลต้องรักษาร่างสองคนซ้อนกันค่ะ ซึ่งเป็น production strategy จากข้อจำกัดด้าน occlusion ค่ะ citeturn15view2

### Tactical Gun-Fu Room Breach

เป้าหมาย benchmark นี้คือทดสอบตัวละครสองคนพร้อม prop (วัตถุประกอบ) โดยไม่สั่ง hand-to-hand weapon transfer ซึ่งเป็น constraint สูง

**Shot A — Entry**

```text
Wide shot from inside a dim concrete room, slight handheld tracking backward.

TACTICAL OPERATOR enters through the doorway in one controlled forward step,
holding the compact prop firearm close to the torso.

ROOM GUARD stands three meters ahead on screen right and turns toward the doorway.

The damaged door swings inward once.
A small burst of wood dust follows the door.
```

**Shot B — Broad Contact**

```text
Medium shot, camera tracking sideways.

ROOM GUARD advances one step toward TACTICAL OPERATOR.

TACTICAL OPERATOR pivots outside the guard's forward line
and drives one shoulder into ROOM GUARD's upper torso.

ROOM GUARD recoils sideways.
A short burst of plaster dust falls from the nearby wall.

One sharp camera jolt at contact.
The prop firearm remains in TACTICAL OPERATOR's hands throughout the shot.
```

การเลือก `shoulder into upper torso` แทน `wrist lock/disarm` ลด precision requirement (ความละเอียดที่ต้องแม่น) จากมือ+นิ้ว+อาวุธ+เจ้าของอาวุธ เหลือ broad-body collision (การชนพื้นที่กว้างของลำตัว) ซึ่งเป็น production simplification ค่ะ

**Shot C — Result State**

```text
Medium-wide shot, locked camera.

ROOM GUARD is already separated from TACTICAL OPERATOR
and falls backward across the edge of a lightweight table.
The table tips once and loose papers scatter into the air.

TACTICAL OPERATOR steps past the falling guard
while keeping the same two-handed prop position.

The shot ends with clear separation between both characters.
```

นี่ใช้ environment (สภาพแวดล้อม) เป็นหลักฐานของแรงแทนการบังคับให้คนสองคนติดกันนาน ๆ ค่ะ

### Medieval Sword Strike and Shield Clash

ฉากอาวุธมีข้อได้เปรียบตรงที่จุด contact สามารถถูกซ่อนด้วย sparks/splinters และ silhouette ของดาบ+โล่อ่านง่ายกว่ามือสองคนเกาะกัน

**Shot A — Weapon Paths Established**

```text
Medium-wide low-angle shot.

SWORDSMAN stands on screen left,
plants his forward foot and raises one longsword over his right shoulder.

SHIELD BEARER stands on screen right,
braces behind a round wooden shield with the shield rim angled toward the incoming blade.

Both weapons remain clearly separated.
The camera slowly pushes toward the space between them.
```

**Shot B — Single Clash**

```text
Medium close shot.

SWORDSMAN rotates his hips and delivers one downward diagonal sword strike,
traveling from upper right to lower left.

SHIELD BEARER holds the shield firm.

The blade strikes only the outer shield rim.
A brief fan of sparks and wooden splinters bursts sideways from the contact point.

One short impact camera shake.
```

Hailuo adapter:

```text
[Push in]
SWORDSMAN delivers one downward diagonal strike toward the shield rim.
The blade strikes the rim; sparks and splinters burst sideways.
[Shake]
```

MiniMax ระบุว่าคำสั่งสามารถวางตามลำดับได้ และ `[Shake]` เป็น camera command ที่รองรับโดยตรงค่ะ citeturn17view0

**Shot C — Momentum Resolution**

```text
Medium shot, slight lateral tracking.

The sword has already separated from the shield.

The blade glances downward and outward from the shield rim.
SWORDSMAN's arms continue the follow-through.

SHIELD BEARER absorbs the force by flexing both knees
and taking one short recovery step backward.

Loose wooden splinters continue falling between them.
```

คำ `glances downward and outward` มีประโยชน์มากกว่า `the sword bounces realistically` เพราะกำหนด vector (เวกเตอร์/ทิศทาง) หลังการชนโดยตรงค่ะ

### Superhuman Tackle Through Drywall

นี่เป็น benchmark ที่ยาก เพราะมีทั้ง human-human collision และ human-environment collision ต่อเนื่อง จึงควร **แยกเหตุการณ์ชนผนังออกจากช่วงสัมผัสคน** แทนการสร้างทั้งหมดใน take เดียวค่ะ หลักนี้สอดคล้องกับผล VideoPhy-2 ที่พบความยากเรื่อง momentum และกับปัญหา close-contact occlusion ค่ะ citeturn17view2turn15view2

**Shot A — Charge**

```text
Wide side-profile shot, fast tracking left to right.

SUPERHUMAN RUNNER accelerates across the room in a straight line.

ARMORED OPPONENT stands directly in the path near a drywall partition,
feet planted and shoulders squared.

SUPERHUMAN RUNNER lowers one shoulder while closing the final distance.
The shot cuts just before prolonged body overlap.
```

สำหรับ Runway เปลี่ยนประโยคสุดท้ายเป็น positive form:

```text
The shot ends as the shoulder reaches the opponent's upper torso.
```

เพื่อสอดคล้องกับ positive prompting ค่ะ citeturn19search0

**Shot B — Collision Hidden by Wall FX**

```text
Medium side shot.

SUPERHUMAN RUNNER drives forward through ARMORED OPPONENT's upper torso
in one continuous straight-line tackle.

Their combined forward momentum reaches the drywall immediately.

At wall impact, a dense burst of plaster dust and lightweight wall fragments
blooms across the contact area and briefly fills the center of frame.

The camera jolts once with the wall impact.
```

Shot นี้ตั้งใจให้ drywall dust เป็น **visual wipe (ภาพปาดบัง)** ช่วงที่ topology ของสองคนและกำแพงซ้อนกันมากที่สุดค่ะ

**Shot C — Post-Breakthrough State**

```text
Wide shot from the opposite side of the broken wall.

Plaster dust clears.

SUPERHUMAN RUNNER emerges forward and skids to a stop.
ARMORED OPPONENT is already separated,
sliding backward across the floor through scattered drywall fragments.

Both bodies retain distinct silhouettes.
The camera tracks backward with their momentum and then settles.
```

สิ่งที่ไม่ควรเขียนคือ:

```text
the superhero tackles him, grabs him, carries him through the wall,
flips over him, lands, and throws him aside
```

เพราะนั่นเปลี่ยนหนึ่ง collision เป็นหลาย ownership/contact states ต่อเนื่อง ซึ่งตรงข้ามกับ one-action-per-shot principle ค่ะ citeturn17view1

### Car Chase Vehicular Impact and Near-Miss

รถมีข้อได้เปรียบกว่ามนุษย์ตรง rigid-body silhouette (รูปร่างวัตถุแข็ง) แต่ต้องระบุ **ใครชนตรงส่วนไหน → รถคันไหนหมุนทิศใด** ให้ชัด ไม่ใช้คำกว้างอย่าง `cars collide dramatically`

**Shot A — Geometry Setup**

```text
Low leading tracking shot facing backward.

SILVER CAR drives straight in the center lane.
BLACK CHASE CAR approaches from behind, offset slightly to the silver car's right side.

BLACK CHASE CAR closes the distance without changing lanes.
Both vehicles remain clearly separated.

Road spray trails behind both cars.
```

**Shot B — Specific Contact and Yaw**

```text
Low three-quarter tracking shot.

BLACK CHASE CAR's front-left bumper clips
SILVER CAR's rear-right quarter panel once.

At contact, small body-panel fragments and road spray burst outward.

SILVER CAR immediately yaws clockwise and begins a lateral skid.
BLACK CHASE CAR continues forward along its original direction.

One brief impact camera shake.
```

นี่ดีกว่า:

```text
the cars smash into each other violently
```

เพราะระบุ contact point (จุดชน), striking body (รถที่กระแทก), receiving body (รถที่รับแรง), resulting rotation (การหมุนหลังชน) และ trajectory หลังชนแยกกันค่ะ การกำหนด consequence แบบนี้ออกแบบมาเพื่อลดปัญหา momentum ambiguity ซึ่ง VideoPhy-2 พบว่าเป็นจุดอ่อนสำคัญของโมเดลวิดีโอค่ะ citeturn17view2

**Shot C — Near-Miss Recovery**

```text
Long-lens leading tracking shot.

SILVER CAR continues the clockwise yaw,
then its front wheels countersteer and the tires regain grip.

The car slides laterally past a concrete barrier with a narrow visible gap.

Tire smoke and road dust trail behind the rear wheels.

BLACK CHASE CAR flashes past in the background.
The camera continues smoothly with SILVER CAR; no additional collision occurs.
```

สำหรับ Runway ให้เปลี่ยนบรรทัดสุดท้ายเป็น:

```text
The shot concludes on the clean near-miss as SILVER CAR regains grip.
```

เพื่อหลีกเลี่ยง negative wording ค่ะ citeturn19search0

### เกณฑ์ Benchmark ที่ควรวัด

เมื่อเทียบโมเดลหรือ seed (ค่าตั้งต้นสุ่ม) ไม่ควรตัดสินเพียงว่า “ดูเท่ไหม” แต่ให้ดูห้าแกนนี้ค่ะ ซึ่งสะท้อนปัญหาฟิสิกส์และ multi-person control ที่งานวิจัยปัจจุบันระบุไว้ citeturn17view2turn17view3

| Metric | สิ่งที่ต้องผ่าน |
|---|---|
| **Identity continuity (ความต่อเนื่องตัวตน)** | A ไม่เปลี่ยนหน้า/เสื้อและไม่กลายเป็น B |
| **Topology integrity (ความถูกต้องโครงสร้างร่างกาย)** | มือแขนยังสังกัดคนเดิม ไม่มีแขนงอก/ร่างหลอมรวม |
| **Actor-action binding (คนถูกคนทำแอ็กชัน)** | ผู้โจมตีเป็นผู้ส่งแรง ผู้รับแรงเป็นผู้ recoil |
| **Momentum continuity (ความต่อเนื่องโมเมนตัม)** | ทิศการเคลื่อนหลังชนสัมพันธ์กับก่อนชน |
| **Editability (ความง่ายในการตัดต่อ)** | มี pre-contact และ post-contact frames ที่สะอาดพอให้ตัดได้ |

แก่นของ production workflow ทั้งหมดจึงสรุปได้เป็น:

```text
ONE SHOT
= one primary actor action
+ one causal reaction
+ one dominant direction
+ one camera move
+ one short FX cue
```

และสำหรับ collision ที่ยากที่สุด:

```text
DO NOT GENERATE THE WHOLE FIGHT.

GENERATE:
PRE-IMPACT STATE
→ one readable kinetic transition
→ FX / camera masking at contact
→ POST-IMPACT STATE
→ CUT
```

นี่เป็นวิธีที่ใช้ประโยชน์จากสิ่งที่โมเดลปัจจุบันทำได้ดี—การสร้าง appearance, short motion และ cinematic camera language—พร้อมลดสิ่งที่หลักฐานงานวิจัยยังแสดงว่าเป็นจุดอ่อน ได้แก่ prolonged contact (การสัมผัสต่อเนื่อง), actor-action assignment (การกำหนดว่าใครทำอะไร), occlusion และ conservation of momentum (ความต่อเนื่องของโมเมนตัม) ค่ะ citeturn17view2turn17view3turn15view2