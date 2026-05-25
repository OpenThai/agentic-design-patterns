# Pattern Comparison — เลือก Pattern ให้เหมาะกับงาน

---

## คำถามที่พบบ่อย

### 1. Simple Q&A — "ถ้าถาม-ตอบทั่วไป"

| Pattern | ความเหมาะสม | เหตุผล |
|---|---|---|
| **Routing** | ⭐⭐⭐⭐⭐ | แยกประเภทคำถาม → ส่งไปยัง Handler ที่เหมาะสม |
| Prompt Chaining | ⭐⭐ | Overhead เกินไป |
| RAG | ⭐⭐⭐ | ถ้าต้องการข้อมูลเพิ่ม |
| Multi-agent | ⭐ | Overkill ชัดๆ |

### 2. Research — "ช่วยค้นคว้าและสรุปข้อมูล"

| Pattern | ความเหมาะสม | เหตุผล |
|---|---|---|
| **Planning** | ⭐⭐⭐⭐⭐ | วางแผนการค้นคว้า → Execute → สรุป |
| Prompt Chaining | ⭐⭐⭐ | ใช้ได้แต่ยืดหยุ่นน้อยกว่า |
| RAG | ⭐⭐⭐⭐ | ถ้ามี knowledge base ในองค์กร |
| Multi-agent | ⭐⭐⭐ | ถ้าต้องการ researcher + analyst แยก |

### 3. Coding Tasks — "ช่วยเขียนโปรแกรม"

| Pattern | ความเหมาะสม | เหตุผล |
|---|---|---|
| **Prompt Chaining** | ⭐⭐⭐⭐ | Spec → Code → Test → Review |
| **Reflection** | ⭐⭐⭐⭐ | Code → Review → Fix |
| Evaluator-Optimizer | ⭐⭐⭐ | ถ้าต้องการ quality สูงมาก |
| Multi-agent | ⭐⭐⭐ | Dev + Reviewer แยก |

### 4. Personal Assistant — "ช่วยจัดการงานส่วนตัว"

| Pattern | ความเหมาะสม | เหตุผล |
|---|---|---|
| **Tool-use** | ⭐⭐⭐⭐⭐ | เรียก tools: calendar, email, search |
| Routing | ⭐⭐⭐⭐ | แยกประเภทคำขอ |
| Human-in-the-loop | ⭐⭐⭐⭐ | ก่อน action สำคัญ |
| Planning | ⭐⭐⭐ | งานซับซ้อน (วางทริป) |

### 5. High-risk Tasks — "การเงิน, กฎหมาย, การแพทย์"

| Pattern | ความเหมาะสม | เหตุผล |
|---|---|---|
| **Human-in-the-loop** | ⭐⭐⭐⭐⭐ | ต้องมีมนุษย์ตรวจสอบ |
| **Guardrails** | ⭐⭐⭐⭐⭐ | ป้องกันความผิดพลาด |
| RAG | ⭐⭐⭐⭐ | อ้างอิงข้อมูลจริง |
| Evaluator-Optimizer | ⭐⭐⭐⭐ | ตรวจสอบหลายรอบ |

---

## Patterns ที่มักถูก Overuse

### 1. Multi-agent Collaboration
**ถูกใช้มากเกินไป:** Developer สร้าง Agent หลายตัวทั้งที่ Agent เดียวทำงานได้
**ควรใช้เมื่อ:** งานต้องการ expertise ที่แตกต่างกันอย่างชัดเจน
**ทางเลือกที่ดีกว่า:** Orchestrator-Worker, Prompt Chaining

### 2. Planning
**ถูกใช้มากเกินไป:** งานที่ลำดับชัดเจนแต่ให้ Agent วางแผนเอง
**ควรใช้เมื่อ:** งานที่ต้องปรับเปลี่ยนแผนตลอดทาง
**ทางเลือกที่ดีกว่า:** Prompt Chaining

### 3. RAG
**ถูกใช้มากเกินไป:** ทุกคำถามต้องค้นเอกสาร
**ควรใช้เมื่อ:** คำตอบต้องอ้างอิงข้อมูลเฉพาะที่ LLM ไม่รู้
**ทางเลือกที่ดีกว่า:** ให้ LLM ตอบจากความรู้ (cheaper, faster)

### 4. Reflection
**ถูกใช้มากเกินไป:** Reflection ทุกครั้งทั้งที่รอบเดียวก็พอ
**ควรใช้เมื่อ:** ต้องการคุณภาพสูงขึ้น และมี budget พอ
**ทางเลือกที่ดีกว่า:** Review รอบเดียวก็พอสำหรับงานทั่วไป

---

## Decision Tree แบบง่าย

```
งานนี้ต้องการอะไร?
│
├─ ตอบคำถาม
│   ├─ มี Knowledge Base → RAG
│   └─ ไม่มี → Routing หรือ Direct LLM
│
├─ สร้างเนื้อหา
│   ├─ ต้องการ quality สูง → Evaluator-Optimizer
│   ├─ ต้องการ quality ปานกลาง → Reflection
│   └─ ต้องการเร็ว → Prompt Chaining
│
├─ ทำงานกับระบบภายนอก
│   └─ Tool-use + Guardrails
│
├─ ต้องการความปลอดภัย
│   └─ Human-in-the-loop + Guardrails
│
└─ งานซับซ้อนมาก
    ├─ ลำดับชัดเจน → Prompt Chaining
    ├─ ต้องผู้เชี่ยวชาญหลายด้าน → Orchestrator-Worker
    └─ ต้องการ collaboration → Multi-agent
```

---

## Cost/Latency/Quality Comparison

| Pattern | Cost | Latency | Quality |
|---|---|---|---|
| Direct LLM | ต่ำ | ต่ำ | ปานกลาง |
| Prompt Chaining | กลาง | กลาง | ดี |
| Routing | ต่ำ-กลาง | ต่ำ | ดี |
| Planning | สูง | สูง | ดีมาก |
| Reflection | กลาง-สูง | กลาง-สูง | ดีมาก |
| Tool-use | กลาง | กลาง | ดี |
| RAG | กลาง | กลาง | ดี |
| Human-in-the-loop | กลาง | สูง | ดีมาก |
| Orchestrator-Worker | สูง | กลาง-สูง | ดีมาก |
| Multi-agent | สูงมาก | สูง | ดีมาก |
| Evaluator-Optimizer | สูงมาก | สูงมาก | ดีเยี่ยม |
| Guardrails | กลาง | ต่ำ-กลาง | ป้องกัน |

---

## คำแนะนำ

1. **เริ่มจาก Simple ก่อน** — ใช้ Direct LLM หรือ Routing
2. **เพิ่ม Pattern เมื่อจำเป็น** — ถ้า quality ไม่พอ → เพิ่ม Reflection หรือ RAG
3. **วัดผลก่อนเพิ่ม** — รู้ baseline ก่อนเพิ่ม complexity
4. **Compose Patterns** — Patterns ใช้ร่วมกันได้ (เช่น RAG + Routing + Human-in-the-loop)
5. **รู้ว่าเมื่อไหร่ควรหยุด** — Cost/Quality trade-off มีจุด diminishing returns
