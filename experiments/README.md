# 📊 GPS Localization Experiments

This folder contains all experiments conducted for the Campus Image-to-GPS Regression project.
We trained and compared **3 architectures** across **10 experiment configurations**.

---

## 🏆 Best Result

| Model | Experiment | Mean Error | Median | Within 5m | Within 10m |
|-------|------------|------------|--------|-----------|------------|
| **EfficientNet** | 100ep, LR=0.001, Full Data | **5.71m** | 2.75m | 71.7% | 86.9% |

---

## � After Downloading - Quick Start

### 1. Install Dependencies
```bash
pip install numpy matplotlib seaborn
```

### 2. Navigate to the Project
```bash
cd GPS_bgu_model
```

### 3. Run Analysis Scripts (Visualize Results)

**These scripts read the existing results and generate plots - no GPU needed:**

```bash
# Analyze 100 epochs experiments (BEST results)
cd results
python ../scripts/analysis/plot_100ep_analysis.py
# Output: results/testing/analysis_100epochs/

# Analyze 50 epochs full data experiment
python ../scripts/analysis/plot_50ep_lr001_fulldata.py
# Output: results/testing/50ep_lr001_fulldata/comparison/

# Analyze 30 epochs with augmented test
python ../scripts/analysis/plot_30ep_lr0001_halfdata_augtest.py

# Geographic error analysis for 50 epochs
python ../scripts/analysis/plot_50ep_geographic.py
```

### 4. Run Comparison Scripts (Compare Experiments)

```bash
cd GPS_bgu_model/results

# Compare ALL experiments - master summary
python ../scripts/comparison/comprehensive_comparison.py
# Output: testing/comprehensive_comparison/

# Compare full data experiments (30/50/100 epochs)
python ../scripts/comparison/comparison_fulldata_all.py
# Output: testing/comparison_fulldata_all/

# Compare half data experiments 
python ../scripts/comparison/comparison_halfdata_all.py
# Output: testing/comparison_halfdata_all/

# Compare effect of test augmentation
python ../scripts/comparison/comparison_100ep_augtest.py
# Output: testing/comparison_augtest_epochs/

# Cross-experiment comparison (all configs)
python ../scripts/comparison/cross_experiment_comparison_v2.py
# Output: testing/cross_experiment_comparison_v2/
```

---

## 📁 Folder Structure

```
GPS_bgu_model/
├── experiments/
│   ├── README.md                # This file
│   └── requirements.txt         # Dependencies
├── scripts/
│   ├── training/                # Training scripts (require GPU + data)
│   ├── comparison/              # Comparison scripts (read results only)
│   └── analysis/                # Analysis scripts (read results only)
├── src/core/                    # Model definitions (for training)
│   ├── models.py                # ResNet18, EfficientNet, ConvNeXt
│   ├── loss.py                  # Haversine loss function
│   └── ...
└── results/
    ├── FINAL_SUMMARY.log        # Complete results summary
    ├── *.npy                    # Base model results
    └── testing/                 # All experiment results
        ├── [10 experiment folders]
        └── [9 comparison folders with plots]
```

---

## 🧪 10 Experiments Overview

### Models Tested
| Model | Parameters | Backbone |
|-------|------------|----------|
| **ResNet18** | 11.7M | Residual blocks |
| **EfficientNet-B0** | 5.3M | MBConv + SE attention |
| **ConvNeXt-Tiny** | 28.6M | Modernized ConvNet |

### Experiment Configurations

| # | Experiment | Epochs | LR | Data | Aug Test | Training Script |
|---|------------|--------|-----|------|----------|-----------------|
| 1 | 30ep Full | 30 | 0.001 | 100% | No | `train_efficientnet_30epochs.py` |
| 2 | 30ep Half | 30 | 0.0001 | 50% | No | `train_all_30epochs_lr0.0001_halfdata.py` |
| 3 | 30ep Half+Aug | 30 | 0.0001 | 50% | Yes | `train_30ep_lr0001_halfdata_augtest.py` |
| 4 | 50ep Half | 50 | 0.0001 | 50% | No | `train_50ep_lr0001_halfdata.py` |
| 5 | 50ep Half+Aug | 50 | 0.0001 | 50% | Yes | `train_50ep_lr0001_halfdata_augtest.py` |
| 6 | 50ep Full | 50 | 0.001 | 100% | No | `train_50ep_lr001_fulldata.py` |
| 7 | 100ep Half | 100 | 0.0001 | 50% | No | `train_100ep_lr0001_halfdata.py` |
| 8 | 100ep Half+Aug | 100 | 0.0001 | 50% | Yes | `train_100ep_lr0001_halfdata_augtest.py` |
| 9 | **100ep Full (LR=0.001)** | 100 | 0.001 | 100% | No | `train_100ep_lr001_fulldata.py` |
| 10 | 100ep Full (LR=0.0001) | 100 | 0.0001 | 100% | No | `train_100ep_lr0001_fulldata.py` |

---

## 📈 Key Results

### Top 5 Best Models (by Mean Error)

| Rank | Experiment | Model | Mean | Median | <10m |
|------|------------|-------|------|--------|------|
| 1 | 100ep Full (lr=0.001) | EfficientNet | 5.71m | 2.75m | 86.9% |
| 2 | 50ep Full (lr=0.001) | EfficientNet | 5.93m | 3.31m | 86.3% |
| 3 | 100ep Full (lr=0.0001) | ConvNeXt | 6.12m | 3.65m | 85.0% |
| 4 | 100ep Full (lr=0.0001) | ResNet18 | 6.43m | 3.77m | 82.5% |
| 5 | 100ep Full (lr=0.0001) | EfficientNet | 7.32m | 4.40m | 79.6% |

