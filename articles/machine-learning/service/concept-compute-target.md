---
title: 'COMPUTE-doelen: waar u trainen en modellen implementeren'
titleSuffix: Azure Machine Learning service
description: Opgeven waar u wilt trainen of implementeren van uw model met Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 07/10/2019
ms.openlocfilehash: a7944b284a9c1c0424af54874554d05d49ad4b20
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806059"
---
#  <a name="what-are-compute-targets-in-azure-machine-learning-service"></a>Wat zijn de compute-doelen in Azure Machine Learning-service? 

Een **compute-doel** is een aangewezen compute-resource/omgeving waarin u uw service-implementatie uw trainingsscript of de host uitvoeren. Deze locatie mogelijk op uw lokale computer of een cloud-gebaseerde compute-resource. Met behulp van compute-doelen kunnen u eenvoudig voor u later uw compute-omgeving wijzigen zonder uw code te wijzigen.  

U kunt in de levenscyclus van een typische model-ontwikkeling:
1. Ontwikkelen en te experimenteren op een kleine hoeveelheid gegevens starten. In deze fase wordt aangeraden uw lokale omgeving bevinden als uw compute-doel (lokale computer of cloud-gebaseerde VM). 
2. Opschalen naar grotere hoeveelheden gegevens of training over het gebruik van een van de volgende gedistribueerd [training van compute-doelen](#train).  
3. Als uw model klaar is, deze implementeren in een web-hosten van de omgeving of IoT-apparaat met een van deze [implementatie van compute-doelen](#deploy).

De rekenresources die u voor uw compute-doelen gebruikt zijn gekoppeld aan een [werkruimte](concept-workspace.md). COMPUTE-resources dan de lokale computer worden gedeeld door gebruikers van de werkruimte.

## <a name="train"></a> Training compute-doelen

Azure Machine Learning-service heeft verschillende ondersteuning voor andere compute-resources.  U kunt ook uw eigen compute-resource koppelen Hoewel ondersteuning voor verschillende scenario's kunnen variëren.

[!INCLUDE [aml-compute-target-train](../../../includes/aml-compute-target-train.md)]

Meer informatie over [instellen en gebruiken van een compute-doel voor modeltraining](how-to-set-up-training-targets.md).

## <a name="deploy"></a>Implementatiedoelen

De volgende rekenresources kunnen worden gebruikt voor het hosten van de implementatie van uw model.

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

Informatie over [waar en hoe u uw model implementeert naar een compute-doel](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Azure Machine Learning-Computing (beheerd)

Een beheerde compute-resource is gemaakt en beheerd door Azure Machine Learning-service. Deze compute is geoptimaliseerd voor workloads van machine learning. Azure Machine Learning-Computing is de enige beheerde compute vanaf 30 mei 2019. Aanvullende beheerde rekenbronnen kunnen in de toekomst worden toegevoegd.

U kunt Azure Machine Learning-Computing kunt gebruiken voor training en inferentietaken batch (Preview).  Met deze compute-resource hebt:

* Één of meerdere node cluster
* Automatisch wordt geschaald telkens wanneer die u een uitvoering verzenden 
* Automatische Clusterbeheer en taakplanning 
* Ondersteuning voor zowel CPU en GPU

U kunt Azure Machine Learning-Computing-exemplaren maken in Azure portal, met de SDK of met de CLI. Wanneer deze is automatisch onderdeel van uw werkruimte in tegenstelling tot andere soorten compute-doelen.

## <a name="unmanaged-compute"></a>Niet-beheerde compute

Een niet-beheerde compute-doel is *niet* beheerd door Azure Machine Learning-service. U maakt dit type compute-doel buiten Azure Machine Learning en deze koppelen aan uw werkruimte. Niet-beheerde compute-resources kunnen extra stappen moeten worden voor u te houden of om de prestaties voor machine learning-workloads te verbeteren.

## <a name="next-steps"></a>Volgende stappen

Leer hoe u het volgende doet:
* [Een compute-doel instellen voor uw model te trainen](how-to-set-up-training-targets.md)
* [Uw model implementeert naar een compute-doel](how-to-deploy-and-where.md)