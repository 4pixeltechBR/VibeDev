# VibeShield does not replace a DPO or lawyer

VibeShield flags code that touches data protection categories (C1, C3, C5). It does not provide legal advice.

## What VibeShield does
- Detects code that processes personal data
- Flags missing consent flows
- Identifies credential handling patterns
- Returns verdicts in technical terms

## What VibeShield does NOT do
- Replace a Data Protection Officer (DPO/Encarregado)
- Provide legal interpretation of LGPD / GDPR / PIPL / 152-ФЗ
- Draft privacy policies
- Conduct DPIA (Data Protection Impact Assessment)
- Make jurisdictional decisions

## For legal compliance, use
- **DPO / Encarregado de Dados** for the organization
- **Lawyer specialized in digital law** for jurisdiction-specific advice
- **Official regulator guidance** (ANPD in Brazil, CNIL in France, CAC in China, etc.)
- **Compliance consultants** for cross-border operations

## Why this is out of scope

VibeShield is a **code audit tool**, not a legal compliance tool. The output of VibeShield (C1-C8 verdicts) is a **signal** to consult counsel, not a substitute for counsel.

The `.out-of-scope/not-pretending-to-be-vibeshield.md` file in the parent project makes the same point: VibeDev does not pretend to be VibeShield; VibeShield does not pretend to be a DPO.
