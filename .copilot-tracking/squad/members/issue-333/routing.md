---
description: "Squad routing: request patterns mapped to roles, autonomy tiers, and parallel eligibility"
---

# Squad Routing

| Pattern / Keyword                          | Role(s)                      | Autonomy Tier | Parallel-Eligible |
|--------------------------------------------|------------------------------|---------------|-------------------|
| research, investigate, explore, find out   | researcher                   | auto          | yes               |
| plan, break down, sequence, design plan    | lead                         | confirm       | no                |
| implement, build, code, fix                | developer                    | confirm       | no                |
| review, validate, check quality            | tester                       | auto          | yes               |
| challenge, pressure-test, poke holes, devil's advocate, what could go wrong | challenger | auto | yes         |
| author prompt, write agent file, refactor instructions, analyse skill | prompt-engineer | confirm | no         |
| security, threat, vulnerability, STRIDE    | security                     | confirm       | yes               |
| document, write up, summarize for stakeholders, readme | technical-writer           | confirm       | no                |
| architecture, system design, components    | architect                    | auto          | yes               |
| observability, instrumentation, telemetry design, spans, traces, metrics, structured logging, OpenTelemetry, what should we emit | observability | confirm | yes |
| author IaC, write Bicep, write Terraform, convert LLD to infra, infrastructure as code | iac-author | confirm | no |
| deploy, provision, what-if, terraform plan, terraform apply, az deployment | deployer | confirm | no |
| as-built, resource inventory, compliance matrix, operations runbook, DR plan, document deployed infrastructure | asbuilt-author | confirm | no |
| diagnose, troubleshoot, resource health, why is resource failing, investigate deployed, policy check, incident, outage, sev1, sev2, on-call, postmortem, root cause | azure-diagnose | auto | yes |
| validate, cross-check, pre-implementation review, council, design review, go/no-go, implement-and-cost, implement-and-risk | architect, security, cost-manager, product-owner, rai (optional) | confirm | yes |
