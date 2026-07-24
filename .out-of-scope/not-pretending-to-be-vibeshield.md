# VibeDev does not pretend to be VibeShield

VibeDev's `/vd-check` validates that a feature **works as intended**. It does not validate that the feature is **secure**.

## Different concerns

| Concern | Handled by |
|---|---|
| "Did the user actually see X happen?" | VibeDev (`/vd-check`) |
| "Is the code vulnerable to X attack?" | VibeShield (C1-C8 categories) |
| "Is the cost realistic?" | VibeDev (Cost Guard) |
| "Is the data properly encrypted?" | VibeShield |
| "Does the feature match the spec?" | VibeDev |
| "Is the API safe from abuse?" | VibeShield |

## What this means in practice

When you complete a sub-task in VibeDev:
1. `/vd-check` confirms it works (user observed the result)
2. VibeShield (if installed and triggered) confirms it's safe (no vulnerabilities introduced)

Both should pass before you commit. VibeDev will not skip VibeShield's role.

## Why this is out of scope

VibeDev's author/team is not positioned to provide security expertise. VibeShield exists as a separate skill precisely so that security knowledge can be contributed, versioned, and updated independently of lifecycle governance.

If `/vd-check` tried to also be a security audit, it would either:
- Be shallow (and give false confidence)
- Be deep (and require VibeShield's expertise anyway)

We chose the honest path: two skills, two concerns, two releases when one needs updating.
