---
title: 'MLOps: Beheren, implementeren en bewaken van ML-modellen'
titleSuffix: Azure Machine Learning service
description: 'Informatie over het gebruik van Azure Machine Learning-Service voor MLOps: implementeren, beheren en bewaken van uw modellen voor het continu te verbeteren. U kunt de modellen die u met Azure Machine Learning-Service is getraind op uw lokale computer of uit andere bronnen kunt implementeren.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: d02318db4deae6f0a6acdae9382ce47116c95110
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800858"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Beheren, implementeren en bewaken van modellen met Azure Machine Learning-Service

In dit artikel leert u hoe u Azure Machine Learning-Service te implementeren, beheren en bewaken van uw modellen voor het continu te verbeteren. U kunt de modellen die u met Azure Machine Learning, getraind op uw lokale computer of uit andere bronnen kunt implementeren. 

Het volgende diagram illustreert de werkstroom van de volledige implementatie: [![Implementatiewerkstroom voor Azure Machine Learning](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

De MLOps / implementatiewerkstroom bevat de volgende stappen uit:
1. **Registreer het model** in een register, gehost in uw Service voor Azure Machine Learning-werkruimte
1. **Gebruik** het model in een webservice in de cloud, op een IoT-apparaat of voor analyses met Power BI.
1. **Controleren en verzamelen van gegevens**
1. **Update** een implementatie van het gebruik van een nieuwe installatiekopie.

Elke stap kan worden uitgevoerd, onafhankelijk van elkaar of als onderdeel van een enkele opdracht. Bovendien kunt u een **CI/CD-werkstroom** zoals geïllustreerd in deze afbeelding.

[!['Azure Machine Learning continue integratie/continue implementatie (CI/CD) cyclus'](media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="step-1-register-model"></a>Stap 1: Model registreren

Registratie van het model kunt u om op te slaan en versie van uw modellen in de Azure-cloud in uw werkruimte. Het register model kunt u eenvoudig is om te organiseren en bijhouden van uw getrainde modellen.

> [!TIP]
> U kunt ook modellen die zijn getraind buiten de Azure Machine Learning-service registreren.
 
Geregistreerde modellen worden aangeduid met de naam en versie. Telkens wanneer die u een model met dezelfde naam als een bestaande resourcegroep registreren in het register wordt het versienummer verhoogd. U kunt ook aanvullende metagegevenstags opgeven tijdens de registratie die kan worden gebruikt bij het zoeken naar voor modellen. De Azure Machine Learning-service ondersteunt elk model dat worden kan geladen met behulp van Python 3.5.2 gebruikt of hoger.

U kunt modellen die worden gebruikt in een actieve implementatie niet verwijderen.

Zie voor meer informatie de sectie voor het model van registreren van [modellen implementeren](how-to-deploy-and-where.md#registermodel).

Zie voor een voorbeeld van het registreren van een model dat is opgeslagen in de indeling van pickle [zelfstudie: Een installatiekopie van classificatie-model te trainen](tutorial-deploy-models-with-aml.md).

## <a name="step-2-use-the-model"></a>Stap 2: Het model te gebruiken

Machine learning-modellen kunnen worden gebruikt als een webservice op IoT Edge-apparaten, of voor services zoals Power BI-analyses.

### <a name="web-service"></a>Webservice

U kunt uw modellen in **webservices** compute-doelen met het volgende:

* Azure Container Instance
* Azure Kubernetes Service

Als u wilt het model als een webservice implementeert, moet u het volgende opgeven:

* Het model of ensembles van modellen.
* Afhankelijkheden die zijn vereist voor het gebruik van het model. Bijvoorbeeld, een script dat aanvragen accepteert en roept het model, conda-afhankelijkheden, enzovoort.
* De configuratie van de implementatie die wordt beschreven hoe en waar het model te implementeren.

Zie voor meer informatie, [modellen implementeren](how-to-deploy-and-where.md).

### <a name="iot-edge-devices"></a>IoT Edge-apparaten

Kunt u modellen maken met IoT-apparaten via **Azure IoT Edge-modules**. IoT Edge-modules worden geïmplementeerd naar apparaten, waardoor inferentietaken op het apparaat.

Zie voor meer informatie, [modellen implementeren](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analytische gegevens

Microsoft Power BI ondersteunt het gebruik van machine learning-modellen voor gegevensanalyse. Zie voor meer informatie, [Azure Machine Learning-integratie in Power BI (Preview)](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="step-3-monitor-models-and-collect-data"></a>Stap 3: Modellen controleren en verzamelen van gegevens

Bewaking, krijgt u inzicht in welke gegevens worden verzonden naar het model en de voorspellingen die worden geretourneerd.

Deze informatie helpt u begrijpen hoe het model wordt gebruikt. De ingevoerde verzamelde gegevens zijn mogelijk ook nuttig in toekomstige versies van de training van het model.

Zie voor meer informatie, [het inschakelen van gegevensverzameling van model](how-to-enable-data-collection.md).

## <a name="step-4-update-the-deployment"></a>Stap 4: De implementatie bijwerken

Implementaties moeten expliciet worden bijgewerkt. Zie voor meer informatie, sectie van de update [modellen implementeren](how-to-deploy-and-where.md#update).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe en waar u modellen kunt implementeren](how-to-deploy-and-where.md) met de Azure Machine Learning-service. Zie voor een voorbeeld van de implementatie van [zelfstudie: Een afbeelding classificatie implementatiemodel in Azure Container Instances](tutorial-deploy-models-with-aml.md).

Meer informatie over het maken van [continue integratie en implementatie van ML-modellen met Azure-pijplijnen](/azure/devops/pipelines/targets/azure-machine-learning). 

Meer informatie over het maken van client-toepassingen en services waar [een model geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md).
