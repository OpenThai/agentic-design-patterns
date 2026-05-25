# Pattern Catalog — เปรียบเทียบ Agentic Design Patterns

| Pattern | ใช้ทำอะไร | เหมาะกับงานแบบไหน | ข้อดี | ข้อควรระวัง | เริ่มอ่าน |
|---|---|---|---|---|---|
| **Prompt Chaining** | แบ่งงานใหญ่เป็นขั้นตอนต่อเนื่อง | งานที่มีลำดับชัดเจน เช่น เขียนบทความ สรุปเอกสาร | Debug ง่าย แต่ละขั้นตรวจสอบได้ | ถ้าขั้นแรกพลาด ขั้นต่อๆ ไปจะผิดตาม เพิ่ม Latency | [docs/02](docs/02-prompt-chaining.md) |
| **Routing** | จัดเส้นทางคำขอไปยังผู้ที่เหมาะสม | ระบบที่มีคำขอหลายประเภท, Customer Support Bot | ลด Complexity ของแต่ละ Handler | ต้องมี Classifier ที่แม่นยำ ถ้าผิดทางตั้งแต่ต้น = ผลลัพธ์ผิด | [docs/03](docs/03-routing.md) |
| **Planning** | วางแผนและดำเนินการตามแผน | งานซับซ้อนหลายขั้นตอน, Research Task | แก้ปัญหาที่ซับซ้อนได้ดี, ปรับแผนได้ | Plan อาจไม่ถูกต้องตั้งแต่แรก, กิน Token เยอะ | [docs/04](docs/04-planning.md) |
| **Reflection** | ตรวจสอบและปรับปรุงผลลัพธ์ตัวเอง | งานที่ต้องการคุณภาพสูง, Writing, Code Review | ช่วยลด Hallucination, ปรับปรุงคุณภาพ | LLM อาจวิจารณ์ไม่ถูก, เสียเวลาและค่าใช้จ่ายเพิ่ม | [docs/05](docs/05-reflection.md) |
| **Tool-use** | เรียกใช้เครื่องมือภายนอก | ต้องเข้าถึงข้อมูลจริง, API, Database | เพิ่มความสามารถของ Agent อย่างมาก | Tool Selection ผิดพลาดได้, Error Handling ซับซ้อน | [docs/06](docs/06-tool-use.md) |
| **RAG** | ดึงข้อมูลภายนอกมาช่วยตอบ | Q&A บนเอกสาร, Chat with PDF, Knowledge Base | ลด Hallucination, อ้างอิงแหล่งที่มาได้ | คุณภาพขึ้นอยู่กับ Retrieval, เพิ่ม Complexity | [docs/07](docs/07-retrieval-augmented-generation.md) |
| **Human-in-the-loop** | ให้มนุษย์ตรวจสอบก่อนดำเนินการ | งานเสี่ยง เช่น สั่งซื้อของ, ส่งอีเมล, อนุมัติ | ปลอดภัย, ได้มนุษย์ตรวจสอบ | ช้า, ต้องออกแบบ UX สำหรับ Approval | [docs/08](docs/08-human-in-the-loop.md) |
| **Orchestrator-Worker** | ตัวกลางมอบหมายงานให้ Worker | งานที่ต้องทำหลายอย่างพร้อมกัน, Data Pipeline | Scale ได้ดี, Worker แต่ละตัวทำเฉพาะทาง | Orchestrator เป็น Single Point of Failure | [docs/09](docs/09-orchestrator-worker.md) |
| **Multi-agent** | Agent หลายตัวทำงานร่วมกัน | งานซับซ้อนที่ต้องหลายความเชี่ยวชาญ | Modular, แต่ละ Agent ทำหน้าที่เฉพาะ | ซับซ้อนมาก, Communication Overhead, ราคาแพง | [docs/10](docs/10-multi-agent-collaboration.md) |
| **Evaluator-Optimizer** | ตรวจสอบและปรับปรุงแบบ Loop | งานที่ต้องคุณภาพสูงมาก, Data Extraction | คุณภาพดีที่สุดในบรรดา Patterns | ช้าที่สุด, แพงที่สุด, Overkill สำหรับงานทั่วไป | [docs/11](docs/11-evaluator-optimizer.md) |
| **Guardrails & Verification** | ป้องกันและตรวจสอบความปลอดภัย | ระบบที่ต้องการ Safety, Production Deployment | ป้องกันความเสียหาย, สร้างความเชื่อมั่น | เพิ่ม Complexity, ต้องออกแบบ Guard ให้ดี | [docs/12](docs/12-guardrails-and-verification.md) |

---

## วิธีเลือก Pattern

1. เริ่มจาก **Start Simple** — ใช้ Prompt Chaining หรือ Routing ก่อน
2. ถ้าต้องการข้อมูลภายนอก → **RAG** หรือ **Tool-use**
3. ถ้าต้องการคุณภาพสูงขึ้น → **Reflection** หรือ **Evaluator-Optimizer**
4. ถ้าปลอดภัยสำคัญที่สุด → **Human-in-the-loop** + **Guardrails**
5. ถ้างานซับซ้อนมาก → **Planning** → **Orchestrator-Worker** → **Multi-agent**

ดูเพิ่มเติม: [notes/pattern-comparison.md](notes/pattern-comparison.md)
