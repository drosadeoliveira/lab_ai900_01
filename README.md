# Exploring Azure ML

## Steps to reproduce the lab:
- Sign into the https://ml.azure.com
- Go to your wrokspace and Automated ML
- Create a new Automated ML job as following:
  - Basic settings:
    - Job name: mslearn-bike-automl
    -  New experiment name: mslearn-bike-rental
    - Description: Automated machine learning for bike rental prediction
    - Tags: none

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.48.52.png)
![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.50.07.png)

  - Task type & data:
    - Select task type: Regression
    - Select dataset: Create a new dataset with the following settings:
    - Data type:
      - Name: bike-rentals
      - Description: Historic bike rental data
      - Type: Tabular
    - Data source:
      - Select From web files
      - Web URL: https://aka.ms/bike-rentals
      - Skip data validation: do not select

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.50.44.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.50.59.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.51.18.png)

    - Settings:
      - File format: Delimited
      - Delimiter: Comma
      - Encoding: UTF-8
      - Column headers: Only first file has headers
      - Skip rows: None
      - Dataset contains multi-line data: do not select
    - Schema:
      - Include all columns other than Path
      - Review the automatically detected types
      - Select Create. After the dataset is created, select the bike-rentals dataset to continue to submit the Automated ML job.

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.51.47.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.52.11.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.52.19.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2010.52.34.png)

  - Task settings:
    - Task type: Regression
    - Dataset: bike-rentals
    - Target column: Rentals (integer)
    - Additional configuration settings:
      - Primary metric: Normalized root mean squared error
      - Eplain best model: Unselected
      - Use all supported models: Unselected. You’ll restrict the job to try only a few specific algorithms.
    - Allowed models: Select only RandomForest and LightGBM — normally you’d want to try as many as possible, but each model added increases the time it takes to run the job.

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2011.02.27.png)

    - Limits: Expand this section
    - Max trials: 3
    - Max concurrent trials: 3
    - Max nodes: 3
    - Metric score threshold: 0.085 (so that if a model achieves a normalized root mean squared error metric score of 0.085 or less, the job ends.)
    - Timeout: 15
    - Iteration timeout: 15
    - Enable early termination: Selected

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2011.03.36.png)

  - Validation and test:
    - Validation type: Train-validation split
    - Percentage of validation data: 10
    - Test dataset: None

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2011.04.00.png)

  - Compute:
    - Select compute type: Serverless
    - Virtual machine type: CPU
    - Virtual machine tier: Dedicated
    - Virtual machine size: Standard_DS3_V2*
    - Number of instances: 1
    
![new Automated ML job](./images/Screenshot%202024-02-19%20at%2011.05.08.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2011.05.21.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2011.05.45.png)


## Review the best model and deploy it:

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2012.03.12.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2012.05.52.png)

![new Automated ML job](./images/Screenshot%202024-02-19%20at%2012.06.54.png)

## Test the deployed service
Using the following JSON:
```json
{
  "Inputs": {
    "data": [
      {
        "day": 0,
        "mnth": 0,
        "year": 0,
        "season": 0,
        "holiday": 0,
        "weekday": 0,
        "workingday": 0,
        "weathersit": 0,
        "temp": 0.0,
        "atemp": 0.0,
        "hum": 0.0,
        "windspeed": 0.0
      }
    ]
  },
  "GlobalParameters": 0.0
}
```

We get the followinf result:
```json
{
  "Results": [
    433.0328464249807
  ]
}
```