---
title: Model Builder Azure Training Resources
description: A guide of resources for Azure Machine Learning
ms.topic: reference
ms.date: 02/25/2020
ms.author: luquinta
author: luisquintanilla
---

# Model Builder Azure Training Resources

The following is a guide to help you learn more about resources used to train models in Azure with Model Builder.

## What is an Azure Machine Learning experiment?

An Azure Machine Learning experiment is a resource that needs to be created before running Model Builder training on Azure.

The experiment encapsulates the configuration and results for one or more machine learning training runs. Experiments belong to a specific workspace. The first time an experiment is created, its name is registered in the workspace. Any subsequent runs - if the same experiment name is used - are logged as part of the same experiment. Otherwise, a new experiment is created.

## What is an Azure Machine Learning workspace?

A workspace is an Azure Machine Learning resource that provides a central place for all Azure Machine Learning resources and artifacts created as part of training run.

To create an Azure Machine Learning workspace, the following are required:

- Name: A name for your workspace between 3-33 characters. Names may only contain alphanumeric characters and hyphens. 
- Region: The geographic location of the data center where your workspace and resources are deployed to. It is recommended that you choose a location close to where you or your customers are.
- Resource group: A container that contains all related resources for an Azure solution.

## What is an Azure Machine Learning compute?

An Azure Machine Learning compute is a cloud-based Linux VM used for training.

To create an Azure Machine Learning workspace, the following are required:

- Name: A name for your workspace between 2-16 characters. Names may only contain alphanumeric characters and hyphens.
- Compute size

    Model Builder can use one of the following GPU-optimized compute types:

    | Size | vCPU | Memory: GiB | Temp storage (SSD) GiB | GPU | GPU memory: GiB | Max data disks | Max NICs |
    |---|---|---|---|---|---|---|---|
    | Standard_NC12   | 12 | 112 | 680  | 2 | 24 | 48 | 2 |
    | Standard_NC24   | 24 | 224 | 1440 | 4 | 48 | 64 | 4 |

    Visit the [NC-series Linux VM documentation](https://docs.microsoft.com/azure/virtual-machines/nc-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) for more details on GPU optimized compute types.

## Training

Training on Azure is only available for the Model Builder image classification scenario. The algorithm used to train these models is a Deep Neural Network based on the ResNet50 architecture. During training, the resources required to train the model are provisioned and the model is trained. This process takes several minutes and the amount of time may vary depending on the size of compute selected as well as amount of data. You can track the progress of your runs by selecting the "Monitor current run in Azure portal" link in Visual Studio.

## Results

Once training is complete, two projects are added to your solution with the following suffixes:

- *ConsoleApp*: A C# .NET Core console application that provides starter code to build the prediction pipeline and make predictions.
- *Model*: A C# .NET Standard application that contains the data models that define the schema of input and output model data as well as the following assets:

  - bestModel.onnx: A serialized version of the model in Open Neural Network Exchange (ONNX) format. ONNX is an open source format for AI models that supports interoperability between frameworks like ML.NET, PyTorch and TensorFlow.
  - bestModelMap.json: A list of categories used when making predictions to map the model output to a text category.
  - MLModel.zip: A serialized version of the ML.NET prediction pipeline that uses the serialized version of the model *bestModel.onnx* to make predictions and maps outputs using the `bestModelMap.json` file.
  
## Troubleshooting

### Cannot create compute

If an error occurs during Azure Machine Learning compute creation, the compute resource may still exist, in an errored state. If you try to re-create the compute resource with the same name, the operation fails. To fix this error, either:

* Create the new compute with a different name
* Go to the Azure portal, and remove the original compute resource
