# IELTS band score calculator & conversion data

**Live tool:** https://rooverr.github.io/ielts-practice/

Convert an IELTS raw score out of 40 into a band for Listening, Academic Reading
or General Training Reading — and see how many more questions stand between you
and your target band. The same tables are published as JSON for anyone building
against them.

## For students

- **Live converter** with a slider, plus the actionable number: how many more
  correct answers reach the next band.
- **Band targets at a glance** — the raw score where each of bands 6 to 8 begins,
  per section.
- **The overall-band rule**, which is officially fixed: average the four skills,
  round to the nearest half band, ties round up.

## For developers

| File | Contents |
|---|---|
| [`data/listening.json`](data/listening.json) | Listening — **one table covers both Academic and General Training** |
| [`data/reading-academic.json`](data/reading-academic.json) | Academic Reading |
| [`data/reading-general.json`](data/reading-general.json) | General Training Reading — noticeably harsher at the top |
| [`data/overall-band-rounding.json`](data/overall-band-rounding.json) | The official overall-band rule + worked examples |

```js
const { thresholds } = await (await fetch('data/reading-academic.json')).json();

// thresholds are ordered high → low, so the first match wins
const rawToBand = (raw) => thresholds.find((t) => raw >= t.rawMin)?.band ?? null;

rawToBand(30); // 7
rawToBand(2);  // null — below the lowest listed threshold
```

Evaluate descending. A naive ascending scan returns the lowest band rather than
the highest.

## Accuracy — please read

The IELTS Partners **do not publish a fixed row-by-row raw-score table**, and the
real boundaries shift by roughly one mark between test versions. These tables are
**indicative**.

What *is* official:

- the band **5 / 6 / 7 / 8** anchor marks, and
- the **overall-band rounding rule** described above.

If you are one mark from a boundary, you are genuinely on the edge rather than
safely across it. Anything presenting these rows as exact official boundaries is
overstating what exists. Every JSON file repeats this caveat in its own `note`
field so it travels with the data.

## Corrections welcome

Sat a real test where a boundary disagreed with these tables? Open an issue with
the section, your raw score and the band you received. Reference data only stays
honest when it gets corrected against reality.

## Why this exists

These are the same tables that score live mock tests at
[IELTS Practice](https://ieltspractice.app), which is the only reason they stay
current. Publishing them separately keeps one source of truth.

Reference data, free to reuse. IELTS is a registered trademark of its respective
owners; this project is independent and unaffiliated.
