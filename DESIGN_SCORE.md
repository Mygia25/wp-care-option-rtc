# Care Option RTC — Design Score Audit
> Evaluated: 2026-04-17 | File: `index.html`

---

## Scorecard Summary

| Criteria | Weight | Raw Score | Weighted Score | Status |
|----------|--------|-----------|----------------|--------|
| Mobile-First | 25% | 88 / 100 | 22.0 | PASS |
| Perceived Speed | 15% | 78 / 100 | 11.7 | PASS |
| Clear CTA | 20% | 91 / 100 | 18.2 | PASS |
| Trust | 20% | 82 / 100 | 16.4 | PASS |
| Friendliness | 20% | 90 / 100 | 18.0 | PASS |

### **TOTAL WEIGHTED SCORE: 86.3 / 100**
### Grade: A- (Excellent)

---

## Detailed Breakdown

### 1. Mobile-First — 88/100 (Weight 25%)

**What was checked:**
- Viewport meta tag: `<meta name="viewport" content="width=device-width, initial-scale=1.0">` — PRESENT
- Responsive breakpoints: 3 tiers — `@media (max-width: 1024px)`, `768px`, `480px` — COMPLETE
- Touch-friendly buttons: `.btn` uses `padding: 14px 28px` minimum — meets 44px touch target requirement. Phone CTA button has `padding: 16px 36px` — COMPLIANT
- Text legibility at 375px: All text uses `clamp()` sizing — `h1: clamp(2rem, 5vw, 3.5rem)`, `h2: clamp(1.6rem, 3.5vw, 2.6rem)` — COMPLIANT
- Mobile menu: Full-screen slide-in menu with hamburger toggle, aria-expanded support — PRESENT
- Navigation hides on mobile: `.nav-links, .nav-phone { display: none; }` at 768px — COMPLIANT
- Hero: collapses to single column at 1024px — PASS
- `.hero-ctas { flex-direction: column }` at 768px — PASS
- `.form-row { grid-template-columns: 1fr }` at 768px — PASS

**Issues noted (−12 pts):**
- Mobile menu `display: flex` only triggered by JS — no CSS-only fallback if JS fails (minor)
- `.stat-card` floating elements at `left: -32px` on desktop may overflow at 375px despite responsive breakpoints trying to shift them to `-16px`
- Footer collapses to single column but stacks are very long on small screens; no "scroll to top" shortcut

---

### 2. Perceived Speed — 78/100 (Weight 15%)

**What was checked:**
- External images: Uses local `images/` folder (hero.png, school.png, activities.png, etc.) — NO heavy CDN images
- CSS gradients: Yes — hero background uses `linear-gradient(160deg, #F5F3FA 0%, #F0EDE6 40%, #FAFAF7 100%)` — PASS
- Fonts preloaded: Uses `<link rel="preconnect" href="https://fonts.googleapis.com">` and `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>` — PARTIAL (preconnect but no `<link rel="preload">` for critical font files)
- Render-blocking JS: Lucide icons `<script src="https://unpkg.com/lucide@latest"></script>` in `<head>` WITHOUT `defer` or `async` — BLOCKING
- No analytics/tracking scripts — GOOD
- No external CSS beyond Google Fonts — GOOD
- Inline `<style>` block — avoids extra CSS file roundtrip — GOOD

**Issues noted (−22 pts):**
- **Lucide script is render-blocking** (in `<head>`, no defer) — biggest performance issue. Should use `defer` or move before `</body>`
- Google Fonts URL does not have `&display=swap` explicitly set in preconnect (it is in the `<link href>` tag itself, so partial credit)
- No explicit `loading="lazy"` attributes on non-above-fold images (`school.png`, `activities.png`, `team-horizontal.png`, `margaret-portrait.png`)
- Blob animations (CSS `filter: blur(60px)`) run on main thread — minor paint cost

**Recommended fixes:**
```html
<!-- Move Lucide to defer -->
<script src="https://unpkg.com/lucide@latest" defer></script>

<!-- Add lazy loading to below-fold images -->
<img src="images/school.png" loading="lazy" alt="...">
<img src="images/activities.png" loading="lazy" alt="...">
```

---

### 3. Clear CTA — 91/100 (Weight 20%)

**What was checked:**
- Phone number in nav: `(281) 809-3653` as `<a href="tel:+12818093653">` — PRESENT AND FUNCTIONAL
- Phone in hero: Primary CTA button with phone icon + number — PRESENT
- Every major section has a CTA:
  - Hero: "Call Us Now" (primary) + "Learn About Our Services" (secondary)
  - Why section: trust points lead naturally to implicit CTA
  - Admission section: "Start the Conversation — (281) 809-3653"
  - Bottom CTA section: "Call (281) 809-3653 — We're Here" + "Send a Message"
  - Contact section: full form + direct phone/email links
