# Example: Planning

**Pattern:** Planning

---

## Scenario

AI Research Assistant ที่ต้องวางแผนและดำเนินการค้นคว้าข้อมูลตามคำขอของผู้ใช้

## Minimal Workflow

```
Input: คำขอวิจัย
  │
  ▼
[Plan] ──→ วิเคราะห์และวางแผนการค้นคว้า
  │
  ▼
[Execute] ──→ ดำเนินการตามแผน ทีละขั้น
  │         │
  │         └─ ปรับแผนเมื่อเจออุปสรรค
  │
  ▼
[Review] ──→ ตรวจสอบผลลัพธ์
  │
  ▼
Output: รายงานผลการวิจัย
```

## Input/Output Example

```
Input: "วิเคราะห์แนวโน้มตลาด AI ในเอเชียตะวันออกเฉียงใต้ปี 2025"

Plan:
  1. ค้นหารายงานตลาด AI ใน SEA
  2. วิเคราะห์แนวโน้มแต่ละประเทศ
  3. เปรียบเทียบการลงทุนในแต่ละ sector
  4. สรุปภาพรวมและแนวโน้ม

(Plan may be adjusted if some data is not found)
```

## Risks

- วางแผนผิด → เสียเวลาค้นหาของไม่เกี่ยวข้อง
- ติด loop เมื่อข้อมูลไม่เพียงพอ → ต้องมี max steps

## Future Implementation

- Agent สร้าง plan เป็น structured JSON
- Execute แต่ละขั้นด้วย tool calls
- Monitor progress และปรับแผนตามผลลัพธ์
- จำกัดจำนวนสูงสุดของ steps
