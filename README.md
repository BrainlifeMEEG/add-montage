# Add Electrode Montage

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Adds a standard MNE electrode or fNIRS montage (10-20, 10-05, BioSemi, EGI and others) to raw MEG/EEG data, with optional channel renaming, and outputs the data, a sensor plot and a QC report.**

Optionally renames channels (old-new pairs) to match the montage naming, builds the montage with mne.channels.make_standard_montage and applies it with raw.set_montage, then saves the raw file, a sensor-position plot, a PSD plot and an HTML QC report. Use it before topographic plotting or source localisation of EEG data that lack channel positions. Channel names must match the montage after renaming.

| | |
|---|---|
| 📥 **Inputs** | `neuro/meeg/mne/raw` |
| 📤 **Outputs** | `neuro/meeg/mne/raw` · `generic/image/png` · `report/html` |
| 🐳 **Container** | `brainlifemeeg/mne:1.12.1` |
| ⚙️ **Resources** | 10 GB memory · 0.25 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.769](https://doi.org/10.25663/brainlife.app.769) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `raw` | `neuro/meeg/mne/raw` | yes | Continuous MEG/EEG recording in MNE raw .fif format. |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir` | `neuro/meeg/mne/raw` | Raw data with channel positions from the montage (raw.fif). |
| `out_figs` | `generic/image/png` | Montage plot |
| `out_report` | `report/html` | HTML report of the process |

<details>
<summary><b>⚙️ Parameters</b> (2)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `montage` | enum |  | Name of the standard MNE montage to apply. Options: `standard_1005`, `standard_1020`, `standard_alphabetic`, `standard_postfixed`, `standard_prefixed`, `standard_primed`, `biosemi16`, `biosemi32`, `biosemi64`, `biosemi128`, `biosemi160`, `biosemi256`, `easycap-M1`, `easycap-M10`, `EGI-256`, `GSN-HydroCel-32`, `GSN-HydroCel-64_1.0`, `GSN-HydroCel-65_1.0`, `GSN-HydroCel-128`, `GSN-HydroCel-129`, `GSN-HydroCel-256`, `GSN-HydroCel-257`, `mgh60`, `mgh70`, `artinis-octamon`, `artinis-brite23`, `brainproducts-RNP-BA-128`. |
| `rename_channels` | string |  | If the names of channels in your data acquisition system are different from the names in a standard montage, you can use this option to rename the channels in the montage to match your data file (e.g. 'Cz' in 'GSN-HydroCel-257' can be named 'E257' in older EGI acquisition software). Usage: type in name of the channel in the montage, dash, and the name used in your dataset (Cz-E257, or E001-E1, for example). |

</details>

📚 **References used by this App:**
- Gramfort A, et al. MEG and EEG data analysis with MNE-Python. Front Neurosci 7(267):1-13 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)

<!-- brainlife-app:end -->

[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.769-blue.svg)](https://doi.org/10.25663/brainlife.app.769)

## Description

Adds a standard electrode montage from the MNE-Python database to raw MEG/EEG data, using
`mne.channels.make_standard_montage` and `raw.set_montage`. This sets channel locations for
proper visualization, 3D topography mapping, and source localization. It also allows optional
channel renaming to match the montage naming convention before the montage is applied.

The app generates:
- A modified `.fif` file with channel locations set
- A PNG visualization of electrode positions (`raw.plot_sensors`)
- A PNG power spectral density plot
- An HTML report with QC information
- A `product.json` file with metadata

## Inputs

- **`raw`** (`neuro/meeg/mne/raw`): continuous MEG/EEG data to add a montage to (required)

## Outputs

- **`out_dir/raw.fif`** (`neuro/meeg/mne/raw`): raw data with montage locations applied
- **`out_figs/montage.png`**: visualization of electrode positions on the scalp
- **`out_figs/psd.png`**: power spectral density plot of the raw data
- **`out_report/report.html`**: QC report containing raw data summary and montage information
- **`product.json`**: metadata with channel information and montage details

## Configuration Parameters

| key | type | default | description |
|-----|------|---------|--------------|
| `montage` | string (enum) | none (required) | Name of the standard MNE montage to apply (e.g. `standard_1020`, `standard_1005`, `GSN-HydroCel-257`, `GSN-HydroCel-128`). See [MNE's built-in montages](https://mne.tools/stable/generated/mne.channels.make_standard_montage.html) for the full list. |
| `rename_channels` | string | `""` (optional) | Comma-separated list of channel renamings to apply to the montage before it is set, format `old_name-new_name,old_name2-new_name2` (e.g. `Cz-E257,Pz-E129`), for when channel names in your data differ from the standard montage's names. |

## Usage

### Running on Brainlife.io

1. Select a raw MEG/EEG `.fif` file as the `raw` input.
2. Choose the standard `montage` matching your acquisition system.
3. Optionally set `rename_channels` if your channel names differ from the montage's naming
   convention.
4. Submit the task.
5. Review the electrode positions and PSD plots in the report to confirm the montage was applied
   correctly.

### Local Testing

```bash
# Update config.json with your data path and parameters
# Then run:
python main.py
```

## Technical Details

- **Execution**: Python with MNE-Python and the shared `brainlife_utils` library
- **Data format**: MNE `.fif` format (compatible with all downstream Brainlife.io apps)
- **Montage source**: MNE-Python's built-in standard montages
- **Visualization**: sensor position plot with channel names, plus a PSD plot
- **Report generation**: automatic HTML report with channel and montage information

## Authors

- [Kamilya Salibayeva](https://github.com/KSalibay) (Indiana University)
- [Maximilien Chaumon](https://github.com/dnacombo), Paris Brain Institute

## Citations

We kindly ask that you cite the following articles when publishing papers and code using this app:

Hayashi, S., Caron, B.A., Heinsfeld, A.S. et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. Nat Methods 21, 809–813 (2024). https://doi.org/10.1038/s41592-024-02237-2

Gramfort, A. et al. MEG and EEG data analysis with MNE-Python. Front. Neurosci. 7, 267 (2013). https://doi.org/10.3389/fnins.2013.00267

## Funding Acknowledgement

brainlife.io is publicly funded and for the sustainability of the project we kindly ask that you acknowledge the following funding sources:

[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-ACI-1916518](https://img.shields.io/badge/NSF_ACI-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://grantome.com/grant/NIH/R01-EB029272-01)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://grantome.com/grant/NIH/R01-EB030896-01)

## License

Copyright (c) 2026 MEEG Brainlife team. Licensed under AGPL-3.0, see [license.txt](license.txt).
