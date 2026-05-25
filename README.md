# Agentic Design Patterns

**คู่มือ Design Patterns สำหรับสร้าง AI Agent — ใช้ภาษาไทย อธิบายให้เข้าใจง่าย เน้นใช้งานจริง**

> สร้างโดย OpenThai — ชุมชนผู้สนใจ Personal AI, AI Agent, Memory & Context System, Tool-use, MCP, และโครงสร้างพื้นฐาน AI สำหรับคนไทย

---

## Agentic Design Patterns คืออะไร?

Agentic Design Patterns คือ **รูปแบบสถาปัตยกรรมที่ใช้ซ้ำได้** สำหรับการออกแบบระบบที่ขับเคลื่อนด้วย AI Agent แทนที่จะเขียนโค้ดทุกอย่างตั้งแต่เริ่มต้น คุณสามารถใช้ Patterns เหล่านี้เพื่อสร้างระบบที่:

- แก้ปัญหาที่ซับซ้อนได้โดยการแบ่งเป็นขั้นตอน
- ใช้เครื่องมือภายนอกอย่างมีประสิทธิภาพ
- ตรวจสอบและปรับปรุงผลลัพธ์ของตัวเองได้
- ทำงานร่วมกับมนุษย์อย่างปลอดภัย
- รองรับการทำงานแบบ Multi-agent ได้

---

## ทำไม Design Pattern ถึงสำคัญสำหรับ AI Agent?

LLMs และ AI Agents มีข้อจำกัดหลายอย่างที่ Design Patterns ช่วยแก้ได้:

| ปัญหา | Pattern ที่ช่วย |
|---|---|
| LLM ตอบยาวเกินไป ไม่ตรงประเด็น | Prompt Chaining, Routing |
| LLM ไม่มีข้อมูลล่าสุด | RAG, Tool-use |
| LLM ทำงานผิดพลาดโดยไม่รู้ตัว | Reflection, Evaluator-Optimizer |
| LLM ใช้เครื่องมืออันตราย | Guardrails, Human-in-the-loop |
| งานซับซ้อนเกินไปสำหรับ Agent เดียว | Planning, Orchestrator-Worker, Multi-agent |

---

## ใครควรอ่าน repos นี้

- **นักพัฒนา** ที่เริ่มสนใจสร้าง AI Agent
- **ทีม产品** ที่ต้องการออกแบบระบบที่ใช้ LLM
- **ผู้เรียน** ที่ผ่าน "AI Agent Fundamentals" มาแล้วและอยากต่อยอด
- **คนไทย** ที่อยากเรียนรู้เรื่อง Agentic Systems ด้วยภาษาไทย

---

## ความสัมพันธ์กับ OpenThai และ AI Agent Fundamentals

Repository นี้เป็นส่วนหนึ่งของ **OpenThai** — โปรเจกต์ต่อเนื่องจาก [AI Agent Fundamentals](https://github.com/OpenThai/ai-agent-fundamentals) ที่เจาะลึกเรื่อง Design Patterns โดยเฉพาะ

---

## เนื้อหา

### พื้นฐาน

| บท | หัวข้อ |
|---|---|
| [01 — Agentic Design Patterns คืออะไร](docs/01-what-are-agentic-design-patterns.md) | แนวคิดพื้นฐาน ประเภทของ Patterns |
| [02 — Prompt Chaining](docs/02-prompt-chaining.md) | แบ่งงานเป็นขั้นตอนต่อเนื่อง |
| [03 — Routing](docs/03-routing.md) | เส้นทางคำขอไปยังผู้ที่เหมาะสม |

### Patterns ควบคุมการทำงาน

| บท | หัวข้อ |
|---|---|
| [04 — Planning](docs/04-planning.md) | วางแผน แบ่งงาน และดำเนินการตามแผน |
| [05 — Reflection](docs/05-reflection.md) | ตรวจสอบตัวเอง ปรับปรุงผลลัพธ์ |
| [11 — Evaluator-Optimizer](docs/11-evaluator-optimizer.md) | ตรวจสอบและปรับปรุงแบบวนซ้ำ |

### Patterns การใช้เครื่องมือ

| บท | หัวข้อ |
|---|---|
| [06 — Tool-use](docs/06-tool-use.md) | การเรียกใช้เครื่องมือและ Function Calling |
| [07 — Retrieval-Augmented Generation](docs/07-retrieval-augmented-generation.md) | ดึงข้อมูลภายนอกมาใช้ประกอบการตอบ |

### Patterns การควบคุมโดยมนุษย์

| บท | หัวข้อ |
|---|---|
| [08 — Human-in-the-loop](docs/08-human-in-the-loop.md) | การอนุมัติและตรวจสอบโดยมนุษย์ |

### Multi-agent Patterns

| บท | หัวข้อ |
|---|---|
| [09 — Orchestrator-Worker](docs/09-orchestrator-worker.md) | ตัวประสานงานกลางมอบหมายงานให้ Worker |
| [10 — Multi-agent Collaboration](docs/10-multi-agent-collaboration.md) | Agent หลายตัวทำงานร่วมกัน |

### Patterns ความปลอดภัย

| บท | หัวข้อ |
|---|---|
| [12 — Guardrails & Verification](docs/12-guardrails-and-verification.md) | การป้องกันและตรวจสอบความถูกต้อง |

---

## เอกสารประกอบอื่นๆ

| ไฟล์ | คำอธิบาย |
|---|---|
| [PRINCIPLES.md](PRINCIPLES.md) | หลักการสำคัญในการออกแบบ Agentic Systems |
| [PATTERNS.md](PATTERNS.md) | ตารางเปรียบเทียบ Patterns ทั้งหมด |
| [GLOSSARY.md](GLOSSARY.md) | คำศัพท์สำคัญ |
| [ROADMAP.md](ROADMAP.md) | แผนการพัฒนา |
| [notes/course-outline.md](notes/course-outline.md) | โครงสร้างหลักสูตร |
| [notes/pattern-comparison.md](notes/pattern-comparison.md) | วิธีเลือก Pattern ให้เหมาะกับงาน |
| [notes/references.md](notes/references.md) | แหล่งอ้างอิงสำหรับศึกษาต่อ |

---

## สถานะ

🟡 **Educational Material ระยะเริ่มต้น** — เนื้อหาพร้อมอ่านและเรียนรู้ ตัวอย่างโค้ดกำลังพัฒนา

---

## การมีส่วนร่วม

Repo นี้เปิดรับ Contribution จากทุกคน โดยเฉพาะ:
- แก้ไขข้อผิดพลาด
- เพิ่มตัวอย่างการใช้งานจริง
- แปลเนื้อหาหรือเพิ่มมุมมองใหม่ๆ
- เสนอ Design Patterns เพิ่มเติม

---

## License

MIT License — ดูรายละเอียดที่ [LICENSE](LICENSE)
