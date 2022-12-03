# batchpipeline-example
A mini POC accelerator for Kubeflow to Azure ML (used for a batch pipeline experiment in the future!)

## Getting Started

* [Create a Data Asset](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-data-assets?tabs=cli)
* This example uses an aggregated version of the [Online Retail Data Set](https://archive.ics.uci.edu/ml/datasets/online+retail)
* [Install the Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
* Install the [Azure ML CLI extension](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-configure-cli?tabs=public)

## Parallel Training on Many Files

You can use a parallel task on an Azure ML pipeline to train on micro batches of a large file or across many individual files.

* [Parallel Job Overview Microsoft Docs](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-use-parallel-job-in-pipeline?tabs=cliv2)
* [CLI V2 Yaml Schema for Parallel Tasks](https://learn.microsoft.com/en-us/azure/machine-learning/reference-yaml-job-parallel)

* Set up the [Online Retail 2 dataset](#online-retail-2)
* Execute the command below:
    ```bash
    az ml job create --file ./parallel-pipeline/main.yaml
    ```

## Registered Components

You can register components and re-use them.

* [Registered Components Microsoft Docs](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui)

* Set up the [Online Retail 2 dataset](#online-retail-2)
* Execute the command below to register a couple components you might re-use
    ```bash
    az ml component create --file ./registered-components/component-lag/lagger.yml
    az ml component create --file ./registered-components/component-dense-dates/densedate.yml
    ```
* If you are running these commands multiple times you may need to add the `--version` flag.
* Execute the command below to run the pipeline using local and registered components
    ```bash
    az ml job create --file ./registered-components/main.yml
    ```

## Data Sources:

The datasets are [registered via the CLI (official docs)](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-data-assets?tabs=cli).

You should configure your defaults for the Azure CLI.

```bash
az account set --subscription <subscription>
az configure --defaults workspace=<workspace> group=<resource-group> location=<location>
```

### Online Retail 2

* [Online Retail 2]()
  * Used in parallel-pipeline
  * Used in registered-components

```powershell
if (-Not (Test-Path -PathType Container .\datasets)){
    New-Item -ItemType Directory -Force -Path .\datasets
}
if (-Not (Test-Path -PathType Container .\datasets\retail)){
    New-Item -ItemType Directory -Force -Path .\datasets\retail
}
$source = 'https://archive.ics.uci.edu/ml/machine-learning-databases/00502/online_retail_II.xlsx'
$destination = '.\datasets\retail\online_retail_II.xlsx'
Invoke-RestMethod -Uri $source -OutFile $destination
az ml data create -f ./datasets/online-retail-ii.yml

```
