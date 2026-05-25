# Example: Human-in-the-loop

**Pattern:** Human-in-the-loop

---

## Scenario

ระบบ AI ที่ช่วยผู้บริหารจัดการเอกสารและการสื่อสาร — แต่ต้องได้รับการอนุมัติก่อนดำเนินการสำคัญ

## Minimal Workflow

```
Agent ต้องการดำเนิน Action
  │
  ▼
[Check Permission] ──→ Action นี้ต้อง Approve ไหม?
  │
  ├─ ไม่ต้อง → ดำเนินการเลย
  │
  └─ ต้อง Approve
       │
       ▼
  [Send Request] ──→ แจ้งมนุษย์ + รอการตัดสินใจ
       │
       ├─ Approve → Agent ดำเนินการ
       ├─ Reject  → Agent ยกเลิก
       └─ Modify  → มนุษย์แก้ → Agent ดำเนินการ
```

## Input/Output Example

```
Agent: "ตรวจพบว่าสัญญาลูกค้า A หมดอายุใน 7 วัน
       ต้องการส่งอีเมลต่ออายุสัญญา?

       ร่างอีเมล:
       'เรียนคุณลูกค้า A สัญญาของท่านจะหมดอายุใน 7 วัน...'

       ยอด: 450,000 บาท/ปี

       อนุมัติหรือไม่?"

มนุษย์: Approve ✅

Agent: "ส่งอีเมลต่ออายุสัญญาเรียบร้อย"
```

## Risks

- Humans approve โดยไม่ตรวจสอบ (rubber-stamping)
- Approval เป็น bottleneck
- ไม่มี timeout — รอมนุษย์นานเกินไป
- มนุษย์ไม่อยู่หรือไม่สะดวกตัดสินใจ

## Future Implementation

- Approval request notification (email, Slack, mobile)
- Timeout policy (auto-escalate)
- Audit log ทุก approval/rejection
- Dashboard สำหรับติดตาม pending approvals
- Mobile-ready approval flow
