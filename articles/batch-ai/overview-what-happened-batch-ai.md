---
title: Wat gebeurt er met Azure Batch AI? | Microsoft Docs
description: Meer informatie over wat er met Azure Batch AI en de compute-optie van de Azure Machine Learning-service gebeurt.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 12/14/2018
ms.author: garye
ms.openlocfilehash: 6803a47ae77c072eff05df65e37156c90aabf3e6
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436871"
---
# <a name="whats-happening-to-azure-batch-ai"></a>Wat gebeurt er met Azure Batch AI?

**De Azure Batch AI-service wordt in maart buiten gebruik gesteld.** De op-schaal-training en scoremogelijkheden van Batch AI zijn nu beschikbaar in de [Azure Machine Learning-service](../machine-learning/service/overview-what-is-azure-ml.md), die algemeen beschikbaar werd op 4 december 2018.

Samen met veel andere machine learning-mogelijkheden bevat de Azure Machine Learning-service een cloud-gebaseerd, beheerd rekendoel voor het trainen, implementeren en scoren van Machine Learning-modellen. Dit rekendoel heet [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Begin met migreren en maak er vandaag nog gebruik van](#migrate). U kunt met de Azure Machine Learning-service communiceren via de [Python-SDK's](../machine-learning/service/quickstart-create-workspace-with-python.md), opdrachtregelinterface en de [Azure Portal](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Ondersteuningstijdlijn

| Date | Ondersteuningsgegevens Batch AI-service |
| ---- |-----------------|
| &nbsp;14&#x2c;&nbsp;december 2018| Blijft uw bestaande Azure Batch AI-abonnementen gebruiken zoals voordien. U kunt echter geen **nieuwe abonnementen** meer registreren en er worden geen nieuwe investeringen in deze service gedaan.|
| &nbsp;31&#x2c;&nbsp;maart 2019 | Na deze datum functioneren bestaande Batch AI-abonnementen niet meer. |

<a name="migrate"></a>
## <a name="how-do-i-migrate"></a>Hoe migreer ik?

Als u onderbrekingen in uw toepassingen wilt voorkomen en wilt profiteren van de nieuwste functies, moet u de volgende stappen uitvoeren vóór 31 maart 2019:

1. Een werkruimte maken in de Azure Machine Learning-service en aan de slag gaan:
    + [Snelstart op basis van Python](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Snelstart op basis van Azure Portal](../machine-learning/service/quickstart-get-started.md)

1. Een [Azure Machine Learning Compute](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute)-instantie installeren voor modeltraining.

1. Werk uw scripts bij voor het gebruik van Azure Machine Learning Compute.

## <a name="support"></a>Ondersteuning

Ondersteuning is beschikbaar voor bestaande klanten die willen migreren naar de uitgebreidere [Azure Machine Learning-service](https://aka.ms/aml-docs).

Als Azure Machine Learning-service niet voldoet aan uw behoeften terwijl er een ondersteunde functionaliteit bestaat in de Batch AI-service, opent u een Batch AI-ondersteuningsaanvraag aan het ondersteuningsteam om uw abonnement op de whitelist te plaatsen voor het gebruikmaken van Batch AI tot stopzetting van de service.

## <a name="next-steps"></a>Volgende stappen

+ Lees [Overzicht van de Azure Machine Learning-service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Configureer een rekendoel voor modeltraining](../machine-learning/service/how-to-set-up-training-targets.md) met de Azure Machine Learning-service.

+ Controleer de [Azure-roadmap](https://azure.microsoft.com/updates/) voor meer informatie over andere Azure-service-updates.
