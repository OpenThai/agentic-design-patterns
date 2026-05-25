# Example: Orchestrator-Worker

**Pattern:** Orchestrator-Worker

---

## Scenario

ระบบสร้างรายงานวิเคราะห์ธุรกิจ ที่ต้องดึงข้อมูล วิเคราะห์ สร้างกราฟ และเขียนรายงาน

## Minimal Workflow

```
User Request
  │
  ▼
[Orchestrator] ──→ วางแผนและมอบหมายงาน
  │
  ├─ [Data Worker] ────→ ดึงข้อมูลจาก Database
  ├─ [Analysis Worker] ──→ วิเคราะห์แนวโน้ม
  ├─ [Chart Worker] ─────→ สร้าง Visualization
  └─ [Writer Worker] ────→ เขียนรายงาน
  │
  ▼
[Orchestrator] ──→ รวบรวมและตรวจสอบ
  │
  ▼
Output: รายงานสมบูรณ์
```

## Input/Output Example

```
Input: "สรุปยอดขาย Q1 2025"

Orchestrator Plan:
  1. Data Worker: Query ยอดขาย Q1 2025 (parallel)
  2. Analytics Worker: วิเคราะห์ % growth (parallel)
  3. Chart Worker: สร้างกราฟ (parallel)
  4. Writer Worker: เขียนรายงาน (sequential - รอข้อมูลจาก 1-3)

Result:
  Data: 12.5M (15% growth)
  Analytics: "อีคอมเมิร์ซโตที่สุด 25%"
  Chart: [bar chart image]
  Report: "Q1 2025 มียอดขาย 12.5M เติบโต 15%..."
```

## Risks

- Orchestrator ล้ม → ระบบล้มทั้งหมด
- Worker ส่งผลลัพธ์ผิด format
- Parallel workers แข่งทรัพยากรกัน
- Result aggregation ซับซ้อนเมื่อ workers ให้ข้อมูลไม่ consistent

## Future Implementation

- Orchestrator สร้าง execution plan
- Workers เป็น modular functions/agents
- Parallel execution ด้วย asyncio หรือ thread pool
- Timeout และ retry สำหรับแต่ละ worker
- Aggregate results และตรวจสอบ consistency