- Email: `careoptionrtc@gmail.com` with `mailto:` link — PRESENT
- Emergency guidance note on form: clearly directs urgent cases to phone — EXCELLENT
- Contact form is complete with required fields and submit feedback

**Issues noted (−9 pts):**
- The "Why Care Option" and "Approach" sections lack a direct CTA button (users reading those sections have no immediate next action except scrolling)
- No sticky "call now" floating button for mobile users who are mid-scroll (common on healthcare sites)
- Team/testimonials section has no inline CTA

---

### 4. Trust — 82/100 (Weight 20%)

**What was checked:**
- License info: "Licensed in Texas" — appears in nav phone badge, hero trust badges, footer badge. Multiple placements — EXCELLENT
- Experience mentioned: "15+ Years Healthcare Leadership" — in hero badges, Why section heading, team credentials — THOROUGH
- Credentials visible: Founder name (Margaret Nwajiaku) visible in footer, team section. Founder title and Medicare-certified background mentioned — PRESENT
- Schema markup: `MedicalOrganization` + `LocalBusiness` dual schema with telephone, address, founder, credentials — WELL-IMPLEMENTED
- Professional design: Deep Harbor palette is authoritative and medical-adjacent without being sterile — VERY STRONG
- Privacy statement on contact form — PRESENT
- Testimonials: 5 testimonials with professional titles (Licensed Case Worker, DFPS Coordinator) — HIGH TRUST VALUE
- Privacy note for facility address — APPROPRIATE

**Issues noted (−18 pts):**
- "15+ years" in index.html but brief mentions "18+ years" — inconsistency in the materials (BRIEF.md vs site copy). Should be aligned to one number
- No specific Texas license number visible (CPS-regulated facilities often post license #)
- Founder bio mentions "Reez Home Health Services" — if that has CMS quality concerns (as noted in task brief), this name appears in the Why section text on line 1556 and should be removed or softened
- No physical address (intentional per privacy policy) — affects local trust for families doing due diligence
- No accreditations, associations, or certifications beyond "Licensed in Texas"

**Recommended fix:**
- Line 1556: Remove "Reez Home Health Services" reference, replace with "a Medicare-certified agency"

---

### 5. Friendliness — 90/100 (Weight 20%)

**What was checked:**
- Tone: "A Safe Place to Heal, Grow, and Be a Kid Again" — warm, child-centered, NOT clinical — EXCELLENT
- Hero sub-copy: mentions "real childhood — school, friends, activities" — humanizing — PASS
- NOT clinical: No medical jargon in hero, about, or CTA sections. Words like "healing," "nurturing," "home-like" dominate — PASS
- Feels like home: Pillar cards use "A Real Home Environment," "family-style meals," "consistent caregivers" — STRONG
- Color palette: warm cream backgrounds (#F0EDE6, #FAFAF7), not cold clinical whites — APPROPRIATE
- Typography: Mix of Plus Jakarta Sans (modern) + Lora serif (warmth) — GOOD BALANCE
- Testimonials: Written from family/caseworker perspective, focus on emotional outcomes — EXCELLENT
- "Let's Talk About Your Son" — direct, personal, not bureaucratic — STRONG
- Emergency note: "We have time for you" on initial contact step — reassuring tone
- Photo content: hero.png, family-connection.png, activities.png reinforce home-like feel

**Issues noted (−10 pts):**
- The "Why Care Option" section dark background (#0E2233) is the most "corporate/clinical" feeling section. Good for trust but slightly breaks the warm home atmosphere
- The team section pivots to credential cards rather than human faces/bios — misses warmth opportunity (no photos of actual staff)
- "Case Management Coordination (DFPS, courts...)" language in one section is bureaucratic — needed but slightly clinical-sounding

---

## Items Below 70% Threshold
**None.** All 5 criteria score above 70%.

---

## Priority Fixes (Ordered by Impact)

| Priority | Fix | Criteria | Effort |
|----------|-----|----------|--------|
| 1 | Add `defer` to Lucide script tag | Speed | 2 min |
| 2 | Remove "Reez Home Health" from Why section (line 1556) | Trust | 2 min |
| 3 | Add `loading="lazy"` to below-fold images | Speed | 5 min |
| 4 | Add floating "Call Now" sticky button on mobile | CTA | 30 min |
| 5 | Add CTA button at end of Why section | CTA | 10 min |
| 6 | Align "15+ years" vs "18+ years" across all materials | Trust | 5 min |

---

## Summary

The Care Option RTC website is a well-executed, professionally designed single-page site. It performs strongest on Friendliness (90) and CTA clarity (91), with solid Trust signals (82) and good Mobile-First implementation (88). The main performance gap is Perceived Speed (78) due to a render-blocking Lucide icon script — an easy 2-minute fix. No criteria fall below the 70% threshold, meaning the site is production-ready as-is, with the listed improvements as post-launch optimizations.
