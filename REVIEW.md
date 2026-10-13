# Code Review Findings

| # | Where (function / line) | What is wrong | How you'd notice it (test, input, or symptom) | How you fixed it |
|---|---|---|---|---|
| 1 | `work_hours` logic / Rounding | Employs native Python `round()`, which introduces Banker's rounding adjustments. | A calculated work block of `4.245` hours resolves down to `4.24`, missing strict Rule **R4** parameters. | Shifted logic to `decimal.Decimal` utilizing explicit `ROUND_HALF_UP` flags. |
| 2 | Incoming timestamps validation | Accepts fractional or sub-second timestamps natively into database pipelines. | Millisecond values containing fine-grained remainders populate collections, breaking Rule **R1**. | Applied integer truncation `epoch_ms // 1000` to guarantee whole second structural boundaries. |
| 3 | Timezone processing / Naive dates | Processes database objects natively as naive values. | Operational matching schedules drift out of sync due to missing explicit IST adjustments. | Forced explicitly defined time adjustments using `.replace(tzinfo=timezone.utc).astimezone(IST)`. |
| 4 | `/employees` / Creation logic | Validates schema uniqueness checks purely within application layer execution sequences. | Concurrent structural registration calls bypass validation checks, creating duplicate documents. | Established database unique indexes on startup, intercepting `DuplicateKeyError` to clean throw `409` codes. |
