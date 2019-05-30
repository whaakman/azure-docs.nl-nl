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
ms.openlocfilehash: ce8b117a3cbe0e3a5c4265729ccf5c0264241013
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391808"
---
1. Volg de instructies op [maken van een werkruimte van Azure Machine Learning-service](../articles/machine-learning/service/setup-create-workspace.md#portal) het volgende doen:
    * Een Miniconda-omgeving maken
    * De Azure Machine Learning-SDK voor Python installeren
    * Een werkruimte maken
    * Write a workspace configuration file (**aml_config/config.json**).

1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Start de notebookserver vanuit de gekloonde map.

    ```shell
    jupyter notebook
    ```