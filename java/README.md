# Java Fixtures

This directory contains JVM dependency fixtures for the two common Java build systems represented in this repository.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `gradle/` | `build.gradle` | dependency extraction from Gradle string coordinates across multiple configurations |
| `maven/` | `pom.xml` | dependency extraction from Maven XML dependency blocks |

## Expected Detection Behavior

Both fixtures use `type: "manifest"` in `expected.json`.

- `gradle/` expects:
  - `org.springframework.boot:spring-boot-starter-web` `2.5.4`
  - `org.junit.jupiter:junit-jupiter` `5.7.2`
  - `com.h2database:h2` `1.4.200`
- `maven/` expects:
  - `org.apache.logging.log4j:log4j-core` `2.14.1`
  - `com.google.guava:guava` `30.1-jre`

The expected ecosystem label is `Maven`.

## Notes

- The Gradle sample includes `implementation`, `testImplementation`, and `runtimeOnly` declarations to verify scanners do not depend on a single configuration name.
- Maven package names in `expected.json` are normalized as `groupId:artifactId`.
- There are no lockfile-style Java fixtures in this directory yet.
