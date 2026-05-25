# 10 — Multi-agent Collaboration

---

## Pattern นี้คืออะไร?

Multi-agent Collaboration คือรูปแบบที่ **Agent หลายตัวทำงานร่วมกัน** โดยแต่ละตัวมีบทบาท ความเชี่ยวชาญ และความรับผิดชอบของตัวเอง สื่อสารและประสานงานกันผ่าน Protocol ที่กำหนด

```
                   ┌─ Researcher Agent
                   │
Input ─────────→ Coordinator ──→ Coder Agent
                   │            │
                   │            └─ Reviewer Agent
                   │
                   └─ สรุปผลจากทุก Agent
```

ต่างจาก Orchestrator-Worker ตรงที่ Multi-agent มักไม่มี Hierarchy ที่ชัดเจน — Agent สามารถสื่อสารกันได้โดยตรง

---

## เมื่อไหร่ควรใช้

- งานที่ต้องใช้ความเชี่ยวชาญที่แตกต่างกันมาก
- งานที่ซับซ้อนเกินกว่า Agent เดียวจะจัดการ
- ต้องการ Simulation หรือ Role-playing
- ต้องการมุมมองที่หลากหลาย

**ตัวอย่าง:**
- Software Development Team (PM → Dev → QA)
- Research Team (Researcher → Analyst → Writer)
- Debate / Deliberation Systems

## เมื่อไหร่ไม่ควรใช้

- งานที่ Agent เดียวทำได้
- เมื่องานไม่จำเป็นต้อง collaboration จริงๆ
- เมื่อ Cost เป็นปัจจัยหลัก (Multi-agent = 3-10x cost)
- เมื่อ Latency เป็นสิ่งสำคัญ
- ระบบที่การสื่อสารระหว่าง Agent ซับซ้อนเกินควบคุม

---

## ข้อดี

- **Modular** — แต่ละ Agent ทำหน้าที่เฉพาะ
- **Flexible** — ปรับบทบาทและจำนวน Agent ได้
- **Robust** — Agent หนึ่งล้ม ตัวอื่นยังทำงานต่อได้
- **Specialized** — แต่ละ Agent เชี่ยวชาญของตัวเอง

## ข้อเสีย

- **ซับซ้อนมาก** — การออกแบบ interaction ระหว่าง Agent
- **Cost สูง** — หลาย Agent = หลาย Token
- **Communication Overhead** — Agent คุยกันเองกิน Token เยอะ
- **Coordination Problem** — Agent อาจทำงานซ้อนหรือขัดแย้งกัน
- **Debug ยากมาก** — ใครทำอะไร? ใครผิด?

---

## รูปแบบการทำงานร่วมกัน

### 1. Debate
Agent สองตัวขึ้นไปถกเถียงกันเพื่อหาข้อสรุป
```
Agent A: "ผมคิดว่า..."
Agent B: "แต่ผมว่า..."
Agent A: "อ้อ ขอบคุณที่ชี้ให้เห็น"
→ ได้ข้อสรุปที่ดีขึ้น
```

### 2. Role-playing
Agent แต่ละตัวมีบทบาทเฉพาะ
```
Product Manager → กำหนด requirements
Developer → เขียนโค้ด
QA → ทดสอบ
```

### 3. Voting
Agent หลายตัวโหวตหาคำตอบที่ดีที่สุด
```
Agent 1: Answer A
Agent 2: Answer A
Agent 3: Answer B
→ Majority: Answer A
```

### 4. Specialized Pipeline
Agent ส่งต่องานตามความเชี่ยวชาญ
```
Designer → Developer → Tester → Deployer
```

---

## การทำงาน

### ตัวอย่าง: Software Development Agent Team

```
Task: "สร้าง REST API สำหรับจัดการ users"

Researcher Agent:
  "ดูแล้วควรใช้ Express.js + Prisma ORM
   อ้างอิงจาก documentation ล่าสุด"

Coordinator:
  "ตกลง ใช้ Express + Prisma
   Dev Agent ได้รับมอบหมาย

Coder Agent:
  "เขียนโค้ดตาม spec:
   - GET /users
   - POST /users
   - PUT /users/:id
   - DELETE /users/:id"

Reviewer Agent:
  "ตรวจสอบโค้ด:
   - Missing input validation
   - ไม่มี rate limiting
   - ควรเพิ่ม try-catch"

Coder Agent:
  "แก้ไขตาม review:
   - เพิ่ม validation
   - เพิ่ม rate limiting
   - เพิ่ม error handling"

Coordinator:
  "ทุกอย่างผ่านแล้ว สรุปผล"
```

---

## ข้อผิดพลาดที่พบบ่อย

1. **Agent Overload** — หนึ่ง Agent รู้งานของ Agent อื่นมากเกินไป
2. **Communication Chaos** — Agent คุยกันไม่รู้จบ
3. **Role Creep** — Agent เผลอทำงานนอกขอบเขต
4. **Deadlock** — Agent รออีกตัวตอบ
5. **Echo Chamber** — Agent เห็นด้วยกันเอง ไม่มี critique
6. **Cost Explosion** — 25 รอบการสนทนาระหว่าง Agent

---

## Multi-agent vs Orchestrator-Worker

| มิติ | Multi-agent | Orchestrator-Worker |
|---|---|---|
| Hierarchy | แบน (Peer-to-peer) | มี Hierarchy |
| Communication | Agent คุยกันเอง | ผ่าน Orchestrator |
| Complexity | สูงมาก | ปานกลาง |
| Flexibility | ยืดหยุ่นสูง | ควบคุมง่าย |
| Debug | ยากมาก | ปานกลาง |
| Cost | สูงมาก | สูง |

---

## Safety และ Verification

- กำหนดขอบเขตของแต่ละ Agent ให้ชัดเจน
- จำกัดจำนวน round ของการสนทนาระหว่าง Agent
- มี Coordinator ตรวจสอบก่อน action สำคัญ
- Log ทุก message ระหว่าง Agent
- ตรวจสอบ Agent drift — Agent ออกนอกบทบาท
- มี Emergency Stop สำหรับระบบ

---

## สรุป

Multi-agent Collaboration เป็น Pattern ที่มีพลังมากที่สุดในแง่ความสามารถ แต่ก็ซับซ้อนและแพงที่สุด ใช้เมื่อจำเป็นจริงๆ และมีระบบ Monitoring ที่ดี

**ข้อควรจำ:** Multi-agent ≠ ดีกว่า — ถ้า Prompt Chaining หรือ Orchestrator-Worker พอ อย่าใช้ Multi-agent

**บทต่อไป:** [11 — Evaluator-Optimizer](11-evaluator-optimizer.md) — การตรวจสอบและปรับปรุงแบบวนซ้ำ
