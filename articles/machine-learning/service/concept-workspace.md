---
title: Wat is een werkruimte
titleSuffix: Azure Machine Learning service
description: De werkruimte is de resource op het hoogste niveau voor Azure Machine Learning-service. Dit houdt een geschiedenis bij van alle trainingsuitvoeringen, met inbegrip van Logboeken, metrische gegevens, uitvoer en een momentopname van uw scripts. U deze informatie gebruiken om te bepalen welke run training levert het beste model
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/21/2019
ms.openlocfilehash: 2f3d9eeca1404fcae121ae5fead222cbde4037b1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059257"
---
# <a name="what-is-an-azure-machine-learning-service-workspace"></a>Wat is een werkruimte van de service Azure Machine Learning?

De werkruimte is de resource op het hoogste niveau voor Azure Machine Learning-service, waarmee u een centrale locatie voor het werken met alle artefacten die u maakt wanneer u Azure Machine Learning-service.  De werkruimte houdt een geschiedenis bij van alle trainingsuitvoeringen, met inbegrip van Logboeken, metrische gegevens, uitvoer en een momentopname van uw scripts. U kunt deze informatie gebruiken om te bepalen welke run training levert het beste model.  

Zodra u een model dat u hebt, kunt u deze registreren met de werkruimte. U vervolgens het geregistreerde model en de scoring-scripts om te implementeren in Azure Container Instances, Azure Kubernetes Service, of op een veld-programmable gate array (FPGA) als een HTTP op basis van REST-eindpunt. U kunt ook het model met een Azure IoT Edge-apparaat implementeren als een module.

## <a name="taxonomy"></a>Taxonomie 

Een taxonomie van de werkruimte wordt weergegeven in het volgende diagram:

[![Taxonomie van werkruimte](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

Het diagram ziet u de volgende onderdelen van een werkruimte:

+ Een werkruimte kan bevatten [Notebook VMs](quickstart-run-cloud-notebook.md), resources die zijn geconfigureerd met de Python-omgeving die nodig zijn om uit te voeren van Azure Machine Learning in de cloud.
+ [Gebruikersrollen](how-to-assign-roles.md) kunt u uw werkruimte delen met andere gebruikers, teams of projecten.
+ [COMPUTE-doelen](concept-azure-machine-learning-architecture.md#compute-targets) worden gebruikt voor het uitvoeren van de experimenten nodig hebben.
+ Wanneer u de werkruimte maakt [gekoppelde resources](#resources) worden ook voor u gemaakt.
+ [Experimenten](concept-azure-machine-learning-architecture.md#experiments) zijn trainingsuitvoeringen u gebruiken om uw modellen te bouwen.  U kunt maken en uitvoeren van experimenten met
    + De [Azure Machine Learning-SDK voor Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).
    + De [machine learning-experimenten (preview) geautomatiseerde](how-to-create-portal-experiments.md) sectie in Azure portal.
    + De [visuele interface (preview)](ui-concept-visual-interface.md).
+ [Pijplijnen](concept-azure-machine-learning-architecture.md#ml-pipelines) zijn herbruikbare werkstromen voor trainings- en opnieuw trainen van uw model.
+ [Gegevenssets](concept-azure-machine-learning-architecture.md#datasets-and-datastores) hulpmiddel bij het beheer van de gegevens die u voor het model training en de pijplijn gebruikt.
+ Zodra u een model dat u wilt implementeren hebt, kunt u een geregistreerde model maken.
+ Het geregistreerde model en een scoring-script gebruiken om u te maken van een [implementatie](concept-azure-machine-learning-architecture.md#deployment).

## <a name="tools-for-workspace-interaction"></a>Hulpprogramma's voor interactie met de werkruimte

U kunt werken met uw werkruimte in de volgende manieren:

+ Op het web:
    + [Azure Portal](https://portal.azure.com)
    + De [visuele interface (preview)](ui-concept-visual-interface.md)
+ Met behulp van Azure Machine Learning in Python [SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
+ Op de opdrachtregel met de Azure Machine Learning [CLI-extensie](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli)

## <a name="machine-learning-with-a-workspace"></a>Machine learning met een werkruimte

Machine learning-taken gelezen en/of artefacten schrijven naar uw werkruimte. 

+ Uitvoeren van een experiment voor het trainen van een model - experimenteren schrijfbewerkingen resultaten van de uitvoering in de werkruimte.
+ Gebruik automatische ML met het trainen van een model - training resultaten schrijft naar de werkruimte.
+ Registreer een model in de werkruimte.
+ Een model implementeren - het geregistreerde model wordt gebruikt om een implementatie te maken.
+ Maak en herbruikbare werkstromen uitvoeren.
+ Weergave voor machine learning-artefacten, zoals experimenten, pijplijnen, modellen, implementaties.
+ Bijhouden en monitor-modellen.

## <a name="workspace-management"></a>Werkruimtebeheer

U kunt ook de volgende werkruimte-beheertaken uitvoeren:

| Taak voor het beheer van werkruimte   | Portal              | SDK        | CLI        |
|---------------------------|------------------|------------|------------|
| Een werkruimte maken        | **&check;**     | **&check;** | **&check;** |
| Compute-resources maken en beheren    | **&check;**   | **&check;** |  **&check;**   |
| Werkruimtetoegang beheren    | **&check;**   | |  **&check;**    |
| Een VM-notebook maken | **&check;**   | |     |

Aan de slag met de service door [het maken van een werkruimte](setup-create-workspace.md).

## <a name="resources"></a> Gekoppelde resources

Wanneer u een nieuwe werkruimte maakt, wordt automatisch verschillende Azure-resources die worden gebruikt door de werkruimte gemaakt:

+ [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): Hiermee registreert u docker-containers die u tijdens de training en wanneer u een model implementeert gebruiken. Als u wilt kosten kunt minimaliseren, de ACR is **langzaam geladen** tot implementatie-installatiekopieën worden gemaakt.
+ [Azure Storage-account](https://azure.microsoft.com/services/storage/): Wordt gebruikt als de standaard gegevensopslag voor de werkruimte.
+ [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): Winkels controlegegevens over uw modellen.
+ [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): Winkels geheimen die worden gebruikt door compute-doelen en andere gevoelige informatie die nodig is door de werkruimte.

> [!NOTE]
> Naast het maken van nieuwe versies, kunt u ook bestaande Azure-services gebruiken.

## <a name="next-steps"></a>Volgende stappen

Als u wilt aan de slag met Azure Machine Learning-service, Zie:

+ [Overzicht van Azure Machine Learning-service](overview-what-is-azure-ml.md)
+ [Een werkruimte maken](setup-create-workspace.md)
+ [Een werkruimte beheren](how-to-manage-workspace.md)
+ [Zelfstudie: een model trainen](tutorial-train-models-with-aml.md)
