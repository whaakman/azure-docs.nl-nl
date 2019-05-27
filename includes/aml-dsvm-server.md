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
ms.openlocfilehash: 02ef0d6c7c8ddc7088938d9c8ea379e3b97f3045
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158729"
---
1. [Een Azure Machine Learning-service-werkruimte maken](../articles/machine-learning/service/setup-create-workspace.md).

1. Kloon [de GitHub-opslagplaats](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Een werkruimte configuratie-bestand toevoegen aan de gekloonde map met een van deze methoden:

    * In de [Azure-portal](https://ms.portal.azure.com), selecteer **downloaden config.json** uit de **overzicht** sectie van uw werkruimte. 

    ![Config.json downloaden](./media/aml-dsvm-server/download-config.png)

    * Maak een nieuwe werkruimte met behulp van code in de notebook [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) in uw gekloonde map.


1. Start de notebookserver vanuit de gekloonde map.
    
    ```shell
    jupyter notebook
    ```