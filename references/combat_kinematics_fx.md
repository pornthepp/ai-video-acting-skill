# Kinematic Body Mechanics, Weight Transfer, and Environmental FX Masking for Believable Action in Generative AI Video

## บทสรุปเชิงปฏิบัติ

หัวใจของฉากต่อสู้ที่ “รู้สึกว่ามีน้ำหนัก” ในวิดีโอ Generative AI (ปัญญาประดิษฐ์สร้างวิดีโอ) ไม่ได้อยู่ที่การทำให้หมัดสัมผัสใบหน้าได้สมบูรณ์ทุกเฟรมค่ะ แต่เกิดจากการทำให้ **เหตุและผลของแรงต่อเนื่องกัน** ได้แก่ เท้ายึดพื้น → เชิงกรานเคลื่อน/หมุน → ลำตัวส่งแรง → แขนเร่ง → เกิดจังหวะกระแทก → ผู้รับแรงเสียสมดุล → สิ่งแวดล้อมตอบสนอง → กล้องตอบสนอง

งานชีวกลศาสตร์การชกพบว่าแรงและความเร็วของหมัดเกี่ยวข้องกับ Kinetic Chain (ห่วงโซ่การเคลื่อนไหวส่งแรง) ซึ่งเชื่อมจากเชิงกราน ลำตัว ไหล่ ศอก ไปถึงข้อมือ ขณะที่ Ground Reaction Force หรือ GRF (แรงปฏิกิริยาจากพื้น) และตำแหน่ง Center of Mass หรือ CoM (จุดศูนย์มวล) มีบทบาทต่อการทรงตัวและการถ่ายแรงค่ะ citeturn4view6turn1search6

สำหรับ Generative AI แนวทางนี้สอดคล้องกับเอกสารของ Runway รุ่น Gen-4.5 ที่แนะนำให้ระบุ Subject Action (การเคลื่อนไหวของตัวละคร), Environmental Motion (การเคลื่อนไหวของสภาพแวดล้อม), Camera Motion (การเคลื่อนไหวของกล้อง), Timing (จังหวะเวลา), Direction (ทิศทาง) และ Speed (ความเร็ว) อย่างชัดเจน โดยหลีกเลี่ยงฉากที่มีคำสั่งหลายเหตุการณ์ซ้อนกันเกินไปค่ะ citeturn9view0turn9view1turn9view3

ดังนั้นสูตรที่ให้ผลน่าเชื่อถือกว่าการเขียนเพียงว่า `powerful punch` คือ

> **Preparation → Plant → Drive → Rotate → Strike → Impact Reaction → Recovery**

หรือในภาษา Prompt (คำสั่งสร้างภาพ)

> **ตั้งฐาน → ส่งน้ำหนัก → หมุนลำตัว → ปล่อยอาวุธ → เหตุการณ์กระแทก → ปฏิกิริยารับแรง → คืนสมดุล**

หลักสำคัญที่สุดคือ **อย่าให้โมเดลต้องแก้โจทย์ “แขนสองคนสัมผัสกันอย่างแม่นยำ + การล้ม + กระจกแตก + กล้องหมุน + ฝน + เสื้อผ้าพริ้ว” พร้อมกันในเฟรมเดียว** เพราะเอกสาร Runway ระบุโดยตรงว่าคำสั่งซับซ้อนมากเกินไปอาจทำให้โมเดลพยายามประนีประนอมข้อกำหนดหลายอย่างจนเกิดผลลัพธ์ผิดธรรมชาติได้ค่ะ citeturn9view2turn9view3

## กลศาสตร์ของผู้โจมตีที่ผู้ชมมองเห็นได้

### จุดศูนย์มวลและการถ่ายน้ำหนัก

ในภาษาภาพยนตร์ คำว่า “ถ่ายน้ำหนัก” ไม่ควรถูกแปลเป็นเพียงการเอียงลำตัวไปข้างหน้า สิ่งที่ผู้ชมอ่านว่า “มีแรง” จริง ๆ คือการเปลี่ยนตำแหน่ง CoM (จุดศูนย์มวล) เมื่อเทียบกับ Base of Support (ฐานรองรับจากตำแหน่งเท้า) พร้อมกับแรงจากพื้น การเคลื่อนของเชิงกราน และการเร่งของส่วนร่างกายที่ต่อเนื่องกันค่ะ งานศึกษาการชกวัดทั้ง GRF, CoM, ความเร็วหมัด และการมีส่วนร่วมของส่วนร่างกายเพื่ออธิบายกลไกดังกล่าวโดยตรง citeturn4view6turn1search19

สำหรับภาพที่ดูเป็นธรรมชาติ ให้คิดว่าร่างกายมีสามสถานะ:

**ก่อนโจมตี:** CoM อยู่ระหว่างเท้าหรือใกล้ฐานที่มั่นคง เข่าไม่ล็อก และสะโพกยังมีพื้นที่สำหรับหมุน

**ขณะส่งแรง:** เชิงกรานและลำตัวเคลื่อนเข้าสู่ทิศทางของการโจมตี โดยน้ำหนักไม่ควรดูเหมือน “ร่างทั้งร่างลอยไปพร้อมกำปั้น”

**หลังจังหวะกระแทก:** โมเมนตัมยังดำเนินต่อเล็กน้อย ก่อนร่างกายเบรกและคืนสมดุล

การรักษา CoM ให้อยู่สัมพันธ์กับฐานรองรับสำคัญมากต่อความน่าเชื่อถือของการทรงตัว และเมื่อ CoM เคลื่อนเกินขอบเขตที่ควบคุมได้ ร่างกายมักต้องสร้างฐานใหม่ด้วยการก้าวค่ะ citeturn8search0turn8search11

