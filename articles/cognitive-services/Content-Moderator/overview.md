---
title: Wat is Content Moderator?
titlesuffix: Azure Cognitive Services
description: Leer hoe u met Content Moderator ongepaste inhoud in door gebruikers gegenereerde inhoud kunt traceren, markeren, beoordelen en filteren.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: overview
ms.date: 06/15/2017
ms.author: sajagtap
ms.openlocfilehash: e109376f47d921fb18d7bb9a6252e80315419ec0
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226038"
---
# <a name="what-is-content-moderator"></a>Wat is Content Moderator?

Content Moderation is het proces van het controleren op mogelijk aanstootgevende, ongewenste en riskant inhoud. De gecontroleerde inhoud kan bestaan uit afbeeldingen, tekst en video's.

## <a name="where-it-is-used"></a>Toepassingen van Content Moderator

De volgende lijst bevat enkele voorbeeldscenario's waarin Content Moderator kan worden gebruikt:

- Online-marktplaatsen waar toezicht op productcatalogi en door gebruikers gegenereerde inhoud van belang is
- Gaming-bedrijven die toezicht willen houden op door gebruikers gegenereerde game-artefacten en chatruimten
- Platform voor sociale berichten waar toezicht nodig is voor afbeeldingen, tekst en video's die worden toegevoegd door gebruikers
- Grote mediabedrijven die gecentraliseerd inhoudstoezicht willen implementeren voor hun inhoud
- Leveranciers van oplossingen voor lager en voortgezet onderwijs die willen filteren op schadelijke en aanstootgevende inhoud voor studenten en docenten

## <a name="what-it-includes"></a>Samenstelling van Content Moderator

Content Moderator bestaat uit verschillende webservice-API's en een ingebouwd programma voor beoordeling door menselijke beoordelaars waarmee afbeeldingen, tekst en video's kunnen worden gecontroleerd.

![Blokschema van Content Moderator](images/content-moderator-block-diagram.png)

## <a name="apis"></a>API's

Content Moderator omvat de volgende API's:
  - [**Teksttoezicht-API**](text-moderation-api.md): gebruik deze API voor het scannen van tekst op mogelijk grof, expliciet, suggestief en aanstootgevend taalgebruik, evenals persoonsgegevens.
  - [**Aangepaste termenlijst-API**](try-terms-list-api.md): gebruik deze API om overeenkomsten te vinden in aangepaste lijsten van termen naast de ingebouwde termen. Gebruik deze lijsten om inhoud volgens de geldende beleidsregels voor inhoud toe te of te weigeren.  
  - [**Afbeeldingstoezicht-API**](image-moderation-api.md): gebruik deze API om afbeeldingen te scannen op erotische en ongepaste inhoud, tekst in afbeeldingen te detecteren met OCR (optische tekenherkenning) en gezichten te detecteren.
  - [**Aangepaste afbeeldingenlijst-API**](try-image-list-api.md): gebruik deze API om overeenkomsten te vinden in aangepaste lijsten van afbeeldingen, bestaande uit vooraf geïdentificeerde inhoud die u niet opnieuw hoeft te classificeren.
  - [**Videotoezicht-API**](video-moderation-api.md): gebruik deze API om video's te scannen op mogelijk erotische en ongepaste inhoud.
  - [**Toezicht-API's**](try-review-api-job.md): gebruik de bewerkingen [Jobs](try-review-api-job.md), [Reviews](try-review-api-review.md) en [Workflow](try-review-api-workflow.md) om werkstromen voor menselijke beoordeling te maken en automatiseren binnen het beoordelingsprogramma.

## <a name="human-review-tool"></a>Programma voor menselijke beoordeling

Uw abonnement op Content Moderator bevat ook het ingebouwde [hulpprogramma voor menselijke beoordeling](Review-Tool-User-Guide/human-in-the-loop.md). Gebruik de eerder genoemde toezicht-API om beoordelingen te maken van tekst, afbeeldingen en video's zodat uw menselijke beoordelaars de uiteindelijke beslissing kunnen nemen.

![Programma voor beoordeling van video's van Content Moderator](images/video-review-default-view.png)

## <a name="next-steps"></a>Volgende stappen

Gebruik de [snelstartgids](quick-start.md) om aan de slag te gaan met Content Moderator.
