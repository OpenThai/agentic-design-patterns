# หลักการ — Agentic Design Patterns

หลักการสำคัญในการออกแบบและเลือกใช้ Agentic Design Patterns

---

## 1. Start Simple

เริ่มจาก Pattern ที่ง่ายที่สุดที่แก้ปัญหาได้ ก่อนเพิ่มความซับซ้อน อย่าใช้ Multi-agent ถ้า Prompt Chaining พอ อย่าใช้ Planning ถ้า Routing พอ

> ยิ่ง Simple ยิ่ง Debug ง่าย ยิ่ง Maintenance ถูก

## 2. Prefer Deterministic Workflow Where Possible

ถ้าสามารถเขียน logic แบบ if-else หรือ state machine ได้โดยไม่ต้องใช้ LLM — ให้ทำ การใช้ LLM เพิ่มค่าใช้จ่าย ความหน่วง และความไม่แน่นอน

> ใช้ AI เฉพาะตรงที่จำเป็น ที่เหลือใช้ Deterministic Logic

## 3. Use Agents Only When Useful

ไม่ทุกปัญหาต้องใช้ Agent คำถามตรงๆ ที่ตอบได้ด้วยความรู้ของ LLM — ไม่ต้องใช้ Tool-use คำถามที่ไม่ต้องหาข้อมูลเพิ่ม — ไม่ต้องใช้ RAG

> Agent เพิ่ม Cost และ Complexity ถ้าไม่จำเป็น อย่าใช้

## 4. Keep Humans in Control

ระบบที่ปล่อยให้ Agent ตัดสินใจเองโดยไม่มีมนุษย์ตรวจสอบ — เสี่ยง ผู้ออกแบบต้องรู้ว่า:
- จุดไหนควรให้มนุษย์ approve
- จุดไหนควรให้มนุษย์ review
- จุดไหนควร alert มนุษย์เมื่อ Agent สงสัย

> Agent ช่วยทำงาน — มนุษย์ตัดสินใจ

## 5. Make Tool Use Explicit

Agent ควรประกาศ intention ก่อนใช้เครื่องมือทุกครั้ง:
- จะเรียก tool อะไร
- ด้วย parameters อะไร
- ผลลัพธ์ที่คาดหวังคืออะไร

การประกาศ intention ช่วยให้ Debug และ Audit ได้

## 6. Verify Important Outputs

อย่าเชื่อผลลัพธ์ของ Agent โดยไม่ตรวจสอบ สำหรับงานที่มีผลกระทบสูง ควรมี Verification Layer เสมอ — Guardrails, Schema Validation, หรือ Human Review

> Trust but Verify — โดยเฉพาะตอนที่ Agent ผิดพลาดแล้วสร้างความเสียหายได้

## 7. Trace Every Important Action

Agentic Systems มีหลายขั้นตอน ถ้าไม่มีการ Trace จะไม่รู้ว่าผิดพลาดตรงไหน ควร Log:
- Input ที่ได้รับ
- การตัดสินใจแต่ละขั้น
- Tool call และผลลัพธ์
- Output ที่ส่งออก

> Observable Systems = Debuggable Systems

## 8. Design for Failure

Agent จะผิดพลาด — นี่คือความจริงที่ต้องยอมรับและออกแบบรองรับ:
- Fallback เมื่อ Agent ทำงานไม่ได้
- Retry logic สำหรับ transient errors
- Escalation เมื่อไม่สามารถตัดสินใจได้
- Timeout สำหรับ operation ที่ค้าง
- Graceful degradation

> Hope for the best, design for the worst

## 9. Avoid Unnecessary Multi-agent Complexity

Multi-agent systems เป็น Pattern ที่ซับซ้อนที่สุด มีปัญหาใหม่ๆ เช่น:
- Agent ทำงานซ้อนกัน
- Agent รอกันตาย (deadlock)
- Agent ส่งข้อมูลผิดกัน
- ค่าใช้จ่ายสูงขึ้นหลายเท่า

ใช้เมื่อจำเป็นจริงๆ เช่น งานที่ต้องใช้ Expert ที่แตกต่างกันอย่างชัดเจน

---

## สรุป

หลักการเหล่านี้ช่วยให้คุณออกแบบ Agentic Systems ที่:
- ง่ายและปลอดภัย
- Debug ได้
- ไม่สิ้นเปลืองโดยไม่จำเป็น
- ไว้ใจได้ในระดับที่ยอมรับได้

ใช้ Principles เป็นเข็มทิศเวลาเลือก Pattern และออกแบบระบบ
