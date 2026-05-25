# 05 — Reflection

---

## Pattern นี้คืออะไร?

Reflection คือการให้ AI Agent **ตรวจสอบผลลัพธ์ของตัวเอง** หาจุดบกพร่อง และปรับปรุงให้ดีขึ้น เป็นการจำลองกระบวนการ "คิดทบทวนก่อนส่ง" ที่มนุษย์ทำ

```
Generate ──→ Output
  │
  ▼
Critique ──→ หาจุดที่ต้องปรับปรุง
  │
  ▼
Refine ────→ ปรับปรุงตาม Critique
  │
  ▼
[Optional] Repeat until quality threshold met
```

---

## เมื่อไหร่ควรใช้

- งานที่ต้องการคุณภาพสูงกว่าการ Generate รอบเดียว
- งานที่ข้อผิดพลาดมีผลกระทบสูง (เช่น สรุปเอกสารสำคัญ)
- งานที่ LLM มัก hallucinate หรือทำผิดซ้ำๆ
- งานที่ต้องการความละเอียดรอบคอบ

**ตัวอย่าง:**
- เขียนบทความ / เอกสาร
- Code Review
- Data Extraction
- Translation
- Logical Reasoning

## เมื่อไหร่ไม่ควรใช้

- งานที่ต้องการความเร็ว (Reflection เพิ่มรอบ)
- งานที่ LLM ทำได้ดีอยู่แล้วในครั้งเดียว
- งานที่ LLM วิจารณ์ตัวเองไม่เก่ง (บาง Model)
- ตัวอย่างคำถาม simple Q&A

---

## ข้อดี

- **คุณภาพดีขึ้น** — LLM มักหาจุดผิดของตัวเองได้
- **ลด Hallucination** — พบและแก้ก่อนส่ง
- **ไม่ต้องใช้เครื่องมือเพิ่ม** — แค่เรียก LLM อีกรอบ
- **ใช้กับงานได้หลากหลาย** — Writing, Code, Analysis

## ข้อเสีย

- **เพิ่ม Cost** — ต้องเรียก LLM เพิ่ม 2x
- **เพิ่ม Latency** — เสียเวลาเพิ่ม 1-2 รอบ
- **LLM อาจวิจารณ์ผิด** — บอกว่าผิดทั้งที่ถูก หรือกลับกัน
- **Over-refinement** — แก้แล้วไม่ได้ดีขึ้น หรือแย่ลง

---

## การทำงาน

### แบบพื้นฐาน (Single Reflection)

```
User: "เขียนบทความสั้นเกี่ยวกับ AI"

Step 1 — Generate
  Output: "AI คือเทคโนโลยีที่..." (200 คำ)

Step 2 — Critique Prompt:
  "ตรวจสอบบทความนี้ หาจุดที่:
   - ข้อมูลไม่ถูกต้อง
   - อธิบายไม่ชัดเจน
   - ขาดตัวอย่าง
   - ภาษายากเกินไป"

Step 3 — Refine Prompt:
  "ปรับปรุงบทความตามคำแนะนำนี้:
   [Critique Output]"
```

### แบบ Loop (Multiple Reflections)

```
Generate → Critique → Refine → Critique → Refine → Done
                                   │
                                   └─ (หยุดเมื่อผ่านเกณฑ์หรือครบรอบ)
```

---

## Reflection มีกี่แบบ

| แบบ | วิธีการ | ใช้เมื่อ |
|---|---|---|
| Self-Reflection | LLM วิจารณ์งานตัวเอง | ทั่วไป |
| External Critique | LLM ตัวอื่นวิจารณ์ | ต้องการมุมมองที่แตกต่าง |
| Structured Critique | ตรวจสอบตาม checklist | ต้องการความครบถ้วน |
| Comparative | เปรียบเทียบกับตัวอย่างที่ดี | มี reference |

---

## ข้อผิดพลาดที่พบบ่อย

1. **Critique ไม่มีประโยชน์** — "ปรับปรุงภาษาให้ดีขึ้น" = vague ไม่ช่วย
2. **Refine แย่กว่าเดิม** — บางครั้ง LLM แก้แล้วแย่ลง
3. **Loop ไม่มีที่สิ้นสุด** — ต้องมี max iterations
4. **Critique เชื่อถือไม่ได้** — LLM วิจารณ์ไม่ถูกต้อง

---

## หลักการทำ Reflection ให้ได้ผล

1. **Critique Prompt ต้อง specific** — ไม่ใช่ "ตรวจสอบหน่อย" แต่ "ตรวจสอบ argument 3 ข้อนี้ว่าสมเหตุสมผลไหม"
2. **มีเกณฑ์วัดคุณภาพ** — รู้ว่าเมื่อไหร่ควรหยุด
3. **จำกัดรอบ** — 2-3 รอบสูงสุด เกินนั้น diminishing returns
4. **ตรวจสอบ Critique** — Critique เองก็ผิดพลาดได้

---

## Safety และ Verification

- เก็บประวัติการ Reflection ทุกรอบ
- จำกัดจำนวน Reflection Loop
- ตรวจสอบว่า Output หลัง Reflection ดีขึ้นจริง
- มี Fallback — ถ้า Reflection ทำให้แย่ลง ให้ใช้ version ก่อนหน้า

---

## สรุป

Reflection เป็นวิธีง่ายๆ ที่ effective ในการปรับปรุงคุณภาพของ Agent โดยเฉพาะงานเขียนและการวิเคราะห์ แต่ต้องออกแบบ Critique ให้ดีและจำกัดรอบ

**ข้อควรจำ:** Reflection ใช้ได้กับหลาย Pattern — เพิ่ม Reflection เป็นขั้นตอนสุดท้ายของ Prompt Chaining หรือใช้เป็น Evaluator ใน Evaluator-Optimizer ก็ได้

**บทต่อไป:** [06 — Tool-use](06-tool-use.md) — การเรียกใช้เครื่องมือภายนอก
