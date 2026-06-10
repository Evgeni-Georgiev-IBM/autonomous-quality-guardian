# Ask Mode Rules

## Project Structure Context

- **Gradle root is `main/`**, not project root - all build commands run from there
- Build outputs in `main/staging/` due to custom `buildDir=staging` property
- Project uses custom name transformation via `projectNameConverter()` in settings.gradle

## Module Organization

- `packages/` contains Integration Server packages (is-pkg-wmdeployer)
- `modules/` contains core modules
- `tests/` contains integration tests (SPM tests disabled by default)
- `tools/` contains CLI and automation tools using Ant (not Gradle)
- `plugin/` contains WSDL plugin definitions
- `installations/` contains product installation configurations

## Build System Duality

- Main build uses Gradle with custom BAS (Build Automation System) plugins
- CLI and DeployerAutomationTool in `main/tools/` use Ant build files
- Ant builds require `${env.shared.dir}` environment variable
- Built jars copied to `${IS_DIR}/packages/WmDeployer/lib/`

## Testing Context

- JUnit 5 (5.8.2) with Mockito 5.19.0 on Java 21
- Test results consolidated at `main/staging/test-results/` (not per-module)
- JaCoCo coverage reports at `main/staging/reports/jacoco/test/`
- **Critical dependencies**: jakarta.mail and g11n-utils required on test classpath

## Dependencies

- Uses `latest.milestone` for internal webMethods dependencies (IS, TN, MWS, etc.)
- External dependencies managed via `main/settings/dependencies.gradle`
- Java 21 source/target compatibility (non-standard for legacy codebase)