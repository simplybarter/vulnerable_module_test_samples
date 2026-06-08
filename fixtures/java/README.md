# Java Fixtures

This directory contains JVM dependency fixtures for the two common Java build systems represented in this repository.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `gradle/` | `build.gradle` | dependency extraction from Gradle string coordinates across multiple configurations |
| `gradle-lockfile/` | `build.gradle`, `gradle.lockfile` | Gradle dependency locking with a checked-in lock state |
| `maven/` | `pom.xml` | dependency extraction from Maven XML dependency blocks |

## Expected Detection Behavior

- `gradle/` expects:
  - `type: "manifest"`
  - `org.springframework.boot:spring-boot-starter-web` `2.5.4`
  - `org.junit.jupiter:junit-jupiter` `5.7.2`
  - `com.h2database:h2` `1.4.200`
- `gradle-lockfile/` expects:
  - `type: "locked"`
  - `org.apache.logging.log4j:log4j-core` `2.14.1`
  - `org.apache.logging.log4j:log4j-api` `2.14.1`
- `maven/` expects:
  - `type: "manifest"`
  - `org.apache.logging.log4j:log4j-core` `2.14.1`
  - `com.google.guava:guava` `30.1-jre`

The expected ecosystem label is `Maven`.

## Notes

- The Gradle sample includes `implementation`, `testImplementation`, and `runtimeOnly` declarations to verify scanners do not depend on a single configuration name.
- `gradle-lockfile/` uses Gradle dependency locking and a checked-in `gradle.lockfile` to verify locked dependency extraction rather than declared coordinates alone.
- `gradle-lockfile/` intentionally uses the historically vulnerable `org.apache.logging.log4j:log4j-core` `2.14.1`, which is affected by Log4Shell-era advisories, and includes the transitive `log4j-api` lock entry.
- Maven package names in `expected.json` are normalized as `groupId:artifactId`.
