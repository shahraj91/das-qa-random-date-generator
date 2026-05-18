# Test Cases — CodeBeautify Random Date Generator

**Application:** https://codebeautify.org/generate-random-date  
**Tester:** Raj Shah  
**Date:** May 15, 2026

---

## Smoke / Sanity Tests

### TC-001: Page loads successfully
| Field | Detail |
|-------|--------|
| **Precondition** | Browser open, internet connected |
| **Steps** | 1. Navigate to https://codebeautify.org/generate-random-date |
| **Expected Result** | Page loads completely. "Generate Random Date" button, format dropdown, count input, start date, and end date fields are all visible and interactive. |
| **Status** | ✅ PASS |

### TC-002: Generate date with all default settings
| Field | Detail |
|-------|--------|
| **Precondition** | Page loaded, no fields modified |
| **Steps** | 1. Click "Generate Random Date" button |
| **Expected Result** | One date is generated in MM-DD-YYYY format and displayed in the output area. |
| **Status** | ✅ PASS |

### TC-003: Output area displays generated dates
| Field | Detail |
|-------|--------|
| **Precondition** | Page loaded |
| **Steps** | 1. Set count to 5. 2. Click "Generate Random Date". |
| **Expected Result** | Five dates appear in the output area, each on a separate line. |
| **Status** | ✅ PASS |

---

## Functional Tests — Output Formats

### TC-004: Format — MM-DD-YYYY
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "MM-DD-YYYY" format. 2. Generate 3 dates. |
| **Expected Result** | All dates follow MM-DD-YYYY pattern (e.g., 05-15-2026). Month and day are zero-padded. |
| **Status** | ✅ PASS |

### TC-005: Format — YYYY-MM-DD hh:mm:ss
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "YYYY-MM-DD hh:mm:ss" format. 2. Generate 3 dates. |
| **Expected Result** | All dates follow YYYY-MM-DD hh:mm:ss pattern with valid time values (hours 00-23, minutes/seconds 00-59). |
| **Status** | ✅ PASS |

### TC-006: Format — YYYY-DD-MM hh:mm:ss
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "YYYY-DD-MM hh:mm:ss" format. 2. Generate 3 dates. |
| **Expected Result** | All dates follow YYYY-DD-MM hh:mm:ss pattern. Day and month positions are swapped from TC-005. |
| **Status** | ✅ PASS |

### TC-007: Format — MM-DD-YYYY hh:mm:ss
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "MM-DD-YYYY hh:mm:ss" format. 2. Generate 3 dates. |
| **Expected Result** | Output follows MM-DD-YYYY hh:mm:ss pattern. |
| **Status** | ✅ PASS |

### TC-008: Format — ISO 8601
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "ISO 8601" format. 2. Generate 3 dates. |
| **Expected Result** | Output follows ISO 8601 format (e.g., 2026-05-15T14:30:00Z or similar with timezone offset). |
| **Status** | ✅ PASS |

### TC-009: Format — Year Month Date hh:mm:ss
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "Year Month Date hh:mm:ss". 2. Generate 3 dates. |
| **Expected Result** | Output includes full month name (e.g., "2026 May 15 14:30:00"). |
| **Status** | ✅ PASS |

### TC-010: Format — Month Date Year hh:mm:ss
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "Month Date Year hh:mm:ss". 2. Generate 3 dates. |
| **Expected Result** | Output follows "May 15 2026 14:30:00" pattern with full month name. |
| **Status** | ✅ PASS |

### TC-011: Format — Custom date format with valid tokens
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "Custom date format". 2. Enter `DD/MM/YYYY` in the custom format field. 3. Generate 3 dates. |
| **Expected Result** | Output follows the custom DD/MM/YYYY pattern (e.g., 15/05/2026). |
| **Status** | ✅ PASS |
| **Notes** | Valid tokens work correctly. See TC-029 for invalid token behavior and TC-033 for token collision with `month`. |

---

## Functional Tests — Date Range

### TC-012: Generate dates within a valid date range
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to 01-01-2025. 2. Set end date to 12-31-2025. 3. Generate 10 dates. |
| **Expected Result** | All 10 generated dates fall between Jan 1, 2025 and Dec 31, 2025 inclusive. |
| **Status** | ✅ PASS |

### TC-013: Start date and end date are the same
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to 06-15-2025. 2. Set end date to 06-15-2025. 3. Generate 5 dates. |
| **Expected Result** | All 5 generated dates are 06-15-2025. |
| **Status** | ✅ PASS |

### TC-014: End date is before start date (reversed range)
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to 12-31-2025. 2. Set end date to 01-01-2025. 3. Generate 5 dates. |
| **Expected Result** | Tool should display an error message (e.g., "End date must be after start date") and not generate dates. |
| **Actual Result** | Tool generates dates without any error. Generated dates may fall outside the logical range or produce inconsistent results. |
| **Status** | ❌ FAIL |
| **Bug Reference** | [BUG-001](../bug-reports/BUG-001_end_date_before_start_date.md) |

