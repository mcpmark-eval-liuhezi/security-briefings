# Log4Shell

## CVE id

CVE-2021-44228

## Affected versions

Apache Log4j 2 `log4j-core`, broken down by Java version line as documented by the project:

- `2.0-beta9` up to (but not including) `2.3.1` — Java 6 line
- `2.4.0` up to (but not including) `2.12.2` — Java 7 line
- `2.13.0` up to (but not including) `2.15.0` — Java 8 and later

Put more simply: every `log4j-core` 2.x release up to and including `2.14.1` is affected. Only the `log4j-core` JAR is affected — applications depending on `log4j-api` alone, without `log4j-core`, are not.

## First fixed version

`2.15.0` for the Java 8 and later line. The Java 7 and Java 6 lines were fixed separately, in `2.12.2` and `2.3.1` respectively.

All three fix versions are published as tags in the upstream repository: `rel/2.15.0`, `rel/2.12.2`, and `rel/2.3.1`.

Note for the study group: `2.15.0` is the first version that fixed CVE-2021-44228 itself, but that fix was incomplete in certain non-default configurations. Follow-up CVE-2021-45046 was fixed in `2.16.0`, and Apache's standing recommendation is to be on `2.17.1` or later. `2.15.0` is the correct answer to "first fixed"; it is not the version you should be running today.

## Upstream repository

https://github.com/apache/logging-log4j2

Canonical project page: https://logging.apache.org/

## What to check in your own project

1. **Find every copy of `log4j-core`, not just the direct dependency.** It rides in transitively far more often than it is declared directly. Check with `mvn dependency:tree -Dincludes=org.apache.logging.log4j:log4j-core` (or the Gradle equivalent), and also grep your shaded/uber JARs and `lib/` directories — the JAR can be bundled inside another artifact.
2. **Confirm you have the `log4j-core` JAR at all.** If you only pull in `log4j-api`, or you are on Log4j 1.x without a `JMSAppender` in the configuration, your exposure is different and lower. Log4j 1.x is end-of-life and has its own CVE (CVE-2021-4104).
3. **Compare the version you found against the ranges above.** Anything in `2.0-beta9` through `2.14.1` needs attention.
4. **Do not stop at `2.15.0`.** If you are upgrading anyway, go to a current Log4j 2 release (`2.17.1`+) so you also pick up the fixes for CVE-2021-45046 and CVE-2021-45105.
5. **Check whether you actually log attacker-controlled input.** The attack requires attacker-influenced strings to reach a logging call. User agents, request parameters, and exception messages containing user data are the usual vectors — but assume any logged user input counts.
6. **Verify transitive bumps landed everywhere.** If you manage versions with `dependencyManagement` or a BOM, a stale pin can silently keep an old `log4j-core` on the classpath after you think you upgraded. Re-run the dependency tree after the change.

## Sources

- Apache Logging Services security page: https://logging.apache.org/security.html
- Apache Software Foundation blog on the Log4j CVEs: https://news.apache.org/foundation/entry/apache-log4j-cves
- GitHub Advisory Database (GHSA-jfh8-c2jp-5v3q): https://github.com/advisories/ghsa-jfh8-c2jp-5v3q
- NVD entry: https://nvd.nist.gov/vuln/detail/CVE-2021-44228
- Upstream tags verified: `rel/2.15.0`, `rel/2.12.2`, `rel/2.3.1` at https://github.com/apache/logging-log4j2/tags
