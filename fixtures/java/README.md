# Java Fixtures

This directory contains JVM dependency fixtures for manifest, lockfile, and vendored repository metadata across Gradle, Maven, and legacy Ivy descriptors.

## Included Fixtures

| Fixture | Files Under Test | What It Exercises |
| --- | --- | --- |
| `gradle/` | `build.gradle` | dependency extraction from Gradle string coordinates across multiple configurations |
| `gradle-catalog/` | `build.gradle`, `settings.gradle`, `gradle/libs.versions.toml` | Gradle version catalog parsing from alias-based dependency declarations |
| `gradle-lockfile/` | `build.gradle`, `gradle.lockfile` | Gradle dependency locking with a checked-in lock state |
| `ivy/` | `ivy.xml` | Apache Ivy dependency extraction from `org`/`name`/`rev` XML attributes |
| `maven/` | `pom.xml` | dependency extraction from Maven XML dependency blocks |
| `maven-repo/` | `pom.xml`, `repository/**/*.pom` | comparison of declared Maven coordinates against vendored repository POM metadata |

## Expected Detection Behavior

- `gradle/` expects:
  - `type: "manifest"`
  - `org.springframework.boot:spring-boot-starter-web` `2.5.4`
  - `org.junit.jupiter:junit-jupiter` `5.7.2`
  - `com.h2database:h2` `1.4.200`
- `gradle-catalog/` expects:
  - `type: "manifest"`
  - `org.apache.logging.log4j:log4j-core` `2.14.1`
  - `org.junit.jupiter:junit-jupiter` `5.7.2`
- `gradle-lockfile/` expects:
  - `type: "locked"`
  - `org.apache.logging.log4j:log4j-core` `2.14.1`
  - `org.apache.logging.log4j:log4j-api` `2.14.1`
- `ivy/` expects:
  - `type: "manifest"`
  - `org.apache.logging.log4j:log4j-core` `2.14.1`
  - `com.google.guava:guava` `30.1-jre`
- `maven/` expects:
  - `type: "manifest"`
  - `org.apache.logging.log4j:log4j-core` `2.14.1`
  - `com.google.guava:guava` `30.1-jre`
- `maven-repo/` expects:
  - `org.apache.logging.log4j:log4j-core` `2.14.1` as both `manifest` and `installed`
  - `org.apache.logging.log4j:log4j-api` `2.14.1` as `installed`

The expected ecosystem label is `Maven`.

## Notes

- The Gradle sample includes `implementation`, `testImplementation`, and `runtimeOnly` declarations to verify scanners do not depend on a single configuration name.
- `gradle-catalog/` covers the `gradle/libs.versions.toml` convention, where dependency identity and versions are declared in a version catalog and referenced indirectly through `libs.*` aliases in `build.gradle`.
- `gradle-catalog/` intentionally includes the historically vulnerable `org.apache.logging.log4j:log4j-core` `2.14.1` so scanners must resolve a catalog alias to a vulnerable coordinate rather than reading a literal `group:name:version` string.
- `gradle-lockfile/` uses Gradle dependency locking and a checked-in `gradle.lockfile` to verify locked dependency extraction rather than declared coordinates alone.
- `gradle-lockfile/` intentionally uses the historically vulnerable `org.apache.logging.log4j:log4j-core` `2.14.1`, which is affected by Log4Shell-era advisories, and includes the transitive `log4j-api` lock entry.
- `ivy/` covers the older Apache Ivy descriptor format, where dependency identity comes from `org`, `name`, and `rev` attributes rather than Maven `<groupId>` and Gradle string coordinates.
- `ivy/` intentionally keeps `org.apache.logging.log4j:log4j-core` `2.14.1` as a recognizable vulnerable coordinate and adds a second dependency under a different Ivy configuration so scanners cannot assume a single configuration name.
- `maven-repo/` uses the standard Maven repository directory layout and vendored `*.pom` metadata so scanners can validate installed or cached Java package discovery without parsing JAR contents.
- `maven-repo/` intentionally keeps `org.apache.logging.log4j:log4j-core` `2.14.1` as both a declared and vendored package while adding vendored-only `org.apache.logging.log4j:log4j-api` `2.14.1` to test source attribution.
- Maven package names in `expected.json` are normalized as `groupId:artifactId`.
