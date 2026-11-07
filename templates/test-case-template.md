# Test Case Template

| Field | Content |
|---|---|
| ID | `UI-<MODULE>-NN` or `API-<GROUP>-NN` |
| Title | What is being verified, phrased as an outcome |
| Requirement | REQ / API-REQ id from the [traceability matrix](../traceability-matrix.md) |
| Priority | High / Medium / Low |
| Type | Functional positive / negative, boundary, security, E2E... |
| Preconditions | State the system must be in before step 1 |
| Test data | Exact values, or the rule used to generate them |
| Steps | Numbered, one action each |
| Expected result | Observable outcome. Exact messages in quotes |
| Status | Not run / Pass / Fail / Blocked / Query |
| Actual result | Filled in during execution |
| Defect | Linked defect ID when the case fails |

The CSV files in `ui/` and `api/` use these same columns, so they can be imported into
TestRail, Jira Xray or Zephyr without remapping.
