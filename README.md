# Student Performance Analytics

A learning analytics and educational data mining project exploring the factors associated with student academic performance and identifying students who may be at risk of under-performing.

The project uses an anonymised multi-feature student dataset and combines exploratory data analysis, feature engineering, regression, classification and interpretable segmentation to support evidence-based educational analysis.

## Project Objectives

The analysis focuses on two main questions:

1. **Performance Drivers**  
   Which academic, demographic, social and behavioural factors are most strongly associated with students' final grades?

2. **At-Risk Students**  
   Can student characteristics and earlier performance indicators be used to identify students at risk of under-performance?

For this project, a student is classified as **at risk** when:

```text
Final grade (G3) < 10
```

## Dataset

The project uses `student_performance.csv`, an anonymised dataset adapted from the publicly available UCI Student Performance Dataset for educational and research training purposes.

The dataset used in the notebook contains:

- **1,044 student records**
- **34 original variables**
- Math and Portuguese course records
- demographic attributes
- family and social characteristics
- study behaviour
- school support variables
- attendance / absence information
- prior and final grades

No personally identifiable information is included.

The dataset is used solely for educational analysis.

## Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- scikit-learn
- Jupyter Notebook

## Analytical Workflow

The notebook follows a structured analysis pipeline:

```text
Raw student data
       |
       v
Data quality checks
       |
       v
Preprocessing
       |
       v
Feature engineering
       |
       v
Exploratory data analysis
       |
       +------------------------+
       |                        |
       v                        v
Performance modelling      At-risk modelling
       |                        |
       v                        v
Ridge regression         Logistic regression
       |                        |
       +-----------+------------+
                   |
                   v
          Interpretation
                   |
                   v
     Student risk segmentation
```

## 1. Data Preparation

The notebook begins by checking:

- dataset shape
- missing values
- course distribution
- variable types
- grade distributions
- categorical and numerical features

The original dataset contains 1,044 rows and 34 columns.

### Feature Engineering

Several additional features are created to make the analysis more useful:

#### Log-transformed absences

```python
absences_log = log1p(absences)
```

This reduces the effect of the strongly skewed absence distribution.

#### Academic trajectory

```python
delta_G2_G1 = G2 - G1
```

This captures whether a student's performance improved or declined between earlier grading periods.

#### At-risk indicator

```python
at_risk = 1 if G3 < 10 else 0
```

Approximately **22% of records** in the dataset are classified as at risk under this definition.

## 2. Exploratory Data Analysis

The exploratory analysis examines how final performance and risk vary across academic, behavioural and contextual factors.

Examples include:

- previous grades
- study time
- absences
- previous failures
- parental education
- school support
- family support
- internet access
- social behaviour
- reason for choosing the school
- course type

The notebook uses summary statistics, grouped analysis, cross-tabulations and visualisations to identify patterns before modelling.

## 3. Performance Drivers

The first modelling task treats final grade `G3` as a continuous target.

A **Ridge Regression** model is used to examine relationships between the available student characteristics and final academic performance.

The modelling pipeline includes:

- numerical variables
- categorical variables
- one-hot encoding
- train/test splitting
- regularised linear regression
- Mean Absolute Error
- R² evaluation

Ridge Regression was selected because it provides an interpretable linear model while reducing instability from correlated predictors.

The purpose of the model is not simply to maximise predictive accuracy, but to help understand which variables contain useful information about student performance.

## 4. Identifying At-Risk Students

The second task reframes the problem as binary classification:

```text
At risk     = G3 < 10
Not at risk = G3 >= 10
```

A **Logistic Regression** pipeline is used to estimate the probability that a student belongs to the at-risk group.

Model evaluation includes:

- classification report
- confusion matrix
- ROC curve
- ROC-AUC

Using an interpretable classifier makes it possible to assess both predictive performance and the direction of relationships between student characteristics and risk.

## 5. Categorical and Contextual Analysis

The notebook also explores relationships that may not be fully represented by a single predictive model.

Examples include:

- guardian type vs school support
- reason for choosing school vs course
- parental education vs at-risk rate
- categorical distributions and average performance

This helps provide context around model outputs and avoids treating predictive relationships as causal explanations.

## 6. Student Segmentation

A simple rule-based segmentation step groups students using study-time and absence bands.

Example study-time bands include:

- <2 hours
- 2-5 hours
- 5-10 hours
- >10 hours

Absence bands include:

- 0
- 1-5
- 6-10
- 11+

Each segment is summarised using:

- number of students
- mean final grade
- at-risk rate

This provides a more interpretable way of identifying groups that may warrant additional attention or support.

For example, the notebook found that the segment combining **less than two hours of study time with 11+ absences** had an at-risk rate of approximately **45%** in this dataset.

This is an observational result and should not be interpreted as evidence that these characteristics directly cause lower performance.

## Repository Structure

```text
learning_data_analytics_project/
|
|-- Student Performance Analysis.ipynb
|-- student_performance.csv
|-- Report.pdf
|-- README.md
```

The notebook also creates an `outputs/` directory when executed for selected analysis artefacts such as category frequency and performance tables.

## Installation

Clone the repository:

```bash
git clone https://github.com/getredportfolio/learning_data_analytics_project.git
cd learning_data_analytics_project
```

Install the core dependencies:

```bash
pip install pandas numpy matplotlib scikit-learn jupyter
```

Start Jupyter:

```bash
jupyter notebook
```

Then open:

```text
Student Performance Analysis.ipynb
```

and run the cells sequentially.

## Project Files

### `Student Performance Analysis.ipynb`

Contains the complete analytical workflow, including preprocessing, EDA, feature engineering, modelling, evaluation and segmentation.

### `student_performance.csv`

The anonymised student dataset used by the analysis.

### `Report.pdf`

The accompanying written report discussing the methodology, results and interpretation of the project.

## What I Learned

This project provided practical experience in:

- structuring an end-to-end data analysis workflow
- cleaning and validating mixed-type datasets
- exploratory data analysis
- feature engineering
- categorical encoding
- building scikit-learn pipelines
- regression modelling
- classification modelling
- model evaluation
- interpreting analytical results
- communicating limitations
- converting statistical outputs into practical student segments

## Limitations

The analysis should be interpreted carefully.

- Relationships in the data are observational and do not establish causation.
- Student outcomes can be influenced by factors not represented in the dataset.
- The at-risk threshold is a simplified analytical definition.
- Model results should support, rather than replace, professional educational judgement.
- Predictive performance on this dataset does not guarantee generalisation to different schools, populations or education systems.

## Possible Improvements

Future development could include:

- cross-validation and systematic hyperparameter tuning
- comparison with tree-based models
- calibration analysis for predicted risk probabilities
- fairness analysis across demographic groups
- SHAP or other model-explanation techniques
- stronger experiment tracking
- a reproducible Python package or pipeline rather than a notebook-only workflow
- an interactive dashboard for exploring student performance and risk patterns

## Disclaimer

This repository is an educational data analytics project. The analysis is intended for learning and research purposes and should not be used as an automated decision-making system for real students.