**Prompt ที่เหมาะ**

> `He plants his lead foot, shifts his body weight forward, rotates his hips and torso, then drives a compact right cross.`  
> เขาปักเท้านำให้มั่น ถ่ายน้ำหนักไปข้างหน้า หมุนสะโพกและลำตัว ก่อนส่งหมัดตรงขวาแบบกระชับ

ดีกว่า

> `He throws an extremely powerful punch.`

เพราะประโยคแรกบอกลำดับการเคลื่อนไหวที่โมเดลสามารถสร้างเป็น Temporal Progression (พัฒนาการตามเวลา) ได้ ขณะที่ Runway แนะนำให้เขียนการเคลื่อนไหวและลำดับเวลาตรง ๆ มากกว่าคำเชิงนามธรรมค่ะ citeturn9view0turn9view2

### Foot Planting (การปักเท้า)

เท้าคือ “สมอภาพ” ที่สำคัญมากค่ะ ถ้า AI ทำให้เท้าลื่น หมุนโดยไม่มีแรงต้าน หรือเปลี่ยนตำแหน่งโดยไม่มีเหตุผล ต่อให้แขนสวย ฉากก็จะรู้สึกเบาทันที

งานเกี่ยวกับการชกพบความสัมพันธ์ระหว่างแรงจากขาส่วนล่างกับการทำงานของหมัด และการวิเคราะห์ชกจริงสร้าง GRF จากความเร่งของ CoM เพื่อประเมินว่าขาแต่ละข้างรับภาระอย่างไร citeturn0search0turn4view6

คำที่ควรใช้ใน Prompt ได้แก่:

`plants his foot` (ปักเท้าให้มั่น)  
`loads onto the rear leg` (ลงน้ำหนักที่ขาหลัง)  
`pushes off the floor` (ถีบแรงออกจากพื้น)  
`steps into the strike` (ก้าวเข้าสู่จังหวะโจมตี)  
`braces through his stance` (ตั้งฐานรับแรงผ่านท่ายืน)

หลีกเลี่ยงการเขียน `his whole body explodes forward` เพราะเป็นคำเชิงเปรียบเทียบและไม่ได้ระบุว่าชิ้นส่วนใดเคลื่อนอย่างไร เอกสาร Runway แนะนำให้ใช้ภาษาภาพที่ตรงและลดความกำกวมค่ะ citeturn9view2

### Hip Rotation (การหมุนสะโพก) และการส่งแรงตามลำดับ

การเคลื่อนไหวที่น่าเชื่อถือควรดูเหมือนแรงไหลจากส่วนใกล้แกนกลางไปสู่ปลายร่างกาย หรือ Proximal-to-Distal Sequencing (การส่งการเคลื่อนไหวจากส่วนต้นสู่ส่วนปลาย) งานวิเคราะห์มวยอธิบาย Kinematic Chain (ห่วงโซ่การเคลื่อนไหว) ตั้งแต่ pelvis → trunk → shoulder → elbow → wrist และงานด้านหมัดตรงยังกล่าวถึงการเร่งและเบรกของส่วนต่าง ๆ ตามลำดับค่ะ citeturn4view6turn0search8

ดังนั้นภาพที่ดีไม่ใช่

> ไหล่ + แขน + สะโพกหมุนพร้อมกันเหมือนหุ่นหนึ่งชิ้น

แต่เป็น

> **พื้น → สะโพกเริ่ม → ลำตัวตาม → ไหล่เปิด → แขนเร่ง → กำปั้นถึงเป้าหมาย**

สำหรับ Hook (หมัดฮุก) การหมุนของไหล่และเส้นทางวงกลมสำคัญกว่า Cross (หมัดตรงข้ามมือ) ซึ่งมีการเปิดศอกแบบเส้นทางตรงกว่า งานเปรียบเทียบ Cross, Hook และ Uppercut พบรูปแบบการมีส่วนร่วมของข้อต่อแตกต่างกันอย่างชัดเจนค่ะ citeturn4view6

### Follow-Through (การเคลื่อนต่อหลังจังหวะโจมตี)

ความผิดพลาดที่พบได้ง่ายใน AI คือมือ “หยุดตาย” ทันทีที่สัมผัสเป้าหมาย หรือทะลุเป้าหมายเป็นระยะไกลผิดธรรมชาติ

ภาษาภาพที่ควรเป็นคือ:

> **เร่ง → กระแทก → เคลื่อนต่อเล็กน้อย → ลดความเร็ว → ดึงกลับ/คืนฐาน**

การมีช่วง Deceleration (การลดความเร็ว) เป็นส่วนหนึ่งของการเคลื่อนไหวตามลำดับในหมัดจริง และช่วยให้ภาพไม่ดูเหมือนแขนที่ถูก animate (ทำแอนิเมชัน) แยกออกจากลำตัวค่ะ citeturn0search8turn1search6

ใช้คำ:

`follows through briefly` (เคลื่อนต่อหลังจังหวะปะทะเล็กน้อย)  
`his shoulder carries through the motion` (ไหล่เคลื่อนต่อไปกับแรง)  
`the arm decelerates naturally` (แขนลดความเร็วอย่างเป็นธรรมชาติ)  
`he retracts the fist and regains his stance` (ดึงหมัดกลับและคืนท่ายืน)

## กลศาสตร์ของผู้รับแรงกระแทก

ส่วนนี้สำคัญกว่าการสัมผัสของหมัดเสียอีกค่ะ เพราะผู้ชมมักตัดสิน “ความแรง” จาก **สิ่งที่เกิดหลังการปะทะ**

