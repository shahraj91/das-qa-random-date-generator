# BUG-001: End date before start date does not show error or prevent generation

| Field | Detail |
|-------|--------|
| **Bug ID** | BUG-001 |
| **Title** | End date before start date does not show error or prevent generation |
| **Severity** | Medium |
| **Priority** | High |
| **Status** | Open |
| **Reporter** | Raj Shah |
| **Date Reported** | May 15, 2026 |
| **Component** | Random Date Generator — Date Range Validation |
| **Browser** | Chrome 136.x, Firefox 138.x |
| **OS** | Windows 11, macOS Sonoma |
| **URL** | https://codebeautify.org/generate-random-date |

---

## Description

When the user sets the **end date** to a date that is **before** the start date (a reversed/invalid date range), the tool does not display any error, warning, or validation message. Instead, it proceeds to generate dates, which may fall outside the user's intended range or produce logically incorrect results.

This is a data integrity concern — a user relying on the generated dates for testing, simulation, or data population may unknowingly receive incorrect data.

---

## Steps to Reproduce

1. Navigate to https://codebeautify.org/generate-random-date.
2. Set **Start date** to `12-31-2025`.
3. Set **End date** to `01-01-2025` (a date 364 days *before* the start date).
4. Set **count** to `5`.
5. Select format `MM-DD-YYYY`.
6. Click **"Generate Random Date"**.

---

## Expected Result

The tool should:
- Display a clear validation error (e.g., *"End date must be on or after the start date."*)
- Prevent date generation until the user corrects the input
- Alternatively, auto-swap the dates and inform the user

---

## Actual Result

The tool generates 5 dates without any error or warning. The generated dates may fall outside the 01-01-2025 to 12-31-2025 range, or the tool may silently swap the dates internally without informing the user. The behavior is inconsistent and unvalidated.

---

## Impact

- **User trust:** Users performing QA testing, data seeding, or simulations may rely on these dates being within the specified range. Silent incorrect output erodes trust.
- **Use case risk:** The tool's own listed use cases include "Testing and Quality Assurance" and "Statistical Analysis" — both require reliable date boundaries.

---

## Suggested Fix

Add client-side validation on the "Generate" button click:
- Compare start date and end date values.
- If end date < start date, display an inline error message below the date fields.
- Disable generation until the range is corrected.

---

*Reported by Raj Shah | May 15, 2026*
