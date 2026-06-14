# 📋 Presenter Brief — SDLC CI/CD Pipeline Presentation

> **วัตถุประสงค์:** เตรียมความพร้อมสำหรับการนำเสนอ — อ่านก่อนขึ้นพูด 15-20 นาที
> **เวลาที่ใช้ presenting:** ~20-25 นาที (14 slides)
> **กลุ่มเป้าหมาย:** ทีม DevOps, Engineering Management, IT Leadership

---

## 🎯 Key Message (จำให้ขึ้นใจ)

> **"เราต้องยกระดับ CI/CD pipeline ด้วย Terraform + AI Automation เพื่อลด manual work, เพิ่มความเร็ว และมั่นใจได้ว่าทุก environment ตรงกัน"**

---

## 📝 Slide-by-Slide Talking Points

### Slide 1 — Title
**พูดอะไร:**
- แนะนำตัวเองและหัวข้อ
- "วันนี้จะมาเล่าเรื่องการยกระดับ SDLC pipeline ของเรา ด้วย CI/CD บน AWS Cloud"
- ให้ภาพรวมว่าจะพูดถึงเครื่องมือที่ใช้อยู่, ปัญหาที่เจอ, และแผนการปรับปรุง

**เวลา:** ~1 นาที

---

### Slide 2 — Agenda
**พูดอะไร:**
- ให้ผู้ฟังเห็นภาพรวมว่าจะพูดถึงอะไรบ้าง
- "เราจะเริ่มจากสถานะปัจจุบัน, ดูปัญหาที่เจอ, แล้วเสนอทางออก"
- เน้นว่าเปรียบเทียบ **เชิง process** ไม่ใช่แค่ tool

**เวลา:** ~1 นาที

---

### Slide 3 — Current SDLC Process
**พูดอะไร:**
- อธิบาย flow: Plan → Code → Build → Test → Deploy → Monitor
- "This is our current process — ครอบคลุมทุก phase"
- เน้นว่า **Deploy phase** คือจุดที่มีปัญหามากที่สุด (manual)

**Tip:** ชี้ที่ flow diagram แล้วไล่ทีละ step

**เวลา:** ~1.5 นาที

---

### Slide 4 — Current CI/CD Tool Chain
**พูดอะไร:**
- แนะนำ tool แต่ละตัว:
  - **GitLab** — Source Code Management + CI trigger
  - **Jenkins** — Build server, run test, deploy
  - **Harbor** — Container registry เก็บ Docker image
  - **BlackDuck** — Scan open source components หา vulnerability
  - **Coverity** — Static analysis (SAST) หา code quality issues
  - **Terraform** — ยังอยู่ระหว่างขออนุมัติ ⏳
- "เครื่องมือเหล่านี้ทำหน้าที่ได้ดี แต่ยังมี gap ที่ต้องปรับปรุง"

**Tip:** ถ้ามีคนถาม "ทำไมไม่ใช้ tool อื่น?" → ตอบว่า "tool เหล่านี้เป็น standard ขององค์กรและ proven แล้ว"

**เวลา:** ~2 นาที

---

### Slide 5 — Security & Quality Gates
**พูดอะไร:**
- **BlackDuck:** ทำหน้าที่ scan open source library ที่เราใช้ ว่ามี vulnerability ไหม
  - "ถ้าใช้ library เก่าที่มีCVE → BlackDuck จะจับได้"
  - ทำงานหลัง build, ก่อน deploy
- **Coverity:** วิเคราะห์ source code ระดับลึก
  - หา security flaw, code quality issue
  - ทำงานระหว่าง build phase
- "ทั้งสองตัวเป็น security gate ที่สำคัญ แต่ตอนนี้ยังเป็น sequential (รอก่อน)"

**Tip:** เน้นว่า security scanning เป็น bottleneck เพราะต้องรอนาน

**เวลา:** ~2 นาที

---

### Slide 6 — Current Pipeline Flow (สำคัญ!)
**พูดอะไร:**
- **นี่คือ flow ปัจจุบันทั้งหมด:**
  1. Developer git push code
  2. GitLab รับแล้ว trigger CI
  3. Jenkins build + test
  4. BlackDuck scan OSS
  5. Coverity scan code
  6. Harbor เก็บ image
  7. Deploy ขึ้น AWS ← **จุดนี้ manual!**
- "ปัญหาหลักอยู่ที่ step สุดท้าย — deploy ยังต้องทำด้วยมือ, infrastructure provisioning ไม่มี IaC"

**Tip:** ชี้ที่ "Deploy ⚠️" แล้วเน้นว่าเป็น pain point หลัก

**เวลา:** ~2 นาที

---

### Slide 7 — Challenges & Pain Points
**พูดอะไร:**
- ทีละข้อ:
  1. **Manual Infrastructure** — สร้าง server, network, DB ด้วยมือ เสียเวลา ผิดพลาดง่าย
  2. **No IaC** — infrastructure config กระจาย ไม่มี version control
  3. **Environment Drift** — Dev ต่างจาก Prod → "works on my machine"
  4. **Slow Deployments** — manual steps หลายจุด → cycle time ยาว
  5. **No Automated Rollback** — deploy แล้วพัง ต้องแก้ด้วยมือ
  6. **Security Bottleneck** — scan ช้า ต้องรอก่อน deploy
