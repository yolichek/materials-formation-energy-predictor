# Materials Formation Energy Predictor
> Predicting DFT-computed formation energy from chemical composition and simple structural descriptors using machine learning.

## 🎯 Overview
This project demonstrates a **physics-informed machine learning workflow** for predicting formation energy per atom (eV/atom) from materials data — directly applicable to high-throughput materials screening, discovery pipelines, and structure-property relationship modeling.

Built with: `pymatgen` + `matminer` + `scikit-learn` + `Python`

## 📊 Results
| Metric | Value | Context |
|--------|-------|---------|
| **MAE** | 0.252 eV/atom | Screening-quality prediction (target: <0.3 eV/atom) |
| **R²** | 0.783 | Explains 78% of variance in formation energy |
| **Baseline MAE** | 0.577 eV/atom | Mean predictor — our model improves by 56% |
| **Samples** | 18,928 perovskites | Real DFT data from Materials Project (matbench) |
| **Features** | 138 total | 135 Magpie ElementProperty + 3 structure-based |

### Top 10 Feature Importances
| Rank | Feature | Importance | Physical Interpretation |
|------|---------|------------|------------------------|
| 1 | `volume_per_atom` | 18.3% | Atomic packing density drives stability |
| 2 | `MagpieData range GSvolume_pa` | 14.1% | Elemental volume variance affects bonding |
| 3 | `MagpieData mean GSbandgap` | 10.0% | Electronic structure influences energetics |
| 4 | `space_group` | 7.0% | Crystal symmetry correlates with stability |
| 5 | `MagpieData mean MeltingT` | 2.9% | Thermal stability proxy |

> ✅ **Physics validation**: Top features align with first-principles understanding of formation energy (packing, electronic structure, symmetry).

## 🔬 Methodology

### 1. Data Source
- **Dataset**: `matbench_perovskites` via `matminer` (Materials Project DFT calculations)
- **Target**: Formation energy per atom (`e_form`, eV/atom)
- **Samples**: 18,928 perovskite structures

### 2. Featurization
- **Composition features**: Magpie ElementProperty preset (135 descriptors)
  - Statistics of elemental properties: electronegativity, atomic radius, ionization energy, electron affinity, melting point, bandgap, etc.
- **Structure features** (added for physics grounding):
  - `volume_per_atom`: Crystal volume normalized by atom count
  - `space_group`: Symmetry number via `SpacegroupAnalyzer`
  - `packing_fraction`: Estimated atomic packing efficiency

### 3. Model
- **Algorithm**: Random Forest Regressor (`scikit-learn`)
- **Hyperparameters**: `n_estimators=200`, `max_depth=30`, `min_samples_leaf=5`
- **Split**: 80/20 random train/test (scaffold splitting recommended for production)

### 4. Evaluation
- **Metrics**: Mean Absolute Error (MAE), R² coefficient of determination
- **Baseline**: Mean predictor (R²=0 by definition)
- **Improvement**: 0.325 eV/atom MAE reduction vs. baseline

## 🚀 Reproducibility

### Option A: Conda (Recommended)
```bash
conda env create -f environment.yml
conda activate materials-ml
jupyter notebook notebook.ipynb