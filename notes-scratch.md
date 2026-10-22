# Scratch notes — DRAFT, do not merge

Working notes while verifying the version facts. Not part of the deliverable.

## Log4Shell (CVE-2021-44228)

- Affected: log4j-core [2.0-beta9, 2.3.1) / [2.4, 2.12.2) / [2.13.0, 2.15.0)
- First fixed: 2.15.0 (Java 8+), 2.12.2 (Java 7), 2.3.1 (Java 6)
- Follow-up CVEs: CVE-2021-45046 (fixed 2.16.0), CVE-2021-45105 (fixed 2.17.0)
- Upstream: apache/logging-log4j2
- Tag check: rel/2.15.0, rel/2.12.2, rel/2.3.1 all exist upstream ✓

## Spring4Shell (CVE-2022-22965)

- Affected: 5.3.0–5.3.17; 5.2.19.RELEASE and earlier; older unsupported lines
- First fixed: 5.3.18 and 5.2.20.RELEASE
- Matching Spring Boot: 2.6.6 / 2.5.12
- Upstream: spring-projects/spring-framework
- Tag check: v5.3.18, v5.2.20.RELEASE both exist upstream ✓

## Open questions

- Verify GHSA ids properly (jfh8-c2jp-5v3q / 36p3-wjmg-h94x) — tracked in follow-up issue
- Pick the third incident to add later
