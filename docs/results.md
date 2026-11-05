# Validation results

The following snapshot reflects a specific manual validation sequence in the local MVP. It is not a production SLA, cumulative total, or ongoing performance measurement.

| Validation step | Observed result |
|---|---:|
| Records retrieved from the public dataset | 100 |
| Records filtered as healthcare product purchases | 8 |
| High-priority candidates delivered to Telegram | 3 |
| Previously registered alert keys skipped on a subsequent run | 3 |

## Deadline caveat

The three candidates sent in the test run did not have a closing date detected in the retrieved data. Their notifications explicitly requested verification in SECOP. They should not be described as confirmed open opportunities.

## How the duplicate test was performed

After the first successful delivery, the alert candidate builder was executed again. The `If Row Does Not Exist` Data Table operation checked the three composite alert keys against `healthtech_alerts_sent` and returned no new items. This prevented repeat Telegram messages for those same alert states.
