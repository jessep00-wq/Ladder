# OTHERWISE — App Blueprint

**Tagline:** *A camera roll from the life you didn't live.*

**Target user:** 25–45, high curiosity and low creative confidence. The person who still thinks about the job they turned down, the city they didn't move to, the version of themselves that kept going with the guitar. They will never open a blank prompt box — so we never show them one.

---

## The one idea everything else serves

The output must look like it was **stolen off a phone**, not shot for a magazine. Slightly overexposed. Mid-blink. Someone's thumb in the corner. A photo of a receipt. This is the opposite of a beauty filter, and it is the entire product: an editorial render says *fantasy*, a bad candid says *this happened*. Every prompt carries the amateur-photography conditioning, and we deliberately **do not upscale**.

---

## Core flow

**1. INPUT — "You, actually" (one-time, ~60 seconds)**
- 8–15 selfies → a reusable identity.
- Four facts, typed once, that make the fork legible: current age, city you actually live in, what you actually do, and one thing you almost did. That last field is the engine.
- Then: **the doors.** Three are dealt to you, face-up, generated from your four facts — not a menu of clichés:
  - *"You took the Lisbon job in 2019."*
  - *"You never stopped playing. 2011."*
  - *"You said no, and stayed."*
  - A fourth door is always **Deal me another three** — free, instant, no credits. Browsing is free; walking through costs.

**2. GENERATE — walking through**
- **Step 0 · Cost sheet (blocking).** Photos requested, credits each, total, balance before/after. *Cancel* / *Confirm & Spend*. Nothing touches the API before Confirm — same gate, every generation type, no bypass, no "don't ask again."
- **Step 1 · Identity lock** (once per user, reused by every door forever).
- **Step 2 · The divergence plan** (text only, zero Higgsfield credits). Builds a timeline from the fork year to today: where you lived, what the rooms looked like, what you wore, who was around — then writes 20 shot specs with dates and places. This is where the life is actually authored; the image models just execute it.
- **Step 3 · The roll.** 20 candid photos, generated in dated clusters so a single afternoon actually looks like a single afternoon.
- **Step 4 · The clip.** The planner flags the single most emotionally loaded frame; you approve it, pay separately, and get 5 seconds of it moving. One clip per life, by design — scarcity is what makes it land.

**3. OUTPUT**
- A scrollable camera roll, sorted by date across years, with location stamps and captions in the voice of someone who was there. One 5s vertical clip. One share card: *"In another life I've been in Lisbon for six years."*

---

## Built-in guardrail (not optional, not a setting)

**No other real person is ever rendered.** The partner you didn't marry, the friend you lost — they appear turned away, out of frame, a hand in the corner, an empty chair. The planner is instructed to compose around absence, and uploads are for *your* face only. Deepfaking an ex is the obvious abuse of this app and the product simply cannot do it.

---

## Batch mode — **THE FORK**

Walk through **4 doors at once**, one cost sheet, one overnight queue. You wake to four lives and a **side-by-side view**: the four of you at the same age, same grid, four different kitchens. That comparison screen is the share asset — it's the thing that doesn't exist anywhere else and the reason someone screenshots this.

Bolted onto the same confirmation, one checkbox each:
- **+10 years** — continue any life forward into a second chapter. Depth multiplier: a life you've already met is worth more than a new one.
- **+clip per life** — 4 clips instead of 1.

Max run: 4 doors × 20 photos × 2 chapters = 160 images, priced in a single total before a credit moves.

---

## History screen — **The Multiverse**

A grid, not a list. Each tile: the best frame from that life as the cover, the door's name, the fork year, chapter count, **credits spent**, status (`Queued` / `Living 14/20` / `Ready` / `Failed — not charged`). Tap to re-enter the roll. Per-tile actions: *+10 years*, *animate another frame*, *deal a door like this one*. Sticky header shows credits spent this month — always visible, never buried.

---

## Higgsfield models per step

| Step | Model | Why |
|---|---|---|
| Identity lock | **Higgsfield Soul ID** | Twenty photos across ten fictional years only work if it is unmistakably one person aging through them. |
| Candid photos | **Higgsfield Soul**, amateur/phone-photo conditioning | Soul carries the photoreal weight; we spend the prompt budget pushing *down* from editorial toward snapshot. |
| Same-day clusters | **Higgsfield Popcorn** (multi-shot, consistent subject) | Three frames from one afternoon must share a room, an outfit and a light source. One call, not three. |
| The 5s clip | **Higgsfield DoP I2V** | Chosen still becomes frame 1; restrained camera motion — a handheld drift, not a dolly. Cinematic moves would break the found-footage lie. |
| Frame preview | **DoP Turbo / Lite** | Cheap draft so she approves the motion before paying for the real render. |
| Upscaling | *None — deliberately* | Print resolution would destroy the effect. Ship it phone-sized. |

> Resolve every slug against the live `GET /models` catalog at startup and price the cost sheet from the API's own per-model credit values — never a hardcoded constant. A 404 on a slug fails loudly *before* the confirmation modal, not after.

---

## Scope for today

Higgsfield OAuth handles sign-in — no user table, no passwords, no payments (her credits, her key). One Next.js app, four routes: `/you` (selfies + four facts), `/doors` (dealt premises → cost sheet → streaming roll), `/life/[id]` (the roll + clip + share card), `/multiverse` (history). Jobs in a queue table keyed by Higgsfield user ID. Every generation route passes through a single `confirmCost()` function, which is also the only place the API key is read.
