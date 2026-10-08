# Kepler Exoplanet Candidate Classification

This project uses machine learning to classify Kepler Objects of Interest (KOIs) as either **planet candidates** or **false positives**. It was completed as an individual project for a Machine Learning course.

Kepler detected possible exoplanets by looking for temporary decreases in a star's brightness. However, similar signals can also be caused by eclipsing binary stars, background objects, stellar variability, or instrumental effects. The aim of this project is to investigate whether measurements of the detected signal and its host star can be used to distinguish candidates from false positives.

## Dataset

The dataset is the [Kepler Objects of Interest cumulative table](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=cumulative) from the NASA Exoplanet Archive.

- 9,564 observations
- 141 original columns
- 4,717 candidates
- 4,847 false positives
- Target: `koi_pdisposition`

The model uses 11 numerical features describing the transit signal, estimated object properties, signal quality, and host star:

`koi_period`, `koi_impact`, `koi_duration`, `koi_depth`, `koi_prad`, `koi_teq`, `koi_insol`, `koi_model_snr`, `koi_steff`, `koi_slogg`, and `koi_srad`.

Columns that directly reveal or strongly encode the assigned disposition were excluded to avoid data leakage.

## Methods

Two classification methods were compared:

1. **Logistic Regression** — used as a simple linear baseline.
2. **Random Forest** — used to model nonlinear relationships and interactions between features.

Missing values were replaced with training-set medians. The Logistic Regression features were also standardized. The data was split by the host-star identifier `kepid`, ensuring that observations associated with the same star did not appear in different subsets.

| Subset | Observations |
|---|---:|
| Training | 6,695 |
| Validation | 1,424 |
| Test | 1,445 |

## Results

| Method | Training accuracy | Validation accuracy | Validation error |
|---|---:|---:|---:|
| Logistic Regression | 76.10% | 76.33% | 23.67% |
| Random Forest | 100.00% | 83.36% | 16.64% |

Random Forest was selected because it achieved the lower validation error. On the held-out test set, it achieved:

- **Test accuracy:** 80.42%
- **Test error:** 19.58%
- **F1-score:** 0.80 for both classes

The perfect training accuracy and lower validation and test accuracy indicate that the Random Forest overfits the training data. Despite this, it performed better on the validation set than Logistic Regression.

## Repository contents

```text
.
├── main.ipynb          # Data analysis, preprocessing, training, and evaluation
├── koi_cumulative.csv  # KOI cumulative table
└── README.md
```


## Running the notebook

1. Download the contents of this repository.
2. Install the required packages:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn jupyter
```

3. Start Jupyter and run the notebook cells in order:

```bash
jupyter notebook
```

## Limitations

The target labels are Kepler dispositions, not direct confirmation that an exoplanet exists. The project also uses derived tabular measurements rather than the original stellar light curves. The model should therefore be treated as a classification experiment rather than a system for confirming exoplanets.

## References

- [NASA Exoplanet Archive — Kepler Objects of Interest](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=cumulative)
- [NASA Exoplanet Archive — KOI column definitions](https://exoplanetarchive.ipac.caltech.edu/docs/API_kepcandidate_columns.html)
- [Scikit-learn documentation](https://scikit-learn.org/stable/)
