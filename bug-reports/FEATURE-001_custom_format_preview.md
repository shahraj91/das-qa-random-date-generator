# FEATURE-001: Add live preview for custom date format

| Field | Detail |
|-------|--------|
| **Feature ID** | FEATURE-001 |
| **Title** | Add live preview for custom date format before generation |
| **Type** | Feature Improvement |
| **Priority** | Medium |
| **Status** | Proposed |
| **Reporter** | Raj Shah |
| **Date Reported** | May 15, 2026 |
| **Component** | Random Date Generator — Custom Format |
| **URL** | https://codebeautify.org/generate-random-date |

---

## Problem Statement

When a user selects "Custom date format" and enters a format string, there is no feedback until they click "Generate." If the format is incorrect, contains unsupported tokens, or produces unexpected output, the user only discovers this after generation. This creates a trial-and-error loop — especially problematic because the tool has 13 supported tokens (`YYYY YY MM month mon DD d hh h mm m ss s`) and no visual distinction between tokens and literal characters in the input field.

This gap is compounded by BUG-004 (token collision in resolved month names), where the output can appear correct at first glance but contain corrupted data.

---

## Current Behavior

1. User selects "Custom date format."
2. A text input appears with a hint: `Use: YYYY YY MM month mon DD d hh h mm m ss s`
3. User types a format string (e.g., `DD/MM/YYYY hh:mm:ss`).
4. No feedback until "Generate" is clicked.
5. If the format is wrong, the user sees unexpected output and has to guess what went wrong.

---

## Proposed Behavior

1. User selects "Custom date format."
2. A text input appears with the existing token hint.
3. **As the user types**, a live preview line appears below the input showing a sample date rendered in the current format. For example:

   | Input | Preview |
   |-------|---------|
   | `DD/MM/YYYY` | `15/05/2026` |
   | `month DD, YYYY` | `May 15, 2026` |
   | `ZZZZ-QQ` | `ZZZZ-QQ` *(with a note: "No date tokens detected")* |
   | *(empty)* | *(no preview shown)* |

4. Recognized tokens in the input field are visually highlighted (e.g., blue text or underline) to distinguish them from literal characters.
5. If the format string contains zero recognized tokens, a subtle warning appears: *"No recognized date tokens found. Supported: YYYY, MM, DD, etc."*

---

## Justification

- **Reduces errors:** Users can verify their format is correct before generating a full batch, avoiding wasted cycles.
- **Educational:** The preview teaches users which tokens are recognized and how they resolve — reducing support burden.
- **Catches BUG-004 class issues:** If token collision produces corrupted output, the preview would make it immediately visible before the user generates hundreds of dates.
- **Low implementation cost:** The preview reuses the same format parsing logic already in the tool, applied to a single hardcoded sample date.

---

## Acceptance Criteria

- [ ] Live preview appears below the custom format input within 300ms of the user typing
- [ ] Preview uses a fixed sample date (e.g., May 15, 2026 14:30:45) for consistency
- [ ] Recognized tokens in the input are visually distinguished from literal characters
- [ ] If no tokens are detected, a non-blocking warning message is shown
- [ ] Preview does not interfere with the "Generate" button or output area
- [ ] Preview disappears when a non-custom format is selected from the dropdown

---

*Proposed by Raj Shah | May 15, 2026*
