---
title: Wat is Azure Content Moderator?
titlesuffix: Azure Cognitive Services
description: Leer hoe u met Content Moderator ongepaste inhoud in door gebruikers gegenereerd materiaal kunt traceren, markeren, beoordelen en filteren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 02/20/2019
ms.author: pafarley
ms.openlocfilehash: 14cf500cff9f1f7f8b1573862c3060326e6518fa
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688852"
---
# <a name="what-is-azure-content-moderator"></a>Wat is Azure Content Moderator?

De Azure Content Moderator-API is een cognitieve service waarmee tekst, afbeeldingen en video-inhoud van materiaal wordt gecontroleerd op mogelijk aanstootgevende, riskante of anderszins ongewenst inhoud. Wanneer dergelijk materiaal wordt gevonden, past de service de relevante labels (vlaggen) op de inhoud toe. Uw app kan gelabelde inhoud vervolgens afhandelen om te voldoen aan de regelgeving of om een beoogde omgeving voor gebruikers te beheren. Zie de [beheer-API's](#moderation-apis) sectie voor meer informatie over wat de inhoud van de verschillende vlaggen geven.

## <a name="where-it-is-used"></a>Toepassingen van Content Moderator

Hieronder vindt u enkele scenario's waarin een softwareontwikkelaar of -team Content Moderator kan gebruiken:

- Online marktplaatsen die gemiddeld productcatalogi en andere gebruikers gegenereerde inhoud.
- Bedrijven die door gebruikers gegenereerde game artefacten en chatruimten gemiddeld spelen van games.
- Sociale messaging-platforms die gemiddeld afbeeldingen, tekst en video's die zijn toegevoegd door hun gebruikers.
- Media ondernemingen die centrale beheer voor hun inhoud implementeren.
- K-12 education leveranciers van oplossingen gefilterd op inhoud die niet geschikt is voor studenten en docenten.

> [!NOTE]
> U kunt Content Moderator niet gebruiken voor het detecteren van ongeldige onderliggende misbruik installatiekopieën. Echter gekwalificeerde organisaties kunnen gebruiken de [PhotoDNA Cloudservice](https://www.microsoft.com/photodna "Cloudservice van Microsoft PhotoDNA") scherm voor dit type inhoud.

## <a name="what-it-includes"></a>Samenstelling van Content Moderator

De Content Moderator-service bestaat uit verschillende webservice-API's die beschikbaar zijn via zowel REST-aanroepen als een SDK voor .NET. Hiertoe behoort ook het hulpprogramma voor menselijke beoordeling, waarmee menselijke revisoren de service kunnen verbeteren of de toezichtfunctie kunnen aanpassen.

## <a name="moderation-apis"></a>Beheer-API's

De Content Moderator-service bevat beheer-API's, die inhoud voor materiaal dat is mogelijk ongepast of aanstootgevend controleren.

![diagram voor Content Moderator afbeeldingstoezicht-API's blokkeren](images/content-moderator-mod-api.png)

De volgende tabel beschrijft de verschillende typen afbeeldingstoezicht-API's.

| API-groep | Description |
| ------ | ----------- |
|[**Teksttoezicht**](text-moderation-api.md)| Tekst voor aanstootgevende inhoud, seksueel expliciet of suggestieve inhoud, grof taalgebruik en persoonlijke gegevens scant.|
|[**Aangepaste terminologielijsten**](try-terms-list-api.md)| Hiermee wordt tekst niet alleen vergeleken met de ingebouwde termen maar ook met een aangepaste terminologielijst. U kunt aangepaste lijsten gebruiken om inhoud te blokkeren of toe te staan volgens uw eigen beleidsregels voor inhoud.|  
|[**Afbeeldingstoezicht**](image-moderation-api.md)| Hiermee worden afbeeldingen gescand op erotische of racistische inhoud, wordt tekst in afbeeldingen gedetecteerd met de OCR-functie (optische tekenherkenning) en worden gezichten gedetecteerd.|
|[**Aangepaste afbeeldingslijsten**](try-image-list-api.md)| Hiermee worden afbeeldingen vergeleken met een aangepaste lijst van afbeeldingen. U kunt aangepaste afbeeldingslijsten gebruiken om te filteren op exemplaren van veelvoorkomende terugkerende inhoud die u niet opnieuw wilt classificeren.|
|[**Videotoezicht**](video-moderation-api.md)| Hiermee worden video's gescand op erotische of racistische inhoud en worden tijdmarkeringen voor deze inhoud geretourneerd.|

## <a name="review-apis"></a>Beoordelings-API's

De beoordeling-API's kunt u uw pijplijn toezicht integreren met menselijke revisoren. Gebruik de [taken](review-api.md#jobs), [beoordelingen](review-api.md#reviews), en [werkstroom](review-api.md#workflows) bewerkingen om te maken en automatiseer werkstromen met human-in-the-loop de [beoordelingsprogramma](#the-review-tool) () Zie hieronder).

> [!NOTE]
> De werkstroom-API is nog niet beschikbaar in de .NET SDK, maar kan worden gebruikt met de REST-eindpunt.

![Blokdiagram voor Content Moderator API's bekijken](images/content-moderator-rev-api.png)

## <a name="the-review-tool"></a>Het beoordelingsprogramma

De Content Moderator-service bevat ook de webgebaseerde [beoordelingsprogramma](Review-Tool-User-Guide/human-in-the-loop.md), die als host fungeert voor de inhoud beoordelingen voor menselijke moderators te verwerken. De service wordt niet getraind door menselijke invoer, maar de combinatie van het werk van de service en de menselijke beoordelingsteams zorgt voor een juiste balans tussen de efficiëntie en de nauwkeurigheid. Het beoordelingsprogramma biedt ook een gebruiksvriendelijke front-end voor een verscheidenheid aan Content Moderator-resources.

![Startpagina van het hulpprogramma voor menselijke beoordeling van Content Moderator](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Gegevensprivacy en -beveiliging

Zoals geldt voor alle services van Cognitive Services, dienen ontwikkelaars die de Content Moderator-service gebruiken op de hoogte te zijn van het beleid van Microsoft inzake klantgegevens. Zie de [pagina Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) (Engelstalig) in het Microsoft Trust Center voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Ga aan de slag met de Content Moderator-service door de instructies in [Content Moderator op het web proberen](quick-start.md) te volgen.