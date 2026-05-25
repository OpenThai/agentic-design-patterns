# 06 — Tool-use

---

## Pattern นี้คืออะไร?

Tool-use คือความสามารถของ AI Agent ในการ **เรียกใช้เครื่องมือภายนอก** ผ่าน Function Calling เพื่อทำงานที่ LLM ทำเองไม่ได้ เช่น คำนวณ ค้นหา อ่านไฟล์ หรือเรียก API

```
User: "สภาพอากาศวันนี้ที่กรุงเทพเป็นไงบ้าง"
  │
  ▼
Agent ตัดสินใจใช้ Tool
  │
  ▼
[Tool: get_weather(location="Bangkok")]
  │
  ▼
API Response: {"temp": 35, "humidity": 70%}
  │
  ▼
Agent สรุปผลให้ผู้ใช้
```

---

## เมื่อไหร่ควรใช้

- ต้องการข้อมูลปัจจุบัน (LLM มี knowledge cutoff)
- ต้องคำนวณหรือประมวลผล (LLM คำนวณไม่แม่น)
- ต้องอ่าน/เขียนข้อมูลในระบบภายนอก
- ต้องการ action ในโลกจริง (ส่งอีเมล สั่งซื้อของ)

**ตัวอย่าง:**
- Web Search
- Database Query
- Code Execution
- File I/O
- API Calls
- Calculator

## เมื่อไหร่ไม่ควรใช้

- คำตอบอยู่ในความรู้ของ LLM อยู่แล้ว
- ข้อมูลไม่ต้องการความสดใหม่
- Tool มีความเสี่ยงสูงโดยไม่มี guardrails
- User ไม่ได้ตั้งใจให้เรียกใช้ tool

---

## ข้อดี

- **เพิ่มความสามารถมหาศาล** — Agent ทำอะไรได้มากกว่าแค่พูด
- **ข้อมูลเป็นปัจจุบัน** — ไม่จำกัดที่ Training Data
- **ลด Hallucination** — อ้างอิงข้อมูลจริง
- **Automate ได้** — สั่งงานระบบอื่นอัตโนมัติ

## ข้อเสีย

- **Tool Selection ผิดพลาด** — เลือก tool ไม่เหมาะสมกับงาน
- **Error จาก Tool** — Tool ล้มเหลว หรือ return ข้อมูลผิด
- **Security Risk** — Agent เรียก tool ที่ dangerous
- **เพิ่ม Complexity** — ต้องจัดการ Tool Definitions, Auth, Rate Limits

---

## การทำงาน

### 1. Tool Definition

```python
{
  "name": "get_weather",
  "description": "ดึงข้อมูลสภาพอากาศของเมืองที่ระบุ",
  "parameters": {
    "location": "string — ชื่อเมือง"
  }
}
```

### 2. Tool Selection

LLM ตัดสินใจเลือก tool จาก:
- คำอธิบายของ tool
- Parameters ที่รับ
- Context ของคำถาม

### 3. Tool Execution

ระบบเรียก tool ด้วย arguments ที่ LLM ส่งกลับ

### 4. Result Handling

ส่งผลลัพธ์จาก tool กลับให้ LLM เพื่อสรุป

---

## Tool Selection Strategies

| Strategy | วิธีการ | เมื่อไหร่ใช้ |
|---|---|---|
| Single Tool | LLM เลือก 1 tool | งานชัดเจน ใช้แค่ tool เดียว |
| Multi-Tool | LLM เลือกหลาย tools พร้อมกัน | ต้องใช้ข้อมูลจากหลายแหล่ง |
| Sequential | ใช้ tool เรียงตามลำดับ | Output ของ tool หนึ่งเป็น Input ของอีก tool |
| Conditional | เลือก tool ตามเงื่อนไข | Routing-based tool use |

---

## ตัวอย่าง: Research Assistant

```
User: "หาข้อมูลบริษัท Apple และคำนวณราคาหุ้นเฉลี่ย 7 วัน"

Step 1 — Search
  Tool: web_search("Apple Inc 2025 financial results")
  Result: [ข้อมูลทางการเงิน]

Step 2 — Calculate
  Tool: calculate("avg(stock_prices: 7 days)")
  Result: $198.50

Step 3 — Summarize
  Agent: "จากข้อมูลที่ค้นหา ราคาหุ้น Apple ..."
```

---

## ข้อผิดพลาดที่พบบ่อย

1. **Tool Selection ไม่เหมาะสม** — ใช้ search tool ทั้งที่ควรใช้ database
2. **Arguments ผิด** — ส่ง parameter ผิด format
3. **ไม่ตรวจสอบผลลัพธ์** — เชื่อ tool result 100% ทั้งที่ tool อาจ error
4. **Tool overload** — มี tools มากเกินไป LLM สับสน
5. **Missing error handling** — Tool error แล้วระบบพัง

---

## Safety และ Verification

- **Tool Permission** — กำหนดว่า tool ไหนใช้ได้บ้าง
- **Input Validation** — ตรวจสอบ arguments ก่อนเรียก tool
- **Output Validation** — ตรวจสอบผลลัพธ์ก่อนใช้
- **Rate Limiting** — ป้องกัน abuse
- **Audit Log** — บันทึกทุก tool call
- **Human Approval** — สำหรับ tools ที่มีความเสี่ยง (ส่งอีเมล, ลบข้อมูล)

### ระดับความเสี่ยงของ Tools

| ระดับ | ตัวอย่าง | ต้องการ |
|---|---|---|
| Read-only | Search, Read File | Logging |
| Write | Create File, Send Message | Approval |
| Destructive | Delete, Update, Execute | Human-in-the-loop |

---

## สรุป

Tool-use เป็น Pattern ที่ทำให้ Agent จาก "พูดได้" กลายเป็น "ทำได้" แต่มาพร้อมความรับผิดชอบด้านความปลอดภัยและการจัดการ error ที่มากขึ้น

**ข้อควรจำ:** ให้ Tool เยอะไปก็ไม่ดี — เลือกเฉพาะ tool ที่จำเป็นและมีคำอธิบายที่ดี

**บทต่อไป:** [07 — Retrieval-Augmented Generation](07-retrieval-augmented-generation.md) — การดึงข้อมูลภายนอกมาตอบคำถาม
