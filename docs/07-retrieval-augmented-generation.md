# 07 — Retrieval-Augmented Generation (RAG)

---

## Pattern นี้คืออะไร?

RAG คือการให้ Agent **ดึงข้อมูลจากแหล่งภายนอก** (Vector Database, Document Store, Search Engine) มาใช้เป็น Context ในการตอบคำถาม ช่วยให้คำตอบถูกต้อง อ้างอิงแหล่งที่มาได้ และอัปเดตได้โดยไม่ต้องเทรน Model ใหม่

```
User Query
  │
  ▼
[1. Retrieve] ──→ ค้นหาเอกสารที่เกี่ยวข้อง
  │
  ▼
[2. Rank] ──────→ จัดลำดับความเกี่ยวข้อง
  │
  ▼
[3. Context] ───→ รวมข้อมูลเป็น Context
  │
  ▼
[4. Generate] ──→ LLM ตอบโดยใช้ Context
  │
  ▼
Answer + Sources
```

---

## เมื่อไหร่ควรใช้

- ตอบคำถามบนข้อมูลเฉพาะขององค์กร
- ข้อมูลเปลี่ยนแปลงบ่อย (ข่าว ราคา เอกสาร)
- ต้องการอ้างอิงแหล่งที่มาได้
- ไม่ต้องการ (หรือไม่สามารถ) Fine-tune Model

**ตัวอย่าง:**
- Chat with PDF
- Customer Support บน Knowledge Base
- ระบบค้นหาข้อมูลภายในองค์กร
- Research Assistant

## เมื่อไหร่ไม่ควรใช้

- คำตอบอยู่ในความรู้ทั่วไปของ LLM แล้ว
- ต้องการคำตอบที่ creative (RAG อาจจำกัด creativity)
- มีเอกสารน้อยเกินไป หรือเอกสารไม่มีคุณภาพ
- ระบบต้องการ Latency ต่ำมาก (RAG เพิ่มเวลาค้นหา)
- ข้อมูลเป็นความลับสูง ไม่ควรส่งให้ LLM API

---

## ข้อดี

- **ลด Hallucination** — ตอบตามเอกสารจริง
- **ข้อมูลเป็นปัจจุบัน** — อัปเดต Knowledge Base โดยไม่ต้องเทรนใหม่
- **อ้างอิงได้** — บอกได้ว่าคำตอบมาจากเอกสารไหน
- **ควบคุมความถูกต้อง** — แก้ไขข้อมูลที่แหล่งต้นทาง

## ข้อเสีย

- **Retrieval Quality** — ถ้าค้นหาไม่เจอของที่เกี่ยวข้อง = คำตอบไม่ดี
- **เพิ่ม Complexity** — ต้องจัดการ Vector DB, Embedding, Chunking
- **Context Window** — เอกสารที่ดึงมาต้องพอดีกับ Context Window
- **Cost เพิ่ม** — ต้องจ่ายค่า Embedding + Storage

---

## การทำงาน

### 1. Indexing (เตรียมข้อมูลล่วงหน้า)
```
Documents → Chunking → Embedding → Vector DB
```

### 2. Retrieval (เมื่อมีคำถาม)
```
Query → Embedding → Search Vector DB → Top-K Results
```

### 3. Generation (ตอบคำถาม)
```
Prompt = Context (Retrieved Docs) + User Query
       → LLM → Answer
```

---

## RAG Variants

| Variant | วิธีการ | เหมาะกับ |
|---|---|---|
| Naive RAG | Retrieve → Generate | Basic Q&A |
| RAG-Fusion | ค้นหาหลายวิธี → รวมผล | ต้องการ Recall สูง |
| Agentic RAG | Agent ตัดสินใจว่าจะ search เมื่อไหร่ | งานซับซ้อน |
| Self-RAG | Retrieve → Check relevance → Generate | ควบคุมคุณภาพ |
| Corrective RAG | Retrieve → Evaluate → Retry ถ้าไม่ดี | ต้องการ Robust |

---

## ข้อผิดพลาดที่พบบ่อย

1. **Chunking ไม่ดี** — chunk ใหญ่/เล็กเกินไป ทำให้ค้นหาไม่เจอ
2. **Embedding ไม่เหมาะสม** — ใช้ embedding model ที่ไม่ตรงกับภาษา/โดเมน
3. **ไม่มีการ Ranking** — ดึงมาหมดทุก chunk ทำให้ Context รก
4. **ไม่บอก LLM ว่า "ไม่รู้"** — LLM อาจพยายามตอบทั้งที่ไม่มีข้อมูล
5. **ไม่ตรวจสอบ Source** — LLM อาจบิดเบือนข้อมูลจากเอกสาร

---

## ตัวอย่าง: HR Knowledge Base

```
Query: "ลาป่วยได้กี่วันต่อปี"

1. Retrieve:
   ค้นหา Vector DB → เจอเอกสาร "นโยบายการลา" 3 chunks
   Score: 0.92, 0.85, 0.71

2. Context:
   [นโยบายการลา] พนักงานสามารถลาป่วยได้ 30 วันต่อปี...

3. Generate:
   "ตามนโยบายบริษัท พนักงานสามารถลาป่วยได้สูงสุด 30 วันต่อปี
   (อ้างอิง: เอกสารนโยบายการลา หน้าที่ 3)"
```

---

## Safety และ Verification

- ตรวจสอบว่า Retrieved Documents เกี่ยวข้องจริง
- มี Fallback เมื่อ Document Retrieval ไม่พบข้อมูล
- อ้างอิงแหล่งที่มาทุกครั้ง
- ตรวจสอบว่า LLM ไม่ได้เติมข้อมูลที่ไม่มีในเอกสาร
- สำหรับข้อมูล sensitive: ควบคุม access rights

---

## สรุป

RAG เป็น Pattern สำคัญที่ช่วยให้ Agent ตอบคำถามได้ถูกต้องและอ้างอิงแหล่งที่มาได้ แต่คุณภาพของ RAG ขึ้นอยู่กับคุณภาพของ Retrieval เป็นหลัก — ลงทุนกับระบบค้นหาให้ดี

**ข้อควรจำ:** RAG ≠ Magic — ถ้าเอกสารต้นทางไม่มีคำตอบ LLM ก็ตอบไม่ได้

**บทต่อไป:** [08 — Human-in-the-loop](08-human-in-the-loop.md) — การควบคุมโดยมนุษย์
