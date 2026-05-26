# Spec-Driven Development with Devin and Copilot

**How we keep AI output consistent across tools and teams**

Derrick · UI Architecture · Opus Deal Manager · spec-agent framework

---

## How to read this document

This is the full content of a 60-minute presentation, written out for reading. Each section corresponds to a slide. For every slide you'll find:

- **What's on the slide** — the headlines, bullets, and visual content
- **What I say** — the spoken narration, in natural language

Time budgets are listed per section. Total target: 60 minutes, with ~3 minutes of Q&A buffer.

---

## Slide 1 — Title

**Kicker:** A practitioner talk
**Title:** Spec-Driven Development with Devin and Copilot
**Subtitle:** How we keep AI output consistent across tools and teams

**What I say** *(~30 seconds)*

Welcome. This is a working session, not a vendor pitch. We're going to spend most of the next hour on how we actually use Devin and Copilot day-to-day, what we've built to make them work together, and where this is heading.

---

## Slide 2 — A bit about me

**Kicker:** Introduction
**Title:** A bit about me

- **ROLE** — Engineering Manager and UI Architect
- **LEAD ON** — Opus Deal Manager — Angular-based enterprise platform
- **OWNER OF** — the spec-agent framework — where we'll spend most of today

**What I say** *(~30 seconds)*

Quick context on me. I'm an engineering manager and UI architect. I lead the work on Opus Deal Manager, our Angular-based platform. I've been hands-on with both Devin and Copilot for the last several months, and the kit we'll look at today came out of that work. I'm not selling anything — this is what we built because we needed it.

---

## Slide 3 — What you'll leave with

**Kicker:** Agenda
**Title:** What you'll leave with

1. **A practical look at Devin** — what it does, how to use it, and where it fits alongside Copilot
2. **The spec-driven kit we built** — to keep AI output consistent across both tools
3. **Skills, schemas, and standards** — as a reusable kit your team can adopt

**What I say** *(~45 seconds)*

Here's what we'll cover. We'll start with Devin specifically — a quick primer on what it can do — then move into the kit we've built that grounds both Devin and Copilot. The last part is about how this scales beyond my team.

A note on questions: feel free to interrupt with clarifying ones, but hold the bigger discussion topics for the end — I've left time for them.

---

## Slide 4 — The opening pain

**Kicker:** The problem
**Title:** Same prompt. Different output. Every time.

> A developer asks Copilot for a new grid widget.
> Two days later, a different developer asks Devin for the same thing.
>
> They get two different implementations.
> Different patterns. Different conventions. Both technically work.
>
> ***Now multiply that by every developer, every tool, every week.***

**What I say** *(~90 seconds)*

This isn't a Devin problem or a Copilot problem. It's what happens when you put powerful tools in front of every developer without shared grounding.

The tools are doing exactly what you'd expect — generating plausible code based on whatever context they have. The problem is the context isn't shared. Each developer ends up with their own informal way of prompting, their own patterns, their own shortcuts. And that's fine when you're prototyping.

It's not fine when you're shipping production code across an enterprise.

*Pause.*

We didn't notice this for a while because each individual output looked reasonable in isolation. The problem only shows up when you start looking at code across the team and seeing how inconsistent it is.

---

## Slide 5 — Autonomous agents raised the stakes

**Kicker:** Why now
**Title:** Autonomous agents raised the stakes

| **Copilot** — Inline. Real time. | **Devin** — Autonomous. Async. |
|---|---|
| You watch every suggestion | Runs for an hour without you |
| You catch drift as it happens | No real-time correction |
| **You are the human in the loop** | **The standards file IS the human in the loop** |

**What I say** *(~60 seconds)*

With Copilot, you've always been the safety net. Bad suggestion? You hit escape. The human in the loop is you, watching every token.

With Devin, that safety net is gone. By the time you're reviewing the PR, the decisions have been made. The session ran for an hour without you. Whatever was in the agent's context — that's what shaped the output.

That changes what grounding has to do. It's no longer convenience. When the agent is running autonomously, the standards file is the only signal it has. It's the only safety net left.

---

## Slide 6 — Devin, quick primer

**Kicker:** Level set
**Title:** Devin — quick primer

Four modes:

- **Sessions** — Autonomous async work. Picks up a ticket, runs in its own VM, opens a PR.
- **Ask Devin** — Conversational mode. Quick questions about a codebase, no session needed.
- **DeepWiki** — Codebase research and Q&A. Synthesized answers across files.
- **Multi-repo** — Cross-cutting questions across multiple repos. Find patterns, services, usages.

