# Security Policy

## Reporting a vulnerability

Do not report security vulnerabilities through public GitHub issues, discussions or pull requests.

Use one of these private channels instead:

1. GitHub private vulnerability reporting (preferred for our public repositories): open the repository's
   Security and quality tab (shield icon) and click Report a vulnerability. This creates a private draft
   advisory visible only to you and the maintainers. See GitHub's guide:
   [Privately reporting a security vulnerability](https://docs.github.com/en/code-security/how-tos/report-and-fix-vulnerabilities/report-a-vulnerability/privately-reporting-a-security-vulnerability).
2. Email [security@untern.ai](mailto:security@untern.ai) for anything else, including our hosted services at
   `untern.ai`, or if the button above is not available.

Include as much of this as you can:

- the affected product, service, repository or URL, and version or commit;
- the type of issue (for example: injection, authentication bypass, data exposure, prompt injection leading to data
  exfiltration);
- step-by-step instructions to reproduce, a proof of concept, and the impact you believe it has;
- whether you have shared the details with anyone else.

## What to expect

| Stage | Target |
|---|---|
| Acknowledgement of your report | within 2 business days |
| Initial assessment and severity (CVSS) | within 5 business days |
| Status updates while we work on it | at least every 7 days |
| Fix or mitigation: critical | within 7 days of confirmation |
| Fix or mitigation: high | within 30 days of confirmation |
| Fix or mitigation: medium / low | within 90 days of confirmation |

These are targets, not guarantees; we will tell you if we expect to miss one. Once a fix is available we will
coordinate disclosure with you, publish an advisory where appropriate, and credit you unless you prefer to remain
anonymous.

## Scope

In scope: repositories in the [`unternai`](https://github.com/unternai) GitHub organization and services hosted
under `untern.ai`.

Out of scope: findings from automated scanners without a demonstrated impact, denial-of-service and volumetric
attacks, social engineering of Untern staff, physical attacks, and vulnerabilities in third-party services that we do
not control (report those to the vendor).

## Safe harbor

We will not pursue or support legal action against anyone who, in good faith, makes an effort to follow this policy:
avoids privacy violations, data destruction and service disruption; only accesses the minimum data needed to
demonstrate the issue; and gives us reasonable time to fix it before any public disclosure.

## Supported versions

Unless a repository states otherwise in its own `SECURITY.md`, only the latest release (or the default branch, for
projects without releases) receives security fixes.
