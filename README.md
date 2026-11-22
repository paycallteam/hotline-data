# PayCall hotline data

Open data on customer-service and government hotlines in the **U.S., Canada, the UK, Australia and Germany**:

- **Official numbers and hours**, each with the organisation's own source URL and the date PayCall checked it
- **Phone-menu routes tested with real calls**: which keys to press or what to say, what the menu said, and where the call ended
- **Measured hold times**: wait until a person, as a range and median from real PayCall calls

Published by [PayCall](https://pay-call.com), a service for calling U.S., Canadian, UK, Australian and German numbers from abroad in the browser.

## Files

| File | What it is |
| --- | --- |
| [`data/hotlines.json`](data/hotlines.json) | Full data set, same as the live API |
| [`data/hotlines.csv`](data/hotlines.csv) | One row per hotline, main fields only, for spreadsheets |

A GitHub Action copies the data from the live API once a day. A commit only happens when something changed.

## Live API

No key, no sign-up, CORS open:

```
https://pay-call.com/hotlines.json
https://pay-call.com/hotlines.json?number=+18008291040   # any number format
https://pay-call.com/hotlines.json?id=irs
https://pay-call.com/hotlines.json?country=gb&kind=bank&status=tested
```

`country`: `us`, `ca`, `gb`, `au`, `de`. `status`: `tested` or `listed`. The JSON lists every allowed `kind`.

## Fields

| Field | Meaning |
| --- | --- |
| `status` | `tested`: PayCall called the line and wrote down the menu route. `listed`: number from the organisation's own page, not tested yet. |
| `phone.e164` | Number in international format |
| `hours` | Opening hours as published by the organisation |
| `source.url`, `source.checkedOn` | Where the number comes from and when PayCall checked it |
| `menuRoute` | Tested phone-menu route with `testedOn`; `null` until tested. Menus change, so check the date. |
| `waitOnHold` | Time on hold in minutes (`lowMinutes`, `highMinutes`, `medianMinutes`); `null` until measured |
| `callLength` | Typical whole call, waiting and talking, same rules |

### How hold times are measured

- Wait = the time **Hold for me** was switched on during a PayCall call (the caller muted and waited for a person).
- Calls shorter than one minute are left out.
- Shown as a range and median, not as a promise.
- No personal data: no callers, no transcripts, no counts per person.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Free to reuse, also commercially, with a credit and link:

> Source: PayCall (https://pay-call.com/hotlines.json)

For numbers and hours, also cite the organisation's own source URL. For menu routes and hold times, cite PayCall with the test date.

The license covers the hotline data, not the texts of PayCall's websites.

## Corrections

Found a wrong number or an outdated menu route? [Open an issue](../../issues) with the hotline `id` and what you found.
