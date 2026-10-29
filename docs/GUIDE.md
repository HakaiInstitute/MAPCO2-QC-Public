# Running MAPCO2-QC on a Mac

For Apple Silicon Macs. You need admin rights and your own MAPCO2 data.

## 1. Install git

Open **Terminal** and run:

```sh
xcode-select --install
```

Click **Install** in the window that appears. If it says the tools are already installed, skip ahead. Check:

```sh
git --version
```

New to git? See the [Hakai git overview](https://data.hakai.org/mobilization/git/overview/).

## 2. Install uv

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Close and reopen Terminal, then check:

```sh
uv --version
```

## 3. Install Positron

Download the macOS installer from [positron.posit.co](https://positron.posit.co/download.html), open the `.dmg`, and drag **Positron** into **Applications**.

## 4. Get the code

```sh
cd ~/Documents ## or wherever you want to store this
git clone https://github.com/HakaiInstitute/MAPCO2-QC-Public.git
cd MAPCO2-QC-Public/xco2qc
uv sync
```

`uv sync` installs Python and every package the project needs into `xco2qc/.venv`.

## 5. Add your data

Copy your raw MAPCO2 file(s) into `MAPCO2-QC-Public/data/raw/`. Results go in `MAPCO2-QC-Public/data/output/`. Nothing in these folders is tracked by git.

## 6. Open the notebook

1. Open Positron.
2. **File > Open Folder...** and choose `MAPCO2-QC-Public/xco2qc`.
3. Open `ocean_CO2_QC.ipynb`.
4. Check that the kernel in the top right is **Python 3.10 (.venv)**. If not, click it and choose that one.
5. In the first code cell, set:
   - `site_id`, for example `'papa'`
   - `raw_file`, for example `'../data/raw/mapco2_papa_0024_dp18_20240731_20250602.txt'`
   - `output_dir`: leave as `'../data/output'`
6. Run the cells in order. Optional cells have their own path to set, marked `# change this!`.

## 7. Notebook steps

| Section | What you do |
|---|---|
| Convert delayed-mode files | Pick the raw file and output folder; convert to netCDF |
| [Optional] External / historical / validation data | Import extra sensor or reference data |
| SBE16 Auxiliary Sensors | Set auxiliary sensor channels |
| Data Reduction | Reduce each cycle to a mean or median |
| Calculate xCO2, pCO2, O2 | Compute derived values |
| Initial Plots | Review the raw deployment |
| Trim Deployment Dates | Set deployment start and end |
| Manual QC #1 | Flag drifting or off-station data |
| Manual QC #2 | LI-COR temperature vs span regression |
| Recalculate xCO2 | Optionally recompute xCO2 |
| Automatic QC | Set up and run algorithmic flags |
| Auxiliary pH / Data Merge | Compute pH and merge everything |
| Map to SOCAT QC | Assign SOCAT flags |
| Manual QC #3 | Review plots and apply final flags |
| MBL / LI-COR Adjustments | Compare to marine boundary layer and adjust |
| Metadata | Enter and apply metadata |
| Generate SOCAT Output | Write final SOCAT files |
| [Optional] Report / Summary | Produce a report and QC summary |

To resume later, use **[Optional] Load previous work** (available after the SOCAT QC step).

## Update

```sh
cd ~/Documents/MAPCO2-QC-Public
git pull
cd xco2qc
uv sync
```

## Troubleshooting

_None yet._

## Appendix: command line

The same steps are available as commands. Run them from `MAPCO2-QC-Public/xco2qc`:

```sh
uv run xco2-nc2csv --help
```

| Command | Step |
|---|---|
| `xco2-import` | Convert raw files to netCDF |
| `xco2-reduce` | Data reduction |
| `xco2-initial-summmary` | Initial summary plots |
| `xco2-pre` | Calculate xCO2, pCO2, O2 |
| `xco2-trim` | Trim deployment dates |
| `xco2-post` | Recalculate xCO2 |
| `xco2-qc` | Automatic QC |
| `xco2-merge` | Data merge |
| `xco2-socat-qc` | SOCAT QC flags |
| `xco2-compare-mbl` | Compare with MBL |
| `xco2-adjustments` | Apply adjustments |
| `xco2-apply-metadata-conventions` | Apply metadata |
| `xco2-gen-socat` | Generate SOCAT XML |
| `xco2-nc2csv` | Convert netCDF to CSV |

Add `--help` to any command for its options.
