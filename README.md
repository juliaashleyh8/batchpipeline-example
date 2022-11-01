# batchpipeline-example
A mini POC accelerator for Kubeflow to Azure ML (used for a batch pipeline experiment in the future!)

## Getting Started

* [Create a Data Asset](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-data-assets?tabs=cli)
* This example uses an aggregated version of the [Online Retail Data Set](https://archive.ics.uci.edu/ml/datasets/online+retail)
* [Install the Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
* Install the [Azure ML CLI extension](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-configure-cli?tabs=public)

### Running without Registered Components

* From the root of the cloned repo, execute:
    ```
    az ml job create --file .\local-file-pipeline.yml \
    --resource-group <YOUR-RG> \
    --workspace-name <YOUR_WKSP> \
    --name <NewPipelineName>
    ```
