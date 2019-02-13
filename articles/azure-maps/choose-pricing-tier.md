---
title: Kies de prijscategorie voor Azure-kaarten rechts | Microsoft Docs
description: Meer informatie over prijzen van lagen die worden aangeboden door Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 01/02/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 56f9226f1bee630895725eb0b3806e294e9a5b51
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56218139"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Kies de prijscategorie van Azure Maps rechts

Azure kaarten biedt twee Prijscategorieën. Het doel van dit artikel is bij het recht voor prijscategorie voor uw behoeften te kiezen. Om te helpen de juiste prijscategorie kiezen, Stel uzelf de volgende twee vragen.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Welke mogelijkheden georuimtelijke plan ik gebruiken?
De S0-laag is geschikt voor u als de georuimtelijke core API's te voldoen aan de servicevereisten van uw. Als u meer geavanceerde mogelijkheden voor uw toepassing wilt, kunt u overwegen voor de prijscategorie S1. Voorbeeld van de mogelijkheden zijn areal plus gecombineerde beelden, aan een bereik van de route en geocodering van batch. De **prijzen laag mogelijkheden** tabel die volgt, kunt u een beter idee van de behoeften van uw toepassing. Hiermee kunt u ook Kies een prijscategorie meest geschikt is voor uw toepassing.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Het aantal gelijktijdige gebruikers wilt ik ondersteuning? 
De Prijscategorieën S0 en S1 verschillende hoeveelheden doorvoer van gegevens worden verwerkt. Voordat u een Azure-kaarten prijscategorie kiezen, vraagt u zelf enkele vragen. Een voorbeeld is "het aantal gelijktijdige gebruikers ik wilt ondersteunen?" De prijscategorie S0 verwerkt tot **50 query's per seconde**. De S1-ingangen voor prijzen laag **meer dan 50 query's per seconde**.

### <a name="pricing-tier-capabilities"></a>Prijzen van de mogelijkheden van laag

| Mogelijkheid                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Search                                  |        ✓           |     ✓    |
| Routering                                 |        ✓           |     ✓    |
| Renderen                                  |        ✓           |     ✓    |
| Verkeer                                 |        ✓           |     ✓    |
| Tijdzones                              |        ✓           |     ✓    |
| Beelden en gecombineerde beelden    |            |     ✓    |
| Route bereik                    |                   |     ✓    |
| Locatie van de IP-2 (preview)                |        ✓           |     ✓    |
| Veelhoeken van zoekopdracht          |                   |     ✓    |
| Batch geocodering (preview)              |                   |     ✓    |
| Batch-routering (preview)                |                   |     ✓    |
| Matrix-routering (preview)               |                   |     ✓    |


Deze aanvullende gegevenspunten zijn moet overwegen:
* Wat voor soort enterprise hebt u?
* Hoe essentieel is de toepassing wordt gemaakt?

Zie de **klanten prijscategorie gericht** tabel een beter idee van de Prijscategorieën S0 en S1 te krijgen. Zie voor meer informatie, [prijzen van Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Prijscategorie gericht klanten

| Prijscategorie  |     Potentiële klanten                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>De prijscategorie S0 is voor klanten die kleine of middelgrote bedrijven. Het is het recht voor prijscategorie voor u, als u niet verwacht grote aantallen gelijktijdige gebruikers dat. Het is ook geschikt als de georuimtelijke core API's die worden weergegeven in de voorgaande tabel voldoet aan de servicevereisten van uw. Deze laag is algemeen beschikbaar. Dit werkt voor toepassingen in alle fasen van de productie: van proof of concept-ontwikkeling en vroege toepassing productie en de implementatie testen.<p>|
| S1            |    <p>De prijscategorie S1 is voor klanten die moeten worden ondersteuning voor grootschalige enterprise, essentiële toepassingen of grote aantallen gelijktijdige gebruikers. Het is ook voor klanten die behoefte hebben aan geavanceerde georuimtelijke services.</p>|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het weergeven en wijzigen van Prijscategorieën:

> [!div class="nextstepaction"] 
> [Een prijscategorie beheren](how-to-manage-pricing-tier.md)
