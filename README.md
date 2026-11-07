# Fairness Gaps in Multi-Label Chest X-Ray Classifiers

This project studies demographic fairness in a multi-label chest X-ray classifier. It evaluates true-positive-rate (TPR) disparities separately for each of the 14 NIH ChestX-ray14 pathologies, tests post-hoc equal-opportunity threshold calibration, and uses Grad-CAM to qualitatively compare model attention across demographic groups.

## Study overview

- **Dataset:** NIH ChestX-ray14, containing 112,120 frontal radiographs from 30,805 patients and labels for 14 pathologies.
- **Model:** ImageNet-pretrained DenseNet-121 with one output per pathology. “No Finding” is treated as an all-negative example, not a fifteenth target.
- **Demographic groups:** Sex and four age bins: under 40, 40–59, 60–79, and 80+.
- **Evaluation:** Per-pathology AUC-ROC and test-set TPR disparities, with bootstrap confidence intervals. Cells with fewer than five positive test cases are treated as underpowered.
- **Additional analyses:** Group-specific threshold calibration and qualitative Grad-CAM comparisons for selected high-disparity pathologies.

## Main findings

- Mean AUC-ROC was **0.807** using TenCrop test-time augmentation.
- Sex-related disparities were pathology-specific and bidirectional: the largest reported gap was for Pneumothorax (**0.130**, with male patients having lower TPR), followed by Fibrosis (**0.113**, with female patients having lower TPR).
- Age-related disparities were larger on average than sex-related disparities. The largest reported age gap was **0.276** for Atelectasis, with the 80+ group most disadvantaged.
- Equal-opportunity threshold calibration was not a reliable general-purpose mitigation: results varied by pathology and demographic axis, and some sex-based gaps worsened.
- Grad-CAM comparisons provide qualitative hypotheses about model attention; they do not establish causal explanations.

## Data and reproduction

The dataset and model weights are not included in this repository. Obtain the NIH ChestX-ray14 files and any required checkpoint through their approved sources, and follow their access and use terms. The analysis uses the dataset metadata, official train/validation and test lists, and image files. The reported experiments ran in Kaggle with two NVIDIA T4 GPUs.

The notebook currently uses Kaggle-mounted paths for the data and checkpoint. Update those paths for your own environment before running it. Install the Python packages used by the notebook in an environment appropriate for your Python and CUDA setup:

```bash
pip install numpy pandas matplotlib seaborn pillow scikit-learn torch torchvision tqdm grad-cam
```

Run the notebook cells in order after configuring the data and checkpoint paths. This checkout currently has no notebook tracked; place the complete, reviewed notebook in `Notebook/` before running the analysis.

## Limitations

The dataset does not provide race or ethnicity labels, and its radiology labels were derived automatically and may contain errors. Some demographic/pathology subgroups have few positive examples. The Grad-CAM analysis is qualitative and cannot distinguish model bias from clinical or imaging differences on its own. These results are research findings, not clinical guidance.