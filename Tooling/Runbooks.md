# Runbooks

This document gives engineers quick, repeatable steps to set up, operate, and troubleshoot the system. It's essential for onboarding new developers and helping everyone respond to incidents consistently.

## Getting Your Local Environment Set Up

Before you start, make sure you have:
- Git, Docker, and your runtime (Node, .NET, Java, etc.)
- Package manager (npm/yarn/pnpm for JavaScript, NuGet/Maven for .NET, etc.)

Then follow these steps:

1. Clone the repository.
2. Copy `.env.example` to `.env.local` and fill in the values. Never commit `.env.local`—it's ignored by git and stays on your machine.
3. Run `docker compose up` from the `infra/local` folder to start the database, cache, and message broker.
4. Install dependencies with `npm install` or the equivalent for your language.
5. Start the dev server: `npm run dev` for Node, `dotnet run` for .NET, etc.
6. Check that everything is working: `curl http://localhost:3000/health` or open the app in your browser.

## Commands You'll Use Regularly

```bash
# Run tests
npm test           # JavaScript
dotnet test        # .NET

# Lint and format
npm run lint       # JavaScript
npm run format     # JavaScript

# Infrastructure
terraform plan     # See what will change
terraform apply    # Apply the changes
```

## Troubleshooting

**Database won't connect**: Check that `.env.local` has the right credentials. Run `docker compose ps` to see if the database container is running. Make sure no other service is using the same port.

**Tests are flaky**: Look at timeouts in the test output. Try disabling parallel test execution temporarily. Check that external service mocks are set up correctly.

**The build pipeline is failing**: Read the lint and test logs carefully. Rerun the failed stage. If infrastructure planning fails, run `terraform fmt` and `terraform validate` locally first.

**Docker Compose won't start**: Check if those ports are already in use (especially 5432 for Postgres). Prune stopped containers with `docker compose down -v`. If you're running out of memory, increase Docker's memory limit in preferences.

## Quick Checks for On-Call Support

When you're on call and something goes wrong:

1. **Check dashboards**: Look at latency, error rate, and saturation. Compare to your Service Level Objectives (SLOs).
2. **See what was deployed**: Check the last deployment. If an SLO breach started right after a deploy, that's probably the culprit.
3. **Inspect queues**: If the message queue is backing up, try scaling workers temporarily if it's safe to do.
4. **Rollback**: If the issue correlates with a recent deploy, roll back to the last healthy version.

## Project-Specific Overrides
- Commands: npm run dev (React), dotnet run (Blazor/server), docker compose up for Postgres/MongoDB/Redis.
- Dashboards: per-cloud (Azure Monitor/App Insights; AWS CloudWatch) for latency/error/RPS.
- Escalation: payments issues → check gateway limits, PSP status, DB health (Postgres); catalog issues → MongoDB health/cache.
