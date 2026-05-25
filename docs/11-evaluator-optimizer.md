# 11 — Evaluator-Optimizer

---

## Pattern นี้คืออะไร?

Evaluator-Optimizer คือรูปแบบที่แยก **ผู้ตรวจสอบ (Evaluator)** และ **ผู้ปรับปรุง (Optimizer)** ออกจากกัน — Evaluator ตรวจสอบคุณภาพของ Output และ Optimizer ปรับปรุงตาม Feedback ทำงานเป็น Loop จนกว่าผลลัพธ์ผ่านเกณฑ์

```
Generate ──→ Output
  │
  ▼
Evaluator ──→ ตรวจสอบคุณภาพ
  │              │
  ├─ ผ่าน ──────→ Done
  │
  └─ ไม่ผ่าน ───→ Feedback → Optimizer → ปรับปรุง → ตรวจสอบอีกครั้ง
```

ต่างจาก Reflection ตรงที่ Evaluator-Optimizer ใช้ **คนละ Agent** หรือ **คนละ mechanism** ในการตรวจสอบและปรับปรุง ทำให้มีความเป็นอิสระและ objectivity มากขึ้น

---

## เมื่อไหร่ควรใช้

- ต้องการคุณภาพสูงมาก
- มีเกณฑ์วัดคุณภาพที่ชัดเจน
- งานที่ต้องปรับปรุงหลายรอบ
- มี evaluator ที่ reliable (human หรือ automated)

**ตัวอย่าง:**
- การเขียนบทความคุณภาพสูง
- Code Generation + Review
- Data Extraction + Validation
- Translation + Quality Check

## เมื่อไหร่ไม่ควรใช้

- งานทั่วไปที่ Reflection พอ
- งานที่ไม่มีเกณฑ์วัดคุณภาพชัดเจน
- งานที่ต้องการความเร็ว (Loop ยาว)
- ระบบที่มี Cost จำกัด

---

## ข้อดี

- **คุณภาพดีที่สุด** — ตรวจสอบและปรับปรุงหลายรอบ
- **Objective** — Evaluator เป็นอิสระจาก Optimizer
- **Flexible** — ใช้ Evaluator ได้หลายแบบ (LLM, Rule, Human)
- **Measurable** — รู้ว่าผ่านเกณฑ์เมื่อไหร่

## ข้อเสีย

- **ช้าที่สุด** — หลายรอบ อาจ 3-5x
- **แพงที่สุด** — แต่ละรอบเสียค่าใช้จ่าย
- **Evaluator ก็ผิดได้** — ถ้า evaluator ไม่ดี = ทั้ง loop ไร้ค่า
- **Over-optimization** — ปรับปรุงจนเกินจำเป็น

---

## การทำงาน

### 1. Initial Generation
Optimizer สร้าง Output ครั้งแรก

### 2. Evaluation
Evaluator ตรวจสอบ Output ตามเกณฑ์:
- ความถูกต้องของข้อมูล
- ความครบถ้วน
- รูปแบบ/โครงสร้าง
- ความปลอดภัย

### 3. Feedback
Evaluator ส่ง Feedback ให้ Optimizer:
- จุดที่ต้องแก้ไข
- ข้อเสนอแนะ

### 4. Optimization
Optimizer ปรับปรุงตาม Feedback

### 5. Loop
ทำซ้ำจนกว่า Evaluator จะพอใจ หรือครบรอบสูงสุด

---

## ตัวอย่าง: Content Writing System

```
Loop 1:
  Optimizer: เขียนบทความ "AI สำหรับธุรกิจ"
  Evaluator: "เนื้อหาดี แต่ขาดตัวอย่างจริง
              และไม่มีข้อมูลสถิติสนับสนุน
              ให้คะแนน: 6/10"
  → Feedback: เพิ่มตัวอย่างและสถิติ

Loop 2:
  Optimizer: ปรับปรุง — เพิ่มตัวอย่าง 3 กรณี, สถิติ
  Evaluator: "ดีขึ้น ตอนนี้มีตัวอย่างและสถิติ
              แต่ส่วนสรุปยังสั้นไป
              ให้คะแนน: 8/10"
  → Feedback: ขยายส่วนสรุป

Loop 3:
  Optimizer: ปรับปรุงส่วนสรุป
  Evaluator: "ผ่าน ครบถ้วนทุกประเด็น
              ให้คะแนน: 9/10"
  → Output ผ่านเกณฑ์
```

---

## ตัวอย่าง Evaluator ประเภทต่างๆ

| ประเภท | วิธีการ | ข้อดี | ข้อเสีย |
|---|---|---|---|
| LLM-as-Judge | ใช้ LLM ตรวจสอบ | ยืดหยุ่น | มี bias |
| Rule-based | ตรวจสอบตามเกณฑ์ | แน่นอน | ไม่ flexible |
| Human Evaluator | มนุษย์ตรวจสอบ | แม่นยำ | ช้า, แพง |
| Test-based | รัน test cases | เชื่อถือได้ | ต้องมี tests |

---

## Design Guidelines

1. **กำหนดเกณฑ์ชัดเจน** — Evaluator ต้องรู้ว่าอะไรคือ "ดี"
2. **จำกัดรอบ** — 3-5 รอบสูงสุด เกินนั้น diminishing returns
3. **ตรวจสอบ Evaluator** — Evaluator เองต้อง calibration
4. **มี Fallback** — ถ้า Loop ไม่จบ ให้ส่ง version ล่าสุด
5. **Save intermediate** — เก็บทุกรุ่น เผื่อต้องย้อนกลับ

---

## ข้อผิดพลาดที่พบบ่อย

1. **Evaluator bias** — ชอบรูปแบบเดิมๆ ไม่ส่งเสริม creativity
2. **วงจรไม่จบ** — Optimizer แก้แล้ว Evaluator ก็ยังไม่พอ
3. **Oscillation** — กลับไป-มาระหว่าง solution เดิม
4. **Metric Gaming** — Optimizer ปรับให้ผ่าน metric แต่ quality ไม่ขึ้นจริง

---

## Safety และ Verification

- จำกัดจำนวน Iteration สูงสุด
- บันทึกทุก version สำหรับ rollback
- ตรวจสอบว่า Output แต่ละรอบดีขึ้นจริง
- Monitor Cost ต่อ Loop
- มี Human Review สำหรับงานสำคัญ

---

## สรุป

Evaluator-Optimizer คือ Pattern ที่ให้คุณภาพสูงที่สุดในบรรดา Patterns ทั้งหมด แต่มาพร้อม Cost และ Latency ที่สูงที่สุด ใช้เมื่อคุณภาพเป็นสิ่งสำคัญสูงสุด และคุณมีทรัพยากรพอ

**ข้อควรจำ:** Evaluator-Optimizer ≠ Magic — ถ้า Evaluator ไม่ดี ระบบก็ไม่ได้คุณภาพ

**บทต่อไป:** [12 — Guardrails & Verification](12-guardrails-and-verification.md) — การป้องกันและตรวจสอบความปลอดภัย
