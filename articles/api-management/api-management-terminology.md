---
title: Azure API Management-terminologie | Microsoft Docs
description: Dit artikel bevat definities voor de voorwaarden die specifiek voor API Management zijn.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 10/11/2017
ms.author: apimpm
ms.openlocfilehash: b99ca444532799d21850058eae0e3f40ed871135
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61074051"
---
# <a name="terminology"></a>Terminologie

Dit artikel bevat definities voor de voorwaarden die specifiek zijn voor API Management (APIM).

## <a name="term-definitions"></a>Term definities

* **Back-end API** -een HTTP-service waarmee uw API en de bewerkingen worden geïmplementeerd. 
* **Front-end-API**/**APIM API** -API's niet als host voor een APIM-API, maakt deze facades voor uw API's om de gevel aan uw wensen aanpassen zonder dat u de back-end API. Zie voor meer informatie, [importeren en publiceren van een API](import-and-publish.md).
* **APIM product** -een product bevat een of meer API's, evenals een gebruiksquotum en de gebruiksvoorwaarden. U kunt een aantal API's opnemen en deze beschikbaar stellen voor ontwikkelaars via de portal voor ontwikkelaars. Zie voor meer informatie, [maken en publiceren van een product](api-management-howto-add-products.md).
* **APIM API-bewerking** -voor elke APIM API vertegenwoordigt een reeks bewerkingen die beschikbaar zijn voor ontwikkelaars. Elke APIM API bevat een verwijzing naar de back-end-service die de API wordt geïmplementeerd, en de bewerkingen zijn toegewezen aan de bewerkingen die zijn geïmplementeerd door de back-end-service. Zie voor meer informatie, [gesimuleerde van een API-reacties](mock-api-responses.md).
* **Versie** - soms wilt u nieuwe publicatie of andere API-functies voor sommige gebruikers, terwijl anderen wilt houden met de API die momenteel geschikt is voor hen. Zie voor meer informatie, [meerdere versies van uw API publiceren](api-management-get-started-publish-versions.md).
* **Revisie** : wanneer uw API is gereed om te gaan en begint met het worden gebruikt door ontwikkelaars, u gewoonlijk moeten voorzichtig zijn bij het aanbrengen van wijzigingen aan die API en op hetzelfde moment niet te aanroepfuncties van uw API verstoren. Het is ook handig om ontwikkelaars te informeren over de aangebrachte wijzigingen. Zie voor meer informatie, [revisies gebruiken om](api-management-get-started-revise-api.md).
* **Portal voor ontwikkelaars** -klanten (ontwikkelaars) moeten de Developer-portal gebruiken voor toegang tot uw API's. De Developer-portal kan worden aangepast. Zie voor meer informatie, [de ontwikkelaarsportal aanpassen](api-management-customize-styles.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een exemplaar maken](get-started-create-service-instance.md)

