# PayCall hotline data

Open data on customer-service and government hotlines in the **U.S., Canada, the UK, Australia, Germany, Mexico, France, Ireland** and more countries as they are added (the data lists all):

- **Official numbers and hours**, each with the organisation's own source URL and the date PayCall checked it; hours also in machine form (time zone, days, opening and closing time) where known
- **Reachable from abroad**: whether calls through PayCall from abroad got through, with the last date. Many toll-free numbers can't be dialled from a foreign phone.
- **What each line says first**: PayCall's test robot calls the hotlines in turn and records the first announcement (a summary, not a recording)
- **Phone-menu routes tested with real calls**: which keys to press or what to say to reach a person, or that there is no way without account data; published after two equal tests
- **Measured hold times**: wait until a person, as a range and median from real PayCall calls

Published by [PayCall](https://pay-call.com), a service for calling numbers in these countries from abroad in the browser: family and friends as well as banks and hotlines.

## Files

| File | What it is |
| --- | --- |
| [`data/hotlines.json`](data/hotlines.json) | Full data set, same as the live API |
| [`data/hotlines.csv`](data/hotlines.csv) | One row per hotline, main fields only, for spreadsheets |
| [`data/hotlines.schema.json`](data/hotlines.schema.json) | JSON Schema of every field (version 7) |

A GitHub Action copies the data from the live API once a day. A commit only happens when something changed.

## Live API

No key, no sign-up, CORS open:

```
https://pay-call.com/hotlines.json
https://pay-call.com/hotlines.json?number=+18008291040   # any number format
https://pay-call.com/hotlines.json?id=irs
https://pay-call.com/hotlines.json?country=gb&kind=bank&status=tested
https://pay-call.com/hotlines.json?lang=es                # texts in 11 languages
```

`country`: ISO code, for example `us`, `ca`, `gb`, `au`, `de`, `mx`, `fr`, `ie` (the JSON lists all). `status`: `tested`, `checked` or `listed`. `abroad=confirmed`: only hotlines reached from abroad. `lang`: texts in `en`, `de`, `fr`, `es`, `it`, `pt`, `nl`, `pl`, `ja`, `zh` or `ar` (English where a translation is missing). The JSON lists every allowed `kind`.

JSON Schema of every field: https://pay-call.com/hotlines.schema.json (version 7)

Human-readable page with method and license: https://pay-call.com/hotline-data

## Fields

| Field | Meaning |
| --- | --- |
| `status` | `tested`: a phone-menu route (to a person, or a tested dead end) is on record. `checked`: PayCall's test robot called the line and recorded what it says first. `listed`: number from the organisation's own source only. |
| `phone.e164`, `otherNumbers` | Number in international format, plus further numbers of the same organisation |
| `hours.text`, `hours.schedule` | Hours in the organisation's words, and in machine form: `timeZone` (IANA) and `periods`, each with `days` (`mon`…`sun`), `opens`, `closes` (local 24-hour time, `24:00` = midnight). `null` where the text is not clear enough, for example "local time" on a line that serves several time zones. Holidays stay in the text. |
| `source.url`, `source.checkedOn`, `source.type` | Where the number comes from, when PayCall checked it, and whether it is an official number page or the organisation's website |
| `reachableFromAbroad.lastConfirmedOn` | Last day a call through PayCall (used from abroad) connected; `null` until then. Many toll-free numbers can't be dialled from a foreign phone. |
| `lastTestCall` | The test robot's newest call: `testedAt`, `heard` (`menu`, `private`, `queue`, `closed`, `message`), `firstAnnouncement` (summary, not a recording), `options`, `asksFirstFor`, `voiceAssistant`, `language` |
| `route` | The way through the phone menu: `method` (`robot` or `manual`), `result` (`queue`, `person`, `private`, `closed`), `steps`, `wayOut`, `triedWithoutSuccess`, `secondsToPerson`, `confirmedOn` (robot routes are published after two equal tests) |
| `waitOnHold`, `callLength` | Minutes (`lowMinutes`, `highMinutes`, `medianMinutes`) with `method` (`measured` or `estimate`); `null` until known |
| `lastTestedOn`, `lastChangedOn` | Date of the newest test call, and the newest date anything in the record was checked or changed |

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
