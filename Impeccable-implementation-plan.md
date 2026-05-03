# Impeccable Implementation Plan — Eve Sujittra Portfolio

> แผนปรับปรุง UI/UX เว็บไซต์ Portfolio โดยใช้ Impeccable Commands
> สร้างจากการ Audit โค้ด `index.html` วันที่ 3 พฤษภาคม 2026

---

## 1. สรุปปัญหาหลักจากการ Audit

เว็บไซต์มี **พื้นฐานที่ดีมาก** (typography สวย, dark mode ครบ, responsive มีครบ 3 breakpoints) แต่พบปัญหาที่ต้องแก้ 4 ระดับ:

### 🚨 Critical (แก้ทันที)
1. **Color contrast ไม่ผ่าน WCAG AA** ทั้ง light และ dark mode — `--text-muted` ใช้ทั่วทั้งเว็บสำหรับ labels, captions, metadata
2. **SRI hash ของ AOS CSS ผิดพลาด** — อาจทำให้ stylesheet โหลดไม่ได้
3. **ใช้ Tailwind CDN** (`cdn.tailwindcss.com`) — ไม่เหมาะกับ production, เพิ่ม TBT
4. **Typo ในเนื้อหา** — "MobiLib 4rd Generation" ควรเป็น "4th"

### ⚠️ Important (แนะนำให้แก้)
1. CSS ซ้ำซ้อน: `.cert-badge` vs `.activity-badge`, `.creative-label` vs `.slider-label`
2. Inline styles เยอะใน Experience section — ทำลาย separation of concerns
3. Alt text ไม่มีคุณภาพ: `alt="2"`, `alt="3"`, `alt="ARCHi"` ที่สั้นเกินไป
4. Heading ซ้ำซ้อนใน About section: `.section-label` + `.section-title` ต่างเขียนว่า "About Me"
5. Emoji ใน Contact CTA (`📸 🎨 ✍️`) ทำลาย tone professional
6. Certificate grid CSS มี duplicate selectors และ competing rules
7. Figma iframe + Cloudinary video ไม่มี loading state / poster image
8. รูป PNG ไม่ได้บีบอัด — ควรเป็น WebP/AVIF

### 💡 Nice-to-have (ขัดเกลา)
1. รูป Hero กับ About ซ้ำกัน (`IMG_2598-Photoroom-1.png`)
2. Theme toggle `aria-label` ไม่เปลี่ยนตาม state
3. `hero-stats` ว่างเปล่าแต่ยัง render ใน DOM
4. ไม่มี `fetchpriority="high"` บน hero image

---

## 2. แผนการปรับปรุงตาม Impeccable Commands

---

### 🛠️ Create — สร้าง/ออกแบบใหม่

#### `/impeccable shape`
**ใช้ทำ:** ออกแบบใหม่ส่วน **Creative Works** ที่ break pattern ของเว็บไซต์
- ตอนนี้ไม่มี `.section-label`, ใช้ inline style (`font-size: 2rem`), มี emoji (`✨`)
- ทำให้เข้ากับ design system ที่เหลือ (ใช้ `.section-label` + `.section-title` + `clamp()`)

**ส่วนที่ได้ประโยชน์:** Creative Works section (บรรทัด ~2562)

---

#### `/impeccable craft`
**ใช้ทำ:** สร้าง **loading skeleton** สำหรับ Figma embed และ Cloudinary video iframe
- ตอนนี้ผู้ใช้เห็นพื้นที่ว่างขณะโหลด รู้สึกเหมือนเว็บเสีย
- ทำ placeholder / spinner / poster image ก่อน iframe โหลด

**ส่วนที่ได้ประโยชน์:** Experience section — Figma frame + Opening Video

---

### 🔎 Evaluate — ประเมิน

#### `/impeccable audit`
**ใช้ทำ:** ตรวจสอบ technical quality 5 มิติ
- SRI hash ที่ผิด (AOS CSS)
- Tailwind CDN ที่ไม่ควรใช้ production
- Inline JS handlers (`onclick="closeMobileMenu()"`, `onerror="..."`)
- ปัญหา performance จาก PNG ไม่บีบอัด
- Duplicate CSS selectors

