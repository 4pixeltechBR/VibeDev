# VibeDev is not a CI/CD tool

VibeDev governs the project lifecycle from idea to launch. It does not run your builds, tests, or deployments in continuous integration.

## What VibeDev does
- Helps you think through what to build (phases 1-2)
- Helps you choose stack and cost (phase 3)
- Walks you through building with safety checks (phases 4-6)
- Validates before launch (phases 7-8)

## What VibeDev does NOT do
- Run `npm test` on every commit
- Deploy to Vercel/Railway/AWS
- Manage secrets in production
- Monitor uptime

## For those things, use
- **GitHub Actions / GitLab CI / CircleCI** for CI
- **Vercel / Netlify / Railway / Render** for deploy
- **Sentry / Datadog / UptimeRobot** for monitoring

These are **complementary** to VibeDev, not competitors. A VibeDev-managed project typically uses all of them.

## Why this is out of scope

CI/CD is its own ecosystem with its own tooling, conventions, and 10+ years of accumulated knowledge. Building CI/CD features into VibeDev would:
- Double the maintenance surface
- Compete poorly with dedicated tools that do this much better
- Confuse users about what VibeDev is responsible for

If a feature feels like it belongs in CI/CD, it doesn't belong in VibeDev.
