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
ms.openlocfilehash: 8d8b314965253dc00b39d0b068b1d6fb3e4aa471
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58395534"
---
1. Volg de instructies op [maken van een werkruimte van de service Azure Machine Learning](../articles/machine-learning/service/setup-create-workspace.md#portal) voor het maken van een omgeving Miniconda, een werkruimte maken en een werkruimte-configuratiebestand schrijven (**aml_config/config.json**) .

1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Voeg een configuratiebestand voor de werkruimte toe met een van deze methoden:
    * Kopieer de **aml_config/config.json** bestand dat u hebt gemaakt met behulp van de vereiste Quick Start in de gekloonde map.
    * Maak een nieuwe werkruimte met behulp van code in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
1. Start de notebookserver vanuit de gekloonde map.
    
    ```shell
    jupyter notebook
    ```