**ส่วนที่ได้ประโยชน์:** ทั้งเว็บไซต์

---

#### `/impeccable critique`
**ใช้ทำ:** รีวิว visual design + persona test
- Emoji ใน Contact CTA อาจดูไม่ professional สำหรับ recruiter
- Figma phone mockup (bezel `#171717` + border-radius 47px) ดูต่างจาก design language ที่เหลือ (flat, minimal cards)
- Creative Works section ดูเหมือน "afterthought" ไม่ใช่ส่วนต่อขยายของ design system

**ส่วนที่ได้ประโยชน์:** Visual Identity, Brand Consistency

---

### ✨ Refine — ปรับแต่งรายละเอียด

#### `/impeccable animate`
**ใช้ทำ:** ปรับ **hero-badge-dot pulse** ที่กระพริบไม่มีที่สิ้นสุด (`animation: pulse 2s infinite`)
- อาจรบกวนผู้ใช้ที่มี ADHD หรือ vestibular disorders
- แนะนำ: หยุดหลัง 3-5 รอบ หรือลด opacity ให้ subtle ลง

**ส่วนที่ได้ประโยชน์:** Hero section

---

#### `/impeccable bolder`
**ใช้ทำ:** เพิ่มความโดดเด่นให้ CTA buttons หรือ hero section โดยไม่ทำให้วุ่นวาย
- ตอนนี้ดู "safe" ไปหน่อย
- อาจเพิ่ม micro-animation บน hover หรือทำให้ primary CTA มีมิติมากขึ้น

**ส่วนที่ได้ประโยชน์:** Hero, Contact section

---

#### `/impeccable colorize`
**ใช้ทำ:** แก้ไข **color system** ที่มีปัญหา contrast
- **Light mode:** `--text-muted: #737373` บน `#fafafa` = ~4.1:1 (ไม่ผ่าน WCAG AA)
- **Dark mode:** `--text-muted: #525252` บน `#0a0a0a` = ~2.4:1 (ไม่ผ่าน WCAG AA)
- **Hero badge:** `#15803d` บน `#dcfce7` = ~3.65:1 (ไม่ผ่านสำหรับขนาดเล็ก)

**แนะนำค่าใหม่:**
- Light mode: `#525252` หรือ `#404040`
- Dark mode: `#a3a3a3` หรือ `#d4d4d4`
- Hero badge text: ทำให้เข้มขึ้น หรือใช้ outline

**ส่วนที่ได้ประโยชน์:** ทั้งเว็บไซต์ (labels, captions, metadata, badges)

---

#### `/impeccable delight`
**ใช้ทำ:** เพิ่ม micro-interactions ที่ขาดหายไป
- Loading state สำหรับ iframe (Figma + Cloudinary)
- Hover effect ที่สม่ำเสมอมากขึ้น
- Focus style สำหรับ image comparison slider (ตอนนี้ใช้ default browser ring)
- Smooth scroll offset ที่เหมาะสมกับ fixed navbar

**ส่วนที่ได้ประโยชน์:** Embeds, Interactive elements, Sliders

---

#### `/impeccable layout`
**ใช้ทำ:** แก้ไขปัญหา layout ที่พบ
1. **Certificate grid CSS ซับซ้อน** — มี duplicate `.cert-grid` selectors, competing centering logic, `.cert-card` ประกาศซ้ำ
2. **Inline styles ใน Experience section** — ควรย้ายไป CSS classes
3. **Activities grid** — มี 5 items ใน `repeat(3, 1fr)` แถวสุดท้ายมี 2 items แต่ไม่ได้จัดให้อยู่ตรงกลาง
4. **`.exp-card-inner`** — ใช้ `grid-template-columns: 1fr 380px` แบบ rigid ควรใช้ `minmax()`
5. **`.skills-grid`** — `repeat(4, 1fr)` ไม่มี `minmax()` อาจ cramped บน tablet

