# BarBell

## What This Is

AI-powered fitness platform with two concurrent business lines running in parallel.

---

## Primary: B2B White-Label Gym Companion

BarBell's core commercial product. Sells to commercial gyms at $500/month as a
white-labeled AI trainer layer that plugs into their existing brand and equipment setup.

Key differentiators:
- Location-aware — knows the specific equipment at THAT gym, not a generic list
- White-labeled into the gym's own app and brand identity
- Conversational and real-time — coaching during the workout, not just a plan
- B2B infrastructure play, not a consumer app

Pricing model: $500/month per gym  
Go-to-market: Direct outreach to gyms → getbarbell.com → Calendly call → deposit to onboard  
Contact: connect@getbarbell.com

---

## Secondary: Military Readiness Intelligence Platform

BarBell's government/DoD line. Active military pilot contract for physical readiness
testing. Positioned as a readiness intelligence platform, not just a fitness app.

The crown jewel is the Continuous Readiness Score (CRS) — a real-time 0-100 readiness
metric replacing twice-yearly pass/fail testing with a continuous leading indicator.
Scales to Unit Readiness Score (URS) at command level.

Computer vision layer: MediaPipe for AI-powered rep counting and form verification.
Camera counts valid reps — no manual logging, no honor system.

---

## Stack

- Frontend: React / Next.js (mobile-first)
- Backend: FastAPI
- Database: Supabase
- AI Layer: Claude API (coaching, personalization, analytics)
- Computer Vision: MediaPipe (military line — rep counting + form verification)
- Hosting: Railway (backend) + Vercel (frontend)
- Version Control: GitHub — TDXIGlobal org

---

## Current Status

### Built
- [x] Landing page — `index.html` deployed at getbarbell.com / barbell-eight.vercel.app
  - Hero with staggered fade-in, oversized BARBELL watermark
  - Problem section with animated stat count-up (50%, 8x, $500)
  - How It Works — 4-feature grid (equipment-aware, real-time coaching, white-label, retention)
  - Military credibility section (no branch names — "military tested and approved")
  - Partnership section — Jacksonville FL + Virginia Beach VA exclusive markets
  - Application form — 6 fields, pill button groups, inline validation
  - Qualification branching: Under 50 members → waitlist state; 50+ → Calendly inline embed
  - Formspree (mgoqrpre) — form submissions email connect@getbarbell.com
  - Calendly (connect-getbarbell/new-meeting) — injected dynamically post-submission
- [x] GitHub repo — github.com/TDXIGlobal/barbell
- [x] Deployed to Vercel — barbell-eight.vercel.app (TDXI's projects team)
- [x] Domain added to Vercel — getbarbell.com (DNS propagation pending via Namecheap)

### What's Next (B2B)
- [ ] Confirm DNS live at getbarbell.com
- [ ] Launch outreach campaign to gyms in Jacksonville + Virginia Beach
- [ ] Build FastAPI scaffold + Supabase connection
- [ ] Build equipment database schema (gym-specific inventory)
- [ ] Build AI trainer conversation layer (Claude API)
- [ ] Build white-label configuration system (gym branding)
- [ ] Build workout session UI (mobile-first)
- [ ] Build gym onboarding flow

### What's Next (Military — Secondary)
- [ ] PRT event logging (run, pushups, plank)
- [ ] CRS scoring algorithm
- [ ] Sailor/operator dashboard
- [ ] MediaPipe computer vision integration
- [ ] Commander dashboard
