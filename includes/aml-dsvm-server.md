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
ms.openlocfilehash: 12d1576c3bfbf96c0445fcd2a6f0bc37d6a68f11
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60753347"
---
1. Voltooi de [Python-quickstart voor Azure Machine Learning](../articles/machine-learning/service/quickstart-create-workspace-with-python.md) om een werkruimte te maken.  U kunt de sectie **De notebook gebruiken** overslaan, als u wilt.
1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```
1. Voeg een configuratiebestand voor de werkruimte toe met een van deze methoden:
    * Kopieer het **aml_config\config.json**-bestand dat u met behulp van de vereiste quickstart hebt gemaakt naar de gekloonde map.
    * Maak een nieuwe werkruimte met behulp van code in de notebook [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) in uw gekloonde map.
1. Start de notebookserver vanuit de gekloonde map.
    
    ```shell
    jupyter notebook
    ```