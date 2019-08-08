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
ms.openlocfilehash: 09a3cc5a623be2ee5a9d50204f0902ca9f400a76
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68857358"
---
1. [Maak een Azure machine learning service-werk ruimte](../articles/machine-learning/service/how-to-manage-workspace.md).

1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Voeg een werkruimte configuratie bestand toe aan de gekloonde Directory met een van de volgende methoden:

    * Selecteer in de [Azure Portal](https://ms.portal.azure.com)de optie **down load config. json** in het gedeelte **overzicht** van uw werk ruimte. 

    ![Config.json downloaden](./media/aml-dsvm-server/download-config.png)

    * Maak een nieuwe werkruimte met behulp van code in de notebook [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) in uw gekloonde map.

1. Start de notebookserver vanuit de gekloonde map.

    ```shell
    jupyter notebook
    ```