### Head Snap (ศีรษะสะบัด)

การกระแทกศีรษะไม่ได้ทำให้ทั้งศีรษะและลำตัวเคลื่อนพร้อมกันเป็นแท่งเดียว การทดลองด้านชีวกลศาสตร์ของการชกพบว่าการกระแทกสามารถสร้างทั้ง Linear Acceleration (ความเร่งเชิงเส้น) และ Rotational Acceleration (ความเร่งเชิงมุม) ของศีรษะ โดยเฉพาะแรงที่ไม่ได้ผ่านจุดศูนย์กลางอาจสร้างองค์ประกอบการหมุนสูงค่ะ citeturn1search1turn1search0turn1search25

ในภาพยนตร์จึงควรเห็นลำดับประมาณนี้:

> Impact (กระแทก) → ศีรษะเริ่มหมุน/เคลื่อน → คอตาม → ไหล่และอกหมุนตาม → สะโพกตอบสนองช้ากว่า

ไม่ใช่

> หมัดแตะหน้า → ตัวละครทั้งคนกระเด็นเป็นวัตถุแข็ง

งานด้าน Head-Neck Biomechanics (ชีวกลศาสตร์ศีรษะและคอ) แสดงให้เห็นปรากฏการณ์ที่ศีรษะและลำตัวสามารถมี Phase Lag (ความล่าช้าของเฟสการเคลื่อนไหว) ต่อกันภายใต้การเร่งอย่างฉับพลัน ซึ่งเป็นเหตุผลว่าทำไม “ศีรษะตามหลังลำตัว” หรือกลับกันบางช่วงจึงอ่านเป็นแรงเฉื่อยได้ดีค่ะ citeturn1search3turn1search15

ในงานสร้างภาพ ควรใช้สิ่งนี้เป็น **visual language (ภาษาภาพ)** เท่านั้น ไม่ใช่ให้ผู้แสดงจริงสะบัดคอรุนแรงค่ะ

### Torso Compression (การยุบตัวของลำตัว)

เมื่อแสดงการถูกกระแทกบริเวณลำตัว ภาพที่น่าเชื่อถือมักมี:

> ไหล่ห่อเข้าเล็กน้อย → ซี่โครง/อกพับตามทิศแรง → เข่างอ → สะโพกถอยหรือหมุน → น้ำหนักหลุดออกจากฐาน

คำว่า `compresses` (ยุบตัว) ควรใช้ในความหมายของ posture (ท่าทาง) ไม่ใช่การยุบร่างกายแบบวัสดุยาง เพราะ Generative Video มีแนวโน้มตีความ deformation (การเปลี่ยนรูป) มากเกินไปเมื่อใช้ถ้อยคำอย่าง `body crushes` หรือ `torso caves in`

Prompt ที่ปลอดภัยกว่าเชิงภาพ:

> `At the impact, his shoulders fold inward slightly, his torso recoils backward, and his knees flex as he loses balance.`  
> เมื่อเกิดจังหวะกระแทก ไหล่ห่อเข้าด้านในเล็กน้อย ลำตัวดีดถอย และเข่างอเมื่อเริ่มเสียสมดุล

### Delayed Stumble (การเซตามหลัง)

นี่เป็นเครื่องมือสำคัญที่สุดอย่างหนึ่งสำหรับสร้าง “น้ำหนัก” ค่ะ

การเสียสมดุลครั้งใหญ่ไม่ได้จำเป็นต้องนำไปสู่การล้มทันที เมื่อ CoM ออกจากตำแหน่งที่สามารถควบคุมด้วยฐานเดิมได้ ระบบทรงตัวสามารถใช้ ankle strategy (การแก้สมดุลด้วยข้อเท้า), hip strategy (การแก้สมดุลด้วยสะโพก) หรือ step strategy (การก้าวสร้างฐานใหม่) และการรบกวนที่รุนแรงขึ้นมักจำเป็นต้องใช้การก้าวเพื่อปรับฐานรองรับให้สัมพันธ์กับ CoM ใหม่ค่ะ citeturn8search1turn8search11turn8search14

ภาษาภาพที่ดีคือ

> **hit → half beat → balance breaks → recovery step → second stumble**

เช่น

> `His head snaps sideways at impact. A fraction later his torso follows, his weight drifts outside his stance, and he takes two uneven recovery steps.`

การมี “fraction later” (ชั่วขณะต่อมา) สำคัญกว่าการสั่ง `flies backward` เพราะมันสื่อ Inertia (ความเฉื่อย) และการสูญเสียการควบคุมแทนการเทเลพอร์ตค่ะ

## Environmental FX เป็นตัวคูณพลังของการปะทะ

Environmental FX (เอฟเฟกต์สิ่งแวดล้อม) มีสองหน้าที่พร้อมกัน:

หนึ่ง คือเป็น **หลักฐานทางฟิสิกส์** ว่ามีบางอย่างส่งพลังงานเข้าสู่ฉาก

สอง คือทำหน้าที่เป็น **Visual Masking (การพรางทางภาพ)** รอบเฟรมสัมผัส ซึ่งช่วยลดการที่ผู้ชมจ้องรายละเอียดนิ้ว มือ ข้อมือ และผิวหน้าที่ Generative Video มักสร้างยาก