**ส่วนที่ได้ประโยชน์:** Certificates, Experience, Activities, Skills

---

#### `/impeccable overdrive`
**ใช้ทำ:** อาจเพิ่ม cinematic transition ให้ hero section
- แต่ต้องระวัง performance — เว็บไซต์มี iframe หนักอยู่แล้ว (Figma + Cloudinary)
- แนะนำทำเบาๆ เช่น hero text reveal animation หรือ subtle gradient shift

**ส่วนที่ได้ประโยชน์:** Hero section (optional, ต่ำ优先级)

---

#### `/impeccable quieter`
**ใช้ทำ:** ลดทอน elements ที่ "ดัง" เกินไป
1. **Figma phone mockup** — bezel `#171717` หนาๆ ดูหนักตาใน light mode, ลดหรือใช้สีที่ adapt ตาม theme
2. **Emoji ใน Contact CTA** (`📸 Photography`, `🎨 UX/UI Design`, `✍️ Content Creation`) — ทำลาย tone professional
3. **Creative Works heading** — มี emoji `✨` และใช้ inline style แทน design system

**ส่วนที่ได้ประโยชน์:** Experience (Figma), Contact, Creative Works

---

#### `/impeccable typeset`
**ใช้ทำ:** แก้ไข typography ที่ไม่สม่ำเสมอ
1. **Label padding ใน before/after slider** — `.slider-label--before` ใช้ `4px 12px` แต่ `.slider-label--after` ใช้ `11px 12px` (unbalanced)
2. **Heading "About Me" ซ้ำกัน** — `.section-label` + `.section-title` ต่างเขียนว่า "About Me"
3. **Creative Works** — ใช้ `font-size: 2rem` แบบ hardcode ไม่มี `clamp()`
4. **`.hero-stats`** — ว่างเปล่าแต่ยัง render border + padding ใน DOM

**ส่วนที่ได้ประโยชน์:** About, Creative Works, Sliders, Hero

---

### 🧹 Simplify — ลดความซับซ้อน

#### `/impeccable adapt`
**ใช้ทำ:** ปรับ responsive design ที่ยังมีช่องโหว่
1. **Figma iframe** — ใช้ `width: 390px` + `transform: scale(1.10)` อาจล้นหน้าจอที่ 400-500px
2. **Skills grid** — กระโดดจาก 4 คอลัมน์เป็น 2 คอลัมน์โดยไม่มี 3 คอลัมน์กลาง (768px-1023px)
3. **Experience cards บน tablet** — media ถูกสั่ง `order: -1` ทำให้ต้อง scroll ผ่านรูปก่อนถึงเนื้อหา
4. **ไม่มี breakpoint สำหรับ landscape phone** (568px-767px)

**ส่วนที่ได้ประโยชน์:** Figma embed, Skills grid, Experience cards, Responsive

---

#### `/impeccable clarify`
**ใช้ทำ:** แก้ไข UX copy ที่สับสนหรือผิด
1. **Alt text** — `alt="2"`, `alt="3"` ไม่มีความหมาย (ควรเป็น "ARCHi promotional poster variant 2")
2. **Typo** — "MobiLib 4rd Generation" → "4th Generation"
3. **วันที่ Education** — "2023 — Expected 2026" แต่เป็น "4th Year" ไม่สมเหตุสมผล (4 ปีควรจบ 2027)
4. **Heading ซ้ำ** — "About Me" / "About Me" ควรเปลี่ยน label เป็น "Background" หรือ "Introduction"
5. **Comment ที่ไม่จำเป็น** — `<!-- end max-w-5xl -->` เป็น Tailwind class comment ที่ไม่ใช้แล้ว

**ส่วนที่ได้ประโยชน์:** ทั้งเว็บไซต์

---

#### `/impeccable distill`
**ใช้ทำ:** ลบส่วนที่ไม่จำเป็นออกอย่างไม่ปรานี
1. **`.hero-stats`** — ว่างเปล่า (children ถูก comment ออก) แต่ยัง render border + padding
2. **Commented-out HTML** — Hero stats, บางส่วนของ nav logo SVG
3. **Inline styles** — ย้ายไป CSS classes ที่ reusable
4. **CSS ที่ไม่ได้ใช้** — `.exp-card-inner--stacked` ถูกอ้างอิงใน HTML แต่ไม่มี definition

