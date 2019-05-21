---
title: bestand opnemen
description: bestand opnemen
services: machine-learning
ms.service: machine-learning
ms.custom: include file
ms.topic: include
author: sgilley
ms.author: sgilley
ms.date: 05/06/2019
ms.openlocfilehash: 623e993dfbe6bbb3297fa6470865ab1a04f55b37
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65920779"
---
>[!IMPORTANT]
>De vereisten voor andere zelfstudies voor Azure Machine Learning-service en de artikelen met procedures kunt u de resources die u hebt gemaakt.


### <a name="delete-everything"></a>Alles verwijderen

Als u niet dat iets dat u hebt gemaakt wilt, verwijdert u de hele resourcegroep, zodat u geen kosten:

1. Selecteer in de Azure portal, **resourcegroepen** aan de linkerkant van het venster.
 
   ![Een resourcegroep verwijderen in de Azure-portal](./media/aml-ui-cleanup/delete-resources.png)

1. Selecteer de resourcegroep die u hebt gemaakt in de lijst.

1. Aan de rechterkant van het venster, selecteer de knop met het weglatingsteken (**...** ).

1. Selecteer **Resourcegroep verwijderen**.

Als u de resourcegroep verwijdert, verwijdert u ook alle resources die u hebt gemaakt in de visuele interface.  

### <a name="delete-only-the-compute-target"></a>Alleen de compute-doel verwijderen

De compute-doel dat u hebt gemaakt, wordt hier *automatisch automatisch wordt geschaald* naar nul knooppunten wanneer deze niet wordt gebruikt. Dit is om kosten te minimaliseren. Als u verwijderen van de compute-doel wilt, voert u deze stappen uit:

1. In de [Azure-portal](https://portal.azure.com), opent u uw werkruimte.

    ![Verwijderen van de compute-doel](./media/aml-ui-cleanup/delete-compute-target.png)

1. In de **Compute** sectie van uw werkruimte, selecteert u de resource.

1. Selecteer **Verwijderen**.

### <a name="delete-individual-assets"></a>Afzonderlijke assets verwijderen

In de visuele interface waar u uw experiment hebt gemaakt, kunt u afzonderlijke assets verwijderen door ze te selecteren en vervolgens te klikken op de **verwijderen** knop.

![Experimenten verwijderen](./media/aml-ui-cleanup/delete-experiment.png)
