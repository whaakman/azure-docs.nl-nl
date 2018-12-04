---
title: Kies de prijscategorie voor Azure-kaarten rechts | Microsoft Docs
description: Meer informatie over prijzen van lagen die worden aangeboden door Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 12/03/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 20c59f95dcdb47ef3e457f177d7ad1a300981907
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52855837"
---
# <a name="choosing-the-right-pricing-tier-in-azure-maps"></a>De prijscategorie van Azure Maps rechts kiezen

Azure kaarten biedt twee Prijscategorieën. Het doel van dit artikel is bij het recht voor prijscategorie voor uw behoeften te kiezen. Om te helpen de juiste prijscategorie kiezen, Stel uzelf twee vragen:

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Welke mogelijkheden georuimtelijke plan ik gebruiken?
Als u dat denkt uw servicevereisten wordt voldaan door de georuimtelijke core API's, wordt de S0-laag is geschikt voor u. Als u wilt meer geavanceerde mogelijkheden voor uw toepassing, zoals areal + hybride beeldmateriaal, route bereik aan batch-geocodering enz., kunt u kiest voor levering van de prijscategorie S1. De onderstaande tabel met **prijzen laag mogelijkheden** zullen u voorzien van een beter idee van de behoeften van uw toepassing en wordt ook hulp die u kiest een prijscategorie meest geschikt is voor uw toepassing.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Het aantal gelijktijdige gebruikers wilt ik ondersteuning? 
S0 en S1 Prijscategorieën kunnen verschillende hoeveelheden doorvoer van gegevens worden verwerkt. Voordat u een Azure-kaarten prijscategorie, rekening houden met vraag uzelf op vragen zoals hoeveel gelijktijdige gebruikers wilt u ondersteunen? De prijscategorie aankan tot S0 **50 query's per seconde** en de prijscategorie aankan S1 **meer dan 50 query's per seconde**.

### <a name="pricing-tier-capabilities"></a>Prijzen van de mogelijkheden van laag

| Mogelijkheid                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Search                                  |        ✓           |     ✓    |
| Routering                                 |        ✓           |     ✓    |
| Renderen                                  |        ✓           |     ✓    |
| Verkeer                                 |        ✓           |     ✓    |
| Tijdzones                              |        ✓           |     ✓    |
| Beelden en gecombineerde beelden (preview)      |                    |     ✓    |
| Route bereik (Preview)                   |                    |     ✓    |
| IP-2-locatie (preview)                 |                    |     ✓    |
| Veelhoeken van zoeken (Preview)          |                    |     ✓    |
| Batch Geocodering (preview)               |                    |     ✓    |
| Batch-routering (preview)                 |                    |     ✓    |
| Matrix-routering (preview)                |                    |     ✓    |


Enkele aanvullende gegevenspunten waard zijn, wat voor soort enterprise hebt u of hoe essentieel de toepassing is wordt gemaakt?

Zie de tabel met **prijscategorie gericht klanten** een beter idee van de Prijscategorieën S0 en S1 te krijgen. Zie voor meer informatie over prijzen voor Azure-kaarten, [prijzen van Azure Maps](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Prijscategorie gericht klanten

| Prijscategorie  |        Potentiële klanten                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>De prijscategorie S0 is voor klanten die kleine of middelgrote bedrijven. Het is een recht prijscategorie voor u, als u grote aantallen gelijktijdige gebruikers niet verwacht en uw servicevereisten wordt voldaan door de georuimtelijke core API's, zoals aangegeven door de onderstaande tabel. Deze laag is algemeen beschikbaar en is van toepassing voor toepassingen in alle fasen van de productie van proof-of-concept-ontwikkeling en vroege toepassing productie en de implementatie testen.<p>|
| S1            |    <p>De prijscategorie S1 is bedoeld voor klanten die moeten worden ondersteuning voor grootschalige enterprise, essentiële toepassingen, grote aantallen gelijktijdige gebruikers, of geavanceerde georuimtelijke services vereist.</p>|


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het weergeven en de prijscategorie wijzigen:

> [!div class="nextstepaction"]
> [Prijscategorie beheren](how-to-manage-pricing-tier.md)