ข้อหลังควรเข้าใจว่าเป็นเทคนิคเชิงกำกับภาพ ไม่ใช่หลักฐานว่าเราสามารถ “บังคับ Attention Mechanism (กลไกความสนใจ) ภายในโมเดล” ได้โดยตรงค่ะ อย่างไรก็ตาม Runway ระบุชัดว่า Environmental Motion สามารถสั่งแยกจาก Subject Motion ได้ เช่น ให้ฝุ่นตามหลังตัวละคร และการบรรยาย environmental reaction (ปฏิกิริยาสภาพแวดล้อม) โดยตรงทำให้โมเดลเน้นองค์ประกอบนั้นมากขึ้น citeturn4view1turn9view3

นอกจากนี้ งานด้านการมองเห็นพบว่าการเคลื่อนไหวในฉากสามารถดึงดูด gaze/attention (สายตาและความสนใจ) และข้อมูลภาพที่เคลื่อนเร็วสามารถลดความง่ายในการแยกรายละเอียดบางอย่างออกเป็นวัตถุที่ชัดเจน โดยเฉพาะนอกจุดจ้องกลางสายตา จึงมีเหตุผลเชิงการรับรู้รองรับการใช้อนุภาคและการเคลื่อนไหวผ่านเฟรมเป็นตัว “แบ่งความสนใจ” ของผู้ชมค่ะ citeturn6search0turn6search2

### น้ำและฝน

น้ำให้หลักฐานเรื่องทิศแรงได้ดีมาก เพราะแนวการกระเด็นสามารถเชื่อมกับทิศการเคลื่อนไหวของร่างกายได้ ในงานจำลอง FLIP ของ Houdini การชนกับวัตถุมีการคำนวณ collision (การชน) และ velocity (ความเร็ว) ขณะที่ระบบ Whitewater แยก Splash (ละอองกระเซ็น) จากหยดหรือโครงสร้างน้ำที่เคลื่อนเร็วค่ะ citeturn11search0turn11search12

Prompt Vocabulary (คลังคำสำหรับ Prompt):

`water bursts outward from the impact`  
(น้ำระเบิดกระจายออกจากจุดกระแทก)

`a directional sheet of water sprays sideways`  
(แผ่นน้ำพุ่งเป็นทิศไปด้านข้าง)

`fine droplets trail behind the motion`  
(ละอองละเอียดลากตามการเคลื่อนไหว)

`rain sprays violently from his jacket at impact`  
(หยดฝนกระเด็นจากเสื้ออย่างแรงในจังหวะกระแทก)

`a fan of rainwater erupts from the pavement beneath his planted foot`  
(น้ำฝนแผ่ออกจากพื้นใต้เท้าที่ปักลง)

สำหรับ AI คำว่า **directional** (มีทิศทาง), `trails behind` (ลากตามหลัง), `from the pavement` (จากพื้น) มีประโยชน์มากกว่าคำกว้าง ๆ เช่น `lots of water everywhere`

### ฝุ่นและเศษวัสดุ

Houdini มีระบบสร้าง Debris Source (แหล่งกำเนิดเศษวัสดุ) จากวัตถุ rigid body (วัตถุแข็งจำลอง) ที่กำลังแยกแตก และ Impact Analysis สามารถใช้จุดชนเป็นแหล่งสร้างกลุ่มฝุ่นตรงตำแหน่งกระแทกได้โดยตรงค่ะ citeturn11search1turn11search9

นี่ให้หลักการ Prompt ที่ดีมาก:

**อย่าสั่ง “dusty scene” อย่างเดียว**  
แต่สั่ง “อะไรทำให้ฝุ่นเคลื่อน”

เช่น

> `His boot plants hard into the concrete. A compact puff of dust kicks outward from beneath the sole.`

หรือ

> `He slams into the wall; plaster dust blooms from the contact point, followed by small fragments dropping downward.`

คำสำคัญ:

`kicks up` (เตะฝุ่นให้ฟุ้ง)  
`bursts outward` (ปะทุออกด้านนอก)  
`blooms from the contact point` (ฟุ้งออกจากจุดปะทะ)  
`trails behind` (ลากตามหลัง)  
`settles slowly` (ค่อย ๆ ตกลง)  
`small fragments scatter first, fine dust hangs afterward`  
(เศษชิ้นเล็กกระจายก่อน แล้วฝุ่นละเอียดค้างในอากาศภายหลัง)

ลำดับ “เศษเร็ว → ฝุ่นละเอียดช้า” อ่านเป็นฟิสิกส์มากกว่ากลุ่มควันก้อนเดียวค่ะ Houdini เองแยกระบบ debris, particles และ pyro สำหรับชั้นรายละเอียดการแตกทำลายลักษณะนี้ citeturn11search13turn11search33

### Sparks (ประกายไฟ)

Sparks (ประกายไฟ) เหมาะกับโลหะ พื้นผิวแข็ง หรืออุปกรณ์ที่มีเหตุผลว่าจะสร้างประกาย ไม่ควรปรากฏจากผิวมนุษย์แบบไม่มีต้นเหตุ

ใช้คำ:

`a short directional burst of sparks`  
(ประกายไฟสั้น ๆ พุ่งเป็นทิศ)

`brief sparks scrape across the metal railing`  
(ประกายสั้น ๆ กระเด็นตามรอยครูดราวโลหะ)

`thin glowing particle trails arc away from the contact point`  
(เส้นอนุภาคเรืองแสงบาง ๆ โค้งออกจากจุดสัมผัส)

Houdini ใช้ particle trails (เส้นทางอนุภาค) สำหรับประกาย ฝน และเอฟเฟกต์ที่มีวิถี และมีเครื่องมือแยก trail เพื่อเลียนแบบประกายที่แตกกระจายได้ค่ะ citeturn11search3turn11search17

### Glass Shatter (กระจกแตก)