**What I say** *(~2 minutes)*

Quick level set on Devin, since I know some of you are using it daily and others are still getting started.

Four main modes. **Sessions** is what most people think of first — autonomous async work. You assign Devin a ticket, it runs in its own VM, opens a PR. Comes back hours later with the work done.

**Ask Devin** is the conversational mode. You don't need to spin up a full session. You just ask a question about the codebase and get an answer. Fast.

**DeepWiki** is codebase research and Q&A. It synthesizes answers across files. If you've been writing a doc and wishing you could just ask "how does our auth flow work" and get a real answer instead of having to read three services — that's DeepWiki.

**Multi-repo** is the one most people underuse. You can ask cross-cutting questions across multiple repos. "Where do we wrap HTTP calls?" "Which services still use the old pattern?" Find patterns, services, usages — across the whole org.

The use cases on the next slide show how these fit together in practice.

---

## Slide 7 — Use cases at our org

**Kicker:** Use cases
**Title:** What this looks like day-to-day

**🔧 Grid-kit improvements across repos** *(Sessions)*
Rolling out grid-kit feature updates to multiple consumer repos. Devin picks up the work async, opens PRs we review in batches.

**🔍 Cross-repo research and porting** *(DeepWiki + Sessions)*
Implementing base-href replacement across repos. Bringing grid-kit features and services from one repo into another. Research what's there, then port it.

**⚡ [TBD — quick win story]** *(Ask Devin)*
[Placeholder: a moment where Ask Devin saved someone significant time on a contextual question. Fill in before the talk.]

**What I say** *(~3 minutes)*

Three things we've actually shipped with Devin.

**First — grid-kit improvements across repos.** We have a grid-kit that consumer repos pull from. When we ship a feature, multiple downstream repos need to adopt it. That's exactly the kind of mechanical, well-scoped work Devin is great at. We hand it the ticket, it picks up the work async, opens PRs in each repo. We review them in batches. That's hours saved per release cycle.

**Second — cross-repo research and porting.** This is where DeepWiki and Sessions work together. We needed to implement base-href replacement across multiple repos. Devin researches how it works in repo A, understands the pattern, then implements it in repo B respecting that repo's conventions. Same with bringing grid-kit features from one repo into another — the research and the implementation in one workflow.

**Third — [your Ask Devin story].**

> *[Fill in before the talk. The strongest version of this is a real moment where a teammate saved significant time by asking Devin a contextual question instead of hunting through code or pinging someone. Even a small example is enough — the point is that Ask Devin pays off for quick contextual saves, not just big tasks.]*

---

## Slide 8 — The bridge

**Kicker:** The bridge
**Title:** So how do we make this consistent?

> *Devin and Copilot are powerful. But on their own, they reach for whatever context they can find — and that context isn't shared across our team.*

We needed:
- Something both tools could read
- Something every developer would inherit automatically
- Something that lived in the repo, not in someone's head

**What I say** *(~45 seconds)*

So this is the problem we kept hitting. Every workaround we tried got us partway — better prompts, sharing prompts in Slack, individual instruction files. None of them scaled.

What we needed was something both tools could read. Something every developer on the team would inherit automatically just by working in the repo. Something that lived in the repo, not in someone's head or someone's Slack history.

What we ended up building is what we'll spend the rest of today on.

---

## Slide 9 — The kit, at a glance

**Kicker:** What we built
**Title:** The kit, at a glance

```
spec-agent/
├── AGENTS.md           ← entry point every agent reads first
├── agents/             ← personas — architect, frontend, backend, QA
├── skills/             ← packaged, callable workflows
├── standards/          ← coding conventions and patterns
├── specs/              ← schemas and domain specs
├── docs/               ← decisions and ADRs
└── spec-agent-kit/     ← the distributable — drop into any repo
    ├── grid-kit/
    ├── ui-config/
    ├── demo-site/
    └── scripts/
```

**What I say** *(~2 minutes)*

This is the kit. One folder you drop into your repo.

**AGENTS.md** is the entry point. Every agent reads this first — Devin, Copilot, Claude, whatever comes next. This file is what routes the agent to everything else.

