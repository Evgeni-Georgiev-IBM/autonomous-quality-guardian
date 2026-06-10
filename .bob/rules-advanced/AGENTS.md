# Advanced Mode Rules

## Build System Quirks

- **Always run Gradle from `main/` directory**, not project root
- Custom `buildDir=staging` means outputs go to `main/staging/`, not `main/build/`
- Project naming uses custom converter: `projectNameConverter()` in settings.gradle transforms directory names

## Testing Requirements

- **jakarta.mail MUST be on test classpath** - Package class static initializer requires XMLCoder which needs jakarta.mail
- **g11n-utils MUST be on test classpath** - Service class requires it
- Test results consolidated at `main/staging/test-results/` (not per-module)
- JaCoCo reports at `main/staging/reports/jacoco/test/`

## Ant Build Coexistence

- CLI and DeployerAutomationTool use Ant (not Gradle) in `main/tools/`
- Ant build excludes `ParseDeployerParamsOld.java` from compilation (see build.xml line 61)
- Built jars copied to `${IS_DIR}/packages/WmDeployer/lib/` after compilation
- Requires `${env.shared.dir}` environment variable for shared libraries

## Encryption Keys

- Obfuscated keys in `main/common/obfkeys/` (seayek.dat, sedyek.dat, camyek.dat)
- DeployerAutomationTool copies these to build output during compilation

## Code Style Exceptions

- Method length: 150 lines allowed (PMD rule modified from default 100)
- Nested if depth: 5 levels allowed (PMD rule modified from default 3)
- Vector/Hashtable allowed (legacy code, migration rules excluded)