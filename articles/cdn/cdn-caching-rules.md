---
title: Azure CDN-cachinggedrag met caching-regels beheren | Microsoft Docs
description: Regels voor caching CDN kunt u instellen of wijzigen van standaardgedrag cache verloopt zowel wereldwijd en met voorwaarden, zoals een URL-pad en -extensies.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: magattus
ms.openlocfilehash: 3a94b8252feb7c5c345d678579c477fce02d6e03
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259734"
---
# <a name="control-azure-cdn-caching-behavior-with-caching-rules"></a>Azure CDN caching-gedrag met caching-regels beheren

> [!NOTE] 
> Regels voor opslaan in cache zijn alleen beschikbaar voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen. Voor **Azure CDN Premium van Verizon** profielen, moet u de [regels-engine van Azure CDN](cdn-rules-engine.md) in de **beheren** portal voor vergelijkbare functionaliteit.
 
Azure Content Delivery Network (CDN) biedt twee manieren om te bepalen hoe uw bestanden in cache zijn opgeslagen: 

- Regels voor opslaan in cache: Dit artikel wordt beschreven hoe u netwerk voor contentlevering (CDN) regels voor caching instellen of wijzigen van standaardgedrag cache verloopt zowel wereldwijd en met aangepaste voorwaarden, zoals een URL-pad en de bestandsnaam-extensie kunt gebruiken. Azure CDN biedt twee typen regels voor opslaan in cache:

   - Algemene regels voor opslaan in cache: U kunt één globale cacheregel voor elk eindpunt instellen in uw profiel, die invloed hebben op alle aanvragen naar het eindpunt. De globale regel voor opslaan in cache overschrijft alle HTTP-headers met cache-instructies, indien aanwezig.

   - Aangepaste cacheregels: U kunt een of meer aangepaste cacheregels voor elk eindpunt instellen in uw profiel. Aangepaste regels voor opslaan in cache komen overeen met specifieke paden en bestandsextensies, worden in volgorde verwerkt en overschrijven de globale regel voor opslaan in cache, indien ingesteld. 

- Query queryreeksen opslaan in cache: U kunt aanpassen hoe het Azure CDN omgaat met aanvragen met queryreeksen opslaan in cache. Zie voor meer informatie, [Control Azure CDN caching-gedrag met queryreeksen](cdn-query-string.md). Als het bestand niet gecachet kan worden, heeft de instelling van querytekenreeksen geen effect, op basis van regels en het standaardgedrag CDN caching.

Zie voor meer informatie over standaard cachegedrag en opslaan in cache richtlijn headers [hoe caching werkt](cdn-how-caching-works.md). 


## <a name="accessing-azure-cdn-caching-rules"></a>Toegang tot Azure CDN caching-regels

1. Open de Azure-portal, selecteert u een CDN-profiel en een eindpunt selecteren.

