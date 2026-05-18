# QA Testing — CodeBeautify Random Date Generator

## Overview

This repository contains QA testing deliverables for the [CodeBeautify Random Date Generator](https://codebeautify.org/generate-random-date), completed as a QA/Test Engineer assessment for Digital Aid Seattle.

**Tester:** Raj Shah  
**Date:** May 15, 2026  
**Application Under Test:** https://codebeautify.org/generate-random-date  
**Scope:** Random Date Generator functionality only

## Repository Structure

```
├── README.md                          # This file
├── .gitignore                         # Ignores reports/, screenshots/, OS files
├── test-reports/
│   └── TEST_REPORT.md                 # Full test report with summary, environment, results
├── bug-reports/
│   ├── BUG-001_end_date_before_start_date.md
│   ├── BUG-002_no_max_limit_on_date_count.md
│   ├── BUG-003_custom_format_no_validation.md
│   ├── BUG-004_custom_format_token_collision.md
│   └── FEATURE-001_custom_format_preview.md
├── test-cases/
│   └── TEST_CASES.md                  # Full test case suite with steps and expected results
└── screenshots/
    └── (placeholder for evidence)
```

## Summary of Findings

| Category | Count |
|----------|-------|
| Total Test Cases | 37 |
| Passed | 27 |
| Failed | 5 |
| Blocked | 0 |
| Not Executed | 5 |
| Bugs Filed | 4 |
| Feature Improvements | 1 |

**Highest Severity Defect:** BUG-004 — Custom format parser corrupts month names by re-parsing resolved values against shorter tokens (e.g., "September" → "Septe24ber"). This is a data corruption bug.

**Review Note:** 3 test cases (TC-035, TC-036, TC-037) were added during Staff QA review and are pending execution.

## How to Navigate

1. **Start here →** [Test Report](test-reports/TEST_REPORT.md) — executive summary, approach, environment, and aggregated results.
2. **Detailed test cases →** [Test Cases](test-cases/TEST_CASES.md) — full test suite with steps, expected vs actual results, and pass/fail status.
3. **Bug reports →** [Bug Reports](bug-reports/) — individual defect reports with reproduction steps, severity, and expected behavior.
4. **Feature improvement →** [FEATURE-001](bug-reports/FEATURE-001_custom_format_preview.md) — live preview for custom format input.

## Tools & Environment

- **Browser:** Google Chrome 136.x (latest stable), Firefox 138.x
- **OS:** Windows 11 / macOS
- **Testing Type:** Manual — functional, boundary, negative, usability
- **Tracking:** GitHub Issues (simulated via markdown bug reports)

## Assumptions

- Testing is limited to the Random Date Generator tool only; no other CodeBeautify tools are in scope.
- The tool runs entirely client-side (JavaScript in the browser); no API or backend testing was performed.
- Default browser settings were used (no extensions, standard viewport).
- "Reasonable user behavior" was assumed for baseline tests; edge cases were tested separately.

---

*Completed by Raj Shah | shah.raj1010@gmail.com | [LinkedIn](https://linkedin.com/in/rajshah1010) | [GitHub](https://github.com/shahraj91)*
