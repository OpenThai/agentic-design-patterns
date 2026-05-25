# 12 — Guardrails & Verification

---

## Pattern นี้คืออะไร?

Guardrails & Verification คือ **ชั้นป้องกันและตรวจสอบ** ที่คอย确保 Agent ทำงานอยู่ในขอบเขตที่ปลอดภัย ถูกต้อง และเหมาะสม — ทั้งก่อน ระหว่าง และหลังการทำงานของ Agent

```
Input ──→ [Input Guard] ──→ Agent ──→ [Output Guard] ──→ Output
              │                              │
              ├─ Blocked: คำถามต้องห้าม      ├─ Blocked: ข้อมูล sensitive
              ├─ Blocked: prompt injection   ├─ Blocked: คำตอบไม่ปลอดภัย
              └─ ผ่าน                         └─ ผ่าน
```

---

## เมื่อไหร่ควรใช้

- ทุกระบบที่ใช้งานจริง (Production)
- ระบบที่เปิดให้ผู้ใช้ทั่วไป
- ระบบที่ Agent มี access 到工具或ข้อมูลสำคัญ
- ระบบที่ต้อง compliance (การเงิน, การแพทย์, กฎหมาย)

**ตัวอย่าง:**
- Chatbot สายการบิน — ต้องไม่คุยเรื่องการเมือง
- AI การเงิน — ต้องไม่แนะนำการลงทุนแบบเจาะจง
- Internal Tool — ต้องไม่เปิดเผยข้อมูลลับ

## เมื่อไหร่ไม่ควรใช้

- Prototype ระยะแรก
- ระบบ offline ที่ผู้ใช้คนเดียว
- เมื่องานวิจัยที่ต้องการดู raw capability ของ LLM

---

## ข้อดี

- **ปลอดภัย** — ป้องกันความเสียหาย
- **Compliance** — ผ่านข้อกำหนดทางกฎหมาย
- **เชื่อมั่นได้** — ผู้ใช้และ stakeholders ไว้วางใจ
- **ควบคุมได้** — กำหนดขอบเขตชัดเจน

## ข้อเสีย

- **เพิ่ม Complexity** — Guardrails layer เป็น code เพิ่ม
- **False Positive** — guardrail บล็อกของที่ดี
- **Performance** — การตรวจสอบทุกครั้งใช้เวลา
- **Maintenance** — ต้องปรับ guardrails เมื่อระบบเปลี่ยน

---

## Guardrails แบ่งตามตำแหน่ง

### 1. Input Guardrails
ตรวจสอบก่อนถึง Agent:
- Content Moderation — คำหยาบ เนื้อหาไม่เหมาะสม
- Prompt Injection Detection
- Topic Restriction — หัวข้อต้องห้าม
- Rate Limiting

### 2. Process Guardrails
ตรวจสอบระหว่างการทำงาน:
- Tool Call Validation — ตรวจ arguments ก่อนเรียก tool
- Budget Control — จำกัด Token/Cost
- Timeout — ป้องกัน infinite loop
- Step Limit — จำกัดจำนวน iteration

### 3. Output Guardrails
ตรวจสอบก่อนส่งให้ผู้ใช้:
- PII Detection — ป้องกันข้อมูลส่วนบุคคลรั่วไหล
- Factual Consistency — ตรวจสอบกับ source
- Policy Compliance — ตรงตามนโยบาย
- Format Validation — JSON valid? Schema ถูก?

---

## การทำงาน

### ตัวอย่าง: Customer Support with Guardrails

```
Input: "บอกวิธีลัดระบบผ่อนชำระหน่อย"

Input Guard:
  ├─ Topic Check: ✅ การเงินส่วนบุคคล (อนุญาต)
  ├─ Prompt Injection: ✅ ไม่พบ
  └─ Rate Limit: ✅ ยังไม่เกิน

Agent: "การลัดระบบผ่อนชำระไม่ใช่แนวทางที่ถูกต้อง..."

Output Guard:
  ├─ PII Check: ✅ ไม่มีข้อมูลส่วนบุคคล
  ├─ Policy Check: ✅ ไม่สนับสนุนการละเมิดกฎ
  └─ Tone Check: ✅ สุภาพ

Output: "..." → ส่งให้ผู้ใช้
```

---

## Verification Approaches

| Approach | วิธีการ | ใช้ตรวจสอบ |
|---|---|---|
| Schema Validation | JSON Schema | Output format |
| Regex / Pattern | จับคู่รูปแบบ | PII, รหัส, URL |
| LLM-as-Judge | LLM ตรวจสอบ | Content quality |
| Factual Check | เทียบกับ source | ความถูกต้อง |
| Policy Engine | Rule engine | Compliance |
| Test Suite | Automated tests | Functional |

---

## ข้อผิดพลาดที่พบบ่อย

1. **Over-guarding** — บล็อกทุกอย่างจน Agent ทำงานไม่ได้
2. **Under-guarding** — ตรวจสอบน้อยเกินไปจนเกิด incident
3. **Bypassable Guardrails** — ผู้ใช้绕过 guardrails ได้
4. **Guardrails ไม่ updated** — พฤติกรรม LLM เปลี่ยน guardrails ไม่ตาม
5. **Silent Blocking** — บล็อก output โดยไม่บอกผู้ใช้

---

## Design Guidelines

1. **Defense in Depth** — หลายชั้น ไม่พึ่งชั้นเดียว
2. **Fail Safe** — ถ้า guardrails ทำงานไม่ได้ = block
3. **Transparent** — บอกผู้ใช้เมื่อถูก block และอธิบายเหตุผล
4. **Testable** — guardrails แต่ละอันต้องทดสอบได้
5. **Monitorable** — รู้ว่า guardrails ทำงานบ่อยแค่ไหน false positive เท่าไหร่

---

## Safety Checklist

- [ ] Input Sanitization
- [ ] Prompt Injection Protection
- [ ] PII / Secret Detection
- [ ] Tool Call Validation
- [ ] Output Policy Check
- [ ] Rate Limiting
- [ ] Cost Budget
- [ ] Audit Log
- [ ] Incident Response Plan

---

## สรุป

Guardrails & Verification เป็น Pattern ที่ **ไม่ควรขาด** ในระบบ Production — ช่วยป้องกันทั้งผู้ใช้และระบบจากความผิดพลาดของ Agent

**ข้อควรจำ:** Guardrails ที่ดีไม่ใช่แค่การ Block แต่คือการ **ให้อิสระในขอบเขตที่ปลอดภัย**

---

## จบหลักสูตร Agentic Design Patterns

ยินดีด้วย! คุณได้เรียนรู้ Patterns ทั้ง 12 แบบแล้ว

**แนะนำต่อไป:**
- [PATTERNS.md](../PATTERNS.md) — เปรียบเทียบ Patterns
- [notes/pattern-comparison.md](../notes/pattern-comparison.md) — วิธีเลือก Pattern
- [notes/course-outline.md](../notes/course-outline.md) — โครงสร้างหลักสูตร