**ส่วนที่ได้ประโยชน์:** Hero, Experience, ทั้งไฟล์

---

### 🏭 Harden — ทำให้พร้อมใช้งานจริง

#### `/impeccable harden`
**ใช้ทำ:** ทำให้ production-ready
1. **แก้ SRI hash ของ AOS CSS** — hash ปัจจุบันสั้นเกินไปและอาจไม่ถูกต้อง
2. **แก้ inline JS handlers** — `onclick="closeMobileMenu()"`, `onerror="this.parentElement.classList.add('img-placeholder')"` → ใช้ event delegation
3. **เพิ่ม error states** — สำหรับ iframe (Figma หรือ Cloudinary โหลดไม่ได้)
4. **i18n consideration** — ตอนนี้เนื้อหาส่วนใหญ่เป็นภาษาอังกฤษ แต่มีบางส่วนเป็นภาษาไทย (certificates) — ควรมี `lang` attribute ที่ถูกต้องบน elements ที่เป็นภาษาไทย

**ส่วนที่ได้ประโยชน์:** ทั้งเว็บไซต์

---

#### `/impeccable onboard`
**ใช้ทำ:** ออกแบบ first-run / loading experience
1. **Figma embed** — สร้าง poster image หรือ skeleton ขณะโหลด
2. **Cloudinary video** — ใช้ thumbnail ก่อน แล้วค่อยโหลด iframe เมื่อกด play
3. **Empty states** — ตรวจสอบว่ามี handling เมื่อรูปโหลดไม่ได้ (onerror handler มีอยู่แล้วแต่ใช้ inline JS)

**ส่วนที่ได้ประโยชน์:** Experience section

---

#### `/impeccable optimize`
**ใช้ทำ:** ปรับ performance
1. **แปลง PNG → WebP/AVIF** — Hero และ About ใช้ PNG ซึ่งไม่เหมาะกับ photography (ลดขนาดได้ 60-80%)
2. **ใส่ `fetchpriority="high"`** ให้ hero image — ช่วย browser prioritize LCP
3. **Lazy load รูป About section** — อยู่ below-the-fold แต่ไม่มี `loading="lazy"`
4. **ลด blocking time จาก Tailwind CDN** — แนะนำ compile เป็น static CSS หรือ purge unused styles
5. **Preload critical fonts** — Inter และ Playfair Display เป็น render-blocking

**ส่วนที่ได้ประโยชน์:** ทั้งเว็บไซต์

---

#### `/impeccable polish`
**ใช้ทำ:** Final pass ก่อนส่งงาน — ระหว่าง "good" กับ "great"
1. ตรวจทุก hover state (consistency)
2. ตรวจ focus state (ทุก interactive element)
3. ความสม่ำเสมอของ shadow, border-radius, spacing
4. ลบ code ที่ไม่ใช้
5. ตรวจ alt text ทุกรูป
6. ตรวจ ARIA attributes (`aria-controls`, `aria-current`, `aria-expanded`)
7. ทดสอบ keyboard navigation ทั้งหมด

**ส่วนที่ได้ประโยชน์:** ทั้งเว็บไซต์

---

### 🗂️ System — จัดระบบ Design System

#### `/impeccable document`
**ใช้ทำ:** สร้าง **DESIGN.md** จาก design system ปัจจุบัน
- Colors (CSS variables ทั้ง light/dark mode)
- Typography (font families, sizes, weights, line-heights)
- Spacing (section padding, gap sizes, container max-width)
- Components (buttons, cards, badges, tags)
- Animation / motion tokens
- Breakpoints

**ส่วนที่ได้ประโยชน์:** ทั้งโปรเจกต์ (สำหรับ AI agent หรือทีมที่มาทำต่อ)

---