### TC-015: Only start date provided (end date empty)
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to 01-01-2025. 2. Leave end date empty. 3. Generate 5 dates. |
| **Expected Result** | Tool generates dates starting from 01-01-2025 through some reasonable default end date, or displays a message indicating the default behavior. |
| **Status** | ✅ PASS |
| **Notes** | Dates generated are all after the start date. Default end range is undocumented. |

### TC-016: Only end date provided (start date empty)
| Field | Detail |
|-------|--------|
| **Steps** | 1. Leave start date empty. 2. Set end date to 12-31-2025. 3. Generate 5 dates. |
| **Expected Result** | Tool generates dates up to 12-31-2025, or displays a message about defaults. |
| **Status** | ✅ PASS |
| **Notes** | Dates generated are all before the end date. Default start range is undocumented. |

### TC-017: Very wide date range (year 1900 to year 2100)
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to `01-01-1900`. 2. Set end date to `12-31-2100`. 3. Set format to `MM-DD-YYYY`. 4. Generate 10 dates. 5. Verify all dates fall within range and format is correct. 6. Repeat with format `YYYY-MM-DD hh:mm:ss`. 7. Repeat with format `month DD, YYYY`. |
| **Expected Result** | All 10 dates fall between 1900 and 2100. Format is consistent regardless of the year. Zero-padding is preserved for all date components. Month names (in named formats) render correctly for dates across the full range. |
| **Actual Result** | With numeric formats (`MM-DD-YYYY`, `YYYY-MM-DD hh:mm:ss`), dates in the early 1900s occasionally produced inconsistent zero-padding or unexpected separators. With the `month` token in custom format, the token collision bug (BUG-004) corrupts month names containing characters matching short tokens (`m`, `s`, `h`, `d`) — e.g., "Dece9ber", "Augu4t". This corruption is present at any date range but is tested here because wide ranges increase the probability of hitting affected months. |
| **Status** | ❌ FAIL |
| **Bug Reference** | Related: [BUG-004](../bug-reports/BUG-004_custom_format_token_collision.md) |
| **Notes** | The numeric format inconsistencies need further isolation — they may be browser-specific date rendering for pre-epoch dates (before 1970). The month name corruption is a confirmed parser bug independent of date range. |

---

## Functional Tests — Count Input

### TC-018: Generate exactly 1 date
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set count to 1. 2. Click generate. |
| **Expected Result** | Exactly 1 date in the output. |
| **Status** | ✅ PASS |

### TC-019: Generate 10 dates
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set count to 10. 2. Click generate. |
| **Expected Result** | Exactly 10 dates displayed, each on a new line. |
| **Status** | ✅ PASS |

### TC-020: Generate 50 dates
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set count to 50. 2. Click generate. |
| **Expected Result** | Exactly 50 dates displayed. Output area is scrollable if needed. |
| **Status** | ✅ PASS |

### TC-021: Generate 0 dates
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set count to 0. 2. Click generate. |
| **Expected Result** | No dates generated. Ideally, a message like "Please enter a count of at least 1" or empty output. |
| **Status** | ✅ PASS |
| **Notes** | No dates generated, no error shown. Acceptable behavior. |

### TC-022: Generate extremely large count (99999)
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set count to 99999. 2. Click generate. |
| **Expected Result** | Tool should either generate the dates within a reasonable time or cap the input with a user-facing message. |
| **Actual Result** | Browser becomes unresponsive or extremely slow. No upper limit warning. |
| **Status** | ❌ FAIL |
| **Bug Reference** | [BUG-002](../bug-reports/BUG-002_no_max_limit_on_date_count.md) |

---

## Boundary / Edge Cases

### TC-023: Leap year date — Feb 29
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to 02-28-2024. 2. Set end date to 03-01-2024. 3. Generate 20 dates in MM-DD-YYYY format. |
| **Expected Result** | Some generated dates should include 02-29-2024 (2024 is a leap year). All dates should be valid. |
| **Status** | ✅ PASS |

### TC-024: Year boundary — Dec 31 to Jan 1
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to 12-30-2025. 2. Set end date to 01-02-2026. 3. Generate 20 dates. |
| **Expected Result** | Generated dates include dates from both years. No off-by-one errors at the year boundary. |
| **Status** | ✅ PASS |

### TC-025: Far-future date — year 9999
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to 01-01-9999. 2. Set end date to 12-31-9999. 3. Generate 5 dates. |
| **Expected Result** | 5 dates in the year 9999. Format is preserved. |
| **Status** | ✅ PASS |

### TC-026: Historical date — year 0001
| Field | Detail |
|-------|--------|
| **Steps** | 1. Set start date to 01-01-0001. 2. Set end date to 12-31-0001. 3. Generate 5 dates. |
| **Expected Result** | 5 dates in the year 0001, or a clear message that the year is out of range. |
| **Status** | ✅ PASS |
| **Notes** | Dates generated. Year display may vary by format. |

### TC-027: Negative count value
| Field | Detail |
|-------|--------|
| **Steps** | 1. Attempt to enter -5 in the count field. |
| **Expected Result** | Input field rejects negative value, or tool shows error. |
| **Status** | 🔲 NOT EXECUTED |
| **Notes** | HTML input type restricts negative entry via UI. Could be tested by manipulating DOM directly, considered out of scope for manual testing. |

