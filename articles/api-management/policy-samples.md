---
title: Voorbeelden van Azure API Management-beleid | Microsoft Docs
description: Meer informatie over het beschikbaar voor gebruik in Azure API Management-beleid.
services: api-management
documentationcenter: 
author: Juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: sample
ms.date: 10/31/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: e951eb47290a2d1b6f892f1ccf923f6ce0c1899c
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="api-management-policy-samples"></a>Voorbeelden van API Management-beleid

[Beleid](api-management-howto-policies.md) zijn een krachtige mogelijkheid van het systeem waarmee de uitgever het gedrag van de API via configuratie wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of het antwoord van een API. De volgende tabel bevat koppelingen naar voorbeelden en geeft u een korte beschrijving van elk voorbeeld.

|||
|---|---|
|**Inkomende beleidsregels**||
|[Een koptekst doorgestuurde zodat de back-end API's voor het maken van de juiste URL's toevoegen](./policies/set-header-to-enable-backend-to-construct-urls.md?toc=api-management/toc.json) |Demonstreert hoe u een koptekst doorgestuurde toevoegen in de binnenkomende aanvraag waarmee de end-API om juiste URL's samen te stellen.|
|[Een header met een correlatie-id toevoegen](./policies/add-correlation-id.md?toc=api-management/toc.json) |Demonstreert hoe u een koptekst met een correlatie-ID moet de binnenkomende aanvraag kunt toevoegen.|
|[Mogelijkheden toevoegen aan een back-endservice en de reactie in cache](./policies/cache-response.md?toc=api-management/toc.json) |Laat zien hoe mogelijkheden toevoegen aan een back-endservice. Bijvoorbeeld, een naam van de locatie in plaats van de breedtegraad en lengtegraad in een prognose weer API accepteren.|
|[Op basis van claims JWT toegang verlenen](./policies/authorize-request-based-on-jwt-claims.md?toc=api-management/toc.json) |Laat zien hoe u toegang verlenen aan specifieke HTTP-methoden op een op basis van claims van JWT-API.|
|[Toestaan van toegang met behulp van Google-OAuth-token](./policies/use-google-as-oauth-token-provider.md?toc=api-management/toc.json) |Laat zien hoe u toegang verlenen aan uw eindpunten voor Google als een provider van OAuth-token.|
|[Shared Access Signature en doorsturen van aanvraag naar Azure storage genereren](./policies/generate-shared-access-signature.md?toc=api-management/toc.json) |Laat zien hoe genereren [Shared Access Signature](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) expressies gebruiken en de aanvraag doorsturen naar Azure storage met herschrijven-uri-beleid. |
|[OAuth2 toegangstoken ophalen uit AAD en deze doorsturen naar de back-end](./policies/use-oauth2-for-authorization.md?toc=api-management/toc.json) |Biedt en voorbeeld van het gebruik van OAuth2 voor autorisatie tussen de gateway en een back-end. Er wordt weergegeven hoe verkrijgen van een toegangstoken van AAD en deze doorsturen naar de back-end.|
|[X-CSRF ophalen van het token uit SAP-gateway met behulp van beleid voor verbindingsaanvragen verzenden](./policies/get-x-csrf-token-from-sap-gateway.md?toc=api-management/toc.json) |Laat zien hoe X CSRF patroon dat wordt gebruikt door veel API's implementeren. In dit voorbeeld is specifiek voor SAP-Gateway. |
|[De aanvraag op basis van de grootte van de hoofdtekst routeren](./policies/route-requests-based-on-size.md?toc=api-management/toc.json) |Demonstreert hoe u aanvragen op basis van de grootte van hun instanties.|
|[Aanvraag contextinformatie sturen naar de back-endservice](./policies/send-request-context-info-to-backend-service.md?toc=api-management/toc.json) |Laat zien hoe sommige contextgegevens te verzenden naar de back-endservice voor logboekregistratie of voor verwerking.|
|[Cacheduur antwoord instellen](./policies/set-cache-duration.md?toc=api-management/toc.json) |Demonstreert hoe u Cacheduur antwoord in Cache-Control-header verzonden door de back-end met maxAge waarde instellen.|
|**Beleid voor uitgaande**||
|[Antwoordinhoud filteren](./policies/filter-response-content.md?toc=api-management/toc.json) | Demonstreert hoe u voor het filteren van gegevenselementen van de nettolading van de reactie op basis van het product dat is gekoppeld aan de aanvraag.|
|**Beleid op fout**||
|[Fouten registreren in Stackify](./policies/log-errors-to-stackify.md?toc=api-management/toc.json) |Laat zien hoe een fout logboekregistratie beleid voor het verzenden van fouten naar Stackify voor logboekregistratie toe te voegen.|