กระจกเป็น Masking Effect (เอฟเฟกต์พราง) ที่แรงมาก แต่ควรให้มีเหตุและจังหวะชัดเจน เช่น

> body contacts glass → fracture radiates → pane separates → shards travel → fragments fall

SideFX ออกแบบ Material Fracture (การแตกตามชนิดวัสดุ) แยกสำหรับกระจก และอธิบาย workflow การแตกวัตถุว่าเกิดเมื่อแรงมากพอเอาชนะแรงยึดระหว่างชิ้นส่วนค่ะ citeturn11search2turn11search10

Prompt:

> `His shoulder collides with the glass panel. A spiderweb fracture flashes outward from the contact point, then the pane breaks into outward-moving shards.`

น่าเชื่อถือกว่า

> `glass explodes everywhere`

เพราะประโยคแรกมี Origin (จุดกำเนิด), Propagation (การลาม) และ Direction (ทิศทาง)

### Steam, Fog และ Smoke (ไอน้ำ หมอก และควัน)

สิ่งเหล่านี้เหมาะสำหรับพรางช่วงปะทะโดยไม่ต้องทำลายฉากค่ะ เช่น

> `the moving bodies disturb the hanging steam`  
> ร่างกายที่เคลื่อนผ่านทำให้ไอน้ำที่ลอยอยู่ปั่นป่วน

> `a dense steam plume briefly crosses the contact point`  
> กลุ่มไอน้ำหนาเคลื่อนผ่านจุดปะทะชั่วขณะ

> `the impact drives a rolling cloud of dust across the foreground`  
> แรงกระแทกผลักกลุ่มฝุ่นให้กลิ้งผ่านด้านหน้าภาพ

หลักสำคัญคือให้ FX “ตอบสนองต่อเหตุการณ์” แทนที่จะเกิดขึ้นสุ่ม ๆ เพราะ Runway แนะนำให้บอก Scene Motion (การเคลื่อนไหวของฉาก) โดยสัมพันธ์กับ Subject Motion อย่างชัดเจนค่ะ citeturn9view3

## Motion Blur และกล้องที่ทำหน้าที่เหมือนมวล

### Motion Blur (ภาพเบลอจากการเคลื่อนไหว)

Motion Blur ไม่ได้เป็นเพียงการซ่อนข้อผิดพลาด แต่เป็นสัญญาณทางภาพของความเร็ว

ในกล้องภาพยนตร์มาตรฐาน Shutter Angle (มุมชัตเตอร์) ประมาณ 180° ที่ 24 fps ให้เวลารับแสงประมาณ 1/48 วินาที และเป็นค่าที่ใช้กันแพร่หลายเพื่อให้การเคลื่อนไหวมีความต่อเนื่องตามแบบภาพยนตร์ มุมที่กว้างขึ้นเพิ่ม smear (การลากเบลอ) ส่วนมุมแคบลงให้ภาพคมขึ้นแต่การเคลื่อนไหวดูสะดุดและแข็งขึ้นค่ะ citeturn5search1turn5search9

ใน Generative Video จึงสามารถใช้ภาษาภาพได้สองแบบ

**Fight แบบดิบ เร็ว สับสน**

> `natural cinematic motion blur`  
> ภาพเบลอจากการเคลื่อนไหวแบบภาพยนตร์ตามธรรมชาติ

> `fast limbs streak slightly during the strike`  
> แขนขาที่เคลื่อนเร็วมีเส้นเบลอเล็กน้อยในช่วงโจมตี

**Fight แบบคม แข็ง กระแทก**

> `crisp high-shutter action aesthetic`  
> ภาพแอ็กชันคมแบบชัตเตอร์เร็ว

> `minimal motion blur, sharp staccato movement`  
> เบลอจากการเคลื่อนไหวน้อย การเคลื่อนไหวคมเป็นจังหวะ

RED ระบุว่าชัตเตอร์ที่เร็วขึ้นลด Motion Blur ขณะที่ชัตเตอร์ที่ช้าลงเพิ่ม Motion Blur และการเปลี่ยน Shutter Angle จึงเปลี่ยนบุคลิกของการเคลื่อนไหวโดยตรงค่ะ citeturn5search9turn5search11

### Whip Pan (กวาดกล้องเร็ว)

Whip Pan (กวาดกล้องอย่างรวดเร็ว) เหมาะกับการข้ามเฟรมที่ contact geometry (เรขาคณิตของการสัมผัส) ซับซ้อนมาก เช่น มือผ่านหน้า เสี้ยววินาทีที่ตัวละครไขว้กัน หรือการเปลี่ยนจากผู้โจมตีไปยังผู้รับแรง

Runway Gen-4.5 มีตัวอย่าง Prompt ที่ใช้ Whip Pan โดยตรง และรองรับการกำกับการเคลื่อนกล้องตามคำศัพท์ภาพยนตร์ค่ะ citeturn9view0turn4view4

Pattern ที่มีประสิทธิภาพ:

> **อ่านท่าโจมตีชัด → whip during contact → กล้องหยุดอ่าน reaction ชัด**

ไม่ควร whip ตลอดทั้งช็อต เพราะผู้ชมจะไม่มีเฟรมอ้างอิงให้อ่านว่าผลของแรงคืออะไร

### Impact Shake (กล้องสะเทือนตามแรงกระแทก)

Camera Shake (กล้องสั่น) จะมีประสิทธิภาพเมื่อมันเป็น **response** ไม่ใช่เป็น noise (การสั่นรบกวน) ตลอดเวลา

ใช้:

