---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 674dd30ff3e493ec4c4036f032f82624a6ca5749
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334254"
---
## <a name="azure-cognitive-service-resource-types"></a>Resource typen voor de Azure cognitieve service

> [!NOTE]
> Abonnements eigenaren kunnen het maken van Cognitive Services resources voor resource groepen en abonnementen uitschakelen door [Azure Policy](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition)toe te passen, een beleids definitie van het type ' niet toegestaan resource typen ' toe te wijzen en op te geven  **Micro soft. CognitiveServices/accounts** als het doel resource type.

U hebt toegang tot Azure Cognitive Services via twee verschillende bronnen: Een resource met meerdere services of één service. Met deze abonnementen kunt u verbinding maken met één service of meerdere services tegelijk.

### <a name="multi-service-resource"></a>Resource met meerdere services

>[!WARNING]
> Op dit moment bieden deze services **geen** ondersteuning voor meerdere service sleutels: QnA Maker, spraak Services, Custom Vision en anomalie detectie.

Abonneren op een resource met meerdere services Cognitive Services:
* Met kunt u één Azure-Resource gebruiken voor de meeste Azure-Cognitive Services.
* Consolideert facturering van de services die u gebruikt. Zie [Cognitive Services prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/) voor aanvullende informatie.

### <a name="single-service-resource"></a>Resource met één service

Bronnen met één service (zoals Computer Vision of spraak Services) zijn beperkt tot de opgegeven service.