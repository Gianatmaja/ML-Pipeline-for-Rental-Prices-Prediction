# ML Pipeline for Short-Term Rental Prices in NYC

A reusable ML pipeline that is used to predict short-term rental prices in NYC, based on the property-related features. This pipeline supports the data cleaning, validation, preprocessing, EDA, as well as the model training and testing process.

## Project Structure

The structure of this repository is as follows:

    .
    ├── components/                          # Utilities & common functions
    │  ├── get_data/                              
    │  ├── test_regression_model/
    │  ├── train_val_test_split/
    │  ├── wandb_utils/
    │  ├── conda.yml
    │  ├── setup.py
    ├── cookie-mlflow-step/                  # Baseline for new step creation
    ├── images/                              # Images & plots
    ├── src/                                 # Main pipeline codes          
    │  ├── basic_cleaning/                              
    │  ├── data_check/
    │  ├── eda/
    │  ├── train_random_forest/                  
    ├── main.py                              # Main runner file
    ├── config.yaml                          # Hydra config file
    ├── conda.yml                            # Conda environment file
    ├── environment.yml
    ├── MLproject                            # Mlflow MLproject file               
    └── README.md


## Setup & Running the Project

### Running Directly from GitHub Repo
The project can be run directly from this GitHub repository using the following command:

```bash
> mlflow run https://github.com/Gianatmaja/ML-Pipeline-for-Rental-Prices-Prediction -v 1.0.0 
```

### Running Locally
To create & activate the environment using the conda.yml file, run the following command:

```bash
> conda env create -f environment.yml
> conda activate nyc_airbnb_dev
```

To run the entire pipeline, use the following command:

```bash
> mlflow run .
```

To run a specific step of the pipeline, run the following command:

```bash
> mlflow run . -P steps={step(s) name}
```

### Connecting to Weights & Biases
To connect to W&B, use the following command:


```bash
> wandb login {your_api_key}
```

## ML Pipeline

### Pipeline DAG

The pipeline DAG can be found below.

![dag](https://github.com/Gianatmaja/ML-Pipeline-for-Rental-Prices-Prediction/blob/main/images/ml_dag.png)

After the data is downloaded (using the codes defined in `components / get_data/`, it undergoes EDA (`src / eda/`) and data cleaning (`src / basic_cleaning/`). Then, the clean data will be validated (`src / data_check/`) and split into training and testing set (`components / train_val_test_split/`). The training set will further be used for model training (`src / train_random_forest/`). 

Post training, the trained model will be exported, and will be used alongside the test data for model testing (`components / test_regression_model/`).

### MLflow Project

Each step of the pipeline follows the format of an MLflow project, which in our case, contains the following files:

    .
    ├── conda.yml
    ├── MLproject
    └── {script_name}.py
    
The environment for a particular step is defined in `conda.yml`, whereas additional details (entry points, command to run, etc.) are defined in the `MLproject` file. The main codes for the particular step are in the `{script_name}.py` file.

### Weights & Biases

Weights & Biases has been incorporated into this project's codebase. Hence, all runs and artifacts are logged and can be viewed in the W&B dashboard.

![W&B Dashboard](https://github.com/Gianatmaja/ML-Pipeline-for-Rental-Prices-Prediction/blob/main/images/wb_dashboard.png)

The dashboard can further be used to compare metrics across runs, add tags to artifacts, view logs, etc.

### Hydra Configuration File


