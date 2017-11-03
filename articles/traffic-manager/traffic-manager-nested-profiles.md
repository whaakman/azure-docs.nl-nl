---
title: Traffic Manager-profielen geneste | Microsoft Docs
description: Dit artikel wordt uitgelegd dat de functie 'Geneste profielen' van Azure Traffic Manager
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f1b112c4-a3b1-496e-90eb-41e235a49609
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 1ac4ec2775ca9f690f5adf4f939908f8cee3f715
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="nested-traffic-manager-profiles"></a>Geneste Traffic Manager-profielen

Traffic Manager bevat een reeks verkeersroutering methoden waarmee u kunt beheren hoe Traffic Manager kiest welk eindpunt verkeer ontvangt van elke gebruiker. Zie voor meer informatie [Traffic Manager-verkeersroutering methoden](traffic-manager-routing-methods.md).

Elke Traffic Manager-profiel geeft één verkeersroutering methode. Er zijn echter scenario's waarvoor meer geavanceerde voor verkeersroutering dan de routering geleverd door een enkele Traffic Manager-profiel. U kunt de Traffic Manager-profielen voor het combineren van de voordelen van meer dan één methode voor verkeersroutering nesten. Geneste profielen kunnen u het standaardgedrag van Traffic Manager ondersteuning grotere en complexere implementaties van toepassingen vervangen.

De volgende voorbeelden ziet het gebruik van geneste Traffic Manager-profielen in verschillende scenario's.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Voorbeeld 1: Combineren 'Prestaties' en 'Gewogen' voor verkeersroutering

Stel dat u een toepassing in de volgende Azure-regio's hebt geïmplementeerd: VS-West, West-Europa en Oost-Azië. U gebruikt van Traffic Manager 'Prestaties' verkeersroutering methode voor het distribueren van het verkeer naar de regio die het dichtst bij de gebruiker.

![Één Traffic Manager-profiel][4]

Stel nu dat u wilt een update voor uw service te testen voordat u veel meer rolling. U wilt de 'gewogen' verkeersroutering methode gebruiken om te leiden van een klein percentage van het verkeer naar uw testimplementatie. U hebt de testimplementatie naast de bestaande productie-implementatie hebt ingesteld in West-Europa.

Beide 'gewogen' kan niet worden gecombineerd en '-verkeer routeren in één profiel. Ter ondersteuning van dit scenario, moet u een Traffic Manager-profiel met behulp van de twee eindpunten voor West-Europa en de methode 'Gewogen' traffic routing maken. Vervolgens voegt u dit profiel 'onderliggend item' als een eindpunt toe aan de 'parent'-profiel. Het profiel van de bovenliggende nog steeds de prestaties voor verkeersroutering methode wordt gebruikt en bevat de algemene implementaties als eindpunten.

Het volgende diagram illustreert het volgende voorbeeld:

![Geneste Traffic Manager-profielen][2]

In deze configuratie is wordt omgeleid via het profiel van de bovenliggende verkeer verkeer over regio's normaal. In West-Europa wordt het profiel voor geneste verkeer naar de productie- en eindpunten volgens het gewicht is toegewezen.

Wanneer het bovenliggende profiel de 'Prestaties' verkeersroutering methode gebruikt wordt, moet elk eindpunt een locatie worden toegewezen. De locatie is toegewezen bij het configureren van het eindpunt. Kies de Azure-regio die het dichtst bij uw implementatie. Azure-regio's zijn de waarden van de locatie wordt ondersteund door de latentie Internet tabel. Zie voor meer informatie [Traffic Manager 'Prestaties' verkeersroutering methode](traffic-manager-routing-methods.md#performance).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Voorbeeld 2: Eindpuntcontrole in geneste profielen

