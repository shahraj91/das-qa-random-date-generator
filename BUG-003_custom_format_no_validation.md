# BUG-003: Custom date format field accepts invalid tokens without warning

| Field | Detail |
|-------|--------|
| **Bug ID** | BUG-003 |
| **Title** | Custom date format field accepts invalid tokens without warning |
| **Severity** | Low |
| **Priority** | Low |
| **Status** | Open |
| **Reporter** | Raj Shah |
| **Date Reported** | May 15, 2026 |
| **Component** | Random Date Generator — Custom Format |
| **Browser** | Chrome 136.x, Firefox 138.x |
| **OS** | Windows 11 |
| **URL** | https://codebeautify.org/generate-random-date |

---

## Description

When the user selects "Custom date format" and enters a format string containing invalid or unsupported tokens (e.g., `ZZZZ-QQ-XX`), the tool does not validate the input. It generates output where invalid tokens are rendered as literal text, without any warning or indication that the format string contains errors. The supported tokens are listed as a hint (`Use: YYYY YY MM month mon DD d hh h mm m ss s`) but there is no enforcement.

---

## Steps to Reproduce

1. Navigate to https://codebeautify.org/generate-random-date.
2. Select **"Custom date format"** from the format dropdown.
3. In the custom format field, enter `ZZZZ-QQ-XX`.
4. Set count to 3.
5. Click **"Generate Random Date"**.

---

## Expected Result

One of the following:
- The tool validates the format string and displays an error (e.g., *"Unrecognized tokens: ZZZZ, QQ, XX. Supported tokens: YYYY, MM, DD, hh, mm, ss."*)
- Invalid tokens are highlighted in red in the input field
- The generate button is disabled until a valid format is entered

---

## Actual Result

The tool generates 3 lines of output containing `ZZZZ-QQ-XX` as literal text (no date substitution for the invalid tokens). No error, warning, or visual indication is shown. A user unfamiliar with the supported tokens may not realize their format is incorrect.

---

## Impact

- **Low severity** — the tool does list supported tokens as a hint, and technically the output is not "wrong" (literals are expected for non-token characters like separators).
- However, the lack of validation means a user could submit an entirely invalid format and receive output that looks like dates but contains no actual date data, which could cause downstream issues in testing or data seeding.

---

## Suggested Fix

- Validate the format string on generate: if no recognized tokens are present, display a warning.
- Alternatively, add a "Preview" showing what the format would produce (e.g., `ZZZZ-QQ-XX → ZZZZ-QQ-XX (no date tokens found)`).
- Consider making the supported tokens list more prominent or adding a tooltip.

---

*Reported by Raj Shah | May 15, 2026*
