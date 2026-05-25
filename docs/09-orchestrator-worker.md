# 09 — Orchestrator-Worker

---

## Pattern นี้คืออะไร?

Orchestrator-Worker คือรูปแบบที่มี **Agent ตัวกลาง (Orchestrator)** ทำหน้าที่วางแผน มอบหมายงาน ติดตามความคืบหน้า และรวบรวมผลลัพธ์จาก **Worker Agents** หลายตัวที่เชี่ยวชาญงานเฉพาะด้าน

```
               ┌─ Worker A (Search)
               │
Input ─────→ Orchestrator ──→ Worker B (Analyze) ──→ Result
               │            │
               │            └─ Worker C (Format)
               │
               └─ ตรวจสอบและรวบรวมผลลัพธ์
```

Orchestrator ไม่เหมือน Router — Router แค่ส่งต่อไป แต่ Orchestrator **วางแผน** **ประสานงาน** และ **รวมผลลัพธ์**

---

## เมื่อไหร่ควรใช้

- งานที่ต้องใช้ความสามารถหลายด้าน
- งานที่สามารถทำ parallel ได้
- แต่ละ subtask ต้องใช้ expertise ต่างกัน
- ต้องการ scalability — เพิ่ม worker เมื่อโหลดเยอะ

**ตัวอย่าง:**
- Data Pipeline ที่ต้อง extract, transform, load
- Research Report ที่ต้องค้นหาหลายแหล่ง
- Content Production ที่ต้องเขียน + ออกแบบ + ตรวจสอบ

## เมื่อไหร่ไม่ควรใช้

- งานเล็กๆ ที่ Agent เดียวทำได้
- งานที่ลำดับชัดเจน (ใช้ Chaining ดีกว่า)
- ระบบที่ Orchestrator เป็น Single Point of Failure
- เมื่อ Cost และ Complexity ไม่คุ้ม

---

## ข้อดี

- **Scale ได้ดี** — เพิ่ม Worker เมื่อจำเป็น
- **Worker เฉพาะทาง** — แต่ละ Agent เชี่ยวชาญงานเฉพาะ
- **Parallel Execution** — ทำงานพร้อมกันได้
- **Centralized Control** — จัดการทุกอย่างจากที่เดียว

## ข้อเสีย

- **Orchestrator Complexity** — ตัว Orchestrator ซับซ้อน
- **Single Point of Failure** — ถ้า Orchestrator ล้ม ทั้งระบบล้ม
- **Communication Overhead** — สื่อสารระหว่าง Orchestrator-Worker
- **Debug ยาก** — ต้อง追踪หลาย Agent พร้อมกัน

---

## การทำงาน

### 1. Orchestrator รับงาน
```
Task: "วิเคราะห์ feedback ลูกค้าและสรุปแนวโน้ม"
```

### 2. Orchestrator วางแผนและมอบหมาย
```
Subtask 1: "ดึงข้อมูล feedback จาก Database"
  → มอบหมาย Worker A (Data Access)

Subtask 2: "วิเคราะห์ sentiment แต่ละรายการ"
  → มอบหมาย Worker B (NLP)

Subtask 3: "สรุปแนวโน้มและทำ图表"
  → มอบหมาย Worker C (Analysis + Viz)
```

### 3. Workers ทำงาน
Worker A, B, C ทำงานของตัวเอง อาจจะ parallel หรือ sequential

### 4. Orchestrator รวบรวมผล
```
Orchestrator:
  - รับผลจาก Worker A ✅
  - รับผลจาก Worker B ✅
  - รับผลจาก Worker C ✅
  - รวมและตรวจสอบความสอดคล้อง
  - สรุปผลลัพธ์สุดท้าย
```

---

## Orchestration Strategies

| Strategy | วิธีการ | เหมาะกับ |
|---|---|---|
| Sequential | ทำทีละ Worker | งานที่ dependencies ชัดเจน |
| Parallel | ทำพร้อมกันทั้งหมด | งาน independent |
| Dynamic | Orchestrator ตัดสินใจตามสถานการณ์ | งานที่ซับซ้อน ปรับเปลี่ยนได้ |
| Hierarchical | Orchestrator → Sub-orchestrator → Workers | ระบบขนาดใหญ่ |

---

## ตัวอย่าง: Report Generation System

```
User: "สร้างรายงานการขายประจำเดือน"

Orchestrator:
  1. Query Worker ──→ ดึงข้อมูลยอดขายจาก Database
       (parallel)
  2. Analytics Worker ──→ วิเคราะห์แนวโน้ม y/y, m/m
       (parallel)
  3. Chart Worker ──→ สร้างกราฟประกอบ

  ── รอ Worker 1, 2, 3 เสร็จ ──

  4. Writer Worker ──→ เขียนรายงานสรุป
  5. Review Worker ──→ ตรวจสอบความถูกต้อง

  Output: รายงาน + กราฟ + สรุป

Orchestrator (monitor):
  - Worker 1 ✅ (2.3s)
  - Worker 2 ✅ (3.1s)
  - Worker 3 ✅ (4.0s)
  - Worker 4 ✅ (5.2s)
  - Worker 5 ✅ (1.0s)
  Total: 6.2s (parallel saved 30%)
```

---

## ข้อผิดพลาดที่พบบ่อย

1. **Orchestrator เป็น God Object** — รู้ทุกอย่าง เปราะบาง
2. **Worker Communication ไม่ชัดเจน** — ส่งข้อมูลผิด format
3. **Parallel Promise ไม่มี timeout** — Worker ค้าง ไม่รู้
4. **Result Aggregation พัง** — รวมผลจาก workers ไม่ถูกต้อง
5. **Over-orchestration** — วางแผนละเอียดเกินความจำเป็น

---

## Safety และ Verification

- มี Timeout สำหรับ Worker แต่ละตัว
- ตรวจสอบผลลัพธ์ของ Worker ก่อนนำไปใช้
- มี Fallback Worker เมื่อ Worker หลักล้มเหลว
- Log ทุก action ของ Orchestrator
- Monitor Health ของ Worker และ Orchestrator
- มีระบบ Retry เมื่อ Worker ล้มเหลวชั่วคราว

---

## สรุป

Orchestrator-Worker ช่วยจัดการงานซับซ้อนที่ต้องใช้หลายความสามารถ โดยมีตัวกลางคอยประสานงาน แต่การออกแบบ Orchestrator ที่ดีต้องบาลานซ์ระหว่าง การควบคุม และ ความยืดหยุ่น

**ข้อควรจำ:** Orchestrator ≠ ทาส — ควรให้ Worker มีอิสระในขอบเขตของตัวเองบ้าง

**บทต่อไป:** [10 — Multi-agent Collaboration](10-multi-agent-collaboration.md) — Agent หลายตัวทำงานร่วมกัน
