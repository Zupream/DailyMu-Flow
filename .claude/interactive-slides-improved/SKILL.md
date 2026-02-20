---
name: interactive-slides
description: |
  **Interactive HTML Slide Deck Generator**: สร้าง slide deck แบบ interactive ในไฟล์ HTML ไฟล์เดียว พร้อม navigation, animation, responsive design
  - MANDATORY TRIGGERS: slide deck, interactive slides, HTML slides, presentation deck, interactive presentation, สร้างสไลด์, ทำ slide
  - ใช้ skill นี้เมื่อ user ต้องการสร้าง slide deck จากข้อมูลที่มี (PDF, ข้อความ, Figma, หรืออะไรก็ได้) เป็นไฟล์ HTML ที่เปิดได้ในเบราว์เซอร์
  - ใช้เมื่อ user พูดถึง "สไลด์", "slide", "deck", "presentation" ที่ต้องการเป็น interactive HTML
  - ไม่ใช้สำหรับ .pptx (ใช้ pptx skill แทน)
---

# Interactive HTML Slide Deck Generator

สร้าง single-file interactive HTML slide deck ที่มี keyboard/touch/dropdown navigation, smooth animations, responsive 16:9 layout, preset color themes, และ accessibility support

## Workflow

```
1. รับข้อมูล (PDF/text/Figma/อื่นๆ) + สี + รูปภาพ
2. ถาม user เรื่อง theme (ถ้ายังไม่ได้เลือก)
3. วางโครงสร้างสไลด์ — เลือก layout ที่หลากหลาย
4. สร้างไฟล์ HTML ครบจบในไฟล์เดียว (อ่าน template จาก references/template.md)
5. ใส่รูปภาพ (ถ้ามี)
6. ส่งมอบ
```

## Step 1: รวบรวมข้อมูล

เมื่อ user ขอสร้าง slide deck ให้ถามหรือรวบรวมสิ่งเหล่านี้:

**ต้องมี:**
- เนื้อหา/ข้อมูล — อาจเป็น PDF, ข้อความ, Markdown, Figma board, หรืออธิบายปากเปล่า
- จำนวนสไลด์ที่ต้องการ (หรือให้ Claude ตัดสินใจตามเนื้อหา)

**ถ้ามีจะดีมาก:**
- สี/theme ที่ต้องการ (เลือกจาก preset หรือกำหนดเอง)
- รูปภาพสำหรับตกแต่ง (illustrations, screenshots, logos)
- ภาษาหลัก (ไทย/อังกฤษ/อื่นๆ)
- กลุ่มเป้าหมาย (เพื่อปรับโทนเนื้อหา)

## Step 2: เลือก Theme

ถ้า user ยังไม่ได้เลือกสี ให้แนะนำ preset themes:

อ่าน `references/themes.md` เพื่อดู preset themes ทั้งหมดและค่า CSS variables

```
Preset Themes ที่มี:
  1. 🌿 Forest (เขียว/ทอง) — สำหรับ business, startup, eco
  2. 🌊 Ocean (น้ำเงินเข้ม/ขาว) — สำหรับ corporate, tech
  3. 🔥 Sunset (ส้ม/แดง) — สำหรับ creative, marketing
  4. 🌸 Blossom (ชมพู/ม่วง) — สำหรับ lifestyle, beauty
  5. ⚡ Minimal (เทา/ขาว) — สำหรับ clean, professional
  6. 🌙 Dark (เข้ม/accent สว่าง) — สำหรับ tech, gaming, modern
  7. 🎨 Custom — user กำหนด primary + secondary color เอง
```

เมื่อ user เลือก theme ให้ใช้ค่าจาก `references/themes.md` เพื่อ set CSS variables

## Step 3: วางโครงสร้างสไลด์

วิเคราะห์เนื้อหาแล้ววางโครงสร้าง:

```
Slide 0: Cover — ชื่อเรื่อง, subtitle, role chips/tags
Slide 1-N: Content slides — เนื้อหาหลัก (สลับ layout หลากหลาย)
Slide N+1: Thank You / สรุป
```

### Content Slide Types

เลือก layout ที่เหมาะกับเนื้อหาแต่ละสไลด์ — ความหลากหลายของ layout สำคัญมาก เพราะช่วยให้ผู้ชมไม่รู้สึกจำเจ ห้ามใช้ layout แบบเดิมซ้ำติดกันเกิน 2 สไลด์:

| Type | เหมาะกับ | CSS Class |
|------|---------|-----------|
| **Flow** | ขั้นตอน, journey, process | `.flow-container` + `.flow-row` + `.flow-step` |
| **Cards Grid** | รายการ, features, comparison | `.cards-grid.cols-2/3/4` + `.card` |
| **State Machine** | สถานะ, lifecycle | `.state-machine` + `.state-node` |
| **Decision Tree** | การตัดสินใจ, branching logic | `.decision-tree` + `.decision-diamond` |
| **Split Row** | เปรียบเทียบ 2-3 หัวข้อ | `.split-row` + `.mini-card` |
| **Two Column** | ซ้าย-ขวา | `.two-col` + `.col-section` |
| **Timeline** | เหตุการณ์ตามลำดับเวลา, milestones | `.timeline` + `.timeline-item` |
| **Stats Row** | ตัวเลขสำคัญ, metrics, KPIs | `.stats-row` + `.stat-item` |
| **Quote** | คำพูดเด่น, testimonial, highlight | `.quote-block` |
| **Agenda** | สารบัญ, outline, roadmap | `.agenda-list` + `.agenda-item` |

รายละเอียด CSS ของแต่ละ layout อยู่ใน `references/template.md`

## Step 4: สร้าง HTML

