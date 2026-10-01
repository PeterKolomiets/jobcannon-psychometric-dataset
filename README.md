# JobCannon Psychometric Response Dataset

**v3 — 54,431 item-level responses across nine instruments and 25 languages.**

Anonymized, item-level responses to nine open-domain psychometric instruments,
collected from real test-takers on [JobCannon](https://jobcannon.io). Each row is
one completed assessment: the raw per-item answers, the computed dimensional
scores, and the dominant result type.

This is a **first-party** dataset — our own users' responses, not a
re-publication of someone else's data.

## What is new in v3

The corpus grew 22.8% in nine weeks. The part that matters grew more than twice
as fast.

- **Non-English responses are up 52.9%** — 10,535 → **16,112**, against 22.8%
  for the dataset as a whole. Open psychometric corpora are almost entirely
  English; this is the part of the release that is hardest to find elsewhere,
  and it is now 29.6% of every row here.
- **Two languages appear for the first time:** Uzbek (`uz`) and Persian (`fa`).
- **Hebrew and Ukrainian cross into the aggregate tables.** Both had item-level
  rows in v2 but neither reached the n ≥ 100 floor in any instrument, so neither
  appeared in a single aggregate cell. Both do now.
- **Publishable aggregate cells: 42 → 57.** Thirteen locale labels now reach
  n ≥ 100 somewhere, up from eleven.
- **Total: 44,342 → 54,431** item-level responses, +10,089.

One correction carried over from v2's card: the language table there labelled
Ukrainian `uk`. The files have always used **`ua`**, and the table below now
matches them — a reader grepping the CSVs for `uk` found nothing. (Hugging
Face's language index above still lists the ISO code `uk`, because that is the
code its index accepts; the value written in the `locale` column is `ua`.)

## Files

### Item-level — one row per completed assessment

| config | instrument | items | responses |
| --- | --- | --- | --- |
| `mbti` | 16-type indicator | 60 | 17,761 |
| `career_match` | Mini-RIASEC forced choice | 12 | 8,445 |
| `riasec` | Holland Codes | 60 | 5,891 |
| `disc` | DISC | 12 | 5,444 |
| `enneagram` | Enneagram | 36 | 4,412 |
| `multiple_intelligences` | Gardner MI | 40 | 4,161 |
| `big_five` | Five-Factor | 50 | 3,997 |
| `eq` | Emotional intelligence | 10 | 3,100 |
| `dark_triad` | Dark Triad | 18 | 1,220 |
| | | | **54,431** |

Columns: `response_id`, `locale`, `year_month`, `duration_seconds`,
`top_result`, `score_<dimension>` …, `q1` … `qN`.

Item wording is not distributed. The `q` columns hold answer values only.

### Aggregates — by instrument × language

| config | rows | what it holds |
| --- | --- | --- |
| `coverage` | 224 | n, item count, distinct results and median duration for every instrument × locale cell, with a `publishable` flag (n ≥ 100) |
| `result_distribution_by_locale` | 413 | share of each result type, per instrument per locale |
| `dimension_means_by_locale` | 376 | mean of each score dimension, per instrument per locale |

Aggregates suppress small cells: a locale cell needs **n ≥ 100** to appear, and
a result category with **n < 30** is folded into `other_below_30`.

`_manifest.json` is not a config — it records, per instrument, the raw row
count pulled, the count kept after item-count filtering, and the resulting
column count, so the numbers quoted here can be checked against the build.

## Language coverage

Item-level responses per language, all instruments combined:

| | | | | | |
| --- | --- | --- | --- | --- | --- |
| en 33,712 | ja 3,359 | ar 2,656 | ko 2,075 | es 1,471 | id 1,225 |
| fr 944 | ru 870 | pt 739 | th 546 | ua 506 | he 343 |
| zh 325 | it 255 | de 212 | tr 187 | pl 103 | vi 93 |
| sv 56 | fa 55 | nl 34 | hi 23 | uz 14 | nb 11 |
| kk 10 | | | | | |

A further 4,607 responses carry no locale and are labelled `unknown`. Thirteen
locale labels — `ar en es fr he id ja ko pt ru th ua` and `unknown` — reach
n ≥ 100 in at least one instrument and therefore appear in the aggregate tables,
in 57 instrument × locale cells.

The `unknown` bucket is nine rows smaller than in v2 while every instrument
grew. Rows are not deleted from the source table; a response that arrived
without a locale can acquire one later, which moves it out of `unknown` and into
a named language. So v3 is a superset of v2 by instrument but not cell-for-cell,
and the two `unknown` columns are not directly comparable.

## Privacy

No personal data is in these files, and none is read in the first place. The
build selects exactly six columns — `locale`, `answers`, `scores`,
`top_result`, `duration_seconds`, `created_at` — so the identifying columns
that exist on the source table (user id, anonymous id, participant name and
email, referrer, entry host and path, UTM parameters, cohort and organization
ids) cannot reach the output even through a later change to the writer.

Two further reductions: `response_id` is a per-file sequential integer rather
than the database id, and the timestamp is truncated to `year_month`, so a row
cannot be joined back to a session by its time.

## Method

- Rows are filtered to each instrument's **current item count**. A response
  whose answer array is any other length is a legacy test version or an
  incomplete and is dropped — 2,804 of 57,235 raw rows (4.9%).
- `score_*` columns hold the numeric dimensions exactly as scored by the live
  site. Categorical outputs (MBTI's full type with the identity suffix, for
  instance) travel in `top_result`.
- `duration_seconds` is blank where the recorded value was ≤ 0 or ≥ 7,200 s.

## Citation

```bibtex
@dataset{jobcannon_psychometric_2026,
  title  = {JobCannon Psychometric Response Dataset},
  author = {Kolomiets, Peter},
  year   = {2026},
  version = {3.0},
  publisher = {JobCannon},
  doi    = {10.5281/zenodo.20686669},
  url    = {https://doi.org/10.5281/zenodo.20686669}
}
```

That handle is Zenodo's **concept** DOI: it always resolves to the newest
version, so a citation written today keeps working after the next release. If
you need to pin a snapshot instead, every version carries its own DOI, listed on
the Zenodo record.

Figshare mints a second DataCite handle for this release,
`10.6084/m9.figshare.32668833`, and it is current. The third handle we hold,
Harvard Dataverse `10.7910/DVN/RL99KS`, still resolves to **v2** — Harvard's
servers stopped accepting requests from our network partway through this
release, so the deposit there could not be made. For v3, cite Zenodo or
Figshare; the Harvard handle is correct only for v2, and it will be updated once
we can reach them again.

## Mirrors

| host | handle | serves |
| --- | --- | --- |
| Zenodo | [10.5281/zenodo.20686669](https://doi.org/10.5281/zenodo.20686669) | **v3** |
| Hugging Face | [PeterKol/jobcannon-psychometric-responses](https://huggingface.co/datasets/PeterKol/jobcannon-psychometric-responses) | **v3** |
| Kaggle | [peterkolomiets/jobcannon-psychometric-responses](https://www.kaggle.com/datasets/peterkolomiets/jobcannon-psychometric-responses) | **v3** |
| GitHub | [PeterKolomiets/jobcannon-psychometric-dataset](https://github.com/PeterKolomiets/jobcannon-psychometric-dataset) | **v3** |
| Figshare | [10.6084/m9.figshare.32668833](https://doi.org/10.6084/m9.figshare.32668833) | **v3** |
| OSF | [osf.io/dhxp9](https://osf.io/dhxp9/) | **v3** |
| Harvard Dataverse | [10.7910/DVN/RL99KS](https://doi.org/10.7910/DVN/RL99KS) | v2 |

Each mirror is re-deposited on its own cycle, so at any moment some of them lag.
The column above is the only reliable way to tell which snapshot a host is
serving before downloading from it, and it is kept accurate rather than
aspirational — if a host is still on v2 when you read this, this table says so.

A v1 copy also lived on data.world until that platform retired its Open Data
Community on 13 July 2026 and deleted the community datasets; that mirror is
gone, and the seven above are the whole list.

Live aggregate norms, recomputed monthly and larger than this frozen snapshot,
are published at [jobcannon.io/research/distributions](https://jobcannon.io/research/distributions).
Reliability coefficients for the same instruments are at
[jobcannon.io/research/reliability](https://jobcannon.io/research/reliability).

## License

CC-BY-4.0. Use it, build on it, cite it.