#### `/impeccable extract`
**ใช้ทำ:** ดึง patterns ที่ใช้ซ้ำออกมาเป็นระบบ
1. **รวม duplicate badges** — `.cert-badge` + `.activity-badge` (pixel-for-pixel identical) → `.media-badge`
2. **รวม duplicate labels** — `.creative-label` + `.slider-label` (เกือบเหมือนกัน) → `.image-label`
3. **ดึง inline styles ใน Experience** ออกมาเป็น `.exp-media-grid`, `.exp-figma-frame`, `.exp-video-wrapper`
4. **สร้าง utility classes** สำหรับ patterns ที่ใช้บ่อย (เช่น aspect-ratio wrappers)

**ส่วนที่ได้ประโยชน์:** CSS architecture, maintainability

---

#### `/impeccable live`
**ใช้ทำ:** Iterate UI ใน browser สำหรับการทดลองเปลี่ยนแปลง
- ทดลองปรับสี contrast ต่างๆ แล้วดูผล real-time
- ทดลอง layout ใหม่ของ certificate grid
- ทดสอบ hover effects หลายแบบแล้วเลือกที่ดีที่สุด

**ส่วนที่ได้ประโยชน์:** ทดลองเปลี่ยนแปลงทั้งหมดก่อน commit

---

#### `/impeccable teach`
**ใช้ทำ:** สอน Impeccable ว่าเว็บไซต์นี้เป็นอะไร
- **Product:** Portfolio สมัคร internship สำหรับ Event Marketing
- **Target audience:** Recruiters, hiring managers
- **Desired tone:** Professional but approachable — ไม่ใช่ social media profile
- **Key constraints:** ต้อง load เร็ว (recruiter ดูเร็ว), ต้อง accessible, ต้องดูดีบน mobile

**ส่วนที่ได้ประโยชน์:** Context สำหรับทุก command ที่รันต่อจากนี้

---

## 3. ลำดับการทำงานที่แนะนำ

เนื่องจากบาง command ซ้ำซ้อนกัน ผมแนะนำให้รวมเป็น **3 เฟส**:

### เฟส 1: รีบแก้ก่อนส่ง (Critical + Important)
**เป้าหมาย:** เว็บไซต์ใช้งานได้ ปลอดภัย ไม่มี bug ที่ block การใช้งาน

1. **`/impeccable harden`** + **`/impeccable optimize`** + **`/impeccable clarify`**
   - แก้ SRI hash
   - แปลง PNG → WebP
   - แก้ typo "4rd" → "4th"
   - แก้ alt text
   - แก้ contrast สี `--text-muted`
   - ลบ/แก้ inline JS handlers

2. **`/impeccable distill`**
   - ลบ `.hero-stats` ที่ว่างเปล่า
   - ลบ comment ที่ไม่จำเป็น
   - ย้าย inline styles ไป CSS classes

3. **`/impeccable layout`**
   - แก้ certificate grid CSS (ลบ duplicate, รวม selectors)
   - แก้ activities grid (จัดให้ 2 items สุดท้ายอยู่ตรงกลาง)
   - แก้ skills grid (เพิ่ม `minmax()`)

**ระยะเวลาโดยประมาณ:** 2-3 ชั่วโมง

---

### เฟส 2: ปรับให้สวยและสม่ำเสมอ (Refine)
**เป้าหมาย:** เว็บไซต์ดู professional, consistent, มี brand identity ที่ชัดเจน

1. **`/impeccable quieter`**
   - ลบ emoji ออกจาก Contact CTA (`📸 🎨 ✍️`)
   - ลบ emoji ออกจาก Creative Works heading (`✨`)
   - ลดความหนักของ Figma phone mockup (bezel)

2. **`/impeccable typeset`**
   - แก้ label padding ใน before/after slider (ให้เท่ากัน)
   - แก้ heading "About Me" ซ้ำ (เปลี่ยน label เป็น "Background")
   - ใส่ `clamp()` ให้ Creative Works heading

3. **`/impeccable colorize`**
   - ปรับ `--text-muted` ให้ผ่าน WCAG AA ทั้ง light/dark
   - ปรับ hero badge text contrast
   - ทดสอบทุกส่วนที่ใช้ muted text

