# Test Mode Rules

## Purpose

This mode is specialized for executing JUnit tests and generating JaCoCo coverage reports in the webMethods Deployer project.

## Project-Specific Configuration

### Build System
- **Gradle multi-project build** with root at `main/` (NOT project root)
- Custom `buildDir=staging` means outputs go to `main/staging/`
- All Gradle commands MUST be run from `main/` directory

### Test Framework
- JUnit 5 (5.8.2) with Mockito 5.19.0
- Java 21 source/target compatibility
- Test results: `main/staging/test-results/test/`
- JaCoCo reports: `main/staging/reports/jacoco/test/`

### Critical Dependencies
These MUST be on test classpath:
- **jakarta.mail** - Package class static initializer requires XMLCoder
- **g11n-utils** - Service class requires it

## Standard Workflow

### 1. Navigate to main/ Directory
```bash
cd main
```

### 2. Execute Tests
```bash
# Run all tests
./gradlew test

# Run specific test class
./gradlew test --tests "com.wm.deployer.SpecificTest"

# Run tests matching pattern
./gradlew test --tests "*ConnectionTest"

# Clean and test
./gradlew clean test

# Verbose output
./gradlew test --info
```

### 3. Generate Coverage Report
```bash
# Coverage is auto-generated with tests, or explicitly:
./gradlew test jacocoTestReport
```

### 4. Analyze Results

**Test Results Location:** `main/staging/test-results/test/`
- XML files with detailed results
- Check for failures, errors, skipped tests

**JaCoCo Coverage Location:** `main/staging/reports/jacoco/test/`
- `index.html` - Main coverage report
- Shows line, branch, method coverage
- Drill down to see uncovered lines

### 5. Present Summary

Format:
```
✅ Test Execution Complete

📊 Results:
- Tests Run: X
- Passed: X
- Failed: X
- Skipped: X
- Duration: X seconds

📈 Coverage:
- Line Coverage: X%
- Branch Coverage: X%
- Method Coverage: X%

📁 Reports:
- Test Results: main/staging/test-results/test/
- Coverage Report: main/staging/reports/jacoco/test/index.html

[Failures and recommendations if applicable]
```

## Common Commands

```bash
# Full test with coverage
cd main && ./gradlew test jacocoTestReport

# Test specific module
cd main && ./gradlew :module-name:test

# Open coverage report (Windows)
cd main && ./gradlew test jacocoTestReport && start staging/reports/jacoco/test/index.html

# Test with stack traces
cd main && ./gradlew test --stacktrace

# Parallel execution
cd main && ./gradlew test --parallel

# Build without tests
cd main && ./gradlew build -x test
```

## Troubleshooting

### Test Failures
1. Check console output for stack traces
2. Read XML files in `staging/test-results/test/TEST-*.xml`
3. Verify jakarta.mail and g11n-utils on classpath
4. Re-run to check for flaky tests
5. Compare with last successful run

### Build Issues
1. Clean: `./gradlew clean`
2. Verify Java 21 is active
3. Check Gradle wrapper is executable
4. Compile tests first: `./gradlew compileTestJava`

### Coverage Issues
1. Ensure tests ran successfully
2. Check for excluded classes in build.gradle
3. Verify JaCoCo plugin is configured

## Coverage Analysis

### Targets
- Overall: >70% line coverage, >60% branch coverage
- Critical paths: >80% coverage
- Focus on: error handling, edge cases, complex logic

### Recommendations
When coverage is low:
1. Identify uncovered classes in HTML report
2. Prioritize critical business logic
3. Suggest specific test cases for uncovered paths
4. Note any intentionally excluded code

## Key Rules

1. **ALWAYS** run from `main/` directory
2. **NEVER** assume success - check results
3. **ALWAYS** provide report paths
4. **PARSE** test failures and provide clear summaries
5. **RECOMMEND** specific improvements
6. **REMEMBER** SPM tests are disabled by default
7. **USE** `--info` or `--debug` for troubleshooting

## Integration with Other Modes

- **Code mode**: Switch to Code mode to fix failing tests
- **Plan mode**: Switch to Plan mode to design test strategy
- **Ask mode**: Switch to Ask mode for test-related questions

## Notes

- Test results consolidated at project level (not per-module)
- PMD rules allow 150-line methods (non-standard)
- Vector/Hashtable allowed (legacy code)
- Ant build coexists for CLI tools (separate from Gradle tests)