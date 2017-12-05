---
title: Terminologie voor Azure API Management | Microsoft Docs
description: Dit artikel bevat definities voor de voorwaarden die specifiek voor API Management zijn.
services: api-management
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/11/2017
ms.author: apimpm
ms.openlocfilehash: 9391b65a5aade4c050ca964354bfea2d3a2338d8
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="terminology"></a>Terminologie

Dit artikel bevat definities voor de voorwaarden die specifiek voor API Management (APIM) zijn.

## <a name="term-definitions"></a>Definities van de termijn

* **Back-end-API** -een HTTP-service waarmee uw API en bewerkingen worden geïmplementeerd. 
* **Frontend API**/**APIM API** -API's niet als host voor een APIM API, maakt deze façades voor uw API's om de façade naar wens aanpassen zonder dat u de back-end API. Zie voor meer informatie [importeren en publiceren van een API](import-and-publish.md).
* **APIM product** -een product bevat een of meer API's, evenals een gebruiksquotum en de gebruiksvoorwaarden. U kunt een aantal API's en ze bieden voor ontwikkelaars via de portal voor ontwikkelaars. Zie voor meer informatie [maken en publiceren van een product](api-management-howto-add-products.md).
* **APIM API-bewerking** -elke APIM API vertegenwoordigt een reeks bewerkingen die beschikbaar zijn voor ontwikkelaars. Elke API APIM bevat een verwijzing naar de back-end-service waarmee de API en de bewerkingen zijn toegewezen aan de bewerkingen die door de back-end-service geïmplementeerd. Zie voor meer informatie [antwoorden van een model API](mock-api-responses.md).
* **Versie** - soms wilt u voor het publiceren van nieuwe of andere API-functies voor sommige gebruikers, terwijl anderen wilt houden met de API die geschikt is voor deze. Zie voor meer informatie [publiceren van meerdere versies van uw API](api-management-get-started-publish-versions.md).
* **Revisie** : wanneer u uw API is gereed om te gaan en begint met het worden gebruikt door ontwikkelaars, u meestal Wees voorzichtig bij het aanbrengen van wijzigingen die API en op hetzelfde moment niet te verstoren aanroepfuncties van uw API wilt maken. Het is ook handig om te laten weten over de aangebrachte wijzigingen ontwikkelaars. Zie voor meer informatie [revisies gebruiken](api-management-get-started-revise-api.md).
* **Portal voor ontwikkelaars** -moeten uw klanten (ontwikkelaars) de portal voor ontwikkelaars voor toegang tot uw API's gebruiken. De portal voor ontwikkelaars kan worden aangepast. Zie voor meer informatie [de ontwikkelaarsportal aanpassen](api-management-customize-styles.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Geen exemplaar maken](get-started-create-service-instance.md)

