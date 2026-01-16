# 📊 GPS Localization Experiments - Results & Analysis

This folder contains all results from our comprehensive experiments on Campus Image-to-GPS Regression. We tested **3 model architectures** across **10 different configurations** to find the optimal setup for predicting GPS coordinates from campus images.

---

## 🏆 Overall Best Result

| Metric | Value |
|--------|-------|
| **Model** | EfficientNet-B0 |
| **Configuration** | 100 epochs, LR=0.001, Full Data |
| **Mean Error** | **5.71 meters** |
| **Median Error** | 2.75 meters |
| **Within 5m** | 71.7% |
| **Within 10m** | 86.9% |
| **Within 20m** | 95.8% |

This means our best model can predict your location on campus to within **~6 meters on average**, with nearly **87% of predictions within 10 meters** of the true location.

---

## 🧠 Experiment Design & Thinking

### Why These 3 Models?

| Model | Parameters | Why We Chose It |
|-------|------------|-----------------|
| **ResNet18** | 11.7M | Classic baseline - well-understood, fast training, good for comparison |
| **EfficientNet-B0** | 5.3M | Modern efficient architecture - best accuracy/parameter ratio |
| **ConvNeXt-Tiny** | 28.6M | Latest (2022) architecture - modernized ConvNet with transformer insights |

### Why These Configurations?

We designed experiments to answer specific questions:

#### Question 1: How much data do we need?
- **Full Data (100%)**: ~3,600 training images
- **Half Data (50%)**: ~1,800 training images
- **Finding**: Full data is significantly better (5.71m vs 9.62m best results)

#### Question 2: What learning rate works best?
- **LR=0.001**: Faster learning, but may overshoot
- **LR=0.0001**: Slower, more stable convergence
- **Finding**: Depends on model - EfficientNet prefers 0.001, ConvNeXt needs 0.0001

#### Question 3: Do more epochs help?
- **30 epochs**: Quick training baseline
- **50 epochs**: Medium training
- **100 epochs**: Extended training with early stopping
- **Finding**: Yes! 100 epochs consistently outperforms 30/50 epochs

#### Question 4: Does test-time augmentation help?
- **Regular Test**: Test on original images
- **Augmented Test**: Test on augmented images (different viewpoints)
- **Finding**: NO! Augmented test significantly hurts performance (simulates unseen locations)

---

## 📋 All 10 Experiments Summary

### Experiment 1: 30ep Full Data (LR=0.001)
**Folder:** `testing/30epochs/`
**Thinking:** Baseline with standard settings to establish reference performance.
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| EfficientNet | 10.22m | 69.3% |
| ResNet18 | 29.73m | 22.5% |
| ConvNeXt | 70.18m | 4.6% |

**Insight:** EfficientNet clearly superior. ConvNeXt fails with high LR - too unstable.

---

### Experiment 2: 30ep Half Data (LR=0.0001)
**Folder:** `testing/30epochs_lr0.0001_halfdata/`
**Thinking:** Test if lower LR helps with less data, prevent overfitting.
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| ConvNeXt | 10.92m | 59.1% |
| ResNet18 | 12.66m | 47.8% |
| EfficientNet | 14.64m | 44.9% |

**Insight:** Lower LR helps ConvNeXt dramatically (70m→11m). Models more balanced.

---

### Experiment 3: 30ep Half Data + Augmented Test (LR=0.0001)
**Folder:** `testing/30ep_lr0001_halfdata_augtest/`
**Thinking:** Does augmenting test images simulate real-world variation?
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| ConvNeXt | 18.55m | 37.0% |
| ResNet18 | 29.90m | 26.4% |
| EfficientNet | 32.97m | 23.4% |

**Insight:** Augmented test HURTS performance significantly. Models memorize specific viewpoints.

---

### Experiment 4: 50ep Half Data (LR=0.0001)
**Folder:** `testing/50ep_lr0001_halfdata/`
**Thinking:** Do more epochs help with limited data?
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| ConvNeXt | 11.63m | 52.9% |
| ResNet18 | 13.40m | 53.6% |
| EfficientNet | 13.40m | 46.0% |

**Insight:** Slight improvement over 30ep. ConvNeXt still leads with half data.

---

### Experiment 5: 50ep Half Data + Augmented Test (LR=0.0001)
**Folder:** `testing/50ep_lr0001_halfdata_augtest/`
**Thinking:** Does more training help with augmented test?
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| ConvNeXt | 16.70m | 44.7% |
| ResNet18 | 25.42m | 28.2% |
| EfficientNet | 29.60m | 25.6% |