4. **`/impeccable animate`**
   - ปรับ hero pulse ให้หยุดหลังหลายรอบ
   - ตรวจสอบ `prefers-reduced-motion`

5. **`/impeccable layout`** (ต่อ)
   - แก้ `.exp-card-inner` ให้ใช้ `minmax()` แทน fixed `380px`
   - แก้ responsive ของ Figma iframe

**ระยะเวลาโดยประมาณ:** 3-4 ชั่วโมง

---

### เฟส 3: Polish + System
**เป้าหมาย:** ทำให้ production-ready, maintainable, มีเอกสาร

1. **`/impeccable polish`**
   - Final pass ทุก hover/focus state
   - ตรวจ keyboard navigation
   - ตรวจ ARIA attributes
   - ตรวจ consistency ของทุก component

2. **`/impeccable extract`**
   - รวม `.cert-badge` + `.activity-badge` → `.media-badge`
   - รวม `.creative-label` + `.slider-label` → `.image-label`
   - ดึง inline styles ออกมาเป็น reusable classes

3. **`/impeccable document`**
   - สร้าง DESIGN.md จาก design system ปัจจุบัน

4. **`/impeccable adapt`** (ต่อ)
   - ปรับ responsive edge cases (landscape phone, Figma iframe overflow)
   - ทดสอบบน devices จริงหรือ browser dev tools

5. **`/impeccable onboard`** + **`/impeccable craft`**
   - สร้าง loading skeleton / poster image สำหรับ Figma embed
   - สร้าง poster image สำหรับ Cloudinary video

**ระยะเวลาโดยประมาณ:** 3-4 ชั่วโมง

---

## 4. Checklist สรุป (สำหรับใช้ติดตามความคืบหน้า)

### 🚨 Critical
- [ ] แก้ SRI hash ของ AOS CSS (หรือลบ integrity attribute ออก)
- [ ] แก้ color contrast `--text-muted` ใน light mode (ให้ผ่าน WCAG AA ≥ 4.5:1)
- [ ] แก้ color contrast `--text-muted` ใน dark mode (ให้ผ่าน WCAG AA ≥ 4.5:1)
- [ ] แก้ typo "4rd" → "4th" (MobiLib Generation)
- [ ] แก้หรือลบ Tailwind CDN (เปลี่ยนเป็น compiled CSS)

### ⚠️ Important
- [ ] รวม `.cert-badge` + `.activity-badge`
- [ ] รวม `.creative-label` + `.slider-label`
- [ ] ย้าย inline styles ใน Experience section ไป CSS
- [ ] แก้ alt text (`alt="2"`, `alt="3"`, `alt="ARCHi"`)
- [ ] แก้ heading "About Me" ซ้ำ (label → "Background")
- [ ] ลบ emoji จาก Contact CTA
- [ ] ลบ emoji จาก Creative Works heading
- [ ] แก้ certificate grid CSS (duplicate selectors)
- [ ] แก้ activities grid (center 2 orphaned items)
- [ ] เพิ่ม loading state / poster image ให้ Figma iframe
- [ ] เพิ่ม poster image ให้ Cloudinary video
- [ ] แปลง PNG hero/about เป็น WebP/AVIF
- [ ] ใส่ `loading="lazy"` ให้รูป About

### 💡 Nice-to-have
- [ ] เปลี่ยนรูป About ให้ต่างจาก Hero
- [ ] แก้ theme toggle aria-label ให้เปลี่ยนตาม state
- [ ] ลบ `.hero-stats` ที่ว่างเปล่า
- [ ] ใส่ `fetchpriority="high"` ให้ hero image
- [ ] หยุด hero-badge-dot pulse หลัง 3-5 รอบ
- [ ] สร้าง DESIGN.md
- [ ] เพิ่ม `prefers-contrast` media query

---

*สร้างเมื่อ: 3 พฤษภาคม 2026*
*สำหรับ: Eve Sujittra Portfolio — https://thanakornprakobdee.github.io/*