**agents/** holds the personas. Architect, frontend, backend, QA. The agent picks one depending on the task.

**skills/** are packaged, callable workflows. We'll come back to these in a few slides — they're how we make complex workflows repeatable.

**standards/** is where our coding conventions live. Not just "use 2 spaces" stuff — real architectural patterns. How we handle errors. How we structure components. How we name things.

**specs/** holds our schemas and domain specs. These are the contracts.

**docs/** is the decision trail. ADRs, design docs, history. Why we did things.

And **spec-agent-kit/** is the distributable part. This is what you actually copy into a new repo. Grid-kit, ui-config schemas, the demo site for building schemas visually, and scripts that handle the adoption mechanics.

Each of these gets its own moment in the next slides. For now, the picture is: one folder, drops into your repo, every agent reads it the same way.

---

## Slide 10 — Copy what you need. Consistency stays.

**Kicker:** The model
**Title:** Copy what you need. Consistency stays.

> **You copy the kit into your repo. You own it. You can modify it.**
>
> **Schemas and standards keep the output consistent across teams.**

*More on the model as it matures. Today: what's working.*

**What I say** *(~45 seconds)*

The obvious question when someone hears "copy the kit": if every team has their own copy, won't it drift?

Yes — but the parts that *have* to stay consistent are the schemas. The configs every team produces still validate against the same schemas. That's the consistency mechanism. The rest, teams can adapt to their context.

You own your copy. You can modify it. But the output shape stays consistent because the schemas are the contract.

---

## Slide 11 — Same content. Different filenames.

**Kicker:** Platform adapters
**Title:** Same content. Different filenames.

**For Devin** — `.devin/knowledge.md`
```
# Devin Knowledge — spec-agent

## Operating Rules

Before every task:
1. Read spec-agent/AGENTS.md
2. Load the persona...
3. Load standards...
4. Validate against schemas...
```

**For Copilot** — `.github/copilot-instructions.md`
```
# Copilot Instructions — spec-agent

## How to Operate

1. Identify your persona — read AGENTS.md
2. Check for a domain spec
3. Follow standards
4. Produce schema-valid output
```

> *Both files are thin. Both point back to AGENTS.md. One source of truth.*

**What I say** *(~90 seconds)*

This is where it gets practical. Both tools have their own file convention — that's not going to change. Devin reads `.devin/knowledge.md`. Copilot reads `.github/copilot-instructions.md`. Different filenames, different locations.

What we did was make those files *thin*. They're essentially pointers. The real content lives in `AGENTS.md` and the standards. Each adapter is a short list of operating rules that tells the tool how to find and use the kit.

When we update `AGENTS.md`, both tools get the update on their next run. We don't maintain two parallel sets of instructions. One source of truth. Two thin adapters.

---

## Slide 12 — The handoff goes both ways

**Kicker:** Synergy
**Title:** The handoff goes both ways

**Spec-driven · Bounded work inside a repo:**

```
Developer → Copilot (drafts spec) → Merge → Devin (executes async, opens PR) → Review
```

**Research-driven · Cross-repo or cross-service work:**

```
Question → Devin (researches via DeepWiki, multi-repo) → Developer (reads, frames the next step) → Copilot (executes in the editor) → Review
```

> *Same kit grounds both. Use the tool that fits the task.*

**What I say** *(~2.5 minutes)*

There are two ways these tools work together — and which one you use depends on the task.

**The first is the spec-driven path.** You know what you're building, you draft the spec with Copilot inline, you merge it, Devin picks it up and executes. That works when the work is bounded — a feature inside one repo, something where the spec is the contract. This is the cleaner of the two flows, and it's what most "spec-driven" content online describes.

*Pause.*

**The second is the research-driven path.** This is when you don't have enough context to write a spec yet. The work crosses repos, or you need to understand a backend API, or you're looking at a service you didn't write. You ask Devin first — DeepWiki, multi-repo research — get a synthesis, then take that back into Copilot in your editor to actually execute.

Notice the swap: in the first flow, Copilot drafts and Devin executes. In the second, Devin researches and Copilot executes. Same tools, different roles, different task shapes.

**Honest aside on the second path:** this is also the path we fall back to when we've hit our ACU ceiling for the week. Devin does the research, Copilot does the work in the editor. Real constraint, real workflow. We don't pretend ACUs are infinite.

The point isn't that one is right. The kit grounds both. Whichever tool is generating output is reading the same standards. Use the tool that fits the task.

---

## Slide 13 — Specs are the contract

**Kicker:** Consistency mechanism
**Title:** Specs are the contract

| **Specs** — what to build | **Schemas** — what valid output looks like |
|---|---|
| Plain markdown with structured sections | JSON Schema. Machine-readable |
| Capture intent, persona, archetype, behavior | Validates every config the agent produces |

**The flow:**

```
AI reads spec → produces config → schema validates → reviewer focuses on the spec, not the JSON
```

*Schemas are how a copy-paste kit stays coherent across teams.*

**What I say** *(~90 seconds)*

This is the part that makes the rest work.

The **spec** captures intent — what we're building, why, for whom. Persona, archetype, behavior. It's a plain markdown file with structured sections. A human can read it. A reviewer can argue about it. It's the design conversation in written form.

The **schema** captures shape — what valid output looks like. JSON Schema. Machine-readable. The agent reads the spec, generates a config, and the config has to validate against the schema. If it doesn't, it doesn't ship.

What this does to review is the important part. The review conversation moves up a level. We review specs, not generated JSON. We argue about intent and behavior, not about whether someone forgot a closing brace. The schema catches the syntax problems before a human ever sees them.

That's how schemas are the consistency mechanism. Different teams can write different specs, modify their kit, adapt to their context — but the configs they produce all conform to the same shape.

---

## Slide 14 — Skills, packaged callable workflows

**Kicker:** The executable layer
**Title:** Skills — packaged, callable workflows

> *A skill encodes a workflow: "To build a grid spec, read the schema, use the archetype vocabulary, output in this format."*

1. Skills live in the kit, alongside personas and standards
2. AGENTS.md lists which skills are available and when to use them
3. An agent reads AGENTS.md, recognizes the task matches a skill, invokes it

**EXAMPLE** — Our AG Grid schema skill — knows the schema, the archetypes, the output format. Any developer can invoke it. The skill enforces the consistency.

**What I say** *(~2 minutes)*

Skills are the executable layer on top of everything we've talked about.

Without skills, every developer has to know the right prompts, the right files to load, the right output format. They have to be experts in how to use the kit. That doesn't scale.

With skills, that workflow is packaged once. Any developer can invoke it.

The AG Grid schema skill is our example. It knows our schema, knows the archetype vocabulary, and produces output in a shape that validates. A developer just says "generate a grid spec for X" and the skill handles the rest — reads the schema, picks the archetype, outputs the spec in the right format.

How does the agent know to use it? `AGENTS.md`. There's a section in `AGENTS.md` that lists available skills and when to use them. The agent reads that, recognizes the task matches a skill, and routes to it. Same pattern works for Devin and Copilot.

This is the layer that takes the kit from "powerful if you know how to use it" to "any developer on the team can call this."

---

## Slide 15 — Demo 1: Copilot drafts a spec

**Kicker:** Demo 1
**Title:** Copilot drafts a spec

**Setup:**
- **Starting point:** empty spec file in the editor
- **Prompt:** a plain-English request for a new grid widget
- **Watch for:** Copilot reads the kit, produces a schema-valid spec

*This is the AUTHORING half of the workflow.*

**What I say** *(~5 minutes total including the demo)*

Time for a live demo. This is going to be a real Copilot session — I'll talk through what's happening as it runs.

> **[GO LIVE]**
>
> Walk through:
> 1. Show the empty spec file
> 2. Show that the kit is in the repo (briefly show `AGENTS.md`)
> 3. Prompt Copilot in plain English: "Generate a spec for a new grid widget that..."
> 4. Watch it produce structured spec output
> 5. Run schema validation — show it passing
>
> If something fails on stage, fall back to walking through what should happen. Don't apologize, don't panic — keep moving. Have a screenshot or short recording ready as a hard fallback.

The takeaway from this demo: Copilot isn't generating freeform. It's reading the kit in the repo and producing something that conforms. The schema validates. That's the contract. We never wrote elaborate prompting — we just said what we wanted, and the kit did the work of grounding the output.

---

## Slide 16 — Demo 2: Devin replicates a feature across repos

**Kicker:** Demo 2
**Title:** Devin replicates a feature across repos

**Setup:**
- **Source repo:** feature already exists (style + functionality)
- **Target repo:** same feature needed; target's own kit governs the output
- **What Devin does:** reads both repos' standards before generating
- **Watch for:** it doesn't just copy — it adapts to the target's conventions

*This is the kit earning its keep — across repo boundaries.*

**What I say** *(~6–7 minutes including demo)*

This is where the kit really earns its keep. We're going to have Devin replicate a feature from one repo into another. Both repos have their own copy of the kit.

> **[GO LIVE — or show pre-staged Devin session]**
>
> Important: pre-stage this carefully. Long-running Devin sessions are the biggest risk in this talk. Consider kicking off the session 15 minutes before the talk and showing the result mid-stream rather than waiting live.
>
> Walk through:
> 1. Show source repo with the existing feature
> 2. Show target repo and its own kit
> 3. Show the Devin task that asked it to bring the feature over
> 4. Walk through Devin's output — call out where it adapted to the target's conventions vs. just copying
> 5. Show the PR

Devin isn't blindly copy-pasting. It's reading the target repo's standards and conforming the implementation to those standards. Without the kit in the target repo, this demo would produce something that looks wrong to the team that owns it — different naming, different patterns, different structure.

With the kit, the output reads like it was written by someone on the target team. That's the synergy between the kit and the agent. The agent is doing the work; the kit is making sure the work fits.

---

## Slide 17 — Where this is overkill

**Kicker:** Honest tradeoffs
**Title:** Where this is overkill

**Use the kit for:**
- Production code shipping to customers
- Reusable patterns across teams (grids, dashboards, services)
- Anything that crosses team boundaries

**Skip the kit for:**
- Throwaway scripts and one-offs
- Exploration and prototyping
- Personal productivity tasks

**Still figuring out:** persona granularity · schema versioning · when skills should hand off to other skills

**What I say** *(~90 seconds)*

This is where I want to be honest. This isn't a hammer for every nail.

For exploration, ad-hoc prompting is faster. If you're spiking a new idea, you don't need a spec. If you're writing a one-off script, you don't need archetypes. That's fine. That's where Copilot or Devin without the kit is the right answer.

For production code that ships to customers, the upfront cost of the spec pays for itself. For patterns we'll reuse across teams. For anything that crosses team boundaries — that's where consistency matters most, and that's where the kit earns its time.

We're also still figuring things out. I'm not going to pretend this is finished. Persona granularity — how fine-grained should the personas be? Schema versioning — what happens when a schema needs to change? When should one skill hand off to another skill? These are live questions.

*Pause.*

What I will say is that the cost of doing nothing is higher than the cost of getting this 80% right.

---

## Slide 18 — How a team starts using this

**Kicker:** Adoption path
**Title:** How a team starts using this

1. Copy `spec-agent/` into the repo
2. Add the platform adapters: `.github/copilot-instructions.md` and `.devin/knowledge.md`
3. Use the existing schemas and skills as-is
4. Add team-specific standards in `standards/` over time
5. Contribute back when patterns generalize

> **First three steps:** less than an hour. **Steps 4–5:** evolve with the team.

**What I say** *(~2 minutes)*

This is the practical part.

**Step 1:** copy the kit into your repo. We have scripts for this. Takes minutes.

**Step 2:** add the two platform adapter files. We'll share templates — you don't write them from scratch.

**Step 3:** use the existing schemas and skills as-is. They're already battle-tested in our work. You don't have to redesign anything to get started.

**Step 4** is where you'll invest real time — adding your team's specific standards over time. Patterns your team uses. Conventions you've evolved. This is where the kit becomes *yours*.

**Step 5** is how this gets better. When a pattern works for you, contribute it upstream. The next team that adopts the kit inherits what you've learned.

First three steps: less than an hour. Steps 4 and 5: evolve with the team over weeks and months.

---

## Slide 19 — What's next + Q&A

**Kicker:** What's next
**Title:** Where we're heading

- Demo site for visual schema authoring
- Broader skill library across UI patterns
- Adoption support for teams ready to start

# Questions?

**What I say** *(~30 seconds before opening to Q&A)*

That's where we're heading.

The **demo site for visual schema authoring** is the next big piece — it lets you build a schema in a UI before copy-pasting it into your repo. Lowers the bar for non-architects to contribute.

We're **expanding the skill library** across more UI patterns — dashboards, forms, layouts. The grid-kit work is the template.

And if your team wants to start using this — talk to me after. Happy to help you get set up.

With that — questions.

---

## Appendix: Talk structure at a glance

| Act | Section | Slides | Time |
|---|---|---|---|
| Act 1 — Why | Title through "Bridge" | 1–8 | ~13 min |
| Act 2 — What and How | Kit, adapters, workflow, demos | 9–16 | ~32 min |
| Act 3 — So what | Tradeoffs, adoption, what's next | 17–19 | ~12 min |
| | Q&A buffer | | ~3 min |

**Total target:** 60 minutes

## Appendix: Pre-talk checklist

- [ ] Fill in the third use case (Slide 7) with a real Ask Devin story
- [ ] Pre-stage the Demo 2 Devin session ~15 minutes before the talk starts
- [ ] Have screenshot/recording fallbacks ready for both demos
- [ ] Confirm the room's display resolution and font scaling
- [ ] Decide on interrupt-for-questions vs. hold-to-end and announce on Slide 3
- [ ] Test the platform adapter files render legibly from the back of the room