**Insight:** Still bad. Augmented test is fundamentally problematic.

---

### Experiment 6: 50ep Full Data (LR=0.001)
**Folder:** `testing/50ep_lr001_fulldata/`
**Thinking:** Full data + moderate training - good balance?
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| EfficientNet | **5.93m** | **86.3%** |
| ResNet18 | 12.41m | 61.1% |
| ConvNeXt | 81.58m | 1.5% |

**Insight:** EfficientNet achieves excellent results! ConvNeXt still fails with high LR.

---

### Experiment 7: 100ep Half Data (LR=0.0001)
**Folder:** `testing/100ep_lr0001_halfdata/`
**Thinking:** Maximum epochs with half data - can we close the gap?
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| ConvNeXt | 9.62m | 62.8% |
| EfficientNet | 10.67m | 60.2% |
| ResNet18 | 11.34m | 69.7% |

**Insight:** Best half-data results. Still ~4m worse than full data.

---

### Experiment 8: 100ep Half Data + Augmented Test (LR=0.0001)
**Folder:** `testing/100ep_lr0001_halfdata_augtest/`
**Thinking:** Final test of augmented approach with maximum training.
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| ConvNeXt | 14.35m | 49.5% |
| EfficientNet | 24.34m | 35.5% |
| ResNet18 | 24.38m | 34.8% |

**Insight:** Confirms augmented test is not useful for this task.

---

### Experiment 9: 100ep Full Data (LR=0.001) ⭐ BEST
**Folder:** `testing/100ep_lr001_fulldata/`
**Thinking:** Maximum training + full data + optimal LR for EfficientNet.
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| **EfficientNet** | **5.71m** | **86.9%** |
| ResNet18 | 12.99m | 63.5% |
| ConvNeXt | 81.82m | 1.3% |

**Insight:** 🏆 **BEST OVERALL RESULT!** EfficientNet peaks here. ConvNeXt needs lower LR.

---

### Experiment 10: 100ep Full Data (LR=0.0001)
**Folder:** `testing/100ep_lr0001_fulldata/`
**Thinking:** Test lower LR with full data - better for ConvNeXt?
**Results:**
| Model | Mean Error | Within 10m |
|-------|------------|------------|
| ConvNeXt | 6.12m | 85.0% |
| ResNet18 | 6.43m | 82.5% |
| EfficientNet | 7.32m | 79.6% |

**Insight:** ConvNeXt excels here! All models perform well. Lower LR benefits ConvNeXt greatly.

---

## 📈 Comparison Summaries

### By Epochs (Full Data, LR=0.001)
| Epochs | Best Model | Mean Error | Improvement |
|--------|------------|------------|-------------|
| 30 | EfficientNet | 10.22m | baseline |
| 50 | EfficientNet | 5.93m | -4.29m (42%) |
| 100 | EfficientNet | 5.71m | -0.22m (4%) |

**Conclusion:** Biggest gains from 30→50 epochs. Diminishing returns after 50.

---

### By Data Amount (100 epochs, LR=0.0001)
| Data | Best Model | Mean Error | Difference |
|------|------------|------------|------------|
| Full (100%) | ConvNeXt | 6.12m | baseline |
| Half (50%) | ConvNeXt | 9.62m | +3.50m (57% worse) |

**Conclusion:** More data = significantly better results. No substitute for data quantity.

---

### By Learning Rate (100 epochs, Full Data)
| LR | Best Model | Mean Error |
|----|------------|------------|
| 0.001 | EfficientNet | 5.71m |
| 0.0001 | ConvNeXt | 6.12m |

**Conclusion:** Optimal LR depends on architecture:
- EfficientNet/ResNet18: Prefer LR=0.001
- ConvNeXt: REQUIRES LR=0.0001 (fails catastrophically with 0.001)

---

### Augmented vs Regular Test
| Test Type | Best Model | Mean Error |
|-----------|------------|------------|
| Regular | EfficientNet | 5.71m |
| Augmented | ConvNeXt | 14.35m |

**Conclusion:** Test augmentation hurts performance. Models learn specific viewpoints, not general location features.

---

## 🔬 Model-Specific Analysis

### EfficientNet-B0 ⭐ Recommended
- **Best at:** High LR (0.001), full data, long training
- **Strength:** Best accuracy/parameter ratio, consistent performance
- **Weakness:** Struggles with augmented test
- **Best Config:** 100ep, LR=0.001, Full Data → **5.71m**

