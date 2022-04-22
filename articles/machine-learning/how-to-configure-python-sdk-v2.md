---
title: 'Install and set up the Python SDK (v2)'
titleSuffix: Azure Machine Learning
description: Learn how to install and set up the Azure Machine Learning Python SDK v2 
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
author: v-alwallace
ms.author: v-alwallace
ms.date: 04/16/2022
ms.custom: devx-track-azurecli, devplatv2
---

# Install and set up the Python SDK (v2)

[!INCLUDE [python sdk v2](../../includes/machine-learning-python-sdk-v2.md)]
[!INCLUDE [python sdk v2 how to update](../../includes/machine-learning-python-sdk-v2-update-note.md)]

 The Python SDK v2 introduces **new SDK capabilities** like standalone **local jobs**, reusable **components** for pipelines and managed online/batch inferencing. The SDK v2 brings consistency and ease of use across all assets of the platform and is built on top of the same foundation as used in the CLI v2 which is currently in Public Preview.


[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

## Prerequisites

* To use the AML Python SDK v2, you must have an Azure subscription. If you don't have an Azure subscription, create a free account before you begin. Try the [free or paid version of Azure Machine Learning](https://azure.microsoft.com/free/) today.

* You must have a resource group. A resource group can be created [using the Azure portal](../azure-resource-manager/management/manage-resource-groups-portal.md) or using the CLI. A resource group is created in [Install, set up, and use the CLI (v2) (preview)](how-to-configure-cli.md).

* You must have Python 3 installed. 

## Installation

```terminal
pip install azure-ml==2.2.1 --extra-index-url  https://azuremlsdktestpypi.azureedge.net/sdk-cli-v2-public
```
## Clone the examples repository

```terminal 
git clone https://github.com/Azure/azureml-examples --branch sdk-preview
cd azureml-examples/sdk
``` 

## Create a workspace

### Import the required libraries
```python 
from azure.ml import MLClient
from azure.ml.entities import Workspace
from azure.identity import InteractiveBrowserCredential
``` 
### Configure where the workspace needs to be created
Before creating a workspace, we need identifier parameters - a subscription and resource group. We will use these parameters to define where the Azure Machine Learning workspace.

```python
#Enter details of your subscription
subscription_id = '<SUBSCRIPTION_ID>'
resource_group = '<RESOURCE_GROUP>'
```
The `MLClient` from `azure.ml` will be used to create the workspace. We use the default [interactive authentication](https://docs.microsoft.com/en-us/python/api/azure-identity/azure.identity.interactivebrowsercredential?view=azure-python) for this tutorial. More advanced connection methods can be found [here](https://docs.microsoft.com/en-us/python/api/azure-identity/azure.identity?view=azure-python).

```python
# Get a handle to the subscription
ml_client = MLClient(InteractiveBrowserCredential(), subscription_id, resource_group)
```
### Create a basic workspace
To create a basic workspace, we will define the following attributes
- `name` - Name of the workspace
- `location` - The Azure [location](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=machine-learning-service) for workspace. For e.g. eastus, westus etc.
- `display_name` - Display name of the workspace
- `description` - Description of the workspace
- `hbi_workspace` - Flag to define whether the workspace is a High Impact workspace
- `tags` - (Optional) Tags to help search/filter on workspace easily

Using the `MLClient` created earlier, we will create the workspace. This command will start workspace creation and provide a confirmation.

```python
# Creating a unique workspace name with current datetime to avoid conflicts
import datetime
basic_workspace_name = "mlw-basic-prod-" + datetime.datetime.now().strftime("%Y%m%d%H%M")

ws_basic = Workspace(
    name=basic_workspace_name,
    location="eastus",
    display_name="Basic workspace-example",
    description="This example shows how to create a basic workspace",
    hbi_workspace=False,
    tags=dict(purpose="demo")
)
ml_client.workspaces.begin_create(ws_basic)
``` 