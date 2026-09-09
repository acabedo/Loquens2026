# Prosodic metrics per intonation unit (IP) — Loquens 13 (2026)

Derived data set accompanying:

Cabedo Nebot, A. (2026). *An exploratory study of prosodic patterns in Spanish: a comparison
between neurotypical speakers and speakers with ASD level 1*. **Loquens**, 13, eXXX.
https://doi.org/10.3989/loquens.2026.eXXX

## What this repository contains

`loquens2026_ip_metrics.csv` — one row per intonation unit (IP), with the acoustic
measures described in Sections 3.2–3.4 of the article.

`loquens2026_speaker_metadata.csv` — one row per speaker (144 rows), with the
socio-demographic variables used to build the grouping variable of Section 3.3. Join to the
IP file on `speaker_id`.

Nothing else is distributed.

**Not included, by design:**

- audio or video, in any form or excerpt;
- transcriptions or any verbatim stretch of speech;
- absolute time stamps into the source recordings;
- original file names, channel names, URLs, or any speaker identifier;
- the key linking pseudonymous speaker IDs to the original recordings, which is
  retained by the author and not deposited.

This follows the ethical framework set out in Section 3.1 of the article: all analyses
are performed on derivatives, and the deposit does not increase the traceability of the
speakers beyond the exposure they themselves chose.

## Structure

| Column | Type | Description |
|---|---|---|
| `speaker_id` | chr | Pseudonymous speaker code (`PRE_nnn`, `ASD_nnn`). Assignment is random; the order carries no information. |
| `fragment_id` | chr | Pseudonymous recording code within a speaker (`<speaker_id>_fnn`). PRESEEA speakers have one fragment; ASD-1 speakers have several, corresponding to the continuous stretches described in Section 5.3. |
| `corpus` | chr | `PRESEEA` (neurotypical) / `ASD1`. |
| `speaker_role` | chr | `informant` / `fieldworker` for PRESEEA, `target` for ASD-1. |
| `ip_id` | int | Sequential index of the IP within its fragment. Preserves discourse order. |
| `in_analysis` | lgl | `TRUE` for the IPs retained by the filtering criteria of Section 3.4. |
| `n_words`, `n_vowels` | int | Words and vowel nuclei in the IP. |
| `pitch_mean_ip` | num | Mean f0 of the IP (Hz). |
| `intensity_mean_ip` | num | Mean intensity of the IP (dB). See Section 5.3: the between-corpus contrast in this variable is a recording artefact. |
| `inflexion_st` | num | Net tonal displacement within the IP (semitones), final minus initial f0. |
| `range_st` | num | Tonal range, maximum minus minimum (semitones). |
| `ip_duration` | num | IP duration (s). |
| `speech_rate_w` | num | Speech rate (words/s). |
| `first_vowel_dur` | num | Duration of the initial vowel (s). |
| `last_tonic_dur` | num | Duration of the final nuclear vowel (s). |
| `inter_vowel_dur` | num | Mean inter-vowel interval (s). |
| `anacrusis_pct` | num | Tonal movement in the anacrusis (%). |
| `body_pct` | num | Tonal movement in the body of the IP (%). |
| `toneme_pct` | num | Tonal movement in the nuclear toneme (%). |
| `toneme_intensity_pct` | num | Intensity movement in the nuclear toneme (%). |
| `quantity_body_peaks` | int | Number of f0 peaks in the body of the IP. |
| `AMH` | chr | Nuclear contour label, derived from `toneme_pct`, `body_pct`, `anacrusis_pct` and `quantity_body_peaks`. |

### `loquens2026_speaker_metadata.csv`

| Column | Type | Description |
|---|---|---|
| `speaker_id` | chr | Joins to the IP file. |
| `corpus` | chr | `PRESEEA` / `ASD1`. |
| `sex` | chr | `Men` / `Women`. |
| `age_group` | chr | `18-35` / `35-55` / `>55`. |
| `education` | chr | Constant `high`: only speakers with a high educational level enter the study. |
| `category` | chr | Grouping variable of Section 3.3, `corpus \| sex \| age`. |

Date of birth, exact age, occupation, fieldwork codes and interviewer details are held in
the source metadata but are not deposited: the analysis uses only the bracketed variables
above.

Speakers present in the IP file but absent from this table were not retained for analysis
(educational level other than high, or no metadata record); they can be dropped with an
inner join.

## Reproducing the analysed subset

`in_analysis == TRUE` reproduces the working data set of the article: intensity > 40 dB,
duration < 8 s, tonal range < 30 st, `toneme_pct` / `anacrusis_pct` / `body_pct` < 150%,
speech rate < 20 words/s, `last_tonic_dur` and `inter_vowel_dur` < 1 s. IPs with a missing
value on any of these variables are also excluded, since the filter is applied with
`dplyr::filter()`, which drops `NA`.

## Missing values

`NA` marks measures that could not be estimated for a given IP. Coverage is uneven and
corpus-dependent for two variables: `first_vowel_dur` and `toneme_intensity_pct` are
available for essentially all ASD-1 IPs but for under 2% of PRESEEA IPs. Any analysis
requiring complete cases on these two columns therefore works on a small and non-random
subset of the corpus.

## Licence

Data: CC BY 4.0. Please cite the article above.
