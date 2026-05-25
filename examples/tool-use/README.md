# Example: Tool-use

**Pattern:** Tool-use

---

## Scenario

Personal Assistant Agent ที่ใช้เครื่องมือต่างๆ เพื่อตอบคำถามและทำงานให้ผู้ใช้

## Minimal Workflow

```
Input: คำขอจากผู้ใช้
  │
  ▼
[Analyze] ──→ ต้องใช้เครื่องมืออะไร?
  │
  ▼
[Select Tool] ──→ เลือกเครื่องมือที่เหมาะสม
  │
  ▼
[Execute] ──────→ เรียกใช้เครื่องมือ
  │
  ▼
[Handle Result] ──→ ประมวลผลและสรุป
  │
  ▼
Output: คำตอบ
```

## Input/Output Example

```
Input: "เมื่อวานหุ้น Apple ปิดที่เท่าไหร่"

Tool Selection:
  → Function: get_stock_price(symbol="AAPL", date="2025-05-24")
  → Result: $192.50

Output: "หุ้น Apple ปิดที่ $192.50 เมื่อวานนี้"

---

Input: "ส่งอีเมลหาปัญญาว่าประชุมพรุ่งนี้เลื่อนเป็นบ่ายสอง"

Tool: send_email(to="panya@company.com",
                 subject="เลื่อนประชุม",
                 body="...")
Result: "ส่งอีเมลเรียบร้อย"
```

## Risks

- เลือก tool ผิด (เช่น ใช้ search tool ทั้งที่ควรใช้ database)
- Tool arguments ผิด format
- Tool error (API ล่ม, network error)
- Security — เรียก tool อันตรายโดยไม่ได้รับอนุญาต

## Future Implementation

- Tool registry พร้อม schema definition
- Input validation ก่อนเรียก tool
- Error handling และ retry logic
- Logging ทุก tool call
- Permission levels สำหรับแต่ละ tool
