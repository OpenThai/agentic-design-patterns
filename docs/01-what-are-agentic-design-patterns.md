# 01 — Agentic Design Patterns คืออะไร

---

## แนวคิดพื้นฐาน

Agentic Design Pattern คือ **รูปแบบสถาปัตยกรรมที่ใช้ซ้ำได้** สำหรับออกแบบระบบที่ใช้ AI Agent ในการทำงาน เช่นเดียวกับ Gang of Four Design Patterns ในการพัฒนา Software — Patterns ช่วยให้คุณไม่ต้องออกแบบวิธีแก้ปัญหาใหม่ทุกครั้งที่มีปัญหา

ความแตกต่างคือ Agentic Design Patterns ต้องจัดการกับความไม่แน่นอนของ LLMs:
- LLMs ตอบไม่เหมือนเดิมทุกครั้ง (Non-deterministic)
- LLMs ทำผิดพลาดได้ (Hallucination)
- LLMs มีข้อจำกัดด้าน Context Window
- LLMs เรียกใช้เครื่องมือที่อันตรายได้

Patterns ต่างๆ ช่วยลดปัญหาเหล่านี้

---

## ทำไมต้องใช้ Design Patterns?

| ปัญหา | วิธีแก้แบบไม่มี Pattern | วิธีแก้แบบมี Pattern |
|---|---|---|
| Agent ตอบไม่ตรง | สุ่ม Prompt ไปเรื่อย | ใช้ Routing แยกประเภทคำถาม |
| Agent ใช้ข้อมูลผิด | หวังว่า LLM จะรู้ | ใช้ RAG ดึงข้อมูลจริง |
| Agent ทำอันตราย | หวังว่า LLM จะดี | ใช้ Guardrails + Human-in-the-loop |
| Agent ทำงานซับซ้อนไม่ได้ | Prompt ยาวๆ เละๆ | ใช้ Planning หรือ Chaining |

---

## ประเภทของ Patterns

### Workflow Patterns
เน้นการจัดลำดับและควบคุมขั้นตอนการทำงาน:
- **Prompt Chaining** — แบ่งงานเป็นขั้นตอน
- **Routing** — จัดเส้นทางคำขอ
- **Planning** — วางแผนก่อนทำงาน

### Verification Patterns
เน้นการตรวจสอบและปรับปรุงคุณภาพ:
- **Reflection** — ตรวจสอบตัวเอง
- **Evaluator-Optimizer** — Loop ตรวจสอบ-ปรับปรุง
- **Guardrails & Verification** — ป้องกันและตรวจสอบ

### Tool-use Patterns
เน้นการเข้าถึงข้อมูลและเครื่องมือภายนอก:
- **Tool-use** — เรียก API, Database, Calculator
- **RAG** — ดึงข้อมูลจาก Knowledge Base

### Human Oversight Patterns
เน้นการควบคุมโดยมนุษย์:
- **Human-in-the-loop** — อนุมัติ ตรวจสอบ แก้ไข

### Multi-agent Patterns
เน้นการทำงานร่วมกันหลาย Agent:
- **Orchestrator-Worker** — ตัวกลางมอบหมายงาน
- **Multi-agent Collaboration** — Agent ทำงานร่วมกัน

---

## วิธีเลือก Pattern ที่เหมาะสม

```
งานนี้ต้องการอะไร?
│
├─ ตอบคำถาม → Routing หรือ RAG
├─ สร้างเนื้อหา → Prompt Chaining หรือ Reflection
├─ ทำงานกับข้อมูล → Tool-use หรือ RAG
├─ ควบคุมคุณภาพ → Evaluator-Optimizer
├─ ปลอดภัยสำคัญที่สุด → Guardrails + Human-in-the-loop
└─ ซับซ้อนมาก → Planning → Orchestrator → Multi-agent
```

---

## หลักการสำคัญ

1. **Start Simple** — ใช้ Pattern ที่ง่ายที่สุดเท่าที่จำเป็น
2. **Compose Patterns** — Patterns ทำงานร่วมกันได้ (เช่น RAG + Human-in-the-loop)
3. **Know When to Stop** — ไม่ทุกระบบต้องเป็น Multi-agent
4. **Design for Observability** — ต้องรู้ว่า Agent ทำอะไรบ้าง

---

## สรุป

Agentic Design Patterns ช่วยให้คุณสร้างระบบ AI Agent ที่:
- เชื่อถือได้มากขึ้น
- Debug ได้ง่ายขึ้น
- ปลอดภัยมากขึ้น
- ดูแลรักษาได้ง่ายขึ้น

เริ่มจากเข้าใจ Problems ก่อน แล้วค่อยเลือก Pattern ที่เหมาะสม

**บทต่อไป:** [02 — Prompt Chaining](02-prompt-chaining.md) — เรียนรู้การแบ่งงานเป็นขั้นตอนต่อเนื่อง