### Key Findings

1. **Full data >> Half data**: Training on 100% data consistently outperforms 50% data
2. **EfficientNet best overall**: Achieves lowest error with fewest parameters
3. **LR=0.001 better for EfficientNet**: Higher learning rate works well with full data
4. **Test augmentation hurts performance**: Augmenting test images increases error
5. **More epochs help**: 100 epochs > 50 epochs > 30 epochs (with early stopping)

---

## 📜 Scripts Description

### Analysis Scripts (`scripts/analysis/`) - Generate Visualizations

| Script | What it Does | Output Folder |
|--------|--------------|---------------|
| `plot_100ep_analysis.py` | **Main 100ep analysis**: summary table, LR comparison, CDFs, best model detail | `testing/analysis_100epochs/` |
| `plot_50ep_lr001_fulldata.py` | 50ep full data: learning curves, error histogram, boxplot, geographic | `testing/50ep_lr001_fulldata/comparison/` |
| `plot_30ep_lr0001_halfdata_augtest.py` | 30ep augmented test analysis | `testing/30ep_lr0001_halfdata_augtest/comparison/` |
| `plot_50ep_geographic.py` | Geographic error heatmaps for 50ep | `testing/50ep_lr001_fulldata/geographic/` |

### Comparison Scripts (`scripts/comparison/`) - Compare Experiments

| Script | What it Does | Output Folder |
|--------|--------------|---------------|
| `comprehensive_comparison.py` | **Master comparison**: heatmaps, rankings, all dimensions | `testing/comprehensive_comparison/` |
| `comparison_all_experiments.py` | All 10 experiments summary + FINAL ranking | `testing/comparison_all_experiments/` |
| `comparison_fulldata_all.py` | Compare 30/50/100 epochs on full data | `testing/comparison_fulldata_all/` |
| `comparison_halfdata_all.py` | Compare half data experiments ± augmentation | `testing/comparison_halfdata_all/` |
| `comparison_100ep_augtest.py` | Effect of test augmentation at 100 epochs | `testing/comparison_augtest_epochs/` |
| `cross_experiment_comparison_v2.py` | Cross-config comparison with heatmaps | `testing/cross_experiment_comparison_v2/` |
| `focused_comparisons.py` | Targeted comparisons for specific hypotheses | various |

### Training Scripts (`scripts/training/`) - Require GPU + Data

⚠️ **Note**: Training scripts require:
- CUDA-capable GPU
- Dataset at `/path/to/Latest_data/X.npy` and `y.npy`
- `src/core/models.py` and `src/core/loss.py` in path

```bash
# Example: Run training for 100 epochs, LR=0.001, full data
cd GPS_bgu_model/results
python ../scripts/training/train_100ep_lr001_fulldata.py
```

---

## 📂 Result Files Format

Each experiment folder contains per-model results:

```
{experiment}/
├── {Model}/
│   ├── {model}_errors.npy       # Error distances (meters) for each test sample
│   ├── {model}_predictions.npy  # Predicted GPS coordinates (N, 2)
│   ├── {model}_labels.npy       # Ground truth GPS coordinates (N, 2)
│   ├── {Model}_{epochs}ep.pth   # Trained model weights
│   ├── test_results.json        # Summary metrics (mean, median, percentiles)
│   ├── training_history.json    # Loss per epoch
│   └── plots/                   # Training curves, error distributions
├── combined_results.json        # All models combined
├── experiment_log.txt           # Experiment configuration log
└── comparison/                  # Model comparison plots
```

### Example `test_results.json`:
```json
{
  "mean_error": 5.71,
  "median_error": 2.75,
  "std_error": 7.82,
  "min_error": 0.12,
  "max_error": 54.34,
  "within_5m": 71.7,
  "within_10m": 86.9,
  "within_20m": 95.8,
  "p90_error": 11.91,
  "p95_error": 18.01
}
```

---

## 🚀 Running the Scripts - Complete Guide

### Option 1: Just View Results (No Python Required)
The results are already generated! Just open:
- `results/testing/FINAL_SUMMARY.log` - Text summary of all results
- `results/testing/*/` folders contain PNG plots

### Option 2: Regenerate Analysis Plots

```bash
# Install dependencies
pip install numpy matplotlib seaborn

# Navigate to results folder (scripts expect this)
cd GPS_bgu_model/results

# Generate 100 epochs analysis (our best results)
python ../scripts/analysis/plot_100ep_analysis.py

# Generate comprehensive comparison
python ../scripts/comparison/comprehensive_comparison.py

# Compare full data experiments only
python ../scripts/comparison/comparison_fulldata_all.py
```

### Option 3: Re-run Training (Requires GPU)

⚠️ **Prerequisites**:
- NVIDIA GPU with CUDA
- PyTorch with CUDA support
- Dataset files (X.npy, y.npy)

```bash
# Edit the script to update data paths first!
# Then run from the results folder:
cd GPS_bgu_model/results
python ../scripts/training/train_100ep_lr001_fulldata.py
```

---

## 📊 Generated Plots

The comparison scripts generate various plots in `results/testing/`:

- **Error Distribution**: Histograms and box plots of prediction errors
- **Model Comparison**: Bar charts comparing mean/median error across models
- **Geographic Heatmaps**: Spatial distribution of errors on campus map
- **Training Curves**: Loss vs epoch for each model
- **Accuracy Thresholds**: Percentage within 5m, 10m, 20m

---

## 👥 Authors

**Liran Attar** & **Tom Mimran**  
Ben-Gurion University of the Negev  
Deep Learning Course - January 2026
