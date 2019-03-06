---
title: Beheren, registreren, implementeren en bewaken van ML-modellen
titleSuffix: Azure Machine Learning service
description: Informatie over het gebruik van Azure Machine Learning-Service te implementeren, beheren en bewaken van uw modellen voor het continu te verbeteren. U kunt de modellen die u met Azure Machine Learning-Service is getraind op uw lokale computer of uit andere bronnen kunt implementeren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 1/23/2019
ms.custom: seodec18
ms.openlocfilehash: 8dea667b15471accd4fc8b09d0ff1eb7aa5daed5
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57403682"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>Beheren, implementeren en bewaken van modellen met Azure Machine Learning-Service

In dit artikel leert u hoe u Azure Machine Learning-Service te implementeren, beheren en bewaken van uw modellen voor het continu te verbeteren. U kunt de modellen die u met Azure Machine Learning, getraind op uw lokale computer of uit andere bronnen kunt implementeren. 

Het volgende diagram illustreert de werkstroom van de volledige implementatie: [![Implementatiewerkstroom voor Azure Machine Learning](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

De implementatiewerkstroom bevat de volgende stappen uit:
1. **Registreer het model** in een register, gehost in uw Service voor Azure Machine Learning-werkruimte
1. **Registreren van een installatiekopie van een** die een model met een scoring-script en de afhankelijkheden in een container draagbaar paren 
1. **Implementeer** de afbeelding als een webservice in de cloud of naar edge-apparaten
1. **Controleren en verzamelen van gegevens**
1. **Update** een implementatie van het gebruik van een nieuwe installatiekopie.

Elke stap kan worden uitgevoerd, onafhankelijk van elkaar of als onderdeel van een met één implementatieopdracht. Bovendien kunt u integreren implementatie in een **CI/CD-werkstroom** zoals geïllustreerd in deze afbeelding.

[!['Azure Machine Learning continue integratie/continue implementatie (CI/CD) cyclus'](media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

## <a name="step-1-register-model"></a>Stap 1: Model registreren

Registratie van het model kunt u om op te slaan en versie van uw modellen in de Azure-cloud in uw werkruimte. Het register model kunt u eenvoudig is om te organiseren en bijhouden van uw getrainde modellen.
 
Geregistreerde modellen worden aangeduid met de naam en versie. Telkens wanneer die u een model met dezelfde naam als een bestaande resourcegroep registreren in het register wordt het versienummer verhoogd. U kunt ook aanvullende metagegevenstags opgeven tijdens de registratie die kan worden gebruikt bij het zoeken naar voor modellen. De Azure Machine Learning-service ondersteunt een model dat kan worden geladen met behulp van Python 3. 

U kunt modellen die worden gebruikt door een installatiekopie niet verwijderen.

Zie voor meer informatie de sectie voor het model van registreren van [modellen implementeren](how-to-deploy-and-where.md#registermodel).

Zie voor een voorbeeld van het registreren van een model dat is opgeslagen in de indeling van pickle [zelfstudie: Een installatiekopie van classificatie-model te trainen](tutorial-deploy-models-with-aml.md).

Zie voor meer informatie over het gebruik van ONNX-modellen de [ONNX en Azure Machine Learning](how-to-build-deploy-onnx.md) document.

## <a name="step-2-register-image"></a>Stap 2: Registreren van installatiekopie

Afbeeldingen toestaan voor de implementatie van betrouwbare model, samen met alle onderdelen die nodig zijn voor het gebruik van het model. Een installatiekopie bevat de volgende items:

* Het model
* De scoring-engine
* Het scoring-bestand of de toepassing
* Eventuele afhankelijkheden die nodig zijn voor het model beoordelen

De installatiekopie kan ook SDK-onderdelen voor logboekregistratie en bewaking bevatten. De gegevens van de SDK-logboeken kan worden gebruikt om aanpassen of opnieuw trainen van uw model, met inbegrip van de invoer en uitvoer van het model.

Azure Machine Learning biedt ondersteuning voor de meest populaire frameworks, maar in het algemeen een framework dat pip is geïnstalleerd worden kan kan werken.

Als uw werkruimte is gemaakt, zijn dus andere verschillende andere Azure-resources gebruikt door deze werkruimte.
Alle objecten die worden gebruikt voor het maken van de installatiekopie worden opgeslagen in de Azure storage-account in uw werkruimte. Wanneer de installatiekopie is gemaakt, kunt u aanvullende metagegevenstags bieden. De metadata-codes worden ook opgeslagen door het installatiekopieregister, en om uw installatiekopie kunnen worden opgevraagd.

Voor meer informatie, Zie de configureren en registreren sectie van de installatiekopie van [modellen implementeren](how-to-deploy-and-where.md#configureimage).

## <a name="step-3-deploy-image"></a>Stap 3: Installatiekopie implementeren

U kunt geregistreerde installatiekopieën implementeren naar de cloud of naar edge-apparaten. Het implementatieproces maakt alle resources die nodig zijn om te controleren, taakverdeling en automatisch schalen van uw model. Toegang tot de geïmplementeerde services kan worden beveiligd met verificatie op basis van het certificaat door te geven van de activa beveiliging tijdens de implementatie. U kunt ook een bestaande implementatie voor het gebruik van een nieuwe installatiekopie bijwerken.

Webservice-implementaties zijn ook doorzoekbaar. Bijvoorbeeld, u kunt zoeken naar alle implementaties van een specifiek model of een afbeelding.

[![Inferentietaken doelen](media/concept-model-management-and-deployment/inferencing-targets.png)](media/concept-model-management-and-deployment/inferencing-targets.png#lightbox)

U kunt uw installatiekopieën implementeren naar de volgende implementatiedoelen in de cloud:

* Azure Container Instance
* Azure Kubernetes Service
* Azure FPGA-machines
* Azure IoT Edge-apparaten

Als de service is geïmplementeerd, de aanvraag inferentietaken wordt automatisch taakverdeling en het cluster wordt geschaald om te voldoen aan pieken vertoont op aanvraag. [Telemetrie over uw service kan worden vastgelegd](how-to-enable-app-insights.md) in de Azure Application Insights-service die is gekoppeld aan uw werkruimte.

Zie voor meer informatie de sectie implementeren van [modellen implementeren](how-to-deploy-and-where.md#deploy).

## <a name="step-4-monitor-models-and-collect-data"></a>Stap 4: Modellen controleren en verzamelen van gegevens

Een SDK voor het vastleggen van gegevens en vastleggen in een model is beschikbaar, zodat u invoer, uitvoer en andere relevante gegevens uit het model kunt volgen. De gegevens worden opgeslagen als een blob in de Azure Storage-account voor uw werkruimte.

Voor het gebruik van de SDK met het model, importeert u de SDK in uw scoring-script of toepassing. U kunt vervolgens de SDK gebruiken om gegevens, zoals parameters, resultaten of gegevens te registreren.

Als u besluit om in te schakelen van gegevensverzameling van model telkens wanneer u de installatiekopie implementeert, worden de gegevens die nodig zijn om vast te leggen van de gegevens, zoals de referenties voor uw persoonlijke blob-archief, automatisch ingericht.

> [!Important]
> Microsoft heeft niet de gegevens die moeten worden verzameld uit uw model weergegeven. De gegevens is rechtstreeks naar de Azure storage-account voor uw werkruimte verzonden.

Zie voor meer informatie, [het inschakelen van gegevensverzameling van model](how-to-enable-data-collection.md).

## <a name="step-5-update-the-deployment"></a>Stap 5: De implementatie bijwerken

Updates aan uw model zijn niet automatisch worden geregistreerd. Op dezelfde manier wordt voor het registreren van een nieuwe installatiekopie niet automatisch bijgewerkt implementaties die zijn gemaakt op basis van een eerdere versie van de installatiekopie. In plaats daarvan moet u handmatig registreren van het model, de installatiekopie van het registreren en vervolgens het model bijwerken. Zie voor meer informatie, sectie van de update [modellen implementeren](how-to-deploy-and-where.md#update).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe en waar u modellen kunt implementeren](how-to-deploy-and-where.md) met de Azure Machine Learning-service. Zie voor een voorbeeld van de implementatie van [zelfstudie: Een afbeelding classificatie implementatiemodel in Azure Container Instances](tutorial-deploy-models-with-aml.md).

Meer informatie over het maken van client-toepassingen en services waar [een model geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md).