---

## Negative Tests

### TC-028: Special characters in count field
| Field | Detail |
|-------|--------|
| **Steps** | 1. Attempt to enter "abc!@#" in the count field. 2. Click generate. |
| **Expected Result** | Input field rejects non-numeric characters or tool shows validation error. |
| **Status** | ✅ PASS |
| **Notes** | HTML input type="number" prevents non-numeric entry. |

### TC-029: Custom format with invalid tokens
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "Custom date format". 2. Enter "ZZZZ-QQ-XX" in the format field. 3. Generate dates. |
| **Expected Result** | Tool should either show an error for unrecognized tokens or output the invalid tokens as literal text with a clear indication that they are not date components. |
| **Actual Result** | Tool generates output with the invalid tokens rendered as literals without any warning or indication that the format is invalid. User may not realize their format is wrong. |
| **Status** | ❌ FAIL |
| **Bug Reference** | [BUG-003](../bug-reports/BUG-003_custom_format_no_validation.md) |

### TC-030: Empty custom format field
| Field | Detail |
|-------|--------|
| **Steps** | 1. Select "Custom date format". 2. Clear the custom format field completely (empty string). 3. Set count to 5. 4. Click "Generate Random Date". |
| **Expected Result** | Tool should either display a validation error (e.g., "Please enter a format string"), use a sensible default format, or generate empty lines with no date content. |
| **Actual Result** | The tool generates the requested number of empty lines (or blank entries). No error or warning is displayed. The output area shows 5 blank lines — functionally useless output with no indication that the format was empty. |
| **Status** | ✅ PASS (marginal) |
| **Notes** | Technically the tool doesn't crash or error, and an empty format producing empty output is logically consistent. However, from a UX perspective, this is a missed opportunity to guide the user. Downgraded from "not executed" — behavior confirmed. A warning or fallback to a default format would be a better experience. Related to [FEATURE-001](../bug-reports/FEATURE-001_custom_format_preview.md). |

### TC-033: Custom format with `month` token — token collision corruption
| Field | Detail |
|-------|--------|
| **Precondition** | Page loaded, "Custom date format" selected |
| **Steps** | 1. Select "Custom date format". 2. Enter `month` in the custom format field. 3. Set count to `10`. 4. Click "Generate Random Date". 5. Inspect all 10 output lines for correctness. |
| **Expected Result** | 10 full English month names, each correctly spelled (e.g., "January", "September", "December"). No numeric substitutions or garbled characters. |
| **Actual Result** | Month names that contain characters matching short format tokens (`m`, `s`, `h`, `d`) are corrupted. The parser resolves `month` to the month name first, then performs additional token replacement passes on the resolved text. Observed examples: "Dece**9**ber" (`m` → minutes), "Septe**24**ber" (`m` → minutes), "Septe**56**ber" (`m` → minutes), "Augu**4**t" (`s` → seconds). Months without colliding characters (April, January, February, June, July, October) render correctly. |
| **Status** | ❌ FAIL |
| **Bug Reference** | [BUG-004](../bug-reports/BUG-004_custom_format_token_collision.md) |

### TC-034: Custom format — arbitrary text is printed as literal
| Field | Detail |
|-------|--------|
| **Precondition** | Page loaded, "Custom date format" selected |
| **Steps** | 1. Select "Custom date format". 2. Enter a single character `t` in the custom format field. 3. Set count to 3. 4. Click "Generate Random Date". |
| **Expected Result** | Since `t` is not a recognized token, it should be printed as the literal character "t" for each generated date. |
| **Actual Result** | The output shows `t` printed 3 times (one per line). This confirms that unrecognized characters in the custom format are treated as literals and passed through to output. |
| **Status** | ✅ PASS |
| **Notes** | This is consistent behavior — non-token characters are treated as literals (separators, labels, etc.). This is expected and acceptable. However, when combined with the token collision bug (BUG-004), this behavior means that characters within resolved token values (like month names) can be unintentionally matched and replaced by subsequent passes. |

---

## Usability Tests

### TC-031: Copy to clipboard and download functionality
| Field | Detail |
|-------|--------|
| **Steps** | 1. Generate 5 dates. 2. Click the "Copy to Clipboard" button. 3. Paste into a text editor and verify all 5 dates are present. 4. Click the "Download" button. 5. Open the downloaded .txt file and verify all 5 dates match the output. |
| **Expected Result** | Copy button copies all generated dates to the system clipboard. Download button saves a .txt file containing all generated dates. Content matches the output area exactly. |
| **Status** | ✅ PASS |

### TC-032: Mobile responsive layout
| Field | Detail |
|-------|--------|
| **Steps** | 1. Open page in Chrome DevTools mobile emulation (iPhone 14 Pro, 393x852). 2. Set parameters and generate dates. |
| **Expected Result** | All input fields, buttons, and output area are usable without horizontal scrolling. |
| **Status** | 🔲 NOT EXECUTED |
| **Notes** | Partial observation — layout adapts but output area usability on mobile was inconsistent. Requires dedicated mobile testing pass. |

---

*Test cases authored by Raj Shah | May 15, 2026*
