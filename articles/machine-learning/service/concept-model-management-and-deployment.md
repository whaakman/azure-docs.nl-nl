---
title: Beheren en implementeren van modellen in Azure Machine Learning
description: Informatie over het gebruik van Azure Machine Learning te implementeren, beheren en bewaken van uw modellen voor het continu te verbeteren. U kunt de modellen die u met Azure Machine Learning, getraind op uw lokale computer of uit andere bronnen kunt implementeren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: hjerez
ms.author: hjerez
ms.date: 09/24/2018
ms.openlocfilehash: 634f33b6d4ed6e272dfb3d1443b0afc63f822d43
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055880"
---
# <a name="manage-deploy-and-monitor-models-with-azure-machine-learning"></a>Beheren, implementeren en bewaken van modellen met Azure Machine Learning

In dit artikel leert u hoe u met Azure Machine Learning kunt implementeren, beheren en bewaken van uw modellen voor het continu te verbeteren. U kunt de modellen die u met Azure Machine Learning, getraind op uw lokale computer of uit andere bronnen kunt implementeren. 

[!['Azure Machine Learning continue integratie/continue implementatie (CI/CD) cyclus'](media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

## <a name="deployment-workflow"></a>Implementatiewerkstroom

De implementatiewerkstroom bestaat: 
1. **Registreer het model** in een register, gehost in uw Azure Machine Learning-werkruimte
1. **Registreren van een installatiekopie van een** die een model met een scoring-script en de afhankelijkheden in een container draagbaar paren 
1. **Implementeer** de afbeelding als een webservice in de cloud of naar edge-apparaten
1. **Controle en gegevensverzameling**

U kunt elke stap kunt doen, afzonderlijk of als onderdeel van een met één implementatieopdracht. 

Het volgende diagram illustreert de volledige implementatie-pijplijn:

[![Implementatiepijplijn](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

## <a name="step-1-model-registration"></a>Stap 1: Model registratie

Het register model houdt van alle modellen in de Azure Machine Learning-werkruimte.
Modellen worden aangeduid met de naam en versie. Telkens wanneer die u een model met dezelfde naam als een bestaande resourcegroep registreren in het register wordt het versienummer verhoogd. U kunt ook aanvullende metagegevenstags opgeven tijdens de registratie die kan worden gebruikt bij het zoeken naar voor modellen.

U kunt modellen die worden gebruikt door een installatiekopie niet verwijderen.

## <a name="step-2-image-registration"></a>Stap 2: De installatiekopie van inschrijving

Afbeeldingen toestaan voor de implementatie van betrouwbare model, samen met alle onderdelen die nodig zijn voor het gebruik van het model. Een installatiekopie bevat de volgende items:

* Het model
* De scoring-engine
* Het scoring-bestand of de toepassing
* Eventuele afhankelijkheden die nodig zijn voor het model beoordelen

De installatiekopie kan ook SDK-onderdelen voor logboekregistratie en bewaking bevatten. De gegevens van de SDK-logboeken kan worden gebruikt om aanpassen of opnieuw trainen van uw model, met inbegrip van de invoer en uitvoer van het model.

Azure Machine Learning biedt ondersteuning voor de meest populaire frameworks, maar in het algemeen een framework dat pip is geïnstalleerd worden kan kan werken.

Als uw werkruimte is gemaakt, zijn dus andere verschillende andere Azure-resources gebruikt door deze werkruimte.
Alle objecten die worden gebruikt voor het maken van de installatiekopie worden opgeslagen in de Azure storage-account in uw werkruimte. De installatiekopie is gemaakt en opgeslagen in Azure Container Registry. U kunt aanvullende metagegevenstags opgeven bij het maken van de installatiekopie, die ook door het installatiekopieregister worden opgeslagen en kunnen worden opgevraagd om de afbeelding te zoeken.

## <a name="step-3-deployment"></a>Stap 3: implementatie

U kunt geregistreerde installatiekopieën implementeren naar de cloud of naar edge-apparaten. Het implementatieproces maakt de resources die nodig zijn om te controleren, taakverdeling en automatisch schalen uw model. Toegang tot de geïmplementeerde services kan worden beveiligd met verificatie op basis van door te geven van de activa beveiliging tijdens de implementatie. U kunt ook een bestaande implementatie voor het gebruik van een nieuwe installatiekopie bijwerken.

Webservice-implementaties zijn ook doorzoekbaar. Bijvoorbeeld, u kunt zoeken naar alle implementaties van een specifiek model of een afbeelding.

U kunt uw installatiekopieën implementeren naar de volgende doelen in de cloud:

* Azure Container Instance
* Azure Kubernetes Service
* Azure FPGA-machines
* Azure IoT Edge-apparaten

[Meer informatie over waar u kunt implementeren](how-to-deploy-and-where.md).

Als de service is geïmplementeerd, de aanvraag inferentietaken wordt automatisch taakverdeling en het cluster wordt geschaald om te voldoen aan pieken vertoont op aanvraag. Telemetrie over uw service wordt vastgelegd in de Azure Application Insights-service die is gekoppeld aan uw werkruimte.

## <a name="step-4-monitoring-models-and-data-collection"></a>Stap 4: Modellen en het verzamelen van gegevens controleren

Een SDK voor het vastleggen van gegevens en vastleggen in een model is beschikbaar, zodat u invoer, uitvoer en andere relevante gegevens uit het model kunt volgen. De gegevens worden opgeslagen als een blob in de Azure Storage-account voor uw werkruimte.

Voor het gebruik van de SDK met het model, importeert u de SDK in uw scoring-script of toepassing. U kunt vervolgens de SDK gebruiken om gegevens, zoals parameters, resultaten of gegevens te registreren.

Als u te besluit [inschakelen van gegevensverzameling van model](how-to-enable-data-collection.md) telkens wanneer u de installatiekopie implementeert, de gegevens die nodig zijn om vast te leggen van de gegevens, zoals de referenties voor uw persoonlijke blob-archief, automatisch worden ingericht.

> [!Important]
> Microsoft heeft niet de gegevens die moeten worden verzameld uit uw model weergegeven. De gegevens is rechtstreeks naar de Azure storage-account voor uw werkruimte verzonden.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoe en waar u modellen kunt implementeren](how-to-deploy-and-where.md) met de Azure Machine Learning-service.
