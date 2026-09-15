# ÉTERNELLE — App Blueprint

**Tagline:** *Your face. Every era of fashion. Zero closet.*

**Target user:** Fashion-forward women 40+ and personal brands (e.g. justjessica.co) who want editorial-grade imagery of themselves without a shoot, a stylist, or a purchase.

---

## Core flow (one flow, three screens)

**1. INPUT — "Your Muse" (one-time, ~90 seconds)**
- Upload 8–15 selfies (varied angle/lighting). Stored as a reusable **Muse** (character identity).
- Three taps of styling context, used verbatim in every prompt so the output flatters rather than de-ages:
  - Age presence — e.g. *"radiant 52, keep the laugh lines"*
  - Skin tone + undertone — Fitzpatrick chip picker
  - Body & fit — silhouette + "clothes should drape like they were tailored for her"
- Pick a **Collection**: `Gothic Corporate` · `Coastal Grandma` · `Quiet Luxury Milan` · `Desert Couture` · `Archive Y2K` · custom text box.

**2. GENERATE — the Lookbook build**
- **Step 0 · Cost sheet (blocking).** Before a single credit moves, a modal shows: shots requested, credits per shot, **total credits**, current balance, balance after. Buttons: *Cancel* / *Confirm & Spend*. Nothing calls the API until Confirm. Same modal, same code path, for every generation type — no exceptions, no "remember my choice" bypass.
- **Step 1 · Identity lock (one time per Muse).** Train the personalized character model from the uploads.
- **Step 2 · Editorial pages.** 50 stills across the Collection: 12 prompt archetypes (full-length street, studio portrait, detail/texture crop, seated editorial, motion/turn, back-of-garment, etc.) × lighting notes tuned to her skin tone. Streams in as a grid; each page lands individually so she watches the book fill.
- **Step 3 · Runway motion (optional, her pick).** She taps up to 5 favourite stills → each becomes a 5-second runway-walk clip. Separate cost confirmation, because this is the expensive step.
- **Step 4 · Bind.** Pages laid into a Vogue-style spread — cover, contents, full-bleeds, pull-quotes — exported as a scrolling web lookbook + PDF.

**3. OUTPUT**
- A shareable 50-page lookbook URL, a PDF, a ZIP of stills, and 0–5 vertical 5s MP4s sized for Stories/Reels.

---

## Batch mode — **"The Season"**

Pick **up to 4 Collections at once** and one intensity (`Capsule 12` / `Editorial 25` / `Full Book 50`). One cost sheet totals the whole run (e.g. 4 × 50 = 200 pages) and it queues overnight; she wakes up to four finished lookbooks. This is the multiplier: same setup effort, 4× the output, and it converts "pick one aesthetic" anxiety into "see them side by side."

Cross-sell inside the same confirmation: *"+5 runway clips per collection, +N credits?"* — one checkbox, one total.

---

## History screen

A single reverse-chronological list. Each row: cover thumbnail, collection name, date, page count, **credits spent**, status chip (`Queued` / `Building 23/50` / `Ready` / `Failed — not charged`). Tap to reopen the lookbook. Row actions: *Re-run this collection on a new Muse*, *Animate a page*, *Download*. A sticky header shows lifetime credits spent this month — spending stays visible, never buried.

---

## Higgsfield models per step

| Step | Model | Why |
|---|---|---|
| Identity lock | **Higgsfield Soul ID** (character training from a selfie set) | The whole product depends on her being recognisably *her* across 50 frames. |
| Editorial stills | **Higgsfield Soul** conditioned on the Soul ID | Soul is the photoreal/fashion-editorial aesthetic model — the look the target user is actually buying. |
| Multi-angle page variants | **Higgsfield Popcorn** (multi-shot, consistent subject) | Generates a coherent set from one setup instead of 12 unrelated one-offs. |
| Runway walk video | **Higgsfield DoP I2V** (image-to-video, cinematic camera control) | Takes the chosen still as frame 1; camera-motion presets give a real runway dolly/track instead of drift. |
| Fast preview clips | **DoP Turbo / Lite** | Cheap draft pass so she confirms the shot before spending on the full-quality render. |
| Upscale for print/PDF | Higgsfield upscale endpoint | 50-page PDF needs print resolution. |

> Verify these slugs against the live `GET /models` catalog at build time and render the cost sheet from the API's own per-model credit price — never from a hardcoded number. If a slug 404s, fail before the confirmation modal, not after.

---

## Scope for today

Higgsfield OAuth handles sign-in — no user table, no passwords, no payments (credits are hers, spent on her key). One Next.js app: `/muse` (upload + traits), `/build` (collection picker → cost sheet → streaming grid), `/book/[id]` (reader + export), `/history`. Jobs in a queue table keyed by Higgsfield user ID. Every generation route goes through one `confirmCost()` gate; that gate is the only place the API key is touched.
