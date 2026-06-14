---
license: cc-by-4.0
language:
  - en
tags:
  - psychometrics
  - personality
  - career
  - riasec
  - mbti
  - big-five
  - disc
  - enneagram
pretty_name: JobCannon Psychometric Response Dataset
size_categories:
  - 1K<n<10K
---

# JobCannon Psychometric Response Dataset

Anonymized, item-level responses to nine open-domain psychometric instruments,
collected from real test-takers on [JobCannon](https://jobcannon.io). Each row is
one completed assessment: the raw per-item answers, the computed dimensional
scores, and the dominant result type.

This is a **first-party** dataset — these are our own users' responses, not a
re-publication of someone else's data. Released for research, education, and
benchmarking in the spirit of the OpenPsychometrics archive.

## Mirrors & DOI

The same dataset is published openly across several platforms:

- **Hugging Face:** https://huggingface.co/datasets/PeterKol/jobcannon-psychometric-responses
- **Zenodo (DOI):** https://doi.org/10.5281/zenodo.20686670
- **Kaggle:** https://www.kaggle.com/datasets/peterkolomiets/jobcannon-psychometric-responses
- **OSF:** https://osf.io/dhxp9/
- **data.world:** https://data.world/peterkolomiets/jobcannon-psychometric-response-dataset
- **GitHub:** https://github.com/PeterKolomiets/jobcannon-psychometric-dataset
- **Source page:** https://jobcannon.io/research/distributions

## What's inside

| File | Instrument | N | Items | Score dimensions |
|---|---|---:|---:|---:|
| `career_match.csv` | Mini-RIASEC (forced-choice career interest) | 2,005 | 12 | 6 (R I A S E C) |
| `riasec.csv` | RIASEC / Holland Codes (full) | 1,455 | 60 | 6 + percentile |
| `mbti.csv` | 16-type indicator (E/I, S/N, T/F, J/P + A/T) | 1,762 | 60 | 7 |
| `big_five.csv` | Big Five / OCEAN | 880 | 50 | 5 |
| `disc.csv` | DISC behavioral style | 888 | 12 | 4 (D I S C) |
| `enneagram.csv` | Enneagram (9 types) | 487 | 36 | 9 |
| `multiple_intelligences.csv` | Gardner's Multiple Intelligences | 498 | 40 | 8 |
| `eq.csv` | Emotional intelligence (short) | 264 | 10 | 4 |
| `dark_triad.csv` | Dark Triad (SD3-style) | 155 | 18 | 3 |

`_NORMS.md` contains the per-test result distributions and dimensional means.

## Column schema

Every CSV shares the same leading columns, then per-instrument score and item columns:

| Column | Meaning |
|---|---|
| `id` | Anonymous sequential index within the file. **Not** a database identifier. |
| `created_at` | UTC timestamp of completion |
| `locale` | UI language at time of taking (may be blank) |
| `duration_seconds` | Time to complete (may be blank) |
| `top_result` | Dominant result type for this instrument |
| `score_<dim>` | Computed score per dimension |
| `q1 … qN` | Raw per-item answer values (instrument-specific scale) |

Only canonical-length submissions are included: each instrument was filtered to
its current item count, dropping incomplete and legacy-version rows so every
`q1…qN` block is consistent within a file.

## Privacy

No personal data is present. The build pipeline **never selects** user id,
anonymous id, name, email, IP/host, referrer, or UTM parameters. The only
identifier is an anonymous sequential `id` generated at export time; the
underlying database row identifiers are not emitted.

## Instruments & licensing

All instruments are open-domain or public-domain measures (RIASEC/Holland, IPIP
Big Five, DISC, Enneagram, MMI, Dark Triad SD3 family). They are screening and
self-discovery tools, not clinical diagnostics.

## Citation

```
@dataset{jobcannon_psychometric_2026,
  title  = {JobCannon Psychometric Response Dataset},
  author = {JobCannon},
  year   = {2026},
  url    = {https://jobcannon.io}
}
```

License: **CC-BY-4.0** — free to use with attribution.
