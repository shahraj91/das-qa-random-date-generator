# BUG-002: No upper limit validation on number of dates to generate

| Field | Detail |
|-------|--------|
| **Bug ID** | BUG-002 |
| **Title** | No upper limit validation on number of dates to generate |
| **Severity** | Low |
| **Priority** | Medium |
| **Status** | Open |
| **Reporter** | Raj Shah |
| **Date Reported** | May 15, 2026 |
| **Component** | Random Date Generator — Count Input |
| **Browser** | Chrome 136.x |
| **OS** | Windows 11 |
| **URL** | https://codebeautify.org/generate-random-date |

---

## Description

The "How many dates to generate?" input field does not enforce an upper limit. When a very large value (e.g., 99999 or higher) is entered, the tool attempts to generate all requested dates in a single operation, causing the browser tab to become unresponsive or extremely slow. There is no cap, warning, or progressive rendering.

---

## Steps to Reproduce

1. Navigate to https://codebeautify.org/generate-random-date.
2. In the "How many dates to generate?" field, enter `99999`.
3. Click **"Generate Random Date"**.

---

## Expected Result

The tool should either:
- Enforce a reasonable maximum (e.g., 1000 or 5000) and display a message like *"Maximum number of dates is 5000."*
- Generate dates progressively with a cancel option
- Warn the user before attempting to generate a very large number

---

## Actual Result

The browser tab freezes or becomes extremely slow. On Chrome 136.x, the tab displayed a "page unresponsive" dialog after approximately 15–20 seconds. No warning was shown before the operation began.

---

## Impact

- **User experience:** The user's browser tab becomes unusable and may need to be force-closed.
- **Data loss risk:** If the user had unsaved work in other fields or was using the output area, the freeze could cause loss.
- **Perception:** An unresponsive web tool reflects poorly on reliability.

---

## Suggested Fix

- Add a `max` attribute to the HTML input field (e.g., `max="5000"`).
- On submit, validate the count and display an inline error if it exceeds the limit.
- Optionally, for power users, offer a "Download as CSV" option for large counts.

---

*Reported by Raj Shah | May 15, 2026*
