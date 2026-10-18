[![DOI](https://img.shields.io/badge/DOI-10.82901%2Fnemar.on005007-blue)](https://doi.org/10.82901/nemar.on005007)

<!-- nemar:provenance -->
## Provenance

This dataset is mirrored on NEMAR from OpenNeuro.

- Source: [ds005007](https://openneuro.org/datasets/ds005007)
- DOI: [10.18112/openneuro.ds005007.v1.0.0](https://doi.org/10.18112/openneuro.ds005007.v1.0.0)

## Channel names follow the data headers (18 Oct 2026)

In 42 recordings the channel list of channels.tsv did not equal the data-file header (names, order or count). In the 42 with a different channel count, `mne_bids.read_raw_bids` warned and kept the header names, so the channels.tsv types and bad-channel status reached only the channels whose names happen to be identical; a reader that applies the channels.tsv rows by position gives wrong labels wherever the two lists are not aligned. Since 18 Oct 2026 every channels.tsv lists exactly the header channels, in header order, with the header names (MNE names: duplicated header labels numbered -0, -1).

Rows were matched to header channels BY NAME, never by position (exact; then case/whitespace; then the recorder prefix 'EEG '/'POL ', '_' and zero padding; then a trailing '-<reference>' token). Matched rows keep all their columns (type, units, status, ...); 4368 rows took the header name (the header labels carry 'POL ' prefix, 'EEG ' prefix + '-Ref' suffix).

1057 header channels had no channels.tsv row (42 files) and got a new row: type from the name (ECG/EMG/EOG/trigger) or from the other contacts of the same shaft, else MISC; units from the header; status n/a. Examples: `sub-01_ses-01_task-namingtask_run-01_ieeg.edf`: POL ECG, POL L4, POL L5, POL L6; `sub-02_ses-01_task-namingtask_run-01_ieeg.edf`: POL ECG, POL K2, POL K3, POL K4; `sub-03_ses-01_task-namingtask_run-01_ieeg.edf`: POL ECG, EEG M2-Ref, POL M3, POL M4.

55 channels.tsv rows had no channel in the data file (2 files) and were removed; they are kept verbatim under `sourcedata/channels-removed-2026-10-18/` (same relative path). Examples: `sub-08_ses-01_task-namingtask_run-01_ieeg.edf`: F09, F10, G05, G06, G07; `sub-10_ses-01_task-namingtask_run-01_ieeg.edf`: J1, J5, J6, K1, K2.

41 files listed the channels in another order than the header; rows now follow the header order.

Applied by position, the former rows would have given wrong labels to 4337 channels in 42 files (rows missing, extra or out of order before that position).

electrodes.tsv is unchanged: it names the contacts (e.g. `A1`), while channel names follow the recorder (e.g. `A1-AV`); to attach positions in MNE, map channel names to contact names first, e.g. `raw.rename_channels(lambda n: re.sub(r'-[A-Za-z0-9]+$', '', n))`.

## Sampling frequency follows the data headers (18 Oct 2026)

sampling_frequency: 58 channels.tsv rows in 1 files and SamplingFrequency in 1 *_ieeg.json set to the rate of the data-file header (e.g. `sub-02_ses-01_task-namingtask_run-01_channels.tsv` POL J5: 1000 -> 2000; `sub-02_ses-01_task-namingtask_run-01_channels.tsv` POL J6: 1000 -> 2000; `sub-02_ses-01_task-namingtask_run-01_channels.tsv` POL J7: 1000 -> 2000).

Check: read at the header rate, sub-02 shows its mains line at 50 Hz (16.3 dB above the neighbouring bins in the 18 Oct grid), the same line as the subjects recorded at 1000 Hz, so the header rate is the real one.
