# VibeDev does not give "use X framework" recommendations

VibeDev presents **options with trade-offs** for Type 1 decisions (per `/vd-plan`). It does not say "VibeDev recommends Next.js".

## What VibeDev does
- Lists 3 options for stack, database, auth
- Explains pros and cons of each
- Applies Red Team to the recommended option
- Lets you choose or delegate (in leigo mode with confident default)

## What VibeDev does NOT do
- Maintain a "VibeDev-approved stack list"
- Endorse specific vendors
- Update "best practices" as frameworks evolve
- Pick winners in framework wars

## Why this is out of scope

VibeDev's value is **disciplined decision-making**, not **prescriptive recommendations**. The day VibeDev says "use Next.js, not SvelteKit" is the day VibeDev becomes a religious text instead of a framework.

If you want curated stack advice, that's what `references/stack-guide.md` provides: neutral trade-off explanations, not endorsements.

## What about the Confident Default Decision (v3.4.0+)?

In `modo_usuario: leigo`, the framework offers a **default** when the user can't evaluate. The default comes from the option with best Red Team result in **this specific context**, not from a global ranking. Same decision could pick differently in another context.
