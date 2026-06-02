---
theme: nord
title: Decoupling a Safety-Critical SPOF
info: |
  EM case — Home-safety IoT platform.
  Decoupling alarm notifications from the digital twin.
class: text-center
transition: fade
mdc: true
---

# Decoupling a Safety-Critical SPOF

<div class="jp-muted mt-2">Decoupling alarm notifications from the digital twin</div>

<div class="pt-10 jp-muted text-sm">
Home-safety IoT platform
</div>

<style>
/* Custom surfaces tuned to the Nord palette (theme handles bg/fonts/headings) */
.jp-muted { color: #D8DEE9; opacity: 0.85; }
.step { padding-top: 0.4rem; }
.step h3 { margin-top: 0.2rem; }
.step--clay { border-top: 3px solid #81A1C1; }   /* frost blue  → cache step */
.step--sage { border-top: 3px solid #A3BE8C; }   /* aurora green → event step */
.shot { border: 1px solid #434C5E; border-radius: 8px; }
</style>

---

## The problem — a single point of failure

The system ingests IoT telemetry into a **digital twin**. Critical services depend on it for data, creating a **single point of failure**.

- Worst for the **alarm notification** flow — it alerts customers to a **fire, water, or burglary** incident
- Traffic spikes, bugs, and maintenance caused incidents with notifications **delayed minutes, sometimes hours**
- For a safety brand and product, this is one of a few flows that **must always work**

---

## Architecture & the constraint

<div grid="~ cols-2 gap-8" class="items-center">

<div>

The alarm flow made multiple **HTTP calls** to the **digital twin** to build each notification.

- Over-provisioning or read replica: expensive, and doesn't fix the coupling
- The digital twin's health **was** the alarm flow's health

</div>

<img src="/prev.png" alt="Before — the alarm flow calls the digital twin synchronously over HTTP" class="shot max-h-[23rem] mx-auto" />

</div>

<div class="mt-4 jp-muted text-sm">Stack — Node · TypeScript · NestJS · Kubernetes · RabbitMQ · MongoDB · PostgreSQL · Redis</div>

---

## The fix — two deliberate steps

<div grid="~ cols-2 gap-8">

<div class="step step--clay">

### 1 · Quick win — shared cache

<img src="/quick-win.png" alt="Step 1 — read-through Redis cache in front of the digital twin" class="shot max-h-[13.5rem] mx-auto my-2" />

- Read-through **Redis** cache — reads survive **digital twin** degradation
- Tradeoff: the **single point of failure** moved to Redis

</div>

<div class="step step--sage">

### 2 · Event-Carried State Transfer

<img src="/final.png" alt="Step 2 — twin emits events; the alarm flow owns its own Postgres data" class="shot max-h-[13.5rem] mx-auto my-2" />

- Digital twin emits **events**; `alarm-notification` owns its **Postgres** data
- **No synchronous twin calls left** — a reusable pattern for critical flows
- More complexity, but a fully decoupled flow

</div>

</div>

---

## Results, team & leadership

<div grid="~ cols-2 gap-8">

<div>

### Results

- **Critical flow decoupled** — survives digital-twin degradation
- **Fewer incidents, lower severity**; far fewer calls to the twin
- A **reusable decoupling pattern** adopted beyond this flow

### Team

- 2 product teams + a **2-engineer platform team** (most of the work)
- My role (**EM**): architecture, planning, stakeholder alignment

</div>

<div>

### How I drove it

- **Buy-in up** — framed for **CTO/CPO** as reliability & brand risk, with the incident log as evidence → capacity from two product teams
- **Iterative improvements** — quick-win cache first, then the reusable pattern, piloted on `alarm-notification`
- **Reusable pattern** — **ADR** + architecture forums; engineers in design review for shared ownership
- **Handover + roadmap** — product teams adopt it for new work; prioritized migration of the remaining flows

</div>

</div>
