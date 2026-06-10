# Autonomous Quality Guardian - Quick Reference

## 🚀 Quick Start

```bash
guardian WDY-123        # Run full autonomous workflow
auto-fix WDY-123        # Same as guardian
quality-fix WDY-123     # Same as guardian
```

## 📋 Commands

| Command | Description |
|---------|-------------|
| `guardian WDY-123` | Start autonomous quality fix for ticket |
| `auto-fix WDY-123` | Alias for guardian |
| `quality-fix WDY-123` | Alias for guardian |
| `continue guardian` | Resume from current state |
| `guardian status` | Show current progress |

## 🎯 What It Does

1. **Fetches** Jira ticket via MCP
2. **Analyzes** code quality issues
3. **Plans** implementation strategy
4. **Refactors** code to fix violations
5. **Generates** comprehensive tests
6. **Validates** through autonomous iteration
7. **Reports** detailed metrics

## 🔄 Autonomous Loop

```
Run Tests → Pass? → Check Coverage → OK? → Check PMD → Clean? → Done!
    ↑         ↓           ↑           ↓         ↑         ↓
    └─── Fix Code ────────┴─── Add Tests ──────┴─── Fix Violations
```

## ✅ Quality Gates

- **Line Coverage**: ≥ 90%
- **Branch Coverage**: ≥ 80%
- **PMD Violations**: 0
- **Test Pass Rate**: 100%

## 📊 Typical Results

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Coverage | 65% | 92% | +27% |
| PMD Violations | 5 | 0 | -100% |
| Tests | 12 | 24 | +12 |
| Time | 2 days | 10 min | 99.7% faster |

## 🛠️ What Gets Fixed

### Code Smells
- Deep nesting (> 5 levels)
- Long methods (> 150 lines)
- Duplicated code
- Complex conditionals

### Test Coverage
- Missing unit tests
- Uncovered edge cases
- Missing error handling tests

### PMD Violations
- All standard PMD rules
- Project-specific rules

## 📁 Output Locations

- **Test Results**: `main/staging/test-results/test/`
- **Coverage Report**: `main/staging/reports/jacoco/test/index.html`
- **Build Output**: `main/staging/`

## 🔒 Safety Features

- Maximum 5 iterations
- 5-minute timeout per iteration
- 30-minute total timeout
- User confirmation for plan
- Guidance requests when stuck

## 💡 Tips

### For Best Results

1. **Clear Jira tickets** with specific requirements
2. **Realistic targets** (80-95% coverage)
3. **Review results** before committing
4. **Incremental approach** for large tickets

### Ticket Description Template

```markdown
Current Issues:
- [Specific code smell or violation]
- [Test coverage gap]
- [PMD violation details]

Acceptance Criteria:
- Test coverage > 90%
- All PMD violations resolved
- Edge cases covered
```

## 🚨 Troubleshooting

| Issue | Solution |
|-------|----------|
| Guardian doesn't start | Check Jira MCP server configured |
| Iterations don't improve | Review coverage target, check uncovered lines |
| Build failures | Verify Gradle environment, Java 21 |
| Tests fail repeatedly | Guardian asks for help after 2 attempts |

## 📈 Workflow Phases

1. **Fetch & Analyze** (2 min)
   - Get Jira ticket
   - Analyze codebase
   - Create plan

2. **Implement Fix** (3 min)
   - Refactor code
   - Fix violations
   - Update TODO

3. **Generate Tests** (3 min)
   - Create test cases
   - Cover edge cases
   - Follow patterns

4. **Validate & Iterate** (2-5 min)
   - Run tests
   - Check coverage
   - Self-correct
   - Repeat until clean

5. **Generate Report** (1 min)
   - Collect metrics
   - Create summary
   - Present results

## 🎯 Success Criteria

✅ All tests passing  
✅ Coverage meets target  
✅ Zero PMD violations  
✅ Build successful  
✅ Code follows conventions  
✅ Comprehensive report generated  

## 📞 Need Help?

1. Check full guide: `AUTONOMOUS_QUALITY_GUARDIAN_GUIDE.md`
2. Review rule file: `.bob/rules-advanced/AUTONOMOUS_QUALITY_GUARDIAN.md`
3. Ask Bob in Ask mode

## 🎬 Example Session

```
User: guardian WDY-6708

Bob: 📋 Fetching WDY-6708...
     Summary: Remove Runtime Base Deployment
     
     Analyzing codebase...
     - Found 3 files to refactor
     - Identified 5 test gaps
     - Detected 2 PMD violations
     
     Created 6-phase plan. Proceed? (yes/no)

User: yes

Bob: [x] Phase 1: Analysis complete
     [-] Phase 2: Refactoring code...
     [x] Phase 2: Refactoring complete
     [-] Phase 3: Generating tests...
     [x] Phase 3: 12 tests generated
     [-] Phase 4: Validating...
     
     Iteration 1: Coverage 78% → Adding tests
     Iteration 2: Coverage 91% ✅ → Checking PMD
     Iteration 3: Fixed 2 violations ✅
     
     [x] Phase 4: All quality gates passed!
     [-] Phase 5: Generating report...
     
     ✅ Quality Guardian Complete!
     
     Coverage: 65% → 91% (+26%)
     PMD: 2 → 0 violations
     Tests: 12 → 24 (+12)
     Time: 10 minutes
     
     Ready for pull request!
```

---

**Start your autonomous quality fix now:**

```
guardian WDY-YOUR-TICKET
```

🤖 Let Bob handle the heavy lifting!