# 🏥 🧠 Uncertainty Quantification for Pediatric Brain Cancer Prognosis

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Made with Jupyter](https://img.shields.io/badge/Made%20with-Jupyter-orange?logo=Jupyter)](https://jupyter.org/try)

**Master's Thesis Project**  
*University of Neuchâtel - Data Science & AI*  
**Author:** Jules Odje 
**Date:** November 2024

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Key Findings](#-key-findings)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Usage](#-usage)
- [Results](#-results)
- [Methodology](#-methodology)
- [Citation](#-citation)
- [License](#-license)
- [Contact](#-contact)

---

## 🎯 Overview

This project investigates **uncertainty quantification (UQ)** methods for machine learning models in medical prognosis, specifically focusing on brain cancer survival prediction. 

### Why This Matters

Machine learning models in healthcare often produce confident predictions without quantifying their uncertainty. This poses serious risks:
- **Overconfident predictions** can lead to incorrect treatment decisions
- **Lack of uncertainty awareness** prevents safe clinical deployment
- **No mechanism to flag ambiguous cases** for expert review

### Our Approach

We implement and compare three complementary uncertainty quantification methods:

1. **Conformal Prediction** - Provides prediction sets with mathematical coverage guarantees
2. **Bayesian Inference** - Quantifies uncertainty through probability distributions
3. **Model Calibration** - Ensures predicted probabilities reflect true likelihoods

---

## 🔥 Key Findings

### 🚨 The Overconfidence Paradox

We discovered a **counterintuitive phenomenon**:

> Cases where BOTH UQ methods flagged as "LOW confidence" achieved **91.7% accuracy**  
> Cases where BOTH flagged as "HIGH confidence" achieved only **50% accuracy**

This reveals that:
- Models can be **dangerously overconfident** on difficult cases
- **Honest uncertainty** (P ≈ 0.5) paradoxically indicates more reliable predictions
- Multiple UQ methods are **essential** to distinguish overconfidence from true uncertainty

### 📊 Performance Summary

| UQ Method | High Confidence Rate | Key Metric | Advantage |
|-----------|---------------------|------------|-----------|
| Conformal Prediction | 65.9% (29/44) | 68.2% coverage | Mathematical guarantees |
| Bayesian Inference | 25.0% (11/44) | Std = 0.261 | Full distributions |
| Calibration | Baseline best | Brier = 0.244 | Natural calibration |

### 🏥 Clinical Impact

Based on our findings, we propose a **revised 3-tier clinical decision framework**:
```
✅ TIER 1 (27%): Safe Automation - Both methods LOW confidence → 92% accuracy
⚠️  TIER 2 (54%): Assisted Review - Methods disagree → 50% accuracy  
🚨 TIER 3 (18%): Senior Escalation - Both methods HIGH confidence → 50% accuracy
```

**Impact:** This inverted strategy achieves both higher safety (92% vs 50%) and greater efficiency (27% vs 18% automation).

---

## 📁 Project Structure
```
brain-cancer-uncertainty-quantification/
│
├── README.md                          # 👈 You are here
├── requirements.txt                   # Python dependencies
├── LICENSE                            # MIT License
│
├── data/                              # Dataset (not included for privacy)
│   ├── README.md                      # Data description
│   └── .gitkeep
│
├── notebooks/                         # 📓 Jupyter/Colab Notebooks
│   ├── 01_data_preprocessing.ipynb       # Data cleaning & feature engineering
│   ├── 02_baseline_model.ipynb           # Random Forest baseline
│   ├── 03_conformal_prediction.ipynb     # Conformal UQ method
│   ├── 04_bayesian_inference.ipynb       # Bayesian UQ method
│   ├── 05_calibration.ipynb              # Calibration analysis
│   └── 06_comparison.ipynb               # Comparative analysis
│
├── src/                               # 🐍 Python Source Code
│   ├── __init__.py
│   ├── preprocessing.py               # Data preprocessing functions
│   ├── models.py                      # Model definitions
│   ├── conformal.py                   # Conformal prediction implementation
│   ├── bayesian.py                    # Bayesian inference implementation
│   └── calibration.py                 # Calibration methods
│
├── results/                           # 📊 Results & Outputs
│   ├── README.md                      # ⭐ Detailed results documentation
│   ├── figures/                       # All generated plots
│   ├── tables/                        # CSV results tables
│   └── models/                        # Trained models (.pkl)
│
├── docs/                              # 📚 Documentation
│   ├── methodology.md                 # Detailed methodology
│   └── clinical_framework.md          # Clinical decision framework
│
└── tests/                             # ✅ Unit tests (optional)
    └── test_conformal.py
```

---

## 🔧 Installation

### Prerequisites

- Python 3.10 or higher
- pip package manager
- (Optional) Google Colab account for cloud execution

### Setup Instructions

1. **Clone the repository**
```bash
git clone https://github.com/OJules/lung-cancer-uncertainty-quantification.git
cd lung-cancer-uncertainty-quantification
```

2. **Create a virtual environment** 
```bash
# Using venv
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Or using conda
conda create -n lung-cancer-uq python=3.10
conda activate lung-cancer-uq
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Verify installation**
```bash
python -c "import sklearn, numpy, pandas; print('✅ Installation successful!')"
```

---

## 🚀 Usage

### Quick Start (Google Colab)

The easiest way to run the project is via Google Colab:

1. Open any notebook in `notebooks/`
2. Click "Open in Colab" badge
3. Run all cells

### Local Execution
```bash
# Navigate to notebooks directory
cd notebooks/

# Launch Jupyter
jupyter notebook

# Or use JupyterLab
jupyter lab
```

### Running the Full Pipeline

Execute notebooks in order:
```bash
01_data_preprocessing.ipynb      # ~2 minutes
02_baseline_model.ipynb          # ~3 minutes
03_conformal_prediction.ipynb    # ~2 minutes
04_bayesian_inference.ipynb      # ~2 minutes
05_calibration.ipynb             # ~1 minute
06_comparison.ipynb              # ~1 minute
```

**Total runtime:** ~15 minutes on Google Colab (free tier)

---

## 📊 Results

All results are documented in detail in [`results/README.md`](results/README.md).

### Quick Summary

**Baseline Model:**
- Accuracy: 61.4%
- AUC-ROC: 0.598
- Brier Score: 0.244

**Best UQ Configuration:**
- Conformal: 68.2% coverage (90% target)
- Bayesian: 25% high confidence (adaptive thresholds)
- Calibration: Already optimal (ECE = 0.042)

### Key Visualizations

<p align="center">
  <img src="results/figures/uq_methods_comparison.png" width="800" alt="UQ Methods Comparison">
  <br>
  <em>Figure 1: Comparison of three uncertainty quantification methods</em>
</p>

<p align="center">
  <img src="results/figures/uq_paradox_analysis.png" width="800" alt="Overconfidence Paradox">
  <br>
  <em>Figure 2: The overconfidence paradox - low confidence cases achieve higher accuracy</em>
</p>

For detailed results, see **[results/README.md](results/README.md)**.

---

## 🔬 Methodology

### Dataset

- **Source:** cBioPortal - Pediatric Brain Tumor Studies
- **Tumor Types:** Glioblastoma and Astrocytoma
- **Size:** 218 pediatric patients (174 training, 44 test)
- **Features:** 23 clinical and genomic features (mutation status, 
  tumor characteristics, demographics)
- **Target:** 5-year survival (binary: long-term vs short-term survivors)
- **Age Range:** Pediatric population

### Models

**Baseline:** Random Forest Classifier
- n_estimators: 100
- max_depth: 5
- min_samples_split: 10

**Optimization:** RandomizedSearchCV with 3-fold cross-validation

### Uncertainty Quantification Methods

#### 1. Conformal Prediction
- **Framework:** Split conformal prediction
- **Significance levels:** α = 0.05, 0.10
- **Output:** Prediction sets {0}, {1}, or {0,1}

#### 2. Bayesian Inference
- **Approach:** Bootstrap aggregation via Random Forest trees
- **Output:** Probability distributions with mean, std, credible intervals
- **Thresholds:** Adaptive (percentile-based)

#### 3. Model Calibration
- **Methods:** Platt Scaling, Isotonic Regression
- **Metrics:** Brier Score, Expected Calibration Error (ECE)
- **Visualization:** Reliability diagrams

### Evaluation Metrics

- **Accuracy, AUC-ROC** (model performance)
- **Coverage, Set Size** (conformal prediction)
- **Std, Entropy, Credible Intervals** (Bayesian)
- **Brier Score, ECE** (calibration)

For complete methodology, see **[docs/methodology.md](docs/methodology.md)**.

---

## 🎓 Citation

If you use this work in your research, please cite:
```bibtex
@mastersthesis{Odje2024uncertainty,
  title={Uncertainty Quantification for Machine Learning in Medical Prognosis: 
         A Case Study on Pediatric Brain Cancer Survival Prediction},
  author={Odje, Jules },
  year={2024},
  school={University of Neuchâtel},
  type={Master's Thesis},
  address={Neuchâtel, Switzerland}
}
```

---

## 📚 References

### Key Papers

1. **Conformal Prediction:**
   - Angelopoulos, A. N., & Bates, S. (2021). *A Gentle Introduction to Conformal Prediction and Distribution-Free Uncertainty Quantification*. arXiv:2107.07511

2. **Calibration:**
   - Guo, C., et al. (2017). *On Calibration of Modern Neural Networks*. ICML 2017

3. **Bayesian Deep Learning:**
   - Kendall, A., & Gal, Y. (2017). *What Uncertainties Do We Need in Bayesian Deep Learning for Computer Vision?* NIPS 2017

4. **Medical AI:**
   - Kompa, B., et al. (2021). *Second Opinion Needed: Communicating Uncertainty in Medical Machine Learning*. npj Digital Medicine

---

## 🤝 Contributing

Contributions are welcome! Please feel free to:

- 🐛 Report bugs via [Issues](https://github.com/[username]/lung-cancer-uncertainty-quantification/issues)
- 💡 Suggest improvements
- 🔧 Submit pull requests

### Development Setup
```bash
# Install development dependencies
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Check code style
flake8 src/
black src/
```

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

**Note:** The dataset is not included in this repository due to privacy considerations. Please refer to the original data source for access.

---

## 📧 Contact

**Jules Odje**

- 🎓 Master's Student in Data Science & AI
- 🏛️ University of Neuchâtel, Switzerland
- 📧 Email: odjejulesgeraud@gmail.com
- 💼 LinkedIn: [linkedin.com/in/jules-odje](https://linkedin.com/in/jules-odje)
- 🐙 GitHub: [@JulesOdje](https://github.com/OJules)

---

## 🙏 Acknowledgments

- **University of Neuchâtel** for academic support
- **cBioPortal** for providing the lung cancer dataset
- **Scikit-learn contributors** for excellent ML tools

---

## 📈 Project Status

- ✅ Data preprocessing completed
- ✅ Baseline model optimized
- ✅ Three UQ methods implemented
- ✅ Comparative analysis done
- ✅ Clinical framework proposed

**Last Updated:** November 2024

---

<p align="center">
  <strong>⭐ If you find this project useful, please consider giving it a star! ⭐</strong>
</p>

<p align="center">
  Made with ❤️ for safer medical AI
</p>