Traffic Manager wordt actief controleert de status van elke service-eindpunt. Als een eindpunt beschadigd is, stuurt Traffic Manager gebruikers alternatieve eindpunten te behouden van de beschikbaarheid van uw service. Deze controle en failover eindpuntgedrag geldt voor alle verkeersroutering methoden. Zie voor meer informatie [Traffic Manager-eindpunt bewaking](traffic-manager-monitoring.md). Eindpuntcontrole werkt anders voor geneste profielen. Met geneste profielen uitvoeren het profiel van de bovenliggende niet statuscontroles op de onderliggende rechtstreeks. In plaats daarvan wordt de status van de eindpunten van het onderliggende profiel gebruikt voor het berekenen van de algemene status van het onderliggende profiel. Deze informatie wordt doorgegeven omhoog in de hiërarchie geneste profiel. Het profiel voor de bovenliggende gebruikt dit geaggregeerde status om te bepalen of verkeer omleiden naar het onderliggende profiel. Zie de [Veelgestelde vragen over](traffic-manager-FAQs.md#traffic-manager-nested-profiles) voor volledige informatie over de statuscontrole van geneste profielen.

Stel u bent teruggekeerd naar het vorige voorbeeld, mislukt de productie-implementatie in West-Europa. Standaard stuurt het profiel 'onderliggend item' al het verkeer naar de testimplementatie. Als de testimplementatie ook mislukt, wordt op basis van het bovenliggende profiel bepaalt dat het onderliggende profiel niet moeten verkeer verschijnen zou omdat alle onderliggende eindpunten slecht zijn. Het profiel van de bovenliggende distribueert vervolgens verkeer naar de andere regio's.

![Geneste profiel failover (standaardinstelling)][3]

Het is mogelijk dat u tevreden met deze benadering. Of u mogelijk betrokken al het verkeer voor West-Europa gaat nu aan de testimplementatie in plaats van een beperkte subset-verkeer. Ongeacht de status van de testimplementatie u failover naar de andere regio's als de productie-implementatie in West-Europa is mislukt. U kunt de parameter 'MinChildEndpoints' zodat deze failover opgeven bij het configureren van het onderliggende profiel als een eindpunt in het profiel van de bovenliggende. De parameter bepaalt het minimum aantal beschikbare eindpunten in het onderliggende profiel. De standaardwaarde is '1'. Voor dit scenario kunt u de MinChildEndpoints-waarde ingesteld op 2. Onder deze drempelwaarde, wordt het profiel van de bovenliggende beschouwt het hele onderliggende profiel niet beschikbaar is en stuurt het verkeer naar de andere eindpunten.

De volgende afbeelding ziet u deze configuratie:

![Profiel failover met 'MinChildEndpoints' genest = 2][4]

> [!NOTE]
> De methode 'Prioriteit' traffic routing distribueert alle verkeer naar één eindpunt. Er is dus weinig doel in een instelling voor MinChildEndpoints dan '1' voor een onderliggende-profiel.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Voorbeeld 3: Prioriteit failover gebieden in 'Prestaties' voor verkeersroutering

Het standaardgedrag voor de methode 'Prestaties'-voor verkeersroutering is ontworpen om te voorkomen dat te veel bij het laden van de volgende dichtstbijzijnde eindpunt en een trapsgewijze reeks fouten veroorzaakt. Als een eindpunt is mislukt, wordt al het verkeer dat zou zijn omgeleid naar dat eindpunt gelijkmatig gedistribueerd naar de andere eindpunten in alle regio's.

!['Prestaties'-verkeer met standaard failover-routering][5]

Stel u liever de West-Europa verkeerfailover VS-West, en alleen verkeer omleiden naar andere regio's als beide eindpunten niet beschikbaar zijn. U kunt deze oplossing met behulp van een onderliggende-profiel met de methode 'Prioriteit' traffic routing maken.

!['Prestaties'-verkeer met speciale failover-routering][6]

Aangezien het West-Europa-eindpunt heeft een hogere prioriteit dan het eindpunt van de VS-West, worden alle verkeer wordt verzonden naar het eindpunt West-Europa wanneer beide eindpunten online. Als West-Europa mislukt, wordt het netwerkverkeer omgeleid naar VS-West. Met het geneste profiel wordt verkeer omgeleid Oost-Azië alleen wanneer zowel West-Europa en VS-West mislukken.

U kunt dit patroon herhalen voor alle regio's. Vervang alle drie eindpunten in het profiel van de bovenliggende door drie onderliggende profielen elk voor een reeks prioriteit failover.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Voorbeeld 4: Beheren 'Prestaties'-verkeer routeren tussen meerdere eindpunten in dezelfde regio

Stel dat u de prestaties die verkeersroutering methode wordt gebruikt in een profiel dat meer dan één eindpunt in een bepaald gebied is. Standaard worden omgeleid naar deze regio verkeer gelijkmatig verdeeld over alle beschikbare eindpunten in deze regio.

!['Prestaties' verkeer distributie van routering regio-verkeer (standaardinstelling)][7]

In plaats van meerdere eindpunten in West-Europa, worden deze eindpunten zijn ingesloten in een afzonderlijke onderliggende profiel. Het onderliggende profiel wordt toegevoegd aan het bovenliggende als het enige eindpunt in West-Europa. De instellingen op het onderliggende profiel kunnen u de distributie van verkeer met West-Europa beheren door in te schakelen op basis van prioriteit of gewogen verkeersroutering binnen deze regio.

![Routering met aangepaste regio-verkeer distributie 'Prestaties'-verkeer][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Voorbeeld 5: Per eindpunt controle-instellingen

Stel dat u Traffic Manager gebruikt voor het migreren van soepel verkeer van een legacy lokale website naar een nieuwe versie Cloud-gebaseerde gehost in Azure. Voor de oude site die u wilt de startpagina URI gebruiken voor het bewaken van de sitestatus. Maar voor de Cloud-gebaseerde nieuwe versie, implementeert u een aangepaste pagina (pad ' / monitor.aspx') die worden extra controles uitgevoerd.

![Traffic Manager-eindpunt monitoring (standaardinstelling)][9]

Controle-instellingen in een Traffic Manager-profiel van toepassing op alle eindpunten binnen één profiel. Voor geneste profielen gebruikt u een profiel voor een andere onderliggende per site voor het definiëren van verschillende controle-instellingen.

![Bewaking met de eindpuntinstellingen per Traffic Manager-eindpunt][10]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Traffic Manager-profielen](traffic-manager-overview.md)

Meer informatie over hoe [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png
