# Kedro Docker
    Run a Kedro project in a Docker environment

### Prerequisites
+ Docker
+ Kedro 0.16.6
+ Kedro-Docker 0.2.1
+ scikit-learn 0.23.0
+ pickle 0.0.11

### Workflows
+ Read data from csv files and excel file as well, pre-process files and then save csv files
+ Split data and then save pickle files
+ Read pickle files, run train model and then save the regression model(pickle format)
+ Load the regression model and run Predict from the pickle model


### Get data from S3(checkout branch: feature/getting-data-from-S3)
+ Set configs in the conf/*/credentials.yml
    ```
    dev_s3:
        client_kwargs:
            aws_access_key_id: token
            aws_secret_access_key: key
    ```

### Build
+ [Setup Kedro Environment](https://kedro.readthedocs.io/en/0.16.6/02_get_started/01_prerequisites.html)

+ Install Kedro Docker
    ```
    pip install kedro-docker==0.2.1
    ```

+ Generate a Dockerfile
    ```
    example$ kedro docker init
    ```

+ Build a Docker image
    ```
    example$ kedro docker build
    ```
    It will create a Docker image with `example:latest` name

+ Run a Kedro project in a Docker Environment
    ```
    example$ kedro docker run
    ```

### Usage (in Docker)with code from github(Don't install Kedro Environment)
+ Download code from github
+ Build a Docker image
    ```
    $cd example
    $docker build --tag=kedro-docker .
    ```

+ Run a docker image
    ```
    $docker run -it kedro-docker bash
    ```

+ Run a Kedro project
    ```
    $kedro run
    ```

### Result
    ```
    kedro@b792338c69b3:~$ kedro run
    2020-12-28 07:27:43,250 - root - INFO - ** Kedro project kedro
    #### Pipeline execution order ####
    Inputs: companies, parameters, reviews, shuttles

    preprocessing_companies
    preprocessing_shuttles
    master_table
    split_data
    train_model
    predict

    Outputs: None
    ##################################
    fatal: not a git repository (or any of the parent directories): .git
    2020-12-28 07:27:43,259 - kedro.versioning.journal -
    WARNING - Unable to git describe /home/kedro
    /usr/local/lib/python3.7/site-packages/fsspec/implementations/local.py:33: FutureWarning:
    The default value of auto_mkdir=True has been deprecated and will be changed to
    auto_mkdir=False by default in a future release.
    FutureWarning,
    2020-12-28 07:27:43,280 - kedro.io.data_catalog -
    INFO - Loading data from `companies` (CSVDataSet)...
    2020-12-28 07:27:43,338 - kedro.pipeline.node -
    INFO - Running node: preprocessing_companies: preprocess_companies([companies]) -> [preprocessed_companies]
    2020-12-28 07:27:43,406 - kedro.io.data_catalog -
    INFO - Saving data to `preprocessed_companies` (CSVDataSet)...
    2020-12-28 07:27:43,650 - kedro.runner.sequential_runner -
    INFO - Completed 1 out of 6 tasks
    2020-12-28 07:27:43,651 - kedro.io.data_catalog -
    INFO - Loading data from `shuttles` (ExcelDataSet)...
    2020-12-28 07:27:55,006 - kedro.pipeline.node -
    INFO - Running node: preprocessing_shuttles: preprocess_shuttles([shuttles]) -> [preprocessed_shuttles]
    2020-12-28 07:27:55,095 - kedro.io.data_catalog -
    INFO - Saving data to `preprocessed_shuttles` (CSVDataSet)...
    2020-12-28 07:27:55,487 - kedro.runner.sequential_runner -
    INFO - Completed 2 out of 6 tasks
    2020-12-28 07:27:55,487 - kedro.io.data_catalog -
    INFO - Loading data from `preprocessed_shuttles` (CSVDataSet)...
    2020-12-28 07:27:55,567 - kedro.io.data_catalog -
    INFO - Loading data from `preprocessed_companies` (CSVDataSet)...
    2020-12-28 07:27:55,603 - kedro.io.data_catalog -
    INFO - Loading data from `reviews` (CSVDataSet)...
    2020-12-28 07:27:55,682 - kedro.pipeline.node -
    INFO - Running node: master_table:
    create_master_table([preprocessed_companies,preprocessed_shuttles,reviews]) -> [master_table]
    2020-12-28 07:27:59,069 - kedro.io.data_catalog -
    INFO - Saving data to `master_table` (CSVDataSet)...
    2020-12-28 07:28:07,982 - kedro.runner.sequential_runner - INFO - Completed 3 out of 6 tasks
    2020-12-28 07:28:07,983 - kedro.io.data_catalog -
    INFO - Loading data from `master_table` (CSVDataSet)...
    2020-12-28 07:28:09,799 - kedro.io.data_catalog -
    INFO - Loading data from `parameters` (MemoryDataSet)...
    2020-12-28 07:28:09,800 - kedro.pipeline.node -
    INFO - Running node: split_data: split_data([master_table,parameters]) -> [Xtest,Xtrain,Ytest,Ytrain]
    2020-12-28 07:28:10,562 - kedro.io.data_catalog - INFO - Saving data to `Xtrain` (MemoryDataSet)...
    2020-12-28 07:28:10,635 - kedro.io.data_catalog - INFO - Saving data to `Xtest` (MemoryDataSet)...
    2020-12-28 07:28:10,645 - kedro.io.data_catalog - INFO - Saving data to `Ytrain` (MemoryDataSet)...
    2020-12-28 07:28:10,646 - kedro.io.data_catalog - INFO - Saving data to `Ytest` (MemoryDataSet)...
    2020-12-28 07:28:10,711 - kedro.runner.sequential_runner - INFO - Completed 4 out of 6 tasks
    2020-12-28 07:28:10,711 - kedro.io.data_catalog -
    INFO - Loading data from `Xtrain` (MemoryDataSet)...
    2020-12-28 07:28:10,765 - kedro.io.data_catalog - INFO - Loading data from `Ytrain` (MemoryDataSet)...
    2020-12-28 07:28:10,767 - kedro.pipeline.node -
    INFO - Running node: train_model: train_model([Xtrain,Ytrain]) -> [regression_model]
    2020-12-28 07:28:11,173 - kedro.io.data_catalog -
    INFO - Saving data to `regression_model` (MemoryDataSet)...
    2020-12-28 07:28:11,290 - kedro.runner.sequential_runner - INFO - Completed 5 out of 6 tasks
    2020-12-28 07:28:11,290 - kedro.io.data_catalog - INFO - Loading data from `Xtest` (MemoryDataSet)...
    2020-12-28 07:28:11,302 - kedro.io.data_catalog - INFO - Loading data from `Ytest` (MemoryDataSet)...
    2020-12-28 07:28:11,303 - kedro.io.data_catalog -
    INFO - Loading data from `regression_model` (MemoryDataSet)...
    2020-12-28 07:28:11,304 - kedro.pipeline.node -
    INFO - Running node: predict: predict([Xtest,Ytest,regression_model]) -> None
    2020-12-28 07:28:11,367 - example.pipelines.data_science.nodes -
    INFO - Model has a coefficient R^2 of 0.456.
    2020-12-28 07:28:11,411 - kedro.runner.sequential_runner -
    INFO - Completed 6 out of 6 tasks
    ```

### Issues
+ [Could not load Excel Data Set](https://exerror.com/xlrd-biffh-xlrderror-excel-xlsx-file-not-supported/)
    ```
    kedro.io.core.DataSetError: Failed while loading data from data set ExcelDataSet
    (filepath=/home/kedro/data/01_raw/shuttles.xlsx,
    load_args={'engine': xlrd}, protocol=file, save_args={'index': False},
    writer_args={'engine': xlsxwriter}).
    Excel xlsx file; not supported
    ```
    Fixed: xlrd==1.2.0

+ [Load Data from AWS S3](https://github.com/quantumblacklabs/kedro/issues/309)
    ```
    server_1  |     ds_name, ds_config, load_versions.get(ds_name), save_version
    webserver_1  |   File "/usr/local/lib/python3.7/site-packages/kedro/io/core.py",
    line 185, in from_config
    webserver_1  |     ) from err
    webserver_1  | kedro.io.core.DataSetError:
    webserver_1  | get_session() got an unexpected keyword argument 'aws_access_key_id'.
    webserver_1  | DataSet 'companies' must only contain arguments valid for the co
    ```

    ```
    File "/usr/local/lib/python3.7/site-packages/pluggy/callers.py", line 187, in _multicall
        res = hook_impl.function(*args)
    File "/home/kedro/src/example/hooks.py", line 78, in register_catalog
        catalog, credentials, load_versions, save_version, journal
    File "/usr/local/lib/python3.7/site-packages/kedro/io/data_catalog.py", line 328, in from_config
        ds_name, ds_config, load_versions.get(ds_name), save_version
    File "/usr/local/lib/python3.7/site-packages/kedro/io/core.py", line 185, in from_config
        ) from err
    kedro.io.core.DataSetError:
    create_client() got multiple values for keyword argument 'aws_access_key_id'.
    DataSet 'companies' must only contain arguments valid for
    the constructor of `kedro.extras.datasets.pandas.csv_dataset.CSVDataSet`.
    ```

    Fixed: install s3fs==0.4.0 and update credentials.yml
    https://discourse.kedro.community/t/how-do-i-pass-s3-credentials-to-my-datasets/156

    ```
    dev_s3:
        client_kwargs:
            aws_access_key_id: access_key
            aws_secret_access_key: secret_key
    ```

### Reference
+ [Kedro Docker](https://github.com/quantumblacklabs/kedro-docker)
+ [Create a Pipeline](https://kedro.readthedocs.io/en/0.16.6/03_tutorial/04_create_pipelines.html)