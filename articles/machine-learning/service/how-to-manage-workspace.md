---
title: Maken en beheren van werkruimten
titleSuffix: Azure Machine Learning service
description: Informatie over het maken, weergeven en verwijderen van Azure Machine Learning-service toegang tot werkruimten in Azure portal.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: b65bc1dc510a02144e57e9d057254963d0c398ed
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244331"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Maken en beheren van Azure Machine Learning-service werkruimten

In dit artikel, zult u maken, weergeven en verwijderen [ **Azure Machine Learning-service werkruimten** ](concept-azure-machine-learning-architecture.md#workspace) in Azure portal voor [Azure Machine Learning-service](overview-what-is-azure-ml.md).  U kunt ook maken en verwijderen van werkruimten [met behulp van de CLI](reference-azure-machine-learning-cli.md) of [met Python-code](https://aka.ms/aml-sdk).

## <a name="create-a-workspace"></a>Een werkruimte maken 

Een werkruimte te maken, moet u een Azure-abonnement. Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](http://aka.ms/AMLFree).

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
