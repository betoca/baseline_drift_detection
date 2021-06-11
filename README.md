# Baseline Drift Detection Monitor Example
A CHANGE BY BK-MORE CHANGES HERE
This repo is an example Spark data drift monitor model that is conformed for use with ModelOp Center and the ModelOp Spark Runtime Service.

## Assets

Below are the assets that are used to run this example:

| Asset Type | Repo File | HDFS Path | Description |
| --- | --- | --- | --- |
| Model Schema | `df_sample_scored_input_schema.avsc` | n/a (copied via Spark runtime service) | Input schema for the model. Copied from the base model input schema. |
| Input Asset (Baseline/Training Data) | `df_baseline_scored.csv` | `/hadoop/demo/df_baseline_scored.csv` | An attachment on the base model snapshot. Input file for the model `metrics()` function. The HDFS path can vary based on the `external_inputs` param of the `metrics()` function  |
| Input Asset (Comparator Data) | `df_sample_scored.csv` | `/hadoop/demo/df_sample_scored.csv` | An associated asset. Input file for the model `metrics()` function. The HDFS path can vary based on the `external_inputs` param of the `metrics()` function  |
| Output Asset | n/a | `/hadoop/demo/drift_analysis.csv` | Output from the model `metrics()` function that the MLC consumes to display on the ModelOp Center UI. The HDFS path can vary based on the `OUTPUT_FILE` `fileUrl` that is passed to the MLC when fired by API. |

## Testing the Model

1. Verify that your ModelOp Center instance has at least one Spark runtime service connected to an existing Spark cluster
2. Verify that the Spark runtime service has the tag `test`
3. Import this model into ModelOp Center via GitHub link
4. Create a snapshot of this model. Select "Spark" for the runtime type and (optionally) select a Spark runtime service
5. Import the base model [modelop/titanic-spark](https://github.com/modelop/titanic-spark.git)
6. Add the training data asset as a URL HDFS attachment to the base model.
    - URL: `hdfs:///hadoop/demo/df_baseline_scored.csv`
7. Create a snapshot of the base model
   - (Optional) Select the ModelOp Runtime as the runtime type for the base model
   - Add the snapshot of the associated model (this repo) as an associated model
   - Set the associated model type to "Data Drift Model"
   - Add the comparator data as an associated asset HDFS URL
       - URL: `hdfs:///hadoop/demo/df_sample_scored.csv`
   - (Optional) Provide a DMN file for the MLC to parse with the test results
8. Record the UUID of the base model snapshot
9. Create a drift detection job by firing the CronTriggeredDataDriftTest MLC using the URL and JSON payload below:
    - URL: http://mlc.mocaasin.modelop.center/rest/signal
    - JSON payload (**note that `MODEL_ID.value` must be updated**):
10. Wait for the created job to enter a `COMPLETE` state
11. Navigate to the base model snapshot's test results and verify that the test results look similar:

### Manual Tests

The `job_submit` folder includes files that can send a job to MOC directly. you can update the global environment variables in `job_submit/submit.py` to point to your instance and run different types of jobs:


Then run `submit.py` to submit the job:

```
python3 job_submitter/submit.py
```