- "ปัญหาเหล่านี้ส่งผลต่อ **speed, quality, และ reliability**"

**Tip:** ถามผู้ฟัง "มีใครเจอปัญหาแบบนี้บ้าง?" → สร้าง engagement

**เวลา:** ~2 นาที

---

### Slide 8 — Terraform: Infrastructure as Code
**พูดอะไร:**
- "ทางออกข้อแรกคือ **Terraform** — Infrastructure as Code"
- อธิบาย 4 ข้อดี:
  1. **Declarative** — เขียน HCL บอกว่าต้องการ infra แบบไหน
  2. **Repeatable** — สร้าง environment เดียวกันได้ซ้ำแล้วซ้ำอีก
  3. **Version-Controlled** — infra เป็น code อยู่ใน Git → track, review, rollback ได้
  4. **Multi-Cloud** — ใช้กับ AWS, Azure, GCP ได้
- **สถานะ:** อยู่ระหว่างขออนุมัติ procurement + licensing
- "ถ้าได้ Terraform มา → environment provisioning จะเปลี่ยนจาก **วันเหลือเป็นนาที**"

**Tip:** เน้น comparison: "ตอนนี้สร้าง environment ใช้เวลา 2-3 วัน → Terraform ใช้เวลา 10 นาที"

**เวลา:** ~2 นาที

---

### Slide 9 — AI-Powered Automation
**พูดอะไร:**
- "ทางออกข้อที่สองคือ **AI Automation**"
- 4 ฟีเจอร์หลัก:
  1. **AI Code Review** — AI ช่วย review code ก่อน merge, ตรวจ security pattern
  2. **Auto Test Generation** — AI สร้าง test case จาก code → เพิ่ม coverage เร็ว
  3. **Predictive Failure Detection** — ทำนายว่า code change จะมีปัญหาไหม
  4. **Smart Rollback** — AI วิเคราะห์แล้วแนะนำ rollback strategy
- "AI ไม่ได้มาแทนคน แต่มา **accelerate** กระบวนการ"

**Tip:** ตอบข้อกังวล "AI จะเชื่อได้ไหม?" → "AI เป็น tool ช่วย human review ไม่ได้ตัดสินใจแทน"

**เวลา:** ~2 นาที

---

### Slide 10 — Enhanced Pipeline (สำคัญมาก!)
**พูดอะไร:**
- **นี่คือ flow ใหม่ที่เราเสนอ:**
  1. Developer git push
  2. GitLab CI trigger
  3. Jenkins build
  4. **🆕 AI Code Review** — ตรวจ code อัตโนมัติ
  5. Security scan (**parallel** ไม่ต้องรอกัน)
  6. Harbor push image
  7. **🆕 Terraform Provision** — สร้าง infra อัตโนมัติ
  8. **AWS Deploy — Fully Automated!**
- "เปรียบเทียบกับ slide ก่อนหน้า — steps ที่เพิ่มมาช่วยให้ **deploy อัตโนมัติ** ไม่ต้อง manual"

**Tip:** เปรียบเทียบ slide 6 vs slide 10 ให้เห็นชัดๆ

**เวลา:** ~2 นาที

---

### Slide 11 — Process Comparison (สำคัญ!)
**พูดอะไร:**
- ทีละ row ในตาราง:
  - **Infrastructure:** Manual → Terraform IaC
  - **Deployment:** Semi-auto → Fully automated
  - **Security:** Sequential → Parallel + AI triage
  - **Code Review:** Manual only → AI-assisted
  - **Env Setup:** Days → Minutes
  - **Consistency:** Variable → Guaranteed
  - **Rollback:** Manual → AI-assisted
  - **Testing:** Manual test → AI auto-generated
- "ทุก aspect ดีขึ้น — ไม่ใช่แค่ tool แต่เป็น **process improvement**"

**Tip:** เน้นว่า "นี่คือ key slide ที่ผู้บริหารต้องเห็น"

**เวลา:** ~2 นาที

---

### Slide 12 — Benefits & ROI
**พูดอะไร:**
- ตัวเลขสำคัญ:
  - **10x** Deployment Frequency (Weekly → Multiple per day)
  - **80%** Faster Lead Time (Days → Hours)
  - **90%** Faster Recovery / MTTR (Hours → Minutes)
  - **65%** Less Change Failure (15% → <5%)
  - **3-5x** Faster Env Provisioning (Days → Minutes)
  - **30%** Infrastructure Cost Savings
- "ตัวเลขเหล่านี้เป็น industry benchmark จาก DORA metrics"

**Tip:** ถ้าผู้ฟังถาม "มั่นใจได้ไง?" → "เป็น industry standard จาก Google DORA State of DevOps Report"

**เวลา:** ~1.5 นาที

---