> `a brief camera jolt exactly at impact`  
> กล้องสะเทือนสั้น ๆ ตรงจังหวะกระแทก

> `the handheld camera recoils slightly at the hit, then quickly settles`  
> กล้องถือมือดีดถอยเล็กน้อยเมื่อเกิดแรงกระแทก ก่อนกลับมานิ่งอย่างรวดเร็ว

Adobe อธิบายการสร้าง shake effect (เอฟเฟกต์กล้องสั่น) รอบเหตุการณ์กระแทกด้วยการเปลี่ยนตำแหน่งภาพชั่วขณะ และ Runway รองรับ `handheld`, `natural camera shake` และการติดตามตัวละครใน Prompt โดยตรงค่ะ citeturn2search16turn9view0

งานทดลองด้านการรับรู้ภาพยนตร์ยังพบว่าการเคลื่อนกล้องมีผลต่อการตอบสนองทางอารมณ์และการรับรู้ของผู้ชม และสามารถใช้กำกับจุดสนใจของผู้ชมได้ค่ะ citeturn7search0turn7search2

### Crash Zoom (ซูมพุ่งเร็ว)

Crash Zoom (ซูมพุ่งเข้าอย่างฉับพลัน) เหมาะกับการเน้น Consequence Frame (เฟรมผลลัพธ์) มากกว่า Contact Frame

ตัวอย่าง:

> punch begins → **impact FX** → crash zoom onto receiver's recoil

Runway แสดงตัวอย่าง `extremely rapid crash zoom` ในคู่มือ Image-to-Video ของ Gen-4.5 โดยตรง จึงเป็นคำศัพท์ที่มี grounding (การรองรับจากเอกสารโมเดล) ดีกว่าคำประดิษฐ์อย่าง `kinetic impact zoom blast` ค่ะ citeturn4view2

### Leading Tracking Shot (ช็อตติดตามนำหน้า)

สำหรับตัวละครกำลังวิ่ง พุ่งเข้าหา หรือถูกผลักผ่านพื้นที่ ใช้:

> `low handheld tracking shot leading the subject`  
> กล้องถือมือติดตามจากด้านหน้าของตัวละครในมุมต่ำ

หรือ

> `the camera tracks backward ahead of him as he charges forward`  
> กล้องถอยติดตามอยู่ด้านหน้า ขณะตัวละครพุ่งไปข้างหน้า

Tracking Shot (ช็อตติดตาม) เป็นคำศัพท์กล้องที่รองรับทั้งในแนวทาง Veo และ Runway ค่ะ citeturn10view0turn4view1

## คลังคำกริยากายภาพสำหรับฉากต่อสู้

ไม่มีผู้พัฒนาโมเดลรายใดรับประกันว่า “คำกริยาคำนี้จะไม่มีแขนเกิน 100%” ค่ะ สิ่งที่เอกสารปัจจุบันสนับสนุนคือ **คำสั่งตรง ภาษาธรรมชาติ เหตุการณ์ไม่ซับซ้อนเกินไป ผู้กระทำชัด และลำดับเวลาชัดเจน** Runway ระบุว่าภาษาธรรมชาติให้การควบคุมมากกว่าคีย์เวิร์ดโดด ๆ และคำสั่งยาวที่มีเงื่อนไขขัดกันอาจเกิดผลผิดธรรมชาติได้ citeturn9view1turn9view2

ดังนั้นตารางต่อไปนี้เป็น **high-confidence prompting vocabulary (คลังคำ Prompt ที่ลดความกำกวม)** ไม่ใช่คำรับประกันผลลัพธ์ค่ะ

| หน้าที่ | คำกริยา ENG (ความหมายไทย) | เหตุผลที่เหมาะ |
|---|---|---|
| ตั้งฐาน | `plants` (ปัก/วางให้มั่น) | มีจุดเริ่ม-จบชัด |
| ลงน้ำหนัก | `loads onto` (ลงน้ำหนักไปยัง) | ระบุขาหรือด้านได้ |
| เริ่มเคลื่อน | `pushes off` (ถีบส่งตัวจาก) | เชื่อมเท้ากับการเคลื่อน |
| ก้าว | `steps into` (ก้าวเข้าสู่) | ดีกว่า “moves aggressively” |
| หมุนเท้า | `pivots` (หมุนบนจุดรองรับ) | ระบุการหมุนโดยไม่สร้างชิ้นส่วนใหม่ |
| หมุนสะโพก | `rotates his hips` (หมุนสะโพก) | ระบุ segment (ส่วนร่างกาย) ชัด |
| หมุนลำตัว | `turns his torso` (หมุนลำตัว) | ลดความกำกวม |
| ส่งหมัดตรง | `drives a straight punch` (ส่งหมัดตรงเข้าไป) | trajectory (วิถี) ชัด |
| ฮุก | `swings a compact hook` (เหวี่ยงหมัดฮุกแบบกระชับ) | กำหนดเส้นทางโค้ง |
| ถอนหมัด | `retracts` (ดึงกลับ) | ป้องกันแขนค้าง |
| ป้องกัน | `raises his guard` (ยกการ์ดขึ้น) | ท่าง่าย อ่านชัด |
| หลบ | `slips sideways` (เอียงหลบด้านข้าง) | ดีกว่า `dodges wildly` |
| ก้มหลบ | `ducks beneath` (ก้มลอดใต้) | ความสัมพันธ์เชิงพื้นที่ชัด |
| รับแรง | `recoils` (ดีด/สะท้อนถอยจากแรง) | เหมาะกับ reaction |
| ศีรษะตอบสนอง | `his head snaps sideways` (ศีรษะสะบัดด้านข้าง) | direction (ทิศทาง) ชัด |
| ลำตัวพับ | `folds inward slightly` (ห่อตัวเข้าด้านในเล็กน้อย) | ลด rubber morphing (การย้วยเหมือนยาง) |
| เสียสมดุล | `loses his balance` (เสียสมดุล) | ใช้คู่กับก้าวที่ชัด |
| เซ | `stumbles backward` (เซถอยหลัง) | trajectory ชัด |
| ก้าวกู้สมดุล | `takes a recovery step` (ก้าวเพื่อคืนสมดุล) | สอดคล้อง biomechanics |
| ชน | `collides with` (ชนกับ) | ระบุวัตถุคู่สัมพันธ์ |
| กระแทกพื้น | `lands heavily` (ลงพื้นอย่างมีน้ำหนัก) | ดีกว่า “crashes insanely” |
| กลับฐาน | `regains his stance` (คืนท่ายืน) | ปิด motion cycle |

