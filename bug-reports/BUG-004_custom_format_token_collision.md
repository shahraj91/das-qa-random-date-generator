# BUG-004: Custom format parser corrupts output — token replacement collides with resolved values

| Field | Detail |
|-------|--------|
| **Bug ID** | BUG-004 |
| **Title** | Custom format parser corrupts output — token replacement collides with already-resolved month names |
| **Severity** | High |
| **Priority** | High |
| **Status** | Open |
| **Reporter** | Raj Shah |
| **Date Reported** | May 15, 2026 |
| **Component** | Random Date Generator — Custom Format Parser |
| **Browser** | Chrome 136.x, Firefox 138.x |
| **OS** | Windows 11, macOS Sonoma |
| **URL** | https://codebeautify.org/generate-random-date |

---

## Description

When using the custom date format with the `month` token (which resolves to the full month name), the parser corrupts the output by performing a **second pass of token replacement on the already-resolved month name**. Characters within the month name that happen to match shorter tokens (`m`, `h`, `s`, `mm`, etc.) are replaced with their corresponding numeric values, producing garbled output.

This is a **parsing order / escaping bug** in the custom format engine. The parser does not protect already-resolved values from subsequent token replacement passes.

---

## Steps to Reproduce

1. Navigate to https://codebeautify.org/generate-random-date.
2. Select **"Custom date format"** from the format dropdown.
3. In the custom format field, enter exactly: `month`
4. Set **count** to `10`.
5. Click **"Generate Random Date"**.

---

## Expected Result

10 full month names, one per line. For example:

```
April
December
May
September
August
January
October
February
June
March
```

---

## Actual Result

Month names are generated but **corrupted with numeric substitutions** where characters in the month name collide with shorter format tokens. Observed output:

```
April
Dece9ber
May
April
October
Septe24ber
Septe56ber
Augu4t
January
January
```

### Analysis of the corruption pattern

The supported tokens include single-character tokens: `m` (minutes), `h` (hours), `s` (seconds), `d` (day).

What appears to be happening:

| Month Name | Corrupted Output | Explanation |
|------------|-----------------|-------------|
| December | Dece**9**ber | The `m` in "December" is matched by the `m` (minutes) token and replaced with the minute value (e.g., `9`). The `b`, `e`, `r` are literals. |
| September | Septe**24**ber | The `m` in "September" is replaced with a two-digit minute value (`24`). Or `mb` is being parsed as `m` + literal. |
| September | Septe**56**ber | Same pattern, different random minute value (`56`). |
| August | Augu**4**t | The `s` in "August" is matched by the `s` (seconds) token and replaced with seconds value (`4`). The `t` remains as literal. |
| April | April | No collision — none of the characters `a`, `p`, `r`, `i`, `l` match any single-char tokens. Renders correctly. |
| January | January | No collision — `j`, `a`, `n`, `u`, `a`, `r`, `y` don't match. Renders correctly. |
| May | May | No collision — `a`, `y` don't match tokens. The `M` is uppercase and the parser likely matches lowercase `m` only. |
| October | October | No collision — no matching single-char tokens in "October". |

### Root Cause Hypothesis

The custom format parser likely performs **sequential string replacement** — first replacing longer tokens (e.g., `month`, `YYYY`), then shorter tokens (e.g., `mm`, `m`, `hh`, `h`, `ss`, `s`). The problem is that after the `month` token is resolved to (for example) "September", the subsequent replacement pass sees the `m` in "September" and replaces it with the minutes value, and the `s` and gets replaced with seconds.

The parser is not escaping or protecting already-resolved output from further token matching.

---

## Affected Tokens

Based on the supported token list (`YYYY YY MM month mon DD d hh h mm m ss s`), the following month names contain characters that collide with single-character tokens:

| Month | Colliding Characters | Affected Tokens |
|-------|---------------------|-----------------|
| March | m, h | `m` (minutes), `h` (hours) |
| August | s | `s` (seconds) |
| September | s, m | `s` (seconds), `m` (minutes) |
| November | m | `m` (minutes) |
| December | m, d | `m` (minutes), `d` (day) |

Months without collisions (April, January, February, June, July, October) render correctly.

**Note:** May contains a lowercase `m` but was observed rendering correctly in testing. This suggests the parser may be matching `m` only when it is not the first character, or the `M` in "May" is uppercase and the token match is case-sensitive for the leading character. This needs further investigation to confirm whether May is reliably safe or intermittently affected.

**Note:** The `mon` token (abbreviated month name) likely has the same issue — "Mar", "Sep", "Aug", "Nov", "Dec" contain colliding characters. The `DD` and `MM` tokens (uppercase) may not collide if the parser is case-sensitive, but this needs verification.

---

## Impact

- **Severity: High** — The output is **data corruption**, not a cosmetic issue. A user relying on month names for test data, report generation, or data seeding will get garbled values that could propagate into downstream systems.
- **Frequency:** Affects every generation that uses the `month` or likely `mon` token. The corruption is non-deterministic (depends on the random time component), so it won't always appear for the same month — making it harder for users to detect.
- **User trust:** A user who sees "Septe24ber" in their output will question the reliability of all output from the tool.

---

## Suggested Fix

The parser should use one of the following strategies to prevent token collision with resolved values:

1. **Placeholder approach:** Replace recognized tokens with unique placeholders first (e.g., `{{MONTH}}`), then resolve all placeholders to their final values in a single pass at the end. This prevents any resolved value from being re-parsed.

2. **Longest-match-first with output escaping:** Process tokens from longest to shortest, and escape the resolved output so it cannot be matched by subsequent shorter tokens.

3. **Single-pass regex:** Use a single regex that matches all tokens simultaneously and replaces them in one pass, avoiding any re-processing of output.

---

## Additional Test Scenarios to Verify Fix

- Custom format `month` — all 12 months should render correctly
- Custom format `mon` — all 12 abbreviated months should render correctly
- Custom format `month DD, YYYY hh:mm:ss` — compound format with month name + time tokens
- Custom format `DD month YYYY` — month name surrounded by other tokens
- Custom format `month-month` — repeated token (edge case)

---

*Reported by Raj Shah | May 15, 2026*
