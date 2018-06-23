---
title: Overzicht van Azure Content beheerder | Microsoft Docs
description: Informatie over het gebruiken van inhoud beheerder om te volgen, vlag, beoordelen en filteren ongeschikte inhoud in gebruikers gegenereerde inhoud.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/15/2017
ms.author: sajagtap
ms.openlocfilehash: cd9e2e9c10e9dc5ba118c8319f76174bf6f0da9f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345661"
---
# <a name="what-is-content-moderator"></a>Wat is er inhoud beheerder?

Inhoud toezicht is het proces van de bewaking voor mogelijk aanstootgevend ongewenste en riskante inhoud. De gecontroleerde inhoud kan worden afbeeldingen, tekst- en video's.

## <a name="where-it-is-used"></a>Waar deze wordt gebruikt

De volgende lijst bevat enkele voorbeeldscenario's waar inhoud beheerder wordt gebruikt:

- Onlinemarktplaatsen toezicht houden van de product- en gebruiker gegenereerde inhoud
- Games bedrijven toezicht houden gebruiker game artefacten en chatruimten gegenereerd
- Sociale messaging platform toezicht houden afbeeldingen, tekst- en video's die zijn toegevoegd door hun gebruikers
- Media ondernemingen gecentraliseerde controle van inhoud voor hun inhoud implementeren
- K-12 education oplossingsproviders onjuiste en aanstootgevende inhoud voor studenten en onderwijsinstellingen filteren

## <a name="what-it-includes"></a>Wat bevat

Inhoud dat beheerder bestaat uit verschillende webservice-API's en een ingebouwde human-in-the-lus Bekijk hulpprogramma waarmee gemiddeld afbeeldingen, tekst- en video's.

![Diagram van inhoud beheerder blokkeren](images/content-moderator-block-diagram.png)

## <a name="apis"></a>API's

Inhoud beheerder omvat de volgende API's:
  - [**Tekst toezicht API**](text-moderation-api.md): deze API gebruiken om te scannen van tekst voor mogelijke taalgebruik expliciete, suggestieve aanstootgevende en persoonlijk herleidbare informatie (PII).
  - [**Lijst met aangepaste term API**](try-terms-list-api.md): deze API gebruiken voor het vergelijken van de aangepaste lijsten van termen naast de ingebouwde voorwaarden. Gebruik deze lijsten om inhoud aan de hand van uw beleid voor inhoud toestaan of blokkeren.  
  - [**Afbeelding van toezicht API**](image-moderation-api.md): Gebruik deze API om afbeeldingen voor de inhoud voor volwassenen en mooie scannen, de tekst in de afbeeldingen met de mogelijkheid OCR (OCR) detecteren en vlakken detecteren.
  - [**Lijst van de aangepaste installatiekopie API**](try-image-list-api.md): deze API gebruiken voor het vergelijken van de aangepaste lijsten van installatiekopieën, vooraf vastgestelde inhoud die u hoeft niet te classificeren opnieuw.
  - [**Video toezicht API**](video-moderation-api.md): deze API gebruiken om te scannen van video's voor potentiële inhoud voor volwassenen en mooie.
  - [**API's bekijken**](try-review-api-job.md): Gebruik de [taken](try-review-api-job.md), [beoordelingen](try-review-api-review.md), en [werkstroom](try-review-api-workflow.md) bewerkingen voor het maken en human in de lus werkstromen binnen automatiseren het hulpprogramma controleren.

## <a name="human-review-tool"></a>Programma voor menselijke beoordeling

Uw abonnement inhoud beheerder omvat de ingebouwde [menselijke revisie hulpprogramma](Review-Tool-User-Guide/human-in-the-loop.md). De eerder genoemde revisie API gebruiken voor het maken van beoordelingen van tekst, afbeeldingen en video's voor uw menselijke moderators om definitieve beslissingen te nemen.

![Inhoud beheerder video bekijken hulpprogramma](images/video-review-default-view.png)

## <a name="next-steps"></a>Volgende stappen

Gebruik de [Quick Start](quick-start.md) aan de slag met de beheerder van inhoud.