### Slide 13 — Implementation Roadmap
**พูดอะไร:**
- **Phase 1 (Now):** ใช้ GitLab + Jenkins + Harbor + BlackDuck + Coverity ← **สถานะปัจจุบัน**
- **Phase 2 (Q3 2026):** เพิ่ม Terraform → IaC + Auto Provisioning
- **Phase 3 (Q4 2026):** เพิ่ม AI Integration → Code Review + Auto Test + Smart Deploy
- **Phase 4 (Q1 2027):** Full Optimization → Self-healing + Auto-scaling
- "แผนเป็น step-by-step ไม่ต้องทำทีเดียว ค่อยๆ เพิ่มทีละ phase"

**Tip:** เน้นว่า "Phase 2 ต้องรอ Terraform ได้รับอนุมัติก่อน"

**เวลา:** ~1.5 นาที

---

### Slide 14 — Thank You
**พูดอะไร:**
- "ขอบคุณครับ — นี่คือภาพรวมของการยกระดับ CI/CD pipeline"
- "เปิดรับคำถามและ feedback"
- ถ้าไม่มีคำถาม → "ถ้ามีข้อเสนอแนะเพิ่มเติม สามารถ comment ได้ใน shared document"

**Tip:** เตรียมรับคำถาม (ดู Q&A ด้านล่าง)

**เวลา:** ~1 นาที + Q&A

---

## ❓ Q&A Preparation — คำถามที่อาจเจอ

### Q: "ทำไมต้อง Terraform? ไม่ใช้ CloudFormation หรือ Pulumi?"
**A:** "Terraform เป็น industry standard, provider-agnostic ใช้กับ cloud ได้ทุกเจ้า, community ใหญ่, และ proven ใน enterprise environment"

### Q: "AI Code Review เชื่อถือได้ไหม?"
**A:** "AI เป็น tool ช่วย human review ไม่ได้ตัดสินใจแทน developer ทุก code ยังต้องผ่าน human review แต่ AI ช่วยจับ pattern ที่ humans อาจพลาด"

### Q: "ค่าใช้จ่ายเท่าไหร่?"
**A:** "Terraform มี tier ฟรี (Open Source) + tier เสียเงิน (Cloud) อยู่ระหว่างประเมิน licensing. AI tools ต้อง evaluate pricing ตาม use case"

### Q: "Timeline จริงจังแค่ไหน? Q3 ทันไหม?"
**A:** "Phase 2 (Terraform) ขึ้นอยู่กับการอนุมัติ procurement ถ้าได้เร็วก็เริ่มได้เร็ว. Phase 3-4 เป็น plan คร่าวๆ ปรับได้ตามสถานการณ์จริง"

### Q: "ทีมต้อง training อะไรบ้าง?"
**A:** "Phase 2: Terraform training (HCL syntax, workflow). Phase 3: AI tool training (prompt engineering, integration). ประมาณ 2-3 วันต่อคน"

### Q: "ถ้า Terraform ไม่ได้รับอนุมัติล่ะ?"
**A:** "ยังมีทางเลือกอื่น เช่น CloudFormation (AWS native), Pulumi (IaC ด้วย code), หรือ Ansible (configuration management). แต่ Terraform เป็นตัวเลือกที่ดีที่สุด"

---

## 🗣️ Presentation Tips

1. **เปิดด้วยปัญหา** — เริ่มจาก "ตอนนี้เราเจอปัญหาอะไร" แล้วค่อยเสนอทางออก
2. **ชี้ slide เปรียบเทียบ** — Slide 6 vs 10, Slide 11 คือ key slides ที่ต้องเน้น
3. **ถามผู้ฟัง** — สร้าง engagement ด้วยคำถาม เช่น "มีใครเจอปัญหาแบบนี้บ้าง?"
4. **ตอบสั้นๆ** — Q&A ตอบกระชับ ไม่ต้องอธิบายยาว
5. **เน้น ROI** — ผู้บริหารให้ความสำคัญกับตัวเลข → เน้น Slide 12

---

## ⏱️ Timeline Summary

| Slide | Topic | เวลา |
|-------|-------|------|
| 1 | Title | ~1 min |
| 2 | Agenda | ~1 min |
| 3 | SDLC Process | ~1.5 min |
| 4 | Tool Chain | ~2 min |
| 5 | Security Gates | ~2 min |
| 6 | Current Pipeline | ~2 min |
| 7 | Challenges | ~2 min |
| 8 | Terraform | ~2 min |
| 9 | AI Automation | ~2 min |
| 10 | Enhanced Pipeline | ~2 min |
| 11 | Comparison | ~2 min |
| 12 | Benefits/ROI | ~1.5 min |
| 13 | Roadmap | ~1.5 min |
| 14 | Thank You + Q&A | ~1 min + Q&A |
| **Total** | | **~22 min + Q&A** |

---

## 📎 Resources

- **Live Presentation:** https://houstoncmd.github.io/sdlc-cicd-presentation/
- **GitHub Repo:** https://github.com/houstoncmd/sdlc-cicd-presentation
- **Edit Mode:** กดปุ่ม "✏️ Edit Mode" บนหน้าเว็บเพื่อแก้ไข content ได้เลย

---

> *Prepared by Houston ✨ Mission Control Center 🛰️*
> *Last updated: June 14, 2026*
