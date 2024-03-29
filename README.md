# Project: Predict survival of coronary artery disease patients for preemptive medical treatment

Name: Michael Ng  

## Folder structure  
- `data` folder
  - place `survive.db` here
- `output` folder
  - `sample` folder
- `src` folder
  - `config.ini`
  - `pipeline_Classifier.py`
  - `readConfig_loadData.py`
- `eda.ipynb`
- `README.md`
- `requirements.txt`
- `run.sh`

## Install

This project requires **Python 3** and the following packages installed:

- `configparser` == 5.2.0
- `pandas` == 1.3.5
- `numpy` == 1.22.0
- `matplotlib` == 3.5.1
- `sklearn` == 0.0
- `scikit-learn` == 1.0.2
- `seaborn` == 0.11.2

## Instructions
***
1. Open the `config.ini` file in your desired editor. It has these contents and further instructions on permissible values:
    ***
   [`DB`]  
   `DBNAME` = survive.db  
   `TABLENAME` = survive  
   [`PARAM`]  
   `ALGO` = 1  
   `TESTSIZE` = 0.30  
   `SEED` = 42  
   `FEATURES` = Gender,Smoke,Diabetes,Age,Ejection Fraction,Sodium,Creatinine,Pletelets,Creatinine phosphokinase,Blood Pressure,Hemoglobin,Height,Weight,Favorite color
    ***
    Brief description:  
    * `DBNAME` - [`string`] fullname of SQL database file to be placed in `data` folder  
    * `TABLENAME` - [`string`] name of table in database  
    * `ALGO` - [`integer`] classification algorithm  
    * `TESTSIZE` - [`float`] percentage of data to reserve as test set  
    * `SEED` - [`integer`] random state seed for train-test split  
    * `FEATURES` - [`string`] concatenated string of selected features for training the model on
    ***
2. At your shell terminal where you have activated your python environment, type `"sh run.sh"` and enter. Give it a full minute to complete.  
    ***
3. It will run the `run.py` python script in the `src` subfolder, which performs this series of instructions:

    a. Read `config.ini` and assign variables (with `readConfig_loadData.py` module)  
    b. Load SQL data  
    c. Load to dataframe  
    d. Clean data  
    e. Run pipeline* (with `pipeline_Classifier.py` module)  
    f. Save plot as JPG in `output` folder

***
## Sample JPG output
![](./output/sample/sample.jpg)
***
## * Pipeline flow
    1. Separate features and labels
    2. Split data into training set and test set
    3. Select algorithm
    4. Generate string list for Categorical Features from p_ls_features
    5. Generate string list for Numeric Features from the difference
    6. Generate a dynamic dictionary that updates the values when different sets of features are selected
    7. Generate integer list for Numeric Features (needed for scaling pre-processing)
    8. Generate integer list for Categorical Features (needed for one hot encoding pre-processing)
    9. Define preprocessing for numeric columns (make them on the same scale)
    10. Define preprocessing for categorical features (encode them)
    11. Combine preprocessing steps
    12. Create preprocessing and training pipeline
    13. Fit the pipeline to train a logistic regression model on the training set
    14. Get predictions from test data
    15. Calculate ROC curve
    16. Format plots arrangement
    17. Text display of selected Features shown in 1st subplot
    18. Print input parameters in 1st subplot
    19. Plot ROC curve in 2nd subplot
    20. Plot Confusion Matrix in 3rd subplot
    21. Print metrics in 4th subplot
***
## Key findings from the EDA
* We want a high **Recall** value as the cost of a false negative is high. We do not want to miss identifying a patient who truly needs preemptive medical attention (False Negative). Precision is less important as the costs of further tests is not disproportionately costly in risk, time or funds. Accuracy is not ideal too as the population of non-Survivors and Survivors is not symmetrical.
* The most impactful features on predictive ability are **'Ejection Fraction'**, **'Pletelets'**, **'Blood Pressure'**, and **'Hemoglobin'**.
* The **Random Forest Classifier** generates the best predictions, followed by K-Nearest Neighbour.
***
## Algorithms available for experimentation
* Classification
  * Logistic Regression
  * Random Forest Classifier
  * Support Vector Machine
  * K-Nearest Neighbour
***
## Metrics available  
* Accuracy
* Recall (use this)
* Precision
* F1 Score
* AUC (use this)
***
## Other considerations
* It would be useful to test the models on data from another hospital to minimise the impact on data entry/regime errors. 
***
## Updates
* [`eda.ipynb`] `plot_distribution` method renamed to `histo_boxplot` and shifted to new `plotter` package, in submodule `plot_distribution.py`
* [`plotter` package] in `__init__.py`, import `histo_boxplot` to save typing submodule `plot_distribution`
* [`plotter` package] in submodule `plot_distribution.py`, reformatted `histo_boxplot` method to be compliant with PEP-8 using `pycodestyle` check
* [`plotter` package] in submodule `plot_distribution.py`, added docstrings to `histo_boxplot` method
* [`plotter` package] in submodule `plot_distribution.py`, converted `histo_boxplot` method into `Distribution` class (retained `histo_boxplot` method for reference) - benefits are that metrics can now be accessed as instance objects of `Distribution` class
* [`plotter` package] in submodule `plot_distribution.py`, added docstrings to `Distribution` class
* [`eda.ipynb`] in cell with "examining the impact of individual features on the predictions", made loop for generating individual feature's output plot
* [`eda.ipynb`] in cell with `features_dict`, add comments for all steps
* [`readConfig_loadData.py`] Changed from using `ConfigParser()` methods of `get()`, `getint()`, `getfloat()`, to calling its self[key] (see https://docs.python.org/3/reference/datamodel.html#object.__getitem__)
* [`plot_distribution.py`]: in `Distribution` class, removed redundant attributes and non-public methods
* [`readConfig_loadData.py`]: `ConfigParser()` self[key] returns strings, need to explicitly typecast to other types when assigning to variables
* [`readConfig_loadData.py`]: removed redundant SQL connection object code replicated in `run.py`
* [`readConfig_loadData.py`]: renamed to `read_config.py` as SQL data loading is not within and to comply with PEP-8 naming convention; updated `run.py` import of this module
* [`run.py`]: 'Load SQL data' and 'Load to dataframe' section moved to submodule `read_SQL.py` as `df_from_SQL` method
* [`clean_data.py`]: 'Clean data' section in `run.py` made into a method and shifted to submodule `clean_data.py`; updated `run.py` import of this module
* [`read_SQL.py`, `clean_data.py`]: added docstrings
* [`pipeline_classifier.py`]: split `pipeline_classifier` method's output plotting and JPG generation into separate `plot_model_output` method; this permits `pipeline_classifier` method to simply output the trained model for other uses, and also allows `plot_model_output` to be used for other models
* [`pipeline_classifier.py`]: added option of JPG generation into `plot_model_output` method
* [`run.py`, `eda.ipynb`]: updated calling of `pipeline_classifier` method
* [`pipeline_classifier.py`]: reformatted methods to be compliant with PEP-8 using `pycodestyle` check
* [`read_config.py`]: removed assignment to global variables and made into `pop_config_values` method, allowing clear visibility of where they are used in `run.py`
* [`run.py`, `read_config.py`, `read_SQL.py`, `clean_data.py`, `pipeline_classifier.py`]: all modules final compliance with PEP-8 using `pycodestyle` check
* [`read_config.py`] 'ALGO' and 'TESTSIZE' `config.ini` errorneous entries will raise ValueError
* [`read_SQL.py`] 'FEATURES' `config.ini` errorneous entries will raise ValueError
