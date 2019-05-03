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
ms.openlocfilehash: 8d21e41ad487ad17598f2320fab5eebae02309e8
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021749"
---
1. Volg de instructies op [maken van een werkruimte van de service Azure Machine Learning](../articles/machine-learning/service/setup-create-workspace.md#portal) aan:
    * Een Miniconda-omgeving maken
    * De Azure Machine Learning-SDK voor Python installeren
    * Een werkruimte maken
    * Write a workspace configuration file (**aml_config/config.json**).
    
1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Voeg een configuratiebestand voor werkruimte met behulp van een van deze methoden toe:
    * Kopieer de **aml_config/config.json** bestand dat u hebt gemaakt in stap 1 in de gekloonde map.

    * In de [Azure-portal](https://ms.portal.azure.com), selecteer **downloaden config.json** uit de **overzicht** sectie van uw werkruimte. 

    ![Config.json downloaden](./media/aml-dsvm-server/download-config.png)

    * Maak een nieuwe werkruimte met behulp van code in [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Start de notebookserver vanuit de gekloonde map.
    
    ```shell
    jupyter notebook
    ```