# ADHD Machine Learning Project

This project aims to analyze a dataset related to ADHD and mental health, perform data preprocessing, explore clustering with PCA, and build machine learning models to predict ADHD.

## Project Structure

- `main.ipynb`: The primary Jupyter Notebook containing data analysis, preprocessing, and machine learning model implementation.
- `main-checkpoint.ipynb`: A checkpoint version of the main notebook.
- `ADHD.xlsx - Sheet1.csv`: The main dataset containing demographic, academic, and mental health-related features.
- `ADHD.xlsx - Outliers.csv`: A CSV file potentially containing outlier data related to the main dataset.
- `.vscode/settings.json`: VS Code settings for the project, enabling Python auto-import completions.

## Data Description

The `ADHD.xlsx - Sheet1.csv` dataset includes survey-like data with columns such as:

- `age`, `sex`, `home_language`
- Academic metrics: `psy1004_grade`, `nbt_al`, `nbt_math`, `nbt_ql`, `nbt_ave`, `nbt_alql_ave` (National Benchmark Test scores), `matric_mark`
- Mental health scores: `bdi1_total` (Beck Depression Inventory), `audit1_total` (Alcohol Use Disorders Identification Test), `aas1_total` (Adult ADHD Self-Report Scale), `asrs1_total.x`, `asrs1_total.y`, `bai1_total` (Beck Anxiety Inventory)
- Mental health indicators: `PreUniAnxiety`, `Anxiety`, `PreUniDepression`, `Depression`, `PreUniAutism`, `Autism`, `PreUnibipolar`, `bipolar`, `PreUniSuicide`, `Suicide`, `PreUniPanicAttack`, `PanicAttack`, `PreUniOCD`, `OCD`, `PreUniADHD`, `ADHD`
- Categorical features: `UsedPsychMedication_no`, `UsedPsychMedication_yes`, `CurrentPsychMedicationUse_no`, `CurrentPsychMedicationUse_yes`, `TherapyExperience_no`, `TherapyExperience_yes`, `CurrentTherapyStatus_no`, `CurrentTherapyStatus_yes`, `PreUniMentalHealthIssues_no`, `PreUniMentalHealthIssues_yes`, `MentalIllnessDiagnosis_no`, `MentalIllnessDiagnosis_not formally`, `MentalIllnessDiagnosis_yes formally`, `sex_female`, `sex_male`, `sex_other`

Several columns (e.g., individual item scores, `specify`, `home_language`, `nbt_completed`, `nbt_year`, `nbt_did_math`, `ListedDifficulties`, `ListedDiagnoses`, `MentalIllnessStartAge`, `DiagnosisAge`, `aas_change`) were removed during preprocessing to simplify the dataset.

## Analysis and Methodology

The `main.ipynb` notebook outlines the following steps:

1. **Data Loading and Initial Cleaning**: Read `ADHD.xlsx` into a pandas DataFrame and drop irrelevant/redundant columns.
2. **Column Renaming**: Rename long column names to shorter, manageable ones.
3. **Age Processing**: Extract numerical values from `MentalIllnessStartAge` and `DiagnosisAge`, replacing non-numeric entries or values exceeding the maximum age with `NaN`.
4. **Keyword Extraction for Mental Health Conditions**: Use a dictionary of keywords to create binary columns (e.g., `PreUniAnxiety`, `ADHD`) based on `ListedDifficulties` and `ListedDiagnoses`.
5. **Categorical Feature Encoding**:
   - Standardize `MentalIllnessDiagnosis` values (e.g., "not formally diagnosed" to "not formally").
   - Apply One-Hot Encoding to categorical features like `UsedPsychMedication`, `CurrentPsychMedicationUse`, `TherapyExperience`, `CurrentTherapyStatus`, `PreUniMentalHealthIssues`, `MentalIllnessDiagnosis`, and `sex`.
   - Label encode `DiagnosisTiming`.
   - Drop redundant columns post-encoding.
6. **Feature Selection based on Correlation**: Calculate a correlation matrix and drop highly correlated features (correlation > 0.90) to reduce multicollinearity.
7. **Dimensionality Reduction with PCA**: Apply PCA to reduce the dataset to 2 principal components (`PC1`, `PC2`) for visualization and clustering.
8. **Clustering with K-Means**: Perform K-Means clustering on PCA-transformed data, evaluating with a Silhouette Score (~0.49 for 4 clusters). Visualize clusters in a 2D scatter plot.
9. **Model Training and Evaluation**:
   - Split dataset into 70% training and 30% testing sets.
   - **LazyPredict**: Attempted to evaluate multiple models but encountered `ImportError` or `TypeError` issues.
   - **Decision Tree Classifier**: Achieved a weighted F1 score of ~0.91; with cross-validation (cv=5), mean F1 score of ~0.92.
   - **LinearSVC Classifier**: Achieved a weighted F1 score of ~0.91; with cross-validation (cv=5), mean F1 score of ~0.94.

## Usage

To replicate the analysis:

1. Install Python and Jupyter Notebook.
2. Install required libraries: `pandas`, `numpy`, `matplotlib`, `scikit-learn`, `seaborn`, `lazypredict`.
3. Open `main.ipynb` in a Jupyter environment.
4. Run all cells sequentially to perform data processing, analysis, and model training.
