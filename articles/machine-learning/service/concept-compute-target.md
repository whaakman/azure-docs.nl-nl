---
title: COMPUTE-doelen
titleSuffix: Azure Machine Learning service
description: Een compute-doel kunt u om op te geven van de compute-resource waar u uw service-implementatie uw trainingsscript of de host uitvoeren. Deze locatie mogelijk op uw lokale computer of een cloud-gebaseerde compute-resource.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/30/2019
ms.openlocfilehash: 42c0f5460a63b781aafdd43410761e2d7b17944d
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66755348"
---
#  <a name="what-is-a-compute-target-in-azure-machine-learning-service"></a>Wat is een compute-doel in Azure Machine Learning-service? 

Een compute-doel kunt u om op te geven van de compute-resource waar u uw service-implementatie uw trainingsscript of de host uitvoeren. Deze locatie mogelijk op uw lokale computer of een cloud-gebaseerde compute-resource.

COMPUTE-doelen kunnen eenvoudig uw compute-omgeving wijzigen zonder uw code te wijzigen.  Een typische model development lifecycle:

* Met dev/experimenten op een kleine hoeveelheid gegevens starten. In deze fase, wordt u aangeraden een lokale omgeving. Bijvoorbeeld, de lokale computer of een cloud-gebaseerde VM.
* Training op grotere gegevenssets opschalen of gedistribueerd training over het gebruik van een van de [training doelen](#train).  
* Implementeren voor verschillende omgevingen voor webhosting of voor IoT-apparaten met behulp van een van de [implementatiedoelen](#deploy).

De rekenresources die u voor uw compute-doelen gebruikt zijn gekoppeld aan een [werkruimte](concept-workspace.md). COMPUTE-resources dan de lokale computer worden gedeeld door gebruikers van de werkruimte.

## <a name="train"></a> Training-doelen

Azure Machine Learning-service heeft verschillende ondersteuning voor andere compute-resources.  U kunt ook uw eigen compute-resource koppelen, hoewel ondersteuning voor verschillende scenario's als variëren kunnen hieronder uitgelegd:

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]


## <a name="deploy"></a>Implementatiedoelen

De volgende rekenresources kunnen worden gebruikt voor het hosten van de implementatie van uw model.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]


## <a name="managed-compute"></a>Volledig beheerde rekenprocessen

Een beheerde compute-resource is gemaakt en beheerd door Azure Machine Learning-service. Deze compute is geoptimaliseerd voor workloads van machine learning. Azure Machine Learning-Computing is de enige beheerde compute vanaf 30 mei 2019. Aanvullende beheerde rekenbronnen kunnen in de toekomst worden toegevoegd.

### <a name="amlcompute"></a> Azure Machine Learning-Computing

U kunt Azure Machine Learning-Computing kunt gebruiken voor training en inferentietaken batch (Preview).  Met deze compute-resource hebt:

* Één of meerdere node cluster
* Automatisch wordt geschaald telkens wanneer die u een uitvoering verzenden 
* Automatische Clusterbeheer en taakplanning 
* Ondersteuning voor zowel CPU en GPU

U kunt Azure Machine Learning-Computing-exemplaren maken met een van de volgende:

* Azure Portal
* The Azure Machine Learning SDK
* De Azure CLI

Alle andere compute-resources moeten worden gemaakt buiten de werkruimte en vervolgens gekoppeld.

## <a name="unmanaged-compute"></a>Niet-beheerde compute

Een niet-beheerde compute-resource is *niet* beheerd door Azure Machine Learning-service. U maakt dit type reken-en buiten Azure Machine Learning en koppelt deze aan uw werkruimte. Niet-beheerde compute-resources kunnen extra stappen moeten worden voor u te houden of om de prestaties voor machine learning-workloads te verbeteren.

## <a name="next-steps"></a>Volgende stappen

* [Compute-doelen voor modeltraining instellen](how-to-set-up-training-targets.md)
* [Implementeer modellen met de Azure Machine Learning-service](how-to-deploy-and-where.md)