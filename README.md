# paper-multiscale-emd-graphs

## Contents

| Directory | Description |
|------------|-------------|
| `data/` | Input price series (parquet) for the empirical panel in the paper |
| `docs/` | Figure assets and intermediate outputs for the manuscript (`docs/20abr26/`) |
| `src/python/GraphEMD/` | Library: IMF→graph transformation, synthetic signals, and other transformations |
| `src/python/CommonUtils/` | Minimal shared utilities (`DictClass`) |
| `scripts/` | Paper pipelines: empirical panel, emdsynth, MSCI exploration |
| `analysis/` | Jupyter notebooks for experiments |

## Input data (`data/`)

The `data/` folder contains the **input price series** used in the empirical section of the paper. Each file is a daily OHLCV history downloaded from Yahoo Finance and stored in parquet format.

All series share the common window **2012-01-12 → 2026-04-20**. ETFs (XLE, XLP, XLV) are aligned to MSCI World trading days (3,587 observations). XAU/USD (`GC=F`) has 3,584 observations over the same calendar range.

| File | Asset | Yahoo symbol | Observations |
|------|-------|--------------|--------------|
| `msci_world.parquet` | MSCI World index | `^MSWORLD` | 3,587 |
| `xle.parquet` | Energy Select Sector SPDR (XLE) | `XLE` | 3,587 |
| `xlp.parquet` | Consumer Staples Select Sector SPDR (XLP) | `XLP` | 3,587 |
| `xlv.parquet` | Health Care Select Sector SPDR (XLV) | `XLV` | 3,587 |
| `xauusd.parquet` | Gold spot (XAU/USD) | `GC=F` | 3,584 |

Columns: `Open`, `High`, `Low`, `Close`, `Volume`, `Dividends`, `Stock Splits` (and `Capital Gains` for MSCI World). The `Close` price is the primary series used in the decomposition and graph pipelines.

To regenerate the files from Yahoo Finance, use the download scripts under `scripts/` (e.g. `scripts/download_msci_world.py`, `scripts/xle_etf_analysis/01_download_xle.py`).

## Installation

```bash
git clone https://github.com/MultiEMDGraphsTS/paper-multiscale-emd-graphs.git
cd paper-multiscale-emd-graphs
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install -e src/python
```

## Main paper pipelines

```bash
# Full empirical panel (MSCI + XLE/XLP/XLV/XAUUSD)
PYTHONPATH=src/python python scripts/run_vmd_all_assets.py

# Synthetic benchmark (EMD/EEMD/CEEMDAN/VMD)
PYTHONPATH=src/python python scripts/emdsynth/run_emdsynth_decompositions.py

# Figures for the paper (output in docs/20abr26/images/english/)
PYTHONPATH=src/python python scripts/generate_decomposition_panel_figure.py --copia-docs
PYTHONPATH=src/python python scripts/generate_ica_panel_figure.py \
  --salida-png docs/20abr26/images/english/ica_components_panel.png
```

## License

Apache-2.0 — see [LICENSE](LICENSE).
