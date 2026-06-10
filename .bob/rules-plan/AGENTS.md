# Plan Mode Rules

## Architecture Overview

- **Multi-project Gradle build** with root at `main/` (not project root)
- Custom build directory: `main/staging/` instead of standard `main/build/`
- Hybrid build system: Gradle for main build, Ant for CLI tools in `main/tools/`
- Java 21 target (non-standard for legacy webMethods codebase)

## Build System Constraints

- All Gradle commands must run from `main/` directory
- Project naming uses custom `projectNameConverter()` transformation in settings.gradle
- Test results consolidated at root level: `main/staging/test-results/`
- Ant builds require `${env.shared.dir}` and `${IS_DIR}` environment variables

## Module Dependencies

- Uses `latest.milestone` for internal webMethods dependencies (IS, TN, MWS, Broker, etc.)
- Custom BAS (Build Automation System) plugins from webMethods
- External dependencies managed via `main/settings/dependencies.gradle`
- Obfuscated encryption keys in `main/common/obfkeys/` (seayek.dat, sedyek.dat, camyek.dat)

## Testing Architecture

- JUnit 5 (5.8.2) with Mockito 5.19.0 on Java 21
- **Critical constraint**: jakarta.mail required on test classpath (Package class static initializer dependency)
- **Critical constraint**: g11n-utils required for Service class in tests
- JaCoCo coverage reports at `main/staging/reports/jacoco/test/`
- SPM integration tests disabled by default in `main/tests/build.gradle`

## Code Quality

- PMD ruleset at `main/pmd_ruleset.xml` with legacy code accommodations
- Method length limit: 150 lines (increased from default 100)
- Nested if depth: 5 levels (increased from default 3)
- Vector/Hashtable allowed (migration rules excluded for legacy code)

## Deployment Artifacts

- CLI.jar and projectautomator.jar built with Ant, copied to `${IS_DIR}/packages/WmDeployer/lib/`
- Docker build from project root uses Gradle installation task
- Ant build excludes `ParseDeployerParamsOld.java` from compilation