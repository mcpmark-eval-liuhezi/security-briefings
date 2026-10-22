# Spring4Shell

## CVE id

CVE-2022-22965

## Affected versions

Spring Framework:

- `5.3.0` through `5.3.17`
- `5.2.19.RELEASE` and earlier on the 5.2.x line
- older, unsupported lines are also affected

Crucially, the affected component is Spring's data binding, so the vulnerable artifact is `spring-webmvc` or `spring-webflux` in those ranges.

## First fixed version

`5.3.18` for the 5.3.x line, and `5.2.20.RELEASE` for the 5.2.x line.

Both fix versions are published as tags in the upstream repository: `v5.3.18` and `v5.2.20.RELEASE`.

Note for the study group: the matching Spring Boot releases that carry the fixed Spring Framework are Spring Boot `2.6.6` and `2.5.12`. If your demo projects are Spring Boot, that is the version number you will actually be bumping.

## Upstream repository

https://github.com/spring-projects/spring-framework

Canonical advisory page: https://spring.io/security/cve-2022-22965

## What to check in your own project

1. **Establish the full set of exploit prerequisites before you panic.** The specific exploit that was demonstrated requires all four of: JDK 9 or higher, Apache Tomcat as the servlet container, packaged as a WAR, and `spring-webmvc` or `spring-webflux` on the classpath. A Spring Boot executable JAR — the default — is not vulnerable to the demonstrated exploit.
2. **Do not treat "we use executable JARs" as "we are safe."** The Spring team is explicit that the vulnerability is more general than the demonstrated exploit path, and that other ways to exploit it may exist. The prerequisites tell you your immediate risk, not your obligation to patch.
3. **Check the Spring Framework version you actually resolve to, not the Spring Boot version in your `pom.xml`.** Run `mvn dependency:tree` (or the Gradle equivalent) and look at the `spring-webmvc` / `spring-webflux` version. `5.3.0` through `5.3.17` needs a bump.
4. **If you are on Spring Boot, bump to `2.6.6` or `2.5.12`** to inherit the fixed framework versions, then re-check the resolved tree to confirm the pins actually moved.
5. **If you cannot upgrade immediately, apply the documented workaround with care.** Denying fields globally via `@ControllerAdvice` setting `disallowedFields` works, but a controller that sets `disallowedFields` in its own `@InitBinder` overrides the global setting and silently reopens the hole.
6. **Note the JDK angle.** The bug is a bypass of the long-standing CVE-2010-1622 fix, made possible by sandbox-restriction methods that only exist on JDK 9+. Your JDK version is part of your risk profile, not just your Spring version.

## Sources

- Spring security advisory page: https://spring.io/security/cve-2022-22965
- Spring Framework RCE early announcement: https://spring.io/blog/2022/03/31/spring-framework-rce-early-announcement
- GitHub Advisory Database (GHSA-36p3-wjmg-h94x): https://github.com/advisories/GHSA-36p3-wjmg-h94x
- NVD entry: https://nvd.nist.gov/vuln/detail/CVE-2022-22965
- CISA alert: https://www.cisa.gov/news-events/alerts/2022/04/01/spring-releases-security-updates-addressing-spring4shell-and-spring-cloud-function-vulnerabilities
- Upstream tags verified: `v5.3.18`, `v5.2.20.RELEASE` at https://github.com/spring-projects/spring-framework/tags
