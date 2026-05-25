# Example: Prompt Chaining

**Pattern:** Prompt Chaining

---

## Scenario

ระบบช่วยเขียนบทความ โดยแบ่งเป็น 4 ขั้นตอน: Research → Outline → Draft → Review

## Minimal Workflow

```
Input: หัวข้อบทความ
  │
  ▼
[Research] ──→ หาข้อมูลที่เกี่ยวข้อง
  │
  ▼
[Outline] ───→ วางโครงสร้าง
  │
  ▼
[Draft] ─────→ เขียนร่าง
  │
  ▼
[Review] ────→ ตรวจสอบและแก้ไข
  │
  ▼
Output: บทความสมบูรณ์
```

## Input/Output Example

```
Input: "ประโยชน์ของ AI Agent สำหรับธุรกิจขนาดเล็ก"

Step 1 → Research Output: [3 แหล่งข้อมูล, 5 ประเด็นสำคัญ]
Step 2 → Outline Output: [4 ส่วนหลัก, 12 หัวข้อย่อย]
Step 3 → Draft Output: [บทความ 600 คำ]
Step 4 → Review Output: [บทความ 650 คำ + แก้ไข grammar]
```

## Risks

- ถ้า Research ไม่ได้ข้อมูลดี → ทั้ง chain ผิดเพี้ยน
- Review ขั้นเดียวอาจไม่เพียงพอสำหรับเนื้อหาสำคัญ

## Future Implementation

- Python script ที่เรียก LLM API 4 ครั้ง
- แต่ละขั้นตอนมี Prompt แยก
- Human review optional ที่ขั้นตอน Review
- ใช้ Model ถูกสำหรับ Research, Model แพงสำหรับ Draft
