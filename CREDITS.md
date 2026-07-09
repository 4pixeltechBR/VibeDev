# Credits & Acknowledgements

> Formal recognition of people, papers, projects, and communities that
> materially influenced VibeDev's design and roadmap.
>
> Recognition here is **deliberate, dated, and traceable to specific
> releases**. Nothing here is generic flattery — every entry names
> the specific influence.

---

## How to add an entry

1. Open a PR modifying this file.
2. Add the entry under the matching section below.
3. Include:
   - The name / handle (with link)
   - The work being credited (paper / post / talk / code)
   - The VibeDev release where the credit was added
   - Short description of how it influenced VibeDev
4. Maintainers review and merge.

---

## Papers & academic work

### Sandeco Macedo — *Stop Hand-Holding Your Coding Agent* (arXiv:2607.00038)

- **Author:** Sandeco Macedo ([@sandeco](https://github.com/sandeco))
- **Affiliation:** Instituto Federal de Goiás
- **Title:** *Engineering the Loops that Replace Step-by-Step Prompting*
- **Year:** 2026 (preprint, June 28)
- **DOI / arXiv ID:** [arXiv:2607.00038](https://arxiv.org/abs/2607.00038)
- **First credited in release:** v3.1.0 (2026-07-08)
- **Reaffirmed in releases:** v3.2.0, v3.4.0, v3.4.1

**How it influenced VibeDev:**

1. **5-level verification ladder** (deterministic → rule → field truth → model-as-judge → human checkpoint) mirrors the validation protocol in `/vd-check` almost beat-for-beat.

2. **Anti-patterns section** — especially *specification gaming*, *while-true around a stranger*, and *self-approving loop* — directly informed:
   - **Anti-Feature-Creep Guard** (`vibedev/references/anti-creep.md`) — Layer 1 spec-gaming defense
   - **Validação Anti-"tá"** in `/vd-check` (v3.4.0) — same epistemic rigor Macedo called for at the agent-evaluation layer

3. **Distinction prompt → context → harness → loop** helped the VibeDev team decide to **not** import Loop Engineering wholesale. VibeDev governs full project lifecycle; loops are a specific operation within Phase 6 (Construction) or Phase 8 (Operation). The reasoning is documented in `mapeamento-loop-engineering-vibedev.md`.

4. **Epistemic discipline:** the paper's refusal to "retire prompt engineering" in favor of loops anchored VibeDev's position that prompt, context, harness, and loop are **distinct tools with distinct uses** — not a sequence of replacement.

**Ongoing:** the framework explicitly invites Macedo to review, comment, or accept any contribution path he finds useful. Recognition remains permanent in release notes regardless.

---

## Open-source projects we learn from

_Not yet populated. Entries will be added when specific projects measurably influenced a release._

---

## Tooling & infrastructure

### Mavis (MiniMax Agent)
The agent harness used to author and maintain VibeDev itself. VibeDev's full lifecycle was developed inside Mavis sessions, which gave direct empirical feedback on every framework addition.

---

## Community contributors

_This section grows organically. Open a PR to add your contribution after your first merged change._

---

## Rejected attributions (transparency)

VibeDev records attributions carefully. The following categories are NOT credited in this file:

- Generic "AI made it possible" framing — too vague to be honest
- Tools that were tried and not adopted — different from "influenced"
- People who built similar projects without citation linkage — inspiration is not lineage

If you think an attribution is missing or wrong, open an issue. Discussion welcome.
