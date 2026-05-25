# Example: Evaluator-Optimizer

**Pattern:** Evaluator-Optimizer

---

## Scenario

ระบบสร้าง Content คุณภาพสูง ที่ต้องตรวจสอบและปรับปรุงหลายรอบจนกว่าจะผ่านเกณฑ์

## Minimal Workflow

```
Generate Output
  │
  ▼
[Evaluator] ──→ ตรวจสอบตามเกณฑ์
  │
  ├─ ผ่าน → ส่ง Output
  │
  └─ ไม่ผ่าน
       │
       ▼
  [Optimizer] ──→ ปรับปรุงตาม Feedback
       │
       └─ → กลับไป Evaluator อีกครั้ง
```

## Input/Output Example

```
Input: "เขียนคำโปรยสินค้า สำหรับรองเท้าวิ่ง"

Generation: "รองเท้าวิ่งที่เบาที่สุด ที่คุณเคยใส่..."

Evaluator Feedback:
  ✅ จับประเด็นความเบาได้ดี
  ❌ ไม่มี Specific Feature
  ❌ ไม่มี Call-to-action
  ⭐ คะแนน: 6/10

Optimization: "รองเท้าวิ่งน้ำหนักเพียง 180g
              พร้อมเทคโนโลยี BOOST ที่ช่วยส่งแรง
              สั่งซื้อวันนี้ลด 15%"

Evaluator: "ดีขึ้นมาก มี feature และ CTA ⭐ 9/10 ✓"

Output: "รองเท้าวิ่งน้ำหนักเพียง 180g..."
```

## Risks

- Loop ไม่จบสักที → ต้องมี max iterations
- Evaluator bias → ชอบ style เดิมๆ
- Optimizer แก้ผิดจุด → quality ไม่ขึ้น
- Cost สูง (3-5x ของการ generate ครั้งเดียว)

## Future Implementation

- Evaluator checklist (structured criteria)
- Scoring system (1-10)
- Max 3 iterations
- ใช้คนละ LLM สำหรับ Evaluator และ Optimizer
- Track improvement รอบต่อรอบ
