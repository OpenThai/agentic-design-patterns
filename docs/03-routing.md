# 03 — Routing

---

## Pattern นี้คืออะไร?

Routing คือการจัดเส้นทางคำขอของผู้ใช้ไปยัง **Model, Prompt, Workflow, หรือ Agent ที่เหมาะสม** โดยพิจารณาจาก characteristics ของคำขอ

```
            ┌─ Technical Question? ─→ Specialist Agent A
            │
Input ─────→┤─ General Question? ──→ General Agent B
            │
            └─ Sensitive Question? ─→ Human Review → Agent C
```

Routing มักประกอบด้วย 2 ส่วน:
1. **Classifier** — วิเคราะห์และจัดหมวดหมู่คำขอ
2. **Router** — ส่งต่อไปยังปลายทางที่เลือก

---

## เมื่อไหร่ควรใช้

- คำขอมีหลายประเภทที่ต้องใช้ความสามารถต่างกัน
- ต้องการใช้ Model ต่างกันตามความซับซ้อน (ประหยัด cost)
- มี Workflow หลายแบบสำหรับผู้ใช้ประเภทต่างๆ
- ต้องการแยกการจัดการสำหรับคำขอที่ sensitive

**ตัวอย่าง:**
- Support Bot — แยก billing / technical / general
- Content Moderation — แยก safe / review / block
- Writing Assistant — แยก blog / email / code

## เมื่อไหร่ไม่ควรใช้

- คำขอทั้งหมดคล้ายกัน
- มี Workflow แบบเดียว
- Classification error มีผลร้ายแรง
- การเพิ่ม Router เป็น Overhead ที่ไม่จำเป็น

---

## ข้อดี

- **ลด Complexity** — แต่ละ Handler จัดการเฉพาะงานตัวเอง
- **ประหยัด Cost** — ใช้ Model เล็กสำหรับงานง่าย
- **Flexible** — เพิ่ม Route ใหม่ได้ง่าย
- **สังเกตการณ์ง่าย** — รู้ว่าร้อยละเท่าไหร่ไปทางไหน

## ข้อเสีย

- **Classifier Error** — ถ้า classify ผิด ระบบจะทำงานผิดตั้งแต่ต้น
- **เพิ่ม Latency** — ต้องรอ Classifier ทำงานก่อน
- **Maintenance** — ต้องปรับ Routes เมื่อมีประเภทคำขอใหม่
- **Edge Cases** — คำถามก้ำกึ่งระหว่าง Routes

---

## การทำงาน

```
Input: "สั่งซื้อของที่ค้างไว้ได้ไหม"

1. Classifier:
   วิเคราะห์เจตนา → "billing / order management"

2. Router:
   จับคู่กับ Route → "Billing Workflow"

3. Execute:
   ส่งไปยัง Billing Agent
   Prompt: "คุณคือ Agent ด้าน billing..."
```

### Routing Logic แบบต่างๆ

| แบบ | วิธีการ | เมื่อไหร่ใช้ |
|---|---|---|
| LLM-based | ให้ LLM classify | งานซับซ้อน หลายหมวด |
| Keyword-based | จับคำสำคัญ | งานง่าย หมวดชัดเจน |
| Embedding-based | เทียบ vector similarity | ต้องการ accuracy สูง |
| Hybrid | ผสมหลายวิธี | Production จริง |

---

## ตัวอย่าง: Customer Support Router

```
Input → Classifier
         │
         ├─ "billing" ──────────→ Model A (fast, cheap)
         │   Prompt: "คุณคือเจ้าหน้าที่ billing..."
         │
         ├─ "technical" ────────→ Model B (powerful)
         │   Prompt: "คุณคือวิศวกร support..."
         │
         ├─ "account" ──────────→ Model A
         │   Prompt: "คุณคือเจ้าหน้าที่ดูแลบัญชี..."
         │
         └─ "human" ───────────→ Escalate to Human
             (เฉพาะคำขอ sensitive หรือ escalation)
```

---

## ข้อผิดพลาดที่พบบ่อย

1. **Router ไม่ครอบคลุม** — มีคำขอตกหล่น ไม่มี default route
2. **Classifier overconfident** — classify ผิดแต่ไม่บอก confidence
3. **Route ซับซ้อนเกินไป** — 20 routes จัดการไม่ไหว
4. **ไม่มีการ fallback** — เมื่อ route ไม่มี ระบบพัง

---

## Safety และ Verification

- ตรวจสอบ Classification Result ก่อน Route
- มี Default Route สำหรับคำขอที่ไม่ตรงหมวด
- Log Route Decision ทุกครั้ง
- Monitor Distribution ของ Routes — ถ้า route หนึ่งถูกใช้น้อยเกินไป = อาจมีปัญหา
- มี Fallback Route เมื่อ primary route ล้มเหลว

---

## สรุป

Routing ช่วยจัดระเบียบคำขอหลายประเภทให้เป็นระบบ ลด Complexity ของแต่ละ Handler และประหยัด Cost ด้วยการใช้ Model เหมาะกับงาน

**ข้อควรจำ:** คุณภาพของ Routing ขึ้นอยู่กับคุณภาพของ Classifier ลงทุนกับ Classifier ให้ดี

**บทต่อไป:** [04 — Planning](04-planning.md) — การวางแผนและดำเนินการตามแผน
