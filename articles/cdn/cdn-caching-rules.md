---
title: Azure CDN cachegedrag met caching regels beheren | Microsoft Docs
description: CDN caching regels kunt u instellen of wijzigen van standaardgedrag cache verloopt zowel globaal en met voorwaarden, zoals een URL-pad en de bestandsnaam extensies.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: v-deasim
ms.openlocfilehash: 4095ed763de378a673908d033d87b2aa6d72f13c
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260003"
---
# <a name="control-azure-cdn-caching-behavior-with-caching-rules"></a>Besturingselement Azure CDN cachegedrag met caching van regels

> [!NOTE] 
> In het cachegeheugen regels zijn alleen beschikbaar voor **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen. Voor **Azure CDN Premium van Verizon** profielen, moet u de [Azure CDN regelengine](cdn-rules-engine.md) in de **beheren** portal voor vergelijkbare functionaliteit.
 
Azure Content Delivery Network (CDN) biedt twee manieren om te bepalen hoe uw bestanden in de cache opgeslagen: 

- Regels opslaan in cache: dit artikel wordt beschreven hoe u content delivery network (CDN) opslaan in cache regels instellen of wijzigen van standaardgedrag cache verloopt zowel globaal en met aangepaste voorwaarden, zoals een URL-pad en de bestandsnaam uitbreiding kunt gebruiken. Azure CDN biedt twee typen regels voor opslaan in cache:

   - Globale regels voor opslaan in cache: u kunt voor elk eindpunt in uw profiel één globale regel voor opslaan in cache instellen, die geldt voor alle aanvragen op het eindpunt. De globale regel voor opslaan in cache overschrijft alle HTTP-headers met cache-instructies, indien aanwezig.

   - Aangepaste regels voor opslaan in cache: u kunt een of meer aangepaste regels voor opslaan in cache instellen voor elk eindpunt in uw profiel. Aangepaste regels voor opslaan in cache komen overeen met specifieke paden en bestandsextensies, worden in volgorde verwerkt en overschrijven de globale regel voor opslaan in cache, indien ingesteld. 

- Query opslaan in cache: kunt u aanpassen hoe Azure CDN omgaat met aanvragen met querytekenreeksen in cache opslaan. Zie voor informatie [besturingselement Azure CDN cachegedrag met queryreeksen](cdn-query-string.md). Als het bestand geen caching geschikte is, heeft de queryreeks cache-instelling geen effect, op basis van regels en het standaardgedrag CDN caching.

Zie voor meer informatie over standaard cachegedrag en caching richtlijn headers [werking van cacheopslag](cdn-how-caching-works.md). 


## <a name="accessing-azure-cdn-caching-rules"></a>Toegang tot de cache in Azure CDN-regels

1. De Azure portal openen, selecteer een CDN-profiel en selecteer vervolgens een eindpunt.

2. Selecteer in het linkerdeelvenster onder Instellingen de optie **Regels voor opslaan in cache**.

   ![Knop Regels voor CDN-caching](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   De pagina **Regels voor opslaan in cache** wordt weergegeven.

   ![Pagina Regels voor CDN-caching](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="caching-behavior-settings"></a>Van caching-gedraginstellingen
Voor globale en aangepaste regels in het cachegeheugen, kunt u de volgende **cachegedrag** instellingen:

- **Cache overslaan**: niet in de cache en oorsprong opgegeven instructie cache headers negeren.

- **Overschrijven**: negeren oorsprong opgegeven instructie cache headers; gebruik in plaats daarvan de opgegeven Cacheduur.

- **Indien deze ontbreken ingesteld**: EER oorsprong opgegeven instructie cache kopteksten, indien ze bestaan; anders gebruikt u de opgegeven Cacheduur.

![Globale regels voor opslaan in cache](./media/cdn-caching-rules/cdn-global-caching-rules.png)

![Aangepaste regels voor opslaan in cache](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

## <a name="cache-expiration-duration"></a>Vervaltijd van de cache
Voor globale en aangepaste regels in het cachegeheugen, kunt u de vervaldatum Cacheduur opgeven in dagen, uren, minuten en seconden:

- Voor de **overschrijven** en **ingesteld indien deze ontbreken** **cachegedrag** instellingen, het bereik van geldige cache duur tussen 0 en 366 dagen. Voor een waarde van 0 seconden, de CDN slaat de inhoud, maar moet elke aanvraag met de oorspronkelijke server valideren.

- Voor de **cache overslaan** instellen, de Cacheduur van de wordt automatisch ingesteld op 0 seconden en kan niet worden gewijzigd.

## <a name="custom-caching-rules-match-conditions"></a>Aangepaste caching regels identieke voorwaarden

Voor aangepaste cacheregels zijn twee identieke voorwaarden zijn beschikbaar:
 
- **Pad**: deze voorwaarde overeenkomt met het pad van de URL, met uitzondering van de domeinnaam en biedt ondersteuning voor het jokerteken (\*). Bijvoorbeeld: _/myfile.html_, _/mijn/map / *_, en _/my/images/*.jpg_. De maximale lengte is 260 tekens.

- **Extensie**: deze situatie komt overeen met de bestandsextensie van het aangevraagde bestand. U kunt een lijst met bestandsextensies overeenkomen met door komma's gescheiden opgeven. Bijvoorbeeld: _.jpg_, _.mp3_, of _PNG_. Het maximum aantal uitbreidingen is 50 en het maximum aantal tekens per uitbreiding is 16. 

## <a name="global-and-custom-rule-processing-order"></a>De verwerkingsvolgorde globale en aangepaste regel
Globale en aangepaste cachebewerkingen regels worden verwerkt in de volgende volgorde:

- Globale cachebewerkingen regels hebben voorrang op de standaard-CDN-cachegedrag (http-header van de instructie cache-instellingen). 

- Aangepaste cachebewerkingen regels hebben voorrang op globale cache in regels, waar ze van toepassing. Aangepaste regels voor caching worden in volgorde van boven naar beneden verwerkt. Dat wil zeggen, als een aanvraag aan beide voorwaarden voldoet, hebben regels onder aan de lijst voorrang boven regels boven aan de lijst. Daarom, plaatst u specifiekere regels lager in de lijst.

**Voorbeeld**:
- Globale cacheregel: 
   - Cachegedrag: **overschrijven**
   - Duur voor het verlopen in de cache: 1 dag

- Aangepaste regel #1 opslaan in cache:
   - Overeenkomen met de voorwaarde: **pad**
   - Overeenkomen met waarde: _thuis-/ *_
   - Cachegedrag: **overschrijven**
   - Duur voor het verlopen in de cache: 2 dagen

- Aangepaste regel #2 opslaan in cache:
   - Overeenkomen met de voorwaarde: **extensie**
   - Overeenkomen met waarde: _.html_
   - Cachegedrag: **indien deze ontbreken instellen**
   - Duur voor het verlopen in de cache: 3 dagen

Wanneer deze regels zijn ingesteld, een aanvraag voor  _&lt;hostnaam van het eindpunt&gt;_.azureedge.net/home/index.html triggers aangepaste caching #2, die is ingesteld op regel: **ingesteld indien deze ontbreken** en 3 aantal dagen. Daarom als de *index.html* bestand heeft `Cache-Control` of `Expires` HTTP-headers zijn gebruikt; anders, als deze headers zijn niet ingesteld, het bestand is opgeslagen in de cache voor 3 dagen.

> [!NOTE] 
> Bestanden die in de cache voordat de wijziging van een regel opgeslagen behouden hun duur van de oorsprong cache-instelling. Als u de duur van de cache herstellen, moet u [opschonen van het bestand](cdn-purge-endpoint.md). 
>
> Azure CDN configuratiewijzigingen kunnen even duren worden doorgegeven via het netwerk: 
> - Profielen van **Azure CDN Standard van Akamai** worden doorgaans binnen één minuut doorgegeven. 
> - Voor **Azure CDN Standard van Verizon** profielen, doorgeven voltooid gewoonlijk in 10 minuten.  
>

## <a name="see-also"></a>Zie ook

- [Hoe caching werkt](cdn-how-caching-works.md)
- [Zelfstudie: Azure CDN-regels voor opslaan in cache instellen](cdn-caching-rules-tutorial.md)
