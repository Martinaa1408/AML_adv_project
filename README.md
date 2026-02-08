# AML_adv_project - HAM10000 Skin Lesion Classification

This project addresses **multi-class skin lesion classification** on the **HAM10000** dermatoscopic dataset using a pretrained convolutional neural network with explicit handling of **class imbalance**.

The focus is on **robust evaluation**, **reproducibility**, and **error analysis**, rather than aggressive overfitting.

---

## Dataset
- **HAM10000** (Skin Cancer MNIST)
- 10,015 dermatoscopic images
- 7 diagnostic classes:
  - akiec, bcc, bkl, df, mel, nv, vasc
- Strong class imbalance (melanocytic nevi dominate)

Class distribution is shown below:

<p align="center">
  <img src="figures/class_distribution.png" width="500">
</p>

---

## Method
- **Backbone**: EfficientNet-B0 (ImageNet pretrained)
- **Image size**: 224×224
- **Augmentation**: strong geometric and photometric transformations
- **Loss**: Weighted Cross-Entropy
- **Optimizer**: AdamW
- **Scheduler**: Cosine Annealing
- **Mixed precision**: enabled (AMP)
- **Model selection**: best checkpoint by **Validation Macro-F1**

---

## Evaluation Protocol
Due to class imbalance, standard accuracy is insufficient.

**Primary metrics**
- Macro-F1
- Balanced Accuracy

**Additional analysis**
- Confusion matrix
- Misclassification inspection
- Grad-CAM explainability

---

## Results
| Metric | Value |
|------|------|
| Best Validation Macro-F1 | ~0.75 |
| Test Macro-F1 | ~0.74 |
| Test Balanced Accuracy | ~0.80 |

Confusion matrix on the test set:

<p align="center">
  <img src="figures/confusion_matrix.png" width="500">
</p>

---

## Error Analysis
Most misclassifications occur between **visually similar lesions**, particularly:
- melanocytic nevi ↔ melanoma
- benign keratosis ↔ melanoma

Example misclassified samples:

<p align="center">
  <img src="figures/misclassifications_grid.png" width="700">
</p>

---

## Explainability (Grad-CAM)
Grad-CAM was applied to highlight image regions contributing to predictions.

<p align="center">
  <img src="figures/gradcam_examples.png" width="700">
</p>

The model focuses on lesion cores and borders, consistent with dermatological criteria.

---

## Repository Structure

ham10000-skin-lesion-classification/
├─ README.md
│  Project overview, methodology, results, and usage instructions
│
├─ requirements.txt
│  Python dependencies required to run the notebook
│
├─ .gitignore
│  Files and folders excluded from version control
│
├─ notebooks/
│  └─ HAM10000_Classification.ipynb
│     End-to-end pipeline: data loading, training, evaluation, and analysis
│
├─ data/
│  ├─ splits/
│  │  ├─ train.csv
│  │  ├─ val.csv
│  │  └─ test.csv
│  │     Fixed stratified train/validation/test splits for reproducibility
│  │
│  └─ metadata/
│     └─ HAM10000_metadata.csv
│        Original dataset metadata and labels
│
├─ results/
│  ├─ metrics.json
│  │  Final evaluation metrics on the test set
│  │
│  ├─ history.csv
│  │  Training and validation metrics recorded per epoch
│  │
│  ├─ classification_report.txt
│  │  Per-class precision, recall, and F1-score
│  │
│  └─ checkpoints/
│     └─ best_model.pt
│        Best model checkpoint selected by validation Macro-F1
│
├─ figures/
│  ├─ class_distribution.png
│  │  Class imbalance visualization
│  │
│  ├─ samples_grid.png
│  │  Example images for each class
│  │
│  ├─ confusion_matrix.png
│  │  Confusion matrix on the test set
│  │
│  ├─ misclassifications_grid.png
│  │  Qualitative error analysis
│  │
│  └─ gradcam_examples.png
│     Grad-CAM visual explanations
│
└─ LICENSE
   Project license (optional)

## How to Run

This project is designed to be executed in **Google Colab**.

1. Open `notebooks/HAM10000_Classification.ipynb`
2. Enable GPU runtime  
   `Runtime → Change runtime type → GPU`
3. Upload your `kaggle.json` file when prompted
4. Run all cells in order

All results (metrics, figures, and checkpoints) are automatically saved.

---

## Results Summary

The final model was selected based on **validation Macro-F1**.

- Best Validation Macro-F1: ~0.75  
- Test Macro-F1: ~0.74  
- Test Balanced Accuracy: ~0.80  

The confusion matrix and qualitative analyses highlight that most errors occur
between visually similar lesion types, reflecting the intrinsic difficulty of
dermatoscopic diagnosis.

---

## Limitations and Future Work

- The dataset presents strong class imbalance, which remains a challenge
  despite weighted loss strategies.
- No ensemble or multi-scale inference was used.
- The model relies solely on image data without clinical metadata.

Future improvements may include:
- Ensemble models
- Advanced imbalance-aware losses
- Integration of patient-level metadata

---

## Notes

This project was developed for educational purposes.
The focus is on reproducibility, clarity, and realistic evaluation rather than
leaderboard-oriented optimization.

---

## License

This project is released under the MIT License.

