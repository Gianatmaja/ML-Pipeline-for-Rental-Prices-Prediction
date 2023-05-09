# ML Pipeline for Short-Term Rental Prices in NYC

A reusable ML pipeline that is used to predict short-term rental prices in NYC, based on property-related features. This pipeline supports the data cleaning, validation, preprocessing, EDA, as well as the model training and testing process.

The main tools used in the development of this pipeline include Mlflow, Weights & Biases, Hydra, and Pytest.

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
    ├── environment.yml                      # Project environment file
    ├── MLproject                            # Mlflow MLproject file               
    └── README.md


## Setup & Running the Project

### Setup
To create & activate the environment using the conda.yml file, run the following command:

```bash
> conda env create -f environment.yml
> conda activate nyc_airbnb_dev
```

### Connecting to Weights & Biases
To connect to W&B, use the following command:

```bash
> wandb login {your_api_key}
```

### Running Directly from GitHub Repo
The project can be run directly from this GitHub repository using the following command:

```bash
> mlflow run https://github.com/Gianatmaja/ML-Pipeline-for-Rental-Prices-Prediction -v 1.0.0 
```

Desired inputs to the pipeline can also be specified (with the help of Hydra package). For example, to use the `sample2.csv` data instead of `sample1.csv`, run the following command:

```bash
> mlflow run https://github.com/Gianatmaja/ML-Pipeline-for-Rental-Prices-Prediction \
-v 1.0.0 \
-P hydra_options="etl.sample='sample2.csv'"
```

### Running Locally
To run the entire pipeline, use the following command:

```bash
> mlflow run .
```

To run a specific step of the pipeline, run the following command:

```bash
> mlflow run . -P steps={step(s) name}
```


## ML Pipeline

### Pipeline DAG

The pipeline DAG can be found below.

![dag](https://github.com/Gianatmaja/ML-Pipeline-for-Rental-Prices-Prediction/blob/main/images/ml_dag.png)

After the data is downloaded (using the codes defined in `components / get_data/`, it undergoes EDA (`src / eda/`) and data cleaning (`src / basic_cleaning/`). Then, the clean data will be validated (`src / data_check/`) and split into training and testing set (`components / train_val_test_split/`). The training set will further be used for model training (`src / train_random_forest/`). 

Post training, the trained model will be exported, and will be used alongside the test data for model testing (`components / test_regression_model/`).

The main tools that support the development of this pipeline are described below.

### MLflow Project

Each step of the pipeline follows the format of an MLflow project, which in our case, contains the following files:

    .
    ├── conda.yml
    ├── MLproject
    └── {script_name}.py
    
The environment for a particular step is defined in `conda.yml`, whereas additional details (entry points, command to run, etc.) are defined in the `MLproject` file. The main codes for the particular step are in the `{script_name}.py` file.

To create a new step with these files directly present, the cookie-cutter template can be run by using the following command:

```bash
> cookiecutter cookie-mlflow-step -o src

step_name [step_name]: new_step_name
script_name [run.py]: run.py
job_type [my_step]: new_step_category
short_description [My step]: briefly mention what this step does
long_description [An example of a step using MLflow and Weights & Biases]: what this step does, with more details
parameters [parameter1,parameter2]: input_parameter_1,input_parameter_2,input_parameter_3
```

For more information on MLflow Projects, kindly refer to their [documentation](https://mlflow.org/docs/latest/projects.html).

### Weights & Biases

Weights & Biases has been incorporated into this project's codebase. Hence, all runs and artifacts are logged and can be viewed in the W&B dashboard.

![W&B Dashboard](https://github.com/Gianatmaja/ML-Pipeline-for-Rental-Prices-Prediction/blob/main/images/wb_dashboard.png)

The dashboard can further be used to compare metrics across runs, add tags to artifacts, view logs, etc.

View the public dashboard for this project [here](https://wandb.ai/gianatmaja/nyc_airbnb?workspace=user-gianatmaja).

For more information on Weights & Biases, kindly refer to their [documentation](https://docs.wandb.ai/).

### Hydra

The inputs and configurations used throughout the pipeline are defined inside the Hydra configuration file (`config.yaml`). The values inside this file can be changed directly there, or through the command line, for example:

```bash
> mlflow run . \
  -P steps=download,basic_cleaning \
  -P hydra_options="modeling.random_forest.n_estimators=10 etl.min_price=50"
```

In the example above, the `n_estimators` and `min_price` values defined in the `config.yaml` file were not used in the run. Instead, the values in the bash command above were.

For more information on Hydra, kindly refer to their [documentation](https://hydra.cc/docs/intro/).

### Pytest

The Pytest library was used to support the data validation process. The codes can be found inside the `conftest.py` file, in the `src / data_check` directory.

For more information on Pytest, kindly refer to their [documentation](https://docs.pytest.org/en/7.3.x/).

