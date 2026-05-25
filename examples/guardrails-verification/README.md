# Example: Guardrails & Verification

**Pattern:** Guardrails & Verification

---

## Scenario

AI Customer Support สำหรับธุรกิจการเงิน ที่ต้องมี guardrails หลายชั้นเพื่อความปลอดภัยและ compliance

## Minimal Workflow

```
User Input
  │
  ▼
[Input Guard]
  ├─ Content Moderation
  ├─ Topic Restriction
  ├─ Prompt Injection Check
  └─ Rate Limit Check
  │
  ▼
[Agent]
  │
  ▼
[Output Guard]
  ├─ PII Detection
  ├─ Financial Advice Check
  ├─ Policy Compliance
  └─ Tone & Safety Check
  │
  ▼
Output
```

## Input/Output Example

```
Input: "บอกวิธีโกงระบบผ่อนชำระ"

Input Guard:
  Topic Check: ❌ "การโกง" → Blocked
  Response: "ขออภัย ไม่สามารถให้ข้อมูลเกี่ยวกับ
            การกระทำที่ผิดกฎหมายได้"

Input: "ยอดเงินในบัญชี 0 บาท ทำยังไงดี"

Input Guard: ✅ ผ่าน

Agent: "คุณสามารถตรวจสอบยอดเงินล่าสุดได้ที่..."

Output Guard:
  PII Check: ✅ ไม่มีข้อมูลส่วนบุคคล
  Advice Check: ✅ ไม่ใช่คำแนะนำการลงทุน
  Policy: ✅ ผ่าน

Output: "คุณสามารถตรวจสอบยอดเงินล่าสุด..."
```

## Risks

- Over-guarding → ปฏิเสธคำถามที่ถูกต้อง
- Under-guarding → พลาด content อันตราย
- Guardrails ออกแบบไม่รอบคอบ → มีช่องโหว่
- False positive สูง → ผู้ใช้หงุดหงิด

## Future Implementation

- Multi-layer guardrails (input, process, output)
- Configurable rules per use case
- Monitoring dashboard สำหรับ guardrail events
- A/B testing สำหรับ guardrail rules
- Alerting เมื่อ guardrails ทำงานผิดปกติ
