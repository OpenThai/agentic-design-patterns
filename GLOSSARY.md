# GLOSSARY — คำศัพท์ Agentic Design Patterns

---

### Agentic Design Pattern

รูปแบบสถาปัตยกรรมที่ใช้ซ้ำได้สำหรับการออกแบบระบบที่ขับเคลื่อนด้วย AI Agent ช่วยแก้ปัญหาทั่วไปที่เกิดขึ้นเมื่อใช้ LLMs ใน Production เช่น การควบคุมคุณภาพ การเรียกใช้เครื่องมือ และการทำงานร่วมกับมนุษย์

### Prompt Chaining

การแบ่งงานใหญ่ออกเป็นขั้นตอนย่อยๆ ที่ต่อเนื่องกัน แต่ละขั้นตอนรับ Input จากขั้นตอนก่อนหน้าและส่ง Output ไปยังขั้นตอนถัดไป ลดความซับซ้อนของแต่ละ Prompt และทำให้ Debug ได้ง่ายขึ้น

### Routing

การจัดเส้นทางคำขอของผู้ใช้ไปยัง Model, Prompt, Workflow, หรือ Agent ที่เหมาะสม โดยพิจารณาจากประเภทของคำขอ หมวดหมู่ หรือความซับซ้อนของงาน

### Planning

การให้ Agent วางแผนก่อนลงมือทำ แตกงานใหญ่ออกเป็นขั้นตอนย่อย เรียงลำดับการทำงาน และอาจมีการปรับแผนเมื่อพบอุปสรรค

### Reflection

การให้ Agent ตรวจสอบผลลัพธ์ของตัวเอง วิจารณ์จุดบกพร่อง และปรับปรุงให้ดีขึ้น มักใช้ร่วมกับ Evaluator-Optimizer เพื่อคุณภาพที่สูงขึ้น

### Tool-use

ความสามารถของ Agent ในการเรียกใช้เครื่องมือภายนอก เช่น API, Database, Calculator, Web Search, หรือ Code Interpreter ผ่าน Function Calling

### Function Calling

ความสามารถของ LLM ในการเลือกและเรียกใช้ฟังก์ชันที่กำหนดไว้ล่วงหน้า โดย Model จะส่งกลับ structured data (function name + arguments) แทนการตอบด้วยข้อความธรรมดา

### RAG (Retrieval-Augmented Generation)

Pattern ที่ให้ Agent ดึงข้อมูลจากแหล่งภายนอก (Vector Database, Document Store, Search Engine) มาใช้เป็น Context ในการสร้างคำตอบ ช่วยลด Hallucination และเพิ่มความถูกต้องของข้อมูล

### Orchestrator

Agent กลางที่ทำหน้าที่วางแผน มอบหมายงาน ติดตามความคืบหน้า และรวบรวมผลลัพธ์จาก Worker Agents หลายตัว

### Worker Agent

Agent ย่อยที่รับมอบหมายจาก Orchestrator เพื่อทำงานเฉพาะอย่าง มีขอบเขตและความรับผิดชอบที่จำกัด

### Evaluator

Component ที่ตรวจสอบคุณภาพของ Output ว่าเข้าเกณฑ์หรือไม่ สามารถเป็นได้ทั้ง LLM-based, Rule-based, หรือ Human

### Optimizer

Component ที่ปรับปรุง Output ตาม Feedback จาก Evaluator ทำงานเป็น Loop จนกว่าผลลัพธ์ผ่านเกณฑ์

### Guardrails

กลไกป้องกันไม่ให้ Agent ทำสิ่งที่ไม่ควรทำ เช่น พูดในหัวข้อต้องห้าม เรียก Tool ที่ไม่ได้รับอนุญาต หรือส่งข้อมูลที่ละเอียดอ่อนออกไป

### Human-in-the-loop

การให้มนุษย์เข้ามามีส่วนร่วมในกระบวนการทำงานของ Agent เช่น อนุมัติการดำเนินการ ทบทวนผลลัพธ์ หรือตัดสินใจในกรณีที่ Agent ไม่แน่ใจ

### Multi-agent

ระบบที่มี Agent หลายตัวทำงานร่วมกัน แต่ละตัวมีบทบาท ความเชี่ยวชาญ และความรับผิดชอบของตัวเอง สื่อสารและประสานงานกันผ่าน Protocol ที่กำหนด

### Verification

กระบวนการตรวจสอบความถูกต้องของ Output ก่อนส่งมอบให้ผู้ใช้ รวมถึงการตรวจสอบ Schema, Policy Compliance, Factual Accuracy, และ Safety Checks

### Observability

ความสามารถในการสังเกตและเข้าใจสถานะภายในของระบบผ่าน Logs, Metrics, Traces โดยเฉพาะพฤติกรรมของ Agent, Tool Calls, และ Decision Points

### Trace

บันทึกเส้นทางการทำงานของแต่ละ Request — ตั้งแต่ Input จนถึง Output — รวมถึงทุกขั้นตอนที่ Agent ผ่าน, Tool Calls, และการตัดสินใจ

### State

สถานะหรือบริบทที่ Agent ต้องจดจำระหว่างการทำงาน รวมถึง Conversation History, Tool Results, และ Intermediate Outputs

### Agent Runtime

สภาพแวดล้อมที่ใช้รัน Agent รวมถึง LLM Inference, Tool Execution, Memory Management, และ Orchestration Logic

---

> **Tips:** คำศัพท์เหล่านี้อาจมีความหมายแตกต่างกันเล็กน้อยในแต่ละ Framework แต่แนวคิดหลักเหมือนกัน
