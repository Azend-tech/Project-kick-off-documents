# Runbooks

Purpose: Give engineers a concise, repeatable way to set up, operate, and troubleshoot the system. Define abbreviations and expectations so a new developer can start quickly.

## Abbreviation quick ref
- DB: Database
- RPS: Requests per second
- RTO/RPO: Recovery Time Objective / Recovery Point Objective
- SLO: Service Level Objective
- RUM: Real User Monitoring

## Local setup (example)
- Prereqs: git, Docker, runtime (Node/.NET/Java), package manager (npm/yarn/pnpm, NuGet/Maven/Gradle).
- Steps:
  1) Clone repo.
  2) Copy .env.example → .env.local and fill secrets (never commit .env.local).
  3) docker compose up (db/cache/message broker) in the infra/local folder.
  4) npm install (or equivalent); npm run dev (or dotnet run/mvn spring-boot:run).
  5) Verify health: curl http://localhost:3000/health (or appropriate service port) or open app in browser.

## Common scripts
- Tests: npm test / dotnet test / mvn test
- Lint: npm run lint
- Format: npm run format
- Infra dry run: terraform plan (in infra directory)

## Troubleshooting (examples)
- DB connection fails: confirm .env.local values, check docker compose status, ensure no port conflicts.
- Tests flaky: review timeouts, disable test parallelism temporarily, inspect external dependency mocks.
- Pipeline fail: read lint/test logs; rerun failed stage; if infra plan fails, re-run terraform fmt/validate.
- Local compose not starting: check ports (netstat/lsof equivalents), prune stopped containers, increase Docker memory if OOM.

## On-call quick checks (if applicable)
- Dashboards: latency, error rate, saturation; compare to SLOs.
- Deploys: check last deployment; roll back if SLO breach continues.
- Queues: inspect backlog; temporarily scale workers if safe.

## Project-Specific Overrides
- Commands: npm run dev (React), dotnet run (Blazor/server), docker compose up for Postgres/MongoDB/Redis.
- Dashboards: per-cloud (Azure Monitor/App Insights; AWS CloudWatch) for latency/error/RPS.
- Escalation: payments issues → check gateway limits, PSP status, DB health (Postgres); catalog issues → MongoDB health/cache.
