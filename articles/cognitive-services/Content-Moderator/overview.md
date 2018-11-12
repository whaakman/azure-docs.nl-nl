---
title: Wat is Azure Content Moderator?
titlesuffix: Azure Cognitive Services
description: Leer hoe u met Content Moderator ongepaste inhoud in door gebruikers gegenereerd materiaal kunt traceren, markeren, beoordelen en filteren.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: overview
ms.date: 10/22/2018
ms.author: sajagtap
ms.openlocfilehash: 076948e7434802af7f0ad47f279335009817d40e
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209581"
---
# <a name="what-is-azure-content-moderator"></a>Wat is Azure Content Moderator?

De Azure Content Moderator-API is een cognitieve service waarmee tekst, afbeeldingen en video-inhoud van materiaal wordt gecontroleerd op mogelijk aanstootgevende, riskante of anderszins ongewenst inhoud. Wanneer dergelijk materiaal wordt gevonden, past de service de relevante labels (vlaggen) op de inhoud toe. Uw app kan gelabelde inhoud vervolgens afhandelen om te voldoen aan de regelgeving of om een beoogde omgeving voor gebruikers te beheren. Zie de sectie [Content Moderator-API's](#content-moderator-apis) voor meer informatie over wat de verschillende inhoudsvlaggen betekenen.

## <a name="where-it-is-used"></a>Toepassingen van Content Moderator

Hieronder vindt u enkele scenario's waarin een softwareontwikkelaar of -team Content Moderator kan gebruiken:

- Onlinemarktplaatsen die toezicht willen houden op productcatalogi en andere door gebruikers gegenereerde inhoud
- Gamingbedrijven die toezicht willen houden op door gebruikers gegenereerde gameartefacten en chatruimten
- Platforms voor sociale berichten die toezicht willen houden op afbeeldingen, tekst en video's die door gebruikers worden toegevoegd
- Grote mediabedrijven die gecentraliseerd toezicht implementeren voor hun inhoud
- Leveranciers van oplossingen voor lager en voortgezet onderwijs die willen filteren op inhoud die ongepast is voor leerlingen en docenten

## <a name="what-it-includes"></a>Samenstelling van Content Moderator

De Content Moderator-service bestaat uit verschillende webservice-API's die beschikbaar zijn via zowel REST-aanroepen als een SDK voor .NET. Hiertoe behoort ook het hulpprogramma voor menselijke beoordeling, waarmee menselijke revisoren de service kunnen verbeteren of de toezichtfunctie kunnen aanpassen.

![Blokdiagram voor Content Moderator met de Moderation-API's, Review-API's en het hulpprogramma voor menselijke beoordeling](images/content-moderator-block-diagram.png)

### <a name="content-moderator-apis"></a>Content Moderator-API's

De Content Moderator-service omvat API's voor de volgende scenario's.

| Bewerking | Beschrijving |
| ------ | ----------- |
|[**Teksttoezicht**](text-moderation-api.md)| Hiermee wordt tekst gescand op aanstootgevende inhoud, expliciet of suggestief seksuele inhoud, grof taalgebruik en persoonlijk identificeerbare informatie (PII).|
|[**Aangepaste terminologielijsten**](try-terms-list-api.md)| Hiermee wordt tekst niet alleen vergeleken met de ingebouwde termen maar ook met een aangepaste terminologielijst. U kunt aangepaste lijsten gebruiken om inhoud te blokkeren of toe te staan volgens uw eigen beleidsregels voor inhoud.|  
|[**Afbeeldingstoezicht**](image-moderation-api.md)| Hiermee worden afbeeldingen gescand op erotische of racistische inhoud, wordt tekst in afbeeldingen gedetecteerd met de OCR-functie (optische tekenherkenning) en worden gezichten gedetecteerd.|
|[**Aangepaste afbeeldingslijsten**](try-image-list-api.md)| Hiermee worden afbeeldingen vergeleken met een aangepaste lijst van afbeeldingen. U kunt aangepaste afbeeldingslijsten gebruiken om te filteren op exemplaren van veelvoorkomende terugkerende inhoud die u niet opnieuw wilt classificeren.|
|[**Videotoezicht**](video-moderation-api.md)| Hiermee worden video's gescand op erotische of racistische inhoud en worden tijdmarkeringen voor deze inhoud geretourneerd.|
|[**Beoordeling**](try-review-api-job.md)| Gebruik de bewerkingen [Taken](try-review-api-job.md), [Beoordelingen](try-review-api-review.md) en [Werkstroom](try-review-api-workflow.md) om werkstromen voor menselijke beoordeling met het hulpprogramma voor menselijke beoordeling te maken en te automatiseren. De werkstroom-API is nog niet beschikbaar via de SDK voor .NET.|

### <a name="human-review-tool"></a>Programma voor menselijke beoordeling

De Content Moderator-service bevat ook het webgebaseerde [hulpprogramma voor menselijke beoordeling](Review-Tool-User-Guide/human-in-the-loop.md). 

![Startpagina van het hulpprogramma voor menselijke beoordeling van Content Moderator](images/homepage.PNG)

Met de Beoordelen-API's kunt u teambeoordelingen instellen van tekst, afbeeldingen en video-inhoud, op basis van filters die u opgeeft. Daarna kunnen menselijke moderators de uiteindelijke toezichtsbeslissingen nemen. De service wordt niet getraind door menselijke invoer, maar de combinatie van het werk van de service en de menselijke beoordelingsteams zorgt voor een juiste balans tussen de efficiëntie en de nauwkeurigheid.

## <a name="next-steps"></a>Volgende stappen

Volg de [snelstart](quick-start.md) om aan de slag te gaan met Content Moderator.