### ResNet18
- **Best at:** Stable training across all configurations
- **Strength:** Fast, reliable, no catastrophic failures
- **Weakness:** Never reaches top performance
- **Best Config:** 100ep, LR=0.0001, Full Data → **6.43m**

### ConvNeXt-Tiny
- **Best at:** Low LR (0.0001), especially with limited data
- **Strength:** Best half-data performance, modern architecture
- **Weakness:** FAILS with LR=0.001 (70-80m errors!)
- **Best Config:** 100ep, LR=0.0001, Full Data → **6.12m**

---

## 📁 Folder Structure Explained

```
results/
├── *.npy                        # Base model results (11 files)
│   ├── min_val.npy              # Min normalization values
│   ├── max_val.npy              # Max normalization values
│   ├── resnet18_errors.npy      # ResNet18 error distances
│   ├── resnet18_predictions.npy # ResNet18 predictions
│   ├── resnet18_labels.npy      # ResNet18 ground truth
│   ├── efficientnet_*.npy       # EfficientNet results
│   └── convnext_*.npy           # ConvNeXt results
│
testing/                         # All experiment results
    │
    ├── [EXPERIMENT FOLDERS]     # One per experiment configuration
    │   ├── ResNet18/
    │   │   ├── resnet18_errors.npy      # Error distance per sample (meters)
    │   │   ├── resnet18_predictions.npy # Predicted GPS [N, 2] (lat, lon)
    │   │   ├── resnet18_labels.npy      # True GPS [N, 2] (lat, lon)
    │   │   ├── test_results.json        # Summary metrics
    │   │   └── training_history.json    # Loss per epoch
    │   ├── EfficientNet/                # Same structure
    │   ├── ConvNeXt/                    # Same structure
    │   ├── geographic_analysis/         # Error heatmaps, arrows
    │   └── combined_results.json        # All models combined
    │
    ├── [COMPARISON FOLDERS]     # Cross-experiment comparisons
    │   ├── comparison_fulldata_all/     # Full data experiments
    │   ├── comparison_halfdata_all/     # Half data experiments
    │   ├── comparison_augtest_epochs/   # Augmented test analysis
    │   ├── comprehensive_comparison/    # Master comparison
    │   └── analysis_100epochs/          # 100ep detailed analysis
    │
    └── FINAL_SUMMARY.log        # Complete text summary
```

---

## 📊 File Types Explained

| Extension | Count | Description |
|-----------|-------|-------------|
| `.png` | 334 | Visualization plots (histograms, CDFs, heatmaps, comparisons) |
| `.npy` | 119 | NumPy arrays with raw prediction data |
| `.json` | 83 | Metrics and training history |
| `.txt` | 17 | Text summaries and logs |
| `.pth` | 30 | Model weights (not in zip, ~1.7GB total) |

---

## 🎯 Key Takeaways

### What Works ✅
1. **EfficientNet-B0** is the best model for this task
2. **Full dataset** is essential - half data loses ~4m accuracy
3. **100 epochs** with early stopping gives best results
4. **LR=0.001** for EfficientNet, **LR=0.0001** for ConvNeXt

### What Doesn't Work ❌
1. **Test augmentation** - models memorize viewpoints, not locations
2. **High LR for ConvNeXt** - causes complete training failure
3. **30 epochs** - insufficient for convergence on this task

### Surprising Findings 🤔
1. ConvNeXt (28.6M params) isn't better than EfficientNet (5.3M params)
2. Learning rate matters MORE than architecture choice
3. The gap between full and half data is larger than expected

---

## 📈 Visual Results Guide

### Geographic Analysis (`geographic_analysis/` folders)
- `*_error_heatmap.png` - Where errors are highest on campus
- `*_prediction_arrows.png` - Direction of prediction errors
- `*_best_worst.png` - Best and worst prediction locations

### Comparison Plots (`comparison_*/` folders)
- `*_summary_table.png` - Quick metrics comparison
- `*_cdf*.png` - Cumulative error distribution
- `*_boxplot*.png` - Error distribution box plots
- `*_bar*.png` - Mean error comparisons

### Training Plots (within model folders)
- `training_history.json` - Loss curves data
- `test_results.json` - Final metrics

---

## 👥 Authors

**Liran Attar** & **Tom Mimran**  
Ben-Gurion University of the Negev  
Deep Learning Course - January 2026
