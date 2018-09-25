---
title: Azure voordeur Service - ondersteuning voor HTTP-headers-protocol | Microsoft Docs
description: Dit artikel helpt u begrijpen van de ondersteunde protocollen van de HTTP-header met de voordeur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 0dcb769627714be9da55faf2a8e82c8750789498
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038847"
---
# <a name="azure-front-door-service---http-headers-protocol-support"></a>Azure voordeur Service - ondersteuning voor HTTP-headers-protocol
Dit document bevat een overzicht van het protocol dat Azure voordeur Service met de verschillende onderdelen van het pad van de aanroep ondersteunt zoals wordt beschreven door de onderstaande afbeelding. De onderstaande secties vindt u meer inzicht in de HTTP-Headers die ondersteuning biedt voor voordeur.

![Azure voordeur Service HTTP-headers-protocol][1]

>[!WARNING]
>Azure voordeur Service biedt geen garanties op een HTTP-headers die hier niet wordt vermeld.

## <a name="1-client-to-front-door"></a>1. Client naar de voordeur
Voordeur accepteert de meeste headers van de inkomende aanvraag (zonder deze te wijzigen), er zijn echter enkele gereserveerde headers die wordt verwijderd uit de binnenkomende aanvraag als ze worden verzonden. Dit omvat headers met de volgende voorvoegsels:
 - X-FD *
 - X-MS *

## <a name="2-front-door-to-backend"></a>2. Voordeur naar back-end

Voordeur bevat de headers van de inkomende aanvraag, tenzij ze zijn verwijderd vanwege de beperkingen die hierboven worden vermeld. Voordeur wordt ook de volgende headers toevoegen:

| Koptekst  | Voorbeeld en beschrijving |
| ------------- | ------------- |
| X-MS-Ref |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ ==* </br> Dit is een unieke referentie-tekenreeks die een aanvraag die is geleverd door de voordeur aanduidt. Het is essentieel voor het oplossen van problemen zoals het wordt gebruikt om te zoeken naar de logboeken openen.|
| X-MS-RequestChain |  *X-MS-RequestChain: hops voor de = 1* </br> Dit is een header die voordeur wordt gebruikt voor het detecteren van aanvraag lussen en een afhankelijkheid niet op deze gebruikers moeten uitvoeren. |
| X-MS-Via |  *X-MS-Via: Azure* </br> Dit wordt toegevoegd door de voordeur om aan te geven dat Azure/voordeur een tussenliggende ontvanger voor de aanvraag tussen de client en de back-end is. |

## <a name="3-front-door-to-client"></a>3. Voordeur naar client

Hieronder vindt u de headers die zijn verzonden vanaf de voordeur aan clients. Alle headers worden verzonden naar de voordeur vanaf de back-end worden doorgegeven aan de client ook.

| Koptekst  | Voorbeeld |
| ------------- | ------------- |
| X-MS-Ref |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ ==* </br> Dit is een unieke referentie-tekenreeks die een aanvraag die is geleverd door de voordeur aanduidt. Het is essentieel voor het oplossen van problemen zoals het wordt gebruikt om te zoeken naar de logboeken openen.|

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een voordeur](quickstart-create-front-door.md).
- Informatie over [de werking van de voordeur](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png