2. Selecteer in het linkerdeelvenster onder Instellingen de optie **Regels voor opslaan in cache**.

   ![Knop Regels voor CDN-caching](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   De pagina **Regels voor opslaan in cache** wordt weergegeven.

   ![Pagina Regels voor CDN-caching](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="caching-behavior-settings"></a>Van de cache-gedraginstellingen
Voor algemene en aangepaste cacheregels, kunt u de volgende **cachegedrag** instellingen:

- **Cache overslaan**: Niet in de cache en geleverde oorsprong cache-instructies headers negeren.

- **Overschrijven**: Oorsprong-voorwaarde Cacheduur; negeren Gebruik in plaats daarvan de Cacheduur van de opgegeven. Dit overschrijft geen cache-control: niet-cache.

- **Instellen indien ontbrekend**: Headers van de cache-instructies oorsprong worden geleverd, worden gehandhaafd, indien ze bestaan; Gebruik anders de Cacheduur van de opgegeven.

![Globale regels voor opslaan in cache](./media/cdn-caching-rules/cdn-global-caching-rules.png)

![Aangepaste regels voor opslaan in cache](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

## <a name="cache-expiration-duration"></a>Vervaltijd van de cache
U kunt de vervaltijd van de cache voor algemene en aangepaste cacheregels opgeven in dagen, uren, minuten en seconden:

- Voor de **overschrijven** en **instellen indien ontbrekend** **cachegedrag** -instellingen, geldige cache duur bereik tussen 0 seconden en 366 dagen. Voor een waarde van 0 seconden, het CDN de inhoud in de cache opslaat, maar moet elke aanvraag met de oorspronkelijke server valideren.

- Voor de **cache overslaan** instellen, de Cacheduur van de wordt automatisch ingesteld op 0 seconden en kan niet worden gewijzigd.

## <a name="custom-caching-rules-match-conditions"></a>Criteria voor overeenkomst aangepaste caching-regels

Twee identieke voorwaarden zijn beschikbaar voor cacheregels voor aangepaste:
 
- **Pad**: Dit probleem komt overeen met het pad van de URL, met uitzondering van de naam van het domein, en biedt ondersteuning voor het jokerteken (\*). Bijvoorbeeld, _/myfile.html_, _/mijn/map / *_, en _/my/images/*.jpg_. De maximumlengte is 260 tekens.

- **Extensie**: Dit probleem komt overeen met de bestandsextensie van het aangevraagde bestand. U kunt een lijst met bestandsextensies die overeenkomen met door komma's gescheiden opgeven. Bijvoorbeeld, _.jpg_, _.mp3_, of _.png_. Het maximum aantal extensies is 50 en het maximum aantal tekens per extensie is 16. 

## <a name="global-and-custom-rule-processing-order"></a>De verwerkingsvolgorde algemene en aangepaste regel
Algemene en aangepaste cacheregels worden verwerkt in de volgende volgorde:

- Algemene regels voor opslaan in cache hebben voorrang op de standaard-CDN-cachinggedrag (http-header van de cache-instructies instellingen). 

- Aangepaste cacheregels hebben voorrang op de algemene regels voor opslaan in cache, waar ze van toepassing zijn. Aangepaste regels voor opslaan in cache worden verwerkt in volgorde van boven naar beneden. Dat wil zeggen, als een aanvraag aan beide voorwaarden voldoet, hebben regels aan de onderkant van de lijst met voorrang op regels aan de bovenkant van de lijst. Daarom, plaatst u meer specifieke regels lager in de lijst.

**Voorbeeld**:
- Algemene regel voor opslaan in cache: 
   - Cachegedrag: **Override**
   - Vervaltijd van de cache: 1 dag

- Aangepaste caching #1 regel:
   - Voorwaarde voor overeenkomst: **Pad**
   - Komt overeen met waarde:   _/home / *_
   - Cachegedrag: **Override**
   - Vervaltijd van de cache: 2 dagen

- Aangepaste caching regel #2:
   - Voorwaarde voor overeenkomst: **Extensie**
   - Komt overeen met waarde: _.html_
   - Cachegedrag: **Instellen indien ontbrekend**
   - Vervaltijd van de cache: 3 dagen

Wanneer deze regels zijn ingesteld, een aanvraag voor  _&lt;hostnaam van eindpunt&gt;_.azureedge.net/home/index.html triggers aangepaste caching #2, die is ingesteld op regel: **Instellen indien ontbrekend** en 3 dagen. Dus als de *index.html* bestand heeft `Cache-Control` of `Expires` HTTP-headers, ze worden gebruikt; anders als deze headers zijn niet ingesteld, het bestand is opgeslagen in de cache voor 3 dagen.

> [!NOTE] 
> Bestanden die in de cache voordat de wijziging van een regel opgeslagen behouden hun duur van de oorsprong cache-instelling. Als u de duur van hun cache herstellen, moet u [opschonen van het bestand](cdn-purge-endpoint.md). 
>
> Azure CDN-configuratiewijzigingen kunnen even worden doorgegeven via het netwerk: 
> - Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
> - Voor **Azure CDN Standard van Verizon** profielen, doorgifte doorgaans binnen 10 minuten.  
>

## <a name="see-also"></a>Zie ook

- [Hoe caching werkt](cdn-how-caching-works.md)
- [Zelfstudie: Azure CDN-regels voor opslaan in cache instellen](cdn-caching-rules-tutorial.md)
