# Example: Reflection

**Pattern:** Reflection

---

## Scenario

AI Writing Assistant ที่เขียนบทความแล้วตรวจสอบและปรับปรุงตัวเองก่อนส่ง

## Minimal Workflow

```
Input: หัวข้อ / ประเด็นที่ต้องการ
  │
  ▼
[Generate] ──→ เขียนครั้งแรก
  │
  ▼
[Critique] ──→ ตรวจสอบ จุดบกพร่อง
  │
  ▼
[Refine] ────→ ปรับปรุงตามคำวิจารณ์
  │
  ▼
[Optional] ──→ วนซ้ำจนกว่าคุณภาพผ่านเกณฑ์
  │
  ▼
Output: บทความที่ปรับปรุงแล้ว
```

## Input/Output Example

```
Input: "เขียนคำอธิบาย Concept 'Agentic RAG' "

Generate: "Agentic RAG คือ RAG ที่มี Agent ควบคุม..."

Critique: "อธิบายหลักการดี แต่
  - ขาดตัวอย่างการใช้งานจริง
  - ไม่ได้เปรียบเทียบกับ RAG ทั่วไป
  - ภาษาเทคนิคเกินไปสำหรับมือใหม่"

Refine: "Agentic RAG คือระบบที่เพิ่ม Agent เข้ามา
  ควบคุมกระบวนการค้นหาและตอบ..."

Critique: "ดีขึ้นแล้ว ผ่าน!"
```

## Risks

- Critique อาจชี้ผิดหรือแนะนำสิ่งที่ไม่ถูกต้อง
- Refine อาจทำให้ Output แย่ลง
- Multiple rounds เพิ่ม cost และ latency

## Future Implementation

- Structured critique template
- จำกัดรอบที่ 2-3 ครั้ง
- ใช้ evaluation criteria checklist
- เปรียบเทียบ version ก่อน-หลัง reflection
