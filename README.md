![](./BioDCASE2026_Task5.png)

This repository contains the released baseline for the [BioDCASE 2026 Cross-Domain Mosquito Species Classification (CD-MSC)](https://biodcase.github.io/challenge2026/task5) task. It includes the baseline code, recommended development split metadata, extracted feature statistics, released checkpoints and logs for 10 fixed seeds, and report-ready result assets.
 
- Official challenge page: [BioDCASE 2026 CD-MSC challenge](https://biodcase.github.io/challenge2026/task5)
- Development dataset: [Zenodo Development dataset](https://zenodo.org/records/19095788)
  
## Repository Layout

| Path | Purpose |
| --- | --- |
| `extract_features.py` | Extract log-mel features for `training`, `validation`, and `test` |
| `train.py` | Train one experiment and save best/final checkpoints plus evaluations |
| `evaluate.py` | Evaluate a saved checkpoint on `validation` or `test` |
| `predict.py` | Predict one audio file |
| `run_multi_seed_experiments.py` | Run the released 10-seed experiment set |
| `generate_technical_report_assets.py` | Rebuild report tables and figures from saved outputs |
| `plot_official_main_figure.py` | Plot the paper-style per-species official figure |
| `configs/` | Single-run and multi-seed configs |
| `framework/` | Data loading, feature extraction, model, training, and utility code |
| `Development_data/metadata/` | Recommended split files and split summary |
| `Development_data/feature/` | Precomputed split features and training feature statistics |
| `outputs/` | Released checkpoints, logs, per-run metrics, and multi-seed summaries |
| `technical_report_assets_current_split/` | Report-ready tables and figures for the current split |

## Quick Start

1. Install dependencies:

```bash
pip install -r requirements.txt
```

2. Download the development dataset from the [Zenodo Development dataset](https://zenodo.org/records/19095788) and place it under this repository as `Development_data/`.

3. Extract features:

```bash
python extract_features.py --config configs/default_experiment.json
```

This creates:

```text
Development_data/feature/
├── training_features.pkl
├── validation_features.pkl
├── test_features.pkl
└── training_feature_stats.json
```

4. Train one run:

```bash
python train.py --config configs/default_experiment.json
```

5. Evaluate a checkpoint:

```bash
python evaluate.py \
  --config configs/default_experiment.json \
  --checkpoint outputs/MTRCNN_seed42_B64_E100_earlystop_min10_pati5/model/model_best.pth \
  --split test \
  --metrics-out outputs/MTRCNN_seed42_B64_E100_earlystop_min10_pati5/manual_test_metrics.json \
  --predictions-out outputs/MTRCNN_seed42_B64_E100_earlystop_min10_pati5/manual_test_predictions.jsonl
```

6. Predict one file:

```bash
python predict.py \
  --config configs/default_experiment.json \
  --checkpoint outputs/MTRCNN_seed42_B64_E100_earlystop_min10_pati5/model/model_best.pth \
  --audio Development_data/raw_audio/S_1_D_5_16608.wav
```

7. Run the released 10-seed benchmark:

```bash
python run_multi_seed_experiments.py --config configs/multi_seed_experiment.json
```

## Data And Split

- The code uses audio IDs in the form `S_<speciesID>_D_<domainID>_<clipIndex>`.
- The released development setup covers 9 species and 5 domains (`D1` to `D5`).
- Recommended split files:
  - `Development_data/metadata/Training_ids.txt`
  - `Development_data/metadata/Validation_ids.txt`
  - `Development_data/metadata/Test_ids.txt`
  - `Development_data/metadata/TrainVal_ids.txt`
  - `Development_data/metadata/split_summary.json`

#### Dataset overview

<table class="dataset-table">
  <thead>
    <tr>
      <th>Item</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Number of domains</td>
      <td>5</td>
    </tr>
    <tr>
      <td>Number of species</td>
      <td>9</td>
    </tr>
    <tr>
      <td>Total number of clips</td>
      <td>271380</td>
    </tr>
    <tr>
      <td>Total duration</td>
      <td>218388.40 seconds (60.66 hours)</td>
    </tr>
  </tbody>
</table>
 
#### Domain Distribution

<table class="dataset-table">
  <thead>
    <tr>
      <th>Domain</th>
      <th>Number of clips</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>D1</td>
      <td>4065</td>
    </tr>
    <tr>
      <td>D2</td>
      <td>784</td>
    </tr>
    <tr>
      <td>D3</td>
      <td>679</td>
    </tr>
    <tr>
      <td>D4</td>
      <td>200</td>
    </tr>
    <tr>
      <td>D5</td>
      <td>265652</td>
    </tr>
  </tbody>
</table>

#### Species Distribution

<table class="dataset-table">
  <thead>
    <tr>
      <th>Species</th>
      <th>Species ID</th>
      <th>Number of clips</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><i>Aedes aegypti</i></td>
      <td>1</td>
      <td>81587</td>
    </tr>
    <tr>
      <td><i>Aedes albopictus</i></td>
      <td>2</td>
      <td>18517</td>
    </tr>
    <tr>
      <td><i>Culex quinquefasciatus</i></td>
      <td>3</td>
      <td>72056</td>
    </tr>
    <tr>
      <td><i>Anopheles gambiae</i></td>
      <td>4</td>
      <td>46998</td>
    </tr>
    <tr>
      <td><i>Anopheles arabiensis</i></td>
      <td>5</td>
      <td>21117</td>
    </tr>
    <tr>
      <td><i>Anopheles dirus</i></td>
      <td>6</td>
      <td>127</td>
    </tr>
    <tr>
      <td><i>Culex pipiens</i></td>
      <td>7</td>
      <td>29754</td>
    </tr>
    <tr>
      <td><i>Anopheles minimus</i></td>
      <td>8</td>
      <td>550</td>
    </tr>
    <tr>
      <td><i>Anopheles stephensi</i></td>
      <td>9</td>
      <td>674</td>
    </tr>
  </tbody>
</table>

 
The released development dataset is uneven across both species and domains. Participants are encouraged to consider both class balance and domain balance during model development.
 
 
For official test reporting, `evaluate.py` reads `split_summary.json` and adds:

- `BA_seen`: species balanced accuracy on seen-domain test samples
- `BA_unseen`: species balanced accuracy on unseen-domain test samples
- `DSG = |BA_unseen - BA_seen|`

## Model And Training Defaults

Resolved from `configs/default_experiment.json` and the code:

- sample rate: `8000`
- frontend: `64`-bin log-mel spectrogram
- hop length: `80` samples (`10 ms`)
- model: `MTRCNNClassifier`
- trainable parameters: `221267`
- classifier heads: species + domain
- optimizer: `AdamW`
- learning rate: `0.001`
- weight decay: `0.0001`
- training batch size: `64`
- evaluation batch size: `8`
- training crop length: `2.0` seconds
- max epochs: `100`
- early stopping: start at epoch `10`, patience `5`
- checkpoint selection metric: validation `species_balanced_accuracy`

The training pipeline keeps full extracted feature length on disk. Random cropping is applied only during training; validation, test, and single-file prediction use the full sequence with padding masks.

## Released Outputs And Results

Per-seed outputs are stored under:

```text
outputs/MTRCNN_seed*_B64_E100_earlystop_min10_pati5/
├── resolved_config.json
├── run_context.json
├── run_summary.json
├── train.log
├── metrics.csv
├── model/
│   ├── model_best.pth
│   └── model_final.pth
├── best_model_eval/
│   ├── validation_metrics.json
│   ├── validation_predictions.jsonl
│   ├── test_metrics.json
│   └── test_predictions.jsonl
└── final_model_eval/
    ├── validation_metrics.json
    ├── validation_predictions.jsonl
    ├── test_metrics.json
    └── test_predictions.jsonl
```

Canonical multi-seed summaries are under:

- `outputs/multi_seed_summary/best_model_eval/summary_report.md`
- `outputs/multi_seed_summary/best_model_eval/summary_stats.json`
- `outputs/multi_seed_summary/final_model_eval/summary_report.md`
- `outputs/multi_seed_summary/final_model_eval/summary_stats.json`

Released fixed seeds:

- `42, 3407, 1234, 2023, 2024, 1024, 2048, 4096, 8192, 10086`

Supplementary report tables and figures are under:

- `technical_report_assets_current_split/technical_report_summary.md`
- `technical_report_assets_current_split/per_species_official_best_model_main_figure.pdf`
- `technical_report_assets_current_split/per_species_official_best_model_main_figure.png`

For the released 10-seed best-checkpoint summary in `outputs/multi_seed_summary/best_model_eval/summary_stats.json`:

| Metric | Validation | Test |
| --- | --- | --- |
| loss | `0.261275 +- 0.019419` | `1.625349 +- 0.121943` |
| species_accuracy | `0.904649 +- 0.007520` | `0.782688 +- 0.006524` |
| species_balanced_accuracy | `0.852525 +- 0.009247` | `0.548866 +- 0.003982` |
| domain_accuracy | `0.999181 +- 0.000077` | `0.924007 +- 0.026146` |
| BA_seen | `n/a` | `0.879283 +- 0.010147` |
| BA_unseen | `n/a` | `0.185414 +- 0.013971` |
| DSG | `n/a` | `0.693869 +- 0.018579` |

If you need report figures or per-species/per-domain tables, use `technical_report_assets_current_split/`. If you need the raw released experiment summaries, treat `outputs/multi_seed_summary/` as the primary source.

## Reproducibility Notes

- Feature files and training runs are tied to config signatures.
- `extract_features.py` skips re-extraction when an existing feature file matches the current config signature.
- `train.py` and `run_multi_seed_experiments.py` reuse completed outputs when `run_context.json` matches the current resolved config.
- If you change dataset paths, sampling settings, or split files, regenerate features before training or evaluation.


## Citation
 
If you use the development dataset, the released baseline, or refer to the BioDCASE 2026 Cross-Domain Mosquito Species Classification task, please cite the following paper.

BioDCASE 2026 CD-MSC Baseline: <a href="https://arxiv.org/abs/2603.20118" target="_blank">📄 PDF</a>

```bibtex
@misc{hou2026biodcase2026challengebaseline,
      title={BioDCASE 2026 Challenge Baseline for Cross-Domain Mosquito Species Classification}, 
      author={Yuanbo Hou and Vanja Zdravkovic and Marianne Sinka and Yunpeng Li and Wenwu Wang and Mark D. Plumbley and Kathy Willis and Stephen Roberts},
      year={2026},
      eprint={2603.20118},
      archivePrefix={arXiv},
      primaryClass={eess.AS},
      url={https://arxiv.org/abs/2603.20118}, 
}
```

MTRCNN model: <a href="https://doi.org/10.1109/ICASSP49660.2025.10890031" target="_blank">📄 PDF</a>
 
```bibtex
@INPROCEEDINGS{10890031,
  author={Hou, Yuanbo and Ren, Qiaoqiao and Wang, Wenwu and Botteldooren, Dick},
  booktitle={IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)},
  title={Sound-Based Recognition of Touch Gestures and Emotions for Enhanced Human-Robot Interaction},
  year={2025},
  pages={1-5},
  doi={10.1109/ICASSP49660.2025.10890031}
}
```
 