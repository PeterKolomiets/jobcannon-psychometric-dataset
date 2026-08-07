# JobCannon Psychometric Response Dataset

**v2 — 44,342 item-level responses across nine instruments and 23 languages.**

Anonymized, item-level responses to nine open-domain psychometric instruments,
collected from real test-takers on [JobCannon](https://jobcannon.io). Each row is
one completed assessment: the raw per-item answers, the computed dimensional
scores, and the dominant result type.

This is a **first-party** dataset — our own users' responses, not a
re-publication of someone else's data.

## What is new in v2

- **5.3× more data.** 8,394 → **44,342** item-level responses.
- **Multilingual.** **10,535 responses are non-English**, across 22 languages
  besides English. Open psychometric corpora are almost entirely English; this
  is the part of the release we think is hardest to find elsewhere.
- **A by-locale aggregate layer.** Three extra tables give result shares and
  dimension means per instrument *per language*, so cross-cultural comparisons
  do not require re-deriving them from the item-level files.
- **Each instrument is its own config.** In v1 the nine CSVs were served as a
  single `default` config; because their schemas differ, the dataset viewer
  could not build and showed zero rows. Fixed here.

## Files

### Item-level — one row per completed assessment

| config | instrument | items | responses |
| --- | --- | --- | --- |
| `mbti` | 16-type indicator | 60 | 14,735 |
| `career_match` | Mini-RIASEC forced choice | 12 | 6,506 |
| `riasec` | Holland Codes | 60 | 4,947 |
| `disc` | DISC | 12 | 4,610 |
| `enneagram` | Enneagram | 36 | 3,784 |
| `big_five` | Five-Factor | 50 | 3,377 |
| `multiple_intelligences` | Gardner MI | 40 | 2,936 |
| `eq` | Emotional intelligence | 10 | 2,512 |
| `dark_triad` | Dark Triad | 18 | 935 |
| | | | **44,342** |

Columns: `response_id`, `locale`, `year_month`, `duration_seconds`,
`top_result`, `score_<dimension>` …, `q1` … `qN`.

### Aggregates — by instrument × language

| config | rows | what it holds |
| --- | --- | --- |
| `coverage` | 198 | n, item count, distinct results and median duration for every instrument × locale cell, with a `publishable` flag (n ≥ 100) |
| `result_distribution_by_locale` | 330 | share of each result type, per instrument per locale |
| `dimension_means_by_locale` | 294 | mean of each score dimension, per instrument per locale |

Aggregates suppress small cells: a locale cell needs **n ≥ 100** to appear, and
a result category with **n < 30** is folded into `other_below_30`.

`_manifest.json` is not a config — it records, per instrument, the raw row
count pulled, the count kept after item-count filtering, and the resulting
column count, so the numbers quoted here can be checked against the build.

## Language coverage

Item-level responses per language, all instruments combined:

| | | | | | |
| --- | --- | --- | --- | --- | --- |
| en 29,191 | ja 2,682 | ar 1,533 | ko 1,382 | es 855 | ru 788 |
| id 660 | fr 608 | pt 414 | uk 314 | th 301 | he 286 |
| zh 192 | it 144 | de 136 | tr 87 | vi 45 | pl 36 |
| sv 32 | nl 17 | hi 16 | kk 5 | nb 2 | |

A further 4,616 responses carry no locale and are labelled `unknown`. Eleven
locale labels — `ar en es fr id ja ko pt ru th` and `unknown` — reach n ≥ 100
in at least one instrument and therefore appear in the aggregate tables, in 42
instrument × locale cells.

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
  incomplete and is dropped — 2,767 of 47,109 raw rows (5.9%).
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
  version = {2.0},
  publisher = {JobCannon},
  doi    = {10.5281/zenodo.20686669},
  url    = {https://doi.org/10.5281/zenodo.20686669}
}
```

That handle is Zenodo's **concept** DOI: it always resolves to the newest
version, so a citation written today keeps working after the next release. If
you need to pin a snapshot instead, every version carries its own DOI, listed
on the Zenodo record; the one pinned to v1 is `10.5281/zenodo.20686670`.

Figshare carries its own DataCite DOI, `10.6084/m9.figshare.32668833`, and it
also resolves to this release. One handle in the table below — Harvard Dataverse,
`10.7910/DVN/RL99KS` — still points at the **v1** snapshot, 8,394 responses
rather than 44,342. Citing it alongside `version = {2.0}` would name this release
and hand the reader the previous one, so cite it only for v1.

## Mirrors

| host | handle | serves |
| --- | --- | --- |
| Zenodo | [10.5281/zenodo.20686669](https://doi.org/10.5281/zenodo.20686669) | **v2** |
| Hugging Face | [PeterKol/jobcannon-psychometric-responses](https://huggingface.co/datasets/PeterKol/jobcannon-psychometric-responses) | **v2** |
| Kaggle | [peterkolomiets/jobcannon-psychometric-responses](https://www.kaggle.com/datasets/peterkolomiets/jobcannon-psychometric-responses) | **v2** |
| GitHub | [PeterKolomiets/jobcannon-psychometric-dataset](https://github.com/PeterKolomiets/jobcannon-psychometric-dataset) | **v2** |
| Figshare | [10.6084/m9.figshare.32668833](https://doi.org/10.6084/m9.figshare.32668833) | **v2** |
| OSF | [osf.io/dhxp9](https://osf.io/dhxp9/) | **v2** |
| Harvard Dataverse | [10.7910/DVN/RL99KS](https://doi.org/10.7910/DVN/RL99KS) | v1 |

Each mirror is re-deposited on its own cycle, so at any moment some of them lag.
The column above is the only reliable way to tell which snapshot a host is
serving before downloading from it, and it is kept accurate rather than
aspirational.

A v1 copy also lived on data.world until that platform retired its Open Data
Community on 13 July 2026 and deleted the community datasets; that mirror is
gone, and the seven above are the whole list.

Live aggregate norms, recomputed monthly and larger than this frozen snapshot,
are published at [jobcannon.io/research/distributions](https://jobcannon.io/research/distributions).
Reliability coefficients for the same instruments are at
[jobcannon.io/research/reliability](https://jobcannon.io/research/reliability).

## License

CC-BY-4.0. Use it, build on it, cite it.