อ่าน `references/template.md` เพื่อดูโครงสร้าง HTML ที่สมบูรณ์ ใช้เป็น boilerplate

สิ่งสำคัญที่ต้องมีในไฟล์:

### Structure
- Single HTML file — CSS + JS อยู่ในไฟล์เดียว
- CSS custom properties สำหรับ theming (ดู Design Token System ใน template)
- **16:9 aspect ratio** — slide container ใช้ `height: 56.25vw; max-width: 177.78vh` เพื่อให้สัดส่วนคงที่ทุกหน้าจอ (ดู section 2.4 ของ template)
- Responsive typography ด้วย `clamp()`
- Background pattern (grid หรือ gradient ตาม theme)

### Navigation (ต้องมีทุกอัน)
- ⌂ Home button — กลับ slide แรก
- ‹ › Previous/Next buttons
- Nav dots — แสดงตำแหน่งปัจจุบัน (ซ่อนถ้ามี >15 สไลด์)
- ☰ Hamburger menu — dropdown เลือกสไลด์
- ⛶ Fullscreen button
- Keyboard: ← → Space Home End H F Escape
- Touch: swipe ซ้าย-ขวา
- Counter: "1 / N"

### Dark Theme Handling
เมื่อใช้ Dark theme (theme 6) UI components ที่มีพื้นสีขาว/สว่างจะดูผิดเพี้ยนบนพื้นเข้ม จึงต้องปรับ:
- Nav bar background → `rgba(15, 23, 42, 0.9)` แทน `rgba(255,255,255,0.85)`
- Dropdown background → `rgba(15, 23, 42, 0.95)` แทน `rgba(255,255,255,0.95)`
- Nav button borders → `rgba(255,255,255,0.15)`
- Shadows → เพิ่ม opacity เป็น `rgba(0,0,0,0.4)`
- Grid bg → ใช้สี grid-color ที่ contrast กับพื้นเข้ม

ค่าที่ต้องปรับทั้งหมดอยู่ใน `references/themes.md` ส่วน Dark theme

### Animations
- Slide entrance: translateX + opacity + spring easing
- `anim-item` class: staggered fade-in (delay +0.1s ต่อ item)
- Animation cancellation: ใช้ `clearTimeout` pattern
- Cover/end title shimmer: background-position animation

### Accessibility
- `@media (prefers-reduced-motion: reduce)` — ลด animation เป็น 0.01ms
- ปุ่มทุกปุ่มมี `title` attribute
- `font-variant-numeric: tabular-nums` สำหรับ counter

### Highlight Colors
ใช้ highlight classes เพื่อเน้นสี:
```
.highlight-blue    — primary color tint
.highlight-green   — primary-dark tint
.highlight-orange  — secondary tint
.highlight-red     — accent-red tint
.highlight-purple  — accent-purple tint
.highlight-amber   — accent-amber tint
```

## Step 5: ใส่รูปภาพ (ถ้ามี)

ถ้า user ส่งรูปภาพมา ให้วิเคราะห์แต่ละรูปแล้วจัดหมวดหมู่:

| Type | ใช้กับ | CSS Class | ลักษณะ |
|------|--------|-----------|--------|
| **Illustration** | รูปวาด, icon ใหญ่ | `.slide-illust` | Watermark style, opacity 0.18, มุมขวาบน |
| **Screenshot** | ภาพหน้าจอ app | `.slide-phone` | แสดงตรงกลาง, click-to-zoom lightbox |
| **Cover Image** | รูปหน้าปก | `.cover-app-img` | ขอบมน, เงา |

ถ้ามีรูป screenshot ให้เพิ่ม lightbox overlay + `openLightbox()` function

```bash
mkdir -p images/
# Copy user's images to images/ folder
```

## Step 6: ส่งมอบ

1. บันทึกไฟล์ HTML ไปที่ output folder
2. ถ้ามีรูป ให้สร้างโฟลเดอร์ `images/` คู่กับไฟล์ HTML
3. ส่งลิงก์ให้ user: `[ดู slide deck](computer:///path/to/file.html)`

## Content Writing Guidelines

สไลด์ที่ดีต้องกระชับและสื่อสารชัด:

- **หัวข้อ** — ไม่เกิน 6-8 คำ ใช้คำกระทบใจ (เช่น "ปัญหาที่ทุกคนเคยเจอ" ดีกว่า "ปัญหา")
- **Subtitle** — 1 ประโยคสั้นๆ ไม่เกิน 2 บรรทัด
- **Bullet points** — ไม่เกิน 4-5 ข้อต่อสไลด์ แต่ละข้อ 5-10 คำ
- **ตัวเลข** — ถ้ามีตัวเลข ใช้ Stats Row layout ให้โดดเด่น
- **Flow steps** — ไม่เกิน 4 steps ต่อแถว ใช้ emoji เป็น icon
- **Cards** — แต่ละ card มี icon + title + description 1-2 บรรทัด
- **Emoji** — ใช้ emoji เป็น visual icon ช่วยสื่อความหมายเร็วขึ้น

## Tips & Best Practices

- **Font ไทย** — ใช้ IBM Plex Sans Thai สำหรับภาษาไทย, Inter สำหรับภาษาอังกฤษ
- **Responsive** — ใช้ `clamp()` ทุกที่ ห้ามใช้ค่า fixed
- **สลับ layout** — ไม่ใช้ layout เดิมซ้ำหลายสไลด์ติดกัน
- **Nav dots** — ถ้ามีสไลด์เยอะ (>15) ให้ซ่อน dots ใช้แค่ counter + dropdown
- **ทดสอบ dark theme** — ถ้าใช้ Dark theme ตรวจสอบว่า nav bar และ dropdown อ่านได้บนพื้นเข้ม
