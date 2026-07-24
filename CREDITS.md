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

### mattpocock/skills — engineering agent skills

- **Author:** Matt Pocock ([@mattpocock](https://github.com/mattpocock))
- **Repository:** [github.com/mattpocock/skills](https://github.com/mattpocock/skills)
- **First credited in release:** v3.5.0 (2026-07-24)
- **Type of attribution:** competitive reference, not academic citation

**What we adopted from this project (with adaptation):**

1. **User-invoked vs model-invoked distinction** (`.agents/invocation.md`).
   We adopted this with a twist: `model-invoked-anunciado` (announced) instead of
   silent, because VibeDev's leigo audience needs to see what the framework is doing.
   Documented in [`.agents/adr/0006-model-invoked-anunciado.md`](./.agents/adr/0006-model-invoked-anunciado.md).

2. **Router skill pattern** (`ask-matt`).
   We adopted this as `/vd-help` (`vibedev/commands/vd-help.md`).
   Different from `ask-matt`: we mapped the 11 VibeDev commands plus emotional
   situations (lost, exhausted, anxious), and added a 1-line "if you only remember
   ONE command" summary.

3. **`.out-of-scope/` discipline** (3+ files explaining what's NOT supported).
   We adopted this as `4pixeltechBR/VibeDev/.out-of-scope/` with 7 files
   documenting what VibeDev does NOT do and why. Each file follows the same
   pattern: what we do, what we don't, why, what to use instead.

4. **`.agents/adr/` structure** (dated, contextualized, immutable decisions).
   We adopted this as `4pixeltechBR/VibeDev/.agents/adr/` with 6 ADRs covering
   our past decisions (monolithic vs composed, country mode, layman mode, etc.)
   and one new ADR about the model-invoked-anunciado pattern itself.

**What we explicitly did NOT adopt:**

- **Plugin marketplace** (`.claude-plugin/plugin.json`) — VibeDev is
  multi-harness (Claude Code, Cursor, Codex, OpenCode, Antigravity). A Claude-only
  plugin would be lock-in. See `.out-of-scope/not-pretending-to-be-vibeshield.md`
  for related governance thinking.

- **`npx skills@latest add`** — VibeDev skills are meant to be **edited by the user**
  (especially in `modo_usuario: leigo` where the user customizes the framework).
  Read-only installation via npx would conflict with that.

- **Bucketed skill organization** (engineering/, productivity/, misc/, personal/,
  in-progress/, deprecated/) — VibeDev is 1 SKILL.md monolithic by design.
  See [`.agents/adr/0001-skill-monolitica.md`](./.agents/adr/0001-skill-monolitica.md).

- **22 small composable skills** — VibeDev's value is unified state + continuous
  flow for a single leigo. Splitting into 22 skills would make the framework
  opaque to the audience we serve.

**Why we cite this project:**

Pocock's work represents a credible **counter-position** to VibeDev. He explicitly
argues against "vibe coding" frameworks. We are not in ideological agreement,
but his engineering hygiene (ADRs, out-of-scope, invocation contracts) is
empirically useful. Adopting structural patterns from a project with different
philosophy is how frameworks mature. We name him because transparency beats
silence.

---

## Tooling & infrastructure

### Mavis (MiniMax Agent)
The agent harness used to author and maintain VibeDev itself. VibeDev's full lifecycle was developed inside Mavis sessions, which gave direct empirical feedback on every framework addition.

### Changesets (planned v3.6.0)
Adoption of [Changesets](https://github.com/changesets/changesets) for version management. Inspired by mattpocock/skills using the same tool. **Not yet active** — planned for next release.

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
