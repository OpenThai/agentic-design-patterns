# Example: Retrieval-Augmented Generation (RAG)

**Pattern:** RAG

---

## Scenario

ระบบตอบคำถามบนเอกสารภายในองค์กร (HR Policy, ขั้นตอนการทำงาน, ประกาศต่างๆ)

## Minimal Workflow

```
Query จากผู้ใช้
  │
  ▼
[1. Embed Query] ──→ แปลงเป็น Vector
  │
  ▼
[2. Search Vector DB] ──→ ค้นหาเอกสารที่เกี่ยวข้อง
  │
  ▼
[3. Rank Results] ─────→ จัดลำดับความเกี่ยวข้อง
  │
  ▼
[4. Build Context] ────→ รวมเอกสารเป็น Context
  │
  ▼
[5. Generate Answer] ──→ LLM ตอบโดยใช้ Context
  │
  ▼
Answer + Sources
```

## Input/Output Example

```
Query: "ลาพักร้อนได้กี่วัน"

Retrieved:
  - "นโยบายการลา พ.ศ. 2568 — พนักงานประจำมีสิทธิลาพักร้อน 10 วัน..."
  - "คู่มือ employee — การลาพักร้อนต้องแจ้งล่วงหน้า 7 วัน..."

Answer: "พนักงานประจำมีสิทธิลาพักร้อน 10 วันต่อปี
         ต้องแจ้งล่วงหน้า 7 วัน (อ้างอิง: นโยบายการลา พ.ศ. 2568)"
```

## Risks

- Retrieval เจอเอกสารไม่เกี่ยวข้อง → ตอบผิด
- เอกสารต้นทางมีข้อมูลเก่า → คำตอบล้าสมัย
- LLM ไม่ใช้ context ที่ให้ → hallucinate
- Chunking ไม่ดี → ข้อมูลขาดหาย

## Future Implementation

- Document ingestion pipeline (chunk → embed → index)
- Hybrid search (keyword + vector)
- Source citation ในทุกคำตอบ
- การอัปเดต Knowledge Base อัตโนมัติ