ชุดคำที่ควรระวังคือ:

`explodes forward` (พุ่งระเบิดไปข้างหน้า)  
`body warps` (ร่างบิดผิดรูป)  
`limbs whip everywhere` (แขนขาสะบัดไปทั่ว)  
`superhumanly fast` (เร็วเหนือมนุษย์)  
`violently twists in every direction` (บิดรุนแรงทุกทิศ)

ไม่ใช่เพราะคำเหล่านี้ “ผิด” แต่เพราะมี Spatial Ambiguity (ความกำกวมเชิงพื้นที่) สูงและเปิดโอกาสให้โมเดลเติมการเคลื่อนไหวเกินความต้องการ เอกสาร Runway แนะนำโดยตรงให้หลีกเลี่ยง conceptual language (ภาษานามธรรม) และลด ambiguity (ความกำกวม) ค่ะ citeturn9view2

## โครงสร้าง Prompt สำหรับลดแขนเกินและ Rubber Morphing

### หลักหนึ่งผู้กระทำต่อหนึ่งกริยาหลัก

แทนที่จะเขียน:

> `The two fighters exchange a rapid flurry of punches, blocks, counters, spins and kicks.`

ให้แบ่งเป็น:

> `The fighter on the left steps forward and throws one compact right cross. The fighter on the right remains planted until impact.`

จากนั้น:

> `At impact, the fighter on the right recoils sideways and takes one recovery step.`

Runway แนะนำอย่างชัดเจนว่าเมื่อมีหลายตัวละคร ควรใช้ positional language (ภาษาระบุตำแหน่ง) เช่น “subject on the left” และกำหนดการเคลื่อนไหวให้แต่ละคนแยกกันค่ะ citeturn9view3

### แยก Contact (สัมผัส) ออกจาก Consequence (ผลกระทบ)

สำหรับ Generative AI ฉันแนะนำ workflow เชิงกำกับดังนี้:

**ช่วง Approach (เข้าหา)**  
อ่านเท้า สะโพก และ trajectory ให้ชัด

**ช่วง Contact Window (ช่วงสัมผัส)**  
ให้สั้นที่สุด ใช้ motion blur, whip, splash, sparks หรือ foreground debris ช่วยแบ่งความสนใจ

**ช่วง Consequence (ผลลัพธ์)**  
กลับมาชัด ให้เห็น recoil, CoM shift และ recovery step

**ช่วง Recovery (คืนสมดุล)**  
ให้ผู้โจมตีถอนแขนและผู้รับแรงกลับฐานหรือเซต่อ

นี่เป็น inference (ข้อสรุปเชิงประยุกต์) จากหลัก biomechanics, visual masking และแนวทาง prompting ที่แยก Subject Motion, Environmental Motion และ Camera Motion ออกจากกัน ไม่ใช่ฟังก์ชันเฉพาะที่ผู้พัฒนาเรียกว่า “FX masking” ค่ะ citeturn4view6turn6search15turn9view0

### ใช้ Temporal Prompting (การสั่งตามช่วงเวลา)

ทั้ง Veo 3.1 และ Runway Gen-4.5 มีแนวทางสำหรับกำหนดเหตุการณ์ตามเวลา โดย Veo มีตัวอย่าง timestamp segments (ช่วงเวลาที่กำหนด) และ Runway รองรับทั้งรูปแบบ `X occurs, then Y` และ timestamp โดยตรงค่ะ citeturn4view0turn9view0

ตัวอย่างที่เหมาะกับ Action Shot (ช็อตแอ็กชัน):

```text
[00:00-00:01.5]
Medium handheld shot. The fighter plants his lead foot and shifts his weight forward.

[00:01.5-00:02.5]
He rotates his hips and torso and drives one compact right cross toward the opponent.

[00:02.5-00:03.0]
At impact, rainwater bursts sideways from the opponent's wet jacket.
The camera jolts briefly and motion blur streaks across the contact.

[00:03.0-00:05.0]
The opponent's head turns sideways first, then his torso follows.
His balance breaks and he takes two uneven recovery steps backward.

[00:05.0-00:06.0]
The attacker retracts his fist and regains a stable stance.
Rain continues falling naturally.
```

จุดแข็งของ Prompt นี้คือทุกช่วงมี **หนึ่งเหตุการณ์หลัก + หนึ่งผลทางฟิสิกส์** แทนที่จะให้ AI สร้าง choreography (การออกแบบท่าต่อสู้) จำนวนมากในคราวเดียวค่ะ แนวทางนี้สอดคล้องกับคำแนะนำของ Runway ที่ให้คิดหนึ่ง generation (การสร้างหนึ่งคลิป) เป็นฉากเดียวและหลีกเลี่ยงหลายการเปลี่ยนแปลงพร้อมกัน citeturn9view3

