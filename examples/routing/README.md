# Example: Routing

**Pattern:** Routing

---

## Scenario

Customer Support Bot ที่ต้องแยกประเภทคำถามและส่งต่อไปยัง Agent/Hander ที่เหมาะสม

## Minimal Workflow

```
Input: คำถามจากลูกค้า
  │
  ▼
[Classifier] ──→ วิเคราะห์เจตนา
  │
  ├─ "billing" ───────→ Billing Handler
  ├─ "technical" ─────→ Technical Handler
  ├─ "account" ───────→ Account Handler
  └─ "other" ─────────→ General Handler
```

## Input/Output Example

```
Input: "ค่าบริการเดือนนี้ทำไมแพงจัง"
  → Classified: "billing"
  → Routed to: Billing Handler
  → Response: "ค่าบริการเดือนนี้เพิ่มขึ้นเนื่องจาก..."

Input: "เข้าสู่ระบบไม่ได้"
  → Classified: "technical"
  → Routed to: Technical Handler
  → Response: "ลองขั้นตอนต่อไปนี้..."
```

## Risks

- Classifier ผิด → คำตอบไม่ตรงกับปัญหา
- คำถามก้ำกึ่งระหว่างหลายหมวดหมู่
- ต้องมี "catch-all" route สำหรับกรณีที่ไม่ตรงหมวด

## Future Implementation

- LLM-based classifier + keyword fallback
- แต่ละ route มี prompt template ของตัวเอง
- Log route decision ทุกครั้ง
- Human escalation route สำหรับ sensitive issues
