<h1 align="center">Add montage</h1>

<p align="center">
  Adds a standard MNE electrode or fNIRS montage (10-20, 10-05, BioSemi, EGI and others) to raw MEG/EEG data, with optional channel renaming, and outputs the data, a sensor plot and a QC report.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/brainlife.app.769"><img src="https://img.shields.io/badge/Brainlife-brainlife.app.769-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-see_LICENSE-lightgrey.svg" alt="License"></a>
</p>

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Adds a standard MNE electrode or fNIRS montage (10-20, 10-05, BioSemi, EGI and others) to raw MEG/EEG data, with optional channel renaming, and outputs the data, a sensor plot and a QC report.**

Optionally renames channels (old-new pairs) to match the montage naming, builds the montage with mne.channels.make_standard_montage and applies it with raw.set_montage, then saves the raw file, a sensor-position plot, a PSD plot and an HTML QC report. Use it before topographic plotting or source localisation of EEG data that lack channel positions. Channel names must match the montage after renaming.

| | |
|---|---|
| **Inputs** | `neuro/meeg/mne/raw` |
| **Outputs** | `neuro/meeg/mne/raw` · `generic/image/png` · `report/html` |
| **Container** | `brainlifemeeg/mne:1.12.1` |
| **Resources** | 10 GB memory · 0.25 h walltime |
| **Run it** | [doi.org/10.25663/brainlife.app.769](https://doi.org/10.25663/brainlife.app.769) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `raw` | `neuro/meeg/mne/raw` | yes | Continuous MEG/EEG recording in MNE raw .fif format. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `out_dir` | `neuro/meeg/mne/raw` | Raw data with channel positions from the montage (raw.fif). |
| `out_figs` | `generic/image/png` | Montage plot |
| `out_report` | `report/html` | HTML report of the process |

<details>
<summary><b>Parameters</b> (2)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `montage` | enum |  | Name of the standard MNE montage to apply. Options: `standard_1005`, `standard_1020`, `standard_alphabetic`, `standard_postfixed`, `standard_prefixed`, `standard_primed`, `biosemi16`, `biosemi32`, `biosemi64`, `biosemi128`, `biosemi160`, `biosemi256`, `easycap-M1`, `easycap-M10`, `EGI-256`, `GSN-HydroCel-32`, `GSN-HydroCel-64_1.0`, `GSN-HydroCel-65_1.0`, `GSN-HydroCel-128`, `GSN-HydroCel-129`, `GSN-HydroCel-256`, `GSN-HydroCel-257`, `mgh60`, `mgh70`, `artinis-octamon`, `artinis-brite23`, `brainproducts-RNP-BA-128`. |
| `rename_channels` | string |  | If the names of channels in your data acquisition system are different from the names in a standard montage, you can use this option to rename the channels in the montage to match your data file (e.g. 'Cz' in 'GSN-HydroCel-257' can be named 'E257' in older EGI acquisition software). Usage: type in name of the channel in the montage, dash, and the name used in your dataset (Cz-E257, or E001-E1, for example). |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/brainlife.app.769](https://doi.org/10.25663/brainlife.app.769), choose `neuro/meeg/mne/raw` from your project, set the parameters and press **Execute**. The results are saved to your project as `neuro/meeg/mne/raw`, `generic/image/png`, `report/html`.

### Locally

You need [Singularity/Apptainer](https://apptainer.org/) (the App runs in `brainlifemeeg/mne:1.12.1`), [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository.
2. Create `config.json` with the paths to your inputs and the parameters:

   ```json
   {
       "raw": "/path/to/raw/raw",
       "montage": "",
       "rename_channels": ""
   }
   ```

3. Run `./main`.

## Configuration Parameters

| key | type | default | description |
|-----|------|---------|--------------|
| `montage` | string (enum) | none (required) | Name of the standard MNE montage to apply (e.g. `standard_1020`, `standard_1005`, `GSN-HydroCel-257`, `GSN-HydroCel-128`). See [MNE's built-in montages](https://mne.tools/stable/generated/mne.channels.make_standard_montage.html) for the full list. |
| `rename_channels` | string | `""` (optional) | Comma-separated list of channel renamings to apply to the montage before it is set, format `old_name-new_name,old_name2-new_name2` (e.g. `Cz-E257,Pz-E129`), for when channel names in your data differ from the standard montage's names. |

## Local Testing

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

## Citing

If you use this App, please cite it, the software it builds on and brainlife.io:

- Add montage. brainlife.io App. [doi:10.25663/brainlife.app.769](https://doi.org/10.25663/brainlife.app.769)
- Gramfort A, et al. MEG and EEG data analysis with MNE-Python. Front Neurosci 7(267):1-13 (2013). [doi:10.3389/fnins.2013.00267](https://doi.org/10.3389/fnins.2013.00267)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- [Kamilya Salibayeva](https://github.com/KSalibay) (Indiana University)
- [Maximilien Chaumon](https://github.com/dnacombo), Paris Brain Institute

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io, which runs this App, is publicly funded by Wellcome, the U.S. National Institutes of Health and the U.S. National Science Foundation; acknowledging that support helps keep it running.</sub>

## License

See [LICENSE](LICENSE).