### สูตร Prompt ที่แนะนำสำหรับ Text-to-Video

สำหรับ Veo แนวทางอย่างเป็นทางการใช้โครงสร้าง:

`[Cinematography] + [Subject] + [Action] + [Context] + [Style & Ambiance]`

หรือ กล้อง + ตัวละคร + การกระทำ + สภาพแวดล้อม + บรรยากาศค่ะ citeturn10view0

เมื่อนำมาปรับกับ Action:

```text
[Camera]
Medium handheld tracking shot, subtle natural shake.

[Attacker mechanics]
The attacker plants his front foot, shifts his weight,
rotates his hips and torso, and drives one compact straight punch.

[Impact consequence]
At the instant of impact, the receiver's head turns sideways first.
His shoulders and torso recoil a fraction later.
He loses balance and takes one recovery step.

[Environmental reaction]
Rainwater sprays sideways from both jackets.
A shallow puddle bursts beneath the receiver's stepping foot.

[Camera consequence]
A brief camera jolt occurs exactly at impact,
followed by a fast push-in toward the receiver's reaction.

[Motion style]
Natural live-action body mechanics,
cinematic motion blur, grounded physical weight.
```

สำหรับ Image-to-Video ควรตัดคำบรรยายรูปร่าง เสื้อผ้า และฉากที่เห็นอยู่ในภาพต้นฉบับออกให้มากที่สุด แล้วใช้ Prompt ไปกับ Motion (การเคลื่อนไหว) เพราะ Runway ระบุว่าภาพ input (ภาพนำเข้า) เป็นตัวกำหนด composition, subject, lighting และ style อยู่แล้ว การบรรยายสิ่งเดิมซ้ำมากเกินไปอาจลดการเคลื่อนไหวหรือให้ผลผิดคาดค่ะ citeturn9view0turn9view3

Prompt Image-to-Video จึงควรสั้นประมาณ:

```text
The fighter plants his front foot and rotates through one compact right cross.
At impact, the opponent recoils sideways and takes a recovery step.
Rain sprays sharply from their clothing.
The handheld camera jolts once at impact, then settles.
Natural cinematic motion blur.
```

## ข้อสรุปสำหรับงาน Stunt และ Generative Video

สูตรที่ทำให้ Action ดูหนักที่สุดไม่ใช่ **“เพิ่มความเร็วให้แขน”** แต่คือการสร้างความต่อเนื่องของ Momentum (โมเมนตัม) ผ่านระบบทั้งหมดค่ะ:

**ก่อนปะทะ** — เท้าต้องบอกว่ากำลังรับและส่งแรง  
**ระหว่างปะทะ** — สะโพก ลำตัว ไหล่ และแขนต้องมีลำดับ ไม่เคลื่อนเป็นก้อนเดียว  
**หลังปะทะ** — ผู้โจมตีต้องมี Follow-Through และ Deceleration ส่วนผู้รับต้องมี inertial lag (ความล่าช้าจากแรงเฉื่อย), recoil และการเสียฐาน citeturn4view6turn0search8turn8search11

Environmental FX ควรทำหน้าที่เป็น **physics witness (พยานทางฟิสิกส์)**: น้ำต้องกระเด็นจากแหล่งที่ถูกแรง ฝุ่นต้องเกิดจากพื้นหรือผิวที่ถูกกระแทก เศษวัสดุต้องมีจุดกำเนิด และกระจกต้องเริ่มแตกจากบริเวณที่เกิดเหตุ การจำลอง VFX มาตรฐานเองก็ผูก debris, fluid collision และ fracture เข้ากับตำแหน่งและแรงของเหตุการณ์ ไม่ได้กระจายเอฟเฟกต์แบบสุ่มค่ะ citeturn11search0turn11search2turn11search9

Camera Motion ควรเป็น “ร่างกายอีกตัวหนึ่ง” ในฉาก: กล้องติดตาม momentum ก่อนกระแทก, สะเทือนเพียงสั้น ๆ ตรง impact, แล้วกลับมาอ่าน consequence ให้ชัด การเคลื่อนกล้องสามารถกำกับความสนใจและการตอบสนองของผู้ชมได้ และคำศัพท์มาตรฐานอย่าง tracking shot, handheld, whip pan และ crash zoom มีการรองรับโดยคู่มือวิดีโอ Generative AI ปัจจุบันค่ะ citeturn7search0turn9view0turn10view0

และสำหรับการลดปัญหา extra limbs (แขนขาเกิน) กับ rubbery morphing (ร่างย้วยผิดรูป) หลักที่มีประโยชน์ที่สุดคือ **ลดจำนวน interaction (ปฏิสัมพันธ์) พร้อมกัน, ระบุผู้กระทำทุกกริยา, ใช้คำเคลื่อนไหวที่เป็นรูปธรรม, จำกัดแต่ละช่วงให้มี action หลักเพียงหนึ่งอย่าง และใช้ FX/กล้องปิดบังเฉพาะ contact window ที่ซับซ้อน** ค่ะ แนวทางนี้สอดคล้องโดยตรงกับเอกสาร Runway ที่ให้ลด ambiguity, เริ่มจาก motion ที่สำคัญที่สุด และหลีกเลี่ยง prompt ที่มีคำสั่งหลายเหตุการณ์มากเกินไป ขณะที่ Veo แนะนำการแบ่งองค์ประกอบกล้อง ตัวละคร การกระทำ และบริบทอย่างเป็นระบบค่ะ citeturn9view1turn9view2turn10view0