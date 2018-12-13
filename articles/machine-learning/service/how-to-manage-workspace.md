---
title: Maken en beheren van werkruimten
titleSuffix: Azure Machine Learning service
description: Informatie over het maken, weergeven en verwijderen van Azure Machine Learning-service toegang tot werkruimten in Azure portal.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: 73a47929fa26ae0729943e17d0f6c9a054f4f6bb
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53276703"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Maken en beheren van Azure Machine Learning-service werkruimten

In dit artikel, zult u maken, weergeven en verwijderen [ **Azure Machine Learning-service werkruimten** ](concept-azure-machine-learning-architecture.md#workspace) in Azure portal voor [Azure Machine Learning-service](overview-what-is-azure-ml.md).  U kunt ook maken en verwijderen van werkruimten [met behulp van de CLI](reference-azure-machine-learning-cli.md) of [met Python-code](https://aka.ms/aml-sdk).

## <a name="create-a-workspace"></a>Een werkruimte maken 

Een werkruimte te maken, moet u een Azure-abonnement. Als u geen Azure-abonnement hebt, een gratis account maken voordat u begint. Probeer de [gratis of betaalde versie van Azure Machine Learning-service](http://aka.ms/AMLFree) vandaag nog.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view-a-workspace"></a>Een werkruimte weergeven

1. Selecteer in de linkerbovenhoek van de portal, **alle services**. 

1. In de **alle services** filterveld, type **werkruimte van Machine Learning-service**.  

   ![zoeken naar de werkruimte van Azure Machine Learning-service](media/how-to-manage-workspace/allservices-search1.png)

1. Selecteer in de filterresultaten **werkruimte van Machine Learning-service** om een lijst met uw werkruimten weer te geven. 

   ![Lijst met Azure Machine Learning-service werkruimten](media/how-to-manage-workspace/allservices-search.PNG)

1. Bekijk de lijst met werkruimten die zijn gevonden. U kunt filteren op basis van abonnement, resourcegroepen en locaties.  

   ![Werkruimten weergeven](media/how-to-manage-workspace/allservices_view_workspace.PNG)

1. Selecteer de werkruimte die u zojuist hebt gemaakt om de eigenschappen ervan weer te geven.

   ![Eigenschappen van de werkruimte](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Een werkruimte verwijderen

Gebruik de knop verwijderen aan de bovenkant van de werkruimte die u wilt verwijderen.

  ![Knop verwijderen](media/how-to-manage-workspace/delete-workspace.png)


## <a name="clean-up-resources"></a>Resources opschonen 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

Voer de volledige zelfstudie voor informatie over het gebruik van een werkruimte te bouwen, te trainen en implementeer modellen met Azure Machine Learning-service.

> [!div class="nextstepaction"]
> [Zelfstudie: Modellen trainen](tutorial-train-models-with-aml.md)
