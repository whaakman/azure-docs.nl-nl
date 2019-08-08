---
title: bestand opnemen
description: bestand opnemen
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 01/25/2019
ms.openlocfilehash: 6c93d1243db1b3c4277a54cf71e10f6bbc648d26
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68846014"
---
- De Azure Machine Learning SDK voor python is geïnstalleerd. Volg de instructies in [Azure machine learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) om het volgende te doen:


1. Volg de instructies in [Azure machine learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) om het volgende te doen:
    * Een Miniconda-omgeving maken [Azure Machine Learning service-werk ruimten maken en beheren]
    * De Azure Machine Learning SDK voor python installeren

1. Maak een [Azure machine learning service-werk ruimte](../articles/machine-learning/service/how-to-manage-workspace.md).

1. Een [configuratie bestand](../articles/machine-learning/service/how-to-configure-environment.md#workspace) bestand (**aml_config/config. json**) schrijven.

1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Start de notebookserver vanuit de gekloonde map.

    ```shell
    jupyter notebook
    ```