---
title: Geneste Traffic Manager-profielen | Microsoft Docs
description: Dit artikel wordt uitgelegd dat de 'Geneste profielen'-functie van Azure Traffic Manager
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/22/2018
ms.author: kumud
ms.openlocfilehash: 876305c7195a186671c30c4bdd9bb0c6b5331e9a
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648595"
---
# <a name="nested-traffic-manager-profiles"></a>Geneste Traffic Manager-profielen

Traffic Manager omvat een scala van routering van verkeer methoden waarmee u kunt bepalen hoe Traffic Manager kiezen welk eindpunt ontvangt verkeer vanuit elke eindgebruiker. Zie voor meer informatie, [methoden voor Traffic Manager traffic routing](traffic-manager-routing-methods.md).

Elke Traffic Manager-profiel Hiermee geeft u een methode voor eenmalige routering van verkeer. Er zijn echter scenario's waarin meer geavanceerde routering van verkeer dan de routering geleverd door een enkele Traffic Manager-profiel. U kunt Traffic Manager-profielen als u wilt combineren van de voordelen van meer dan één methode voor verkeersroutering nesten. Geneste profielen kunnen u het standaardgedrag voor Traffic Manager voor ondersteuning voor grotere en complexere implementaties van toepassingen vervangen.

De volgende voorbeelden ziet u hoe u het gebruik van geneste Traffic Manager-profielen in verschillende scenario's.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Voorbeeld 1: Combineren 'Prestaties' en 'Gewogen' Routering van verkeer

Stel dat u een toepassing in de volgende Azure-regio's geïmplementeerd: VS-West, West-Europa en Oost-Azië. U gebruikt de Traffic Manager 'Prestaties' Routering van verkeer methode voor het distribueren van verkeer naar de regio het dichtst bij de gebruiker.

![Één Traffic Manager-profiel][4]

Stel nu dat u wilt een update voor uw service te testen voordat u veel meer rolling. U wilt de 'gewogen' verkeersrouteringsmethode gebruiken om een klein percentage van het verkeer naar uw test-implementatie te regelen. Instellen van de test-implementatie samen met de bestaande productie-implementatie in West-Europa.

Beide 'gewogen' kan niet worden gecombineerd en '-verkeer routeren in één profiel. Ter ondersteuning van dit scenario, maakt u een Traffic Manager-profiel met behulp van de eindpunten van West-Europa en de methode 'Gewogen' Routering van verkeer. Vervolgens toevoegen u dit profiel 'onderliggende' als een eindpunt in de 'parent'-profiel. Het profiel van de bovenliggende nog steeds maakt gebruik van de verkeersrouteringsmethode van prestaties en bevat de algemene implementaties als eindpunten.

Het volgende diagram illustreert het volgende voorbeeld:

![Geneste Traffic Manager-profielen][2]

In deze configuratie wordt verdeelt verkeer dat bestemd is via het profiel van de bovenliggende het verkeer tussen regio's normaal. In West-Europa verdeelt het geneste profiel het verkeer naar de eindpunten voor productie en testen op basis van de gewichten toegewezen.

Wanneer het profiel voor bovenliggende de methode 'Prestaties' Routering van verkeer gebruikt, moet elk eindpunt een locatie worden toegewezen. De locatie wordt toegewezen wanneer u het eindpunt configureren. Kies de Azure-regio die het dichtst bij uw implementatie. De Azure-regio's zijn de waarden van de locatie wordt ondersteund door de tabel van de latentie van Internet. Zie voor meer informatie, [Traffic Manager 'Prestaties' verkeersrouteringsmethode](traffic-manager-routing-methods.md#performance).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Voorbeeld 2: Eindpuntbewaking in geneste profielen

Traffic Manager wordt actief bewaakt de status van alle service-eindpunten. Als een eindpunt niet in orde is, stuurt Traffic Manager gebruikers naar alternatieve eindpunten te behouden van de beschikbaarheid van uw service. Dit eindpunt eindpuntbewaking en failover-gedrag is van toepassing op alle methoden van de routering van verkeer. Zie voor meer informatie, [Traffic Manager-eindpunt bewaking](traffic-manager-monitoring.md). Eindpuntbewaking werkt anders voor geneste profielen. Met geneste profielen uitvoeren het profiel van de bovenliggende niet statuscontroles op de onderliggende rechtstreeks. In plaats daarvan wordt de status van de eindpunten van het onderliggende profiel gebruikt voor het berekenen van de algemene status van het onderliggende profiel. Deze gegevens over de servicestatus wordt doorgegeven om de genest-profielhiërarchie. Het profiel voor bovenliggende gebruikt dit wordt de geaggregeerde status om te bepalen of verkeer naar het onderliggende profiel. Zie de [Veelgestelde vragen over](traffic-manager-FAQs.md#traffic-manager-nested-profiles) voor meer informatie over het bewaken van de gezondheid van geneste profielen.

Terugkeren naar het vorige voorbeeld, Stel dat de productie-implementatie in West-Europa is mislukt. Standaard stuurt het profiel 'onderliggende' al het verkeer naar de test-implementatie. Als de test-implementatie ook mislukt, bepaalt het bovenliggende-profiel dat het onderliggende profiel niet verkeer ontvangt omdat alle onderliggende eindpunten niet in orde zijn. Vervolgens verdeelt het profiel van de bovenliggende het verkeer naar de andere regio's.

![Geneste profiel failover (standaardinstelling)][3]

Het is mogelijk dat u tevreden is met deze overeenkomst. Of u mogelijk betrokken al het verkeer voor West-Europa gaat nu aan de testimplementatie in plaats van een beperkte subset verkeer. Ongeacht de status van de test-implementatie, die u wilt een failover uitvoeren naar de andere regio's als de productie-implementatie in West-Europa is mislukt. Om in te schakelen deze failover, kunt u de parameter 'MinChildEndpoints' opgeven bij het configureren van het onderliggende profiel als een eindpunt in het profiel van de bovenliggende. De parameter bepaalt het minimum aantal beschikbare eindpunten in het onderliggende profiel. De standaardwaarde is '1'. Voor dit scenario kunt u de waarde MinChildEndpoints instelt op 2. Onder deze drempelwaarde, wordt het profiel van de bovenliggende rekening gehouden met de gehele onderliggende profiel niet beschikbaar is en verkeer doorgestuurd naar de andere eindpunten.

De volgende afbeelding ziet u deze configuratie:

![Geneste profiel failover met 'MinChildEndpoints' = 2][4]

> [!NOTE]
> De methode 'Prioriteit' Routering van verkeer verdeelt al het verkeer naar één eindpunt. Er is dus weinig zin in een instelling voor MinChildEndpoints dan '1' voor een onderliggende-profiel.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Voorbeeld 3: Prioriteit failover-regio's in de routering van verkeer in 'Prestaties'

Het standaardgedrag voor de methode 'Prestaties' Routering van verkeer is wanneer er eindpunten in verschillende geografische locaties die de eindgebruikers worden doorgestuurd naar het eindpunt van de "dichtstbijzijnde" wat betreft de laagste netwerklatentie.

Stel echter dat u liever de West-Europa verkeerfailover VS-West, en alleen instellen dat verkeer naar andere regio's als beide eindpunten niet beschikbaar zijn. U kunt deze oplossing met behulp van een onderliggende-profiel met de methode 'Prioriteit' Routering van verkeer.

!['Prestaties'-verkeer met speciale failover-routering][6]

Omdat het eindpunt West-Europa heeft een hogere prioriteit dan het eindpunt van de VS-West, wordt al het verkeer wordt verzonden naar het eindpunt West-Europa wanneer beide eindpunten online zijn. Als West-Europa is mislukt, wordt het verkeer wordt omgeleid naar VS-West. Verkeer wordt omgeleid naar Oost-Azië de geneste profiel, alleen wanneer zowel West-Europa en VS-West mislukken.

U kunt dit patroon herhalen voor alle regio's. Vervang alle drie eindpunten in het profiel van de bovenliggende met drie onderliggende-profielen, elk voor een reeks met prioriteit failover.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Voorbeeld 4: Beheer van verkeer tussen meerdere eindpunten in dezelfde regio 'Prestaties'

Stel dat u de prestaties die verkeersrouteringsmethode wordt gebruikt in een profiel dat meer dan één eindpunt in een bepaalde regio is. Verkeer dat bestemd is voor deze regio wordt standaard gelijkmatig verdeeld over alle beschikbare eindpunten in deze regio.

!['Prestaties'-verkeer in de regio verkeersdistributie (standaardinstelling)][7]

In plaats van meerdere eindpunten toevoegen in West-Europa, worden deze eindpunten zijn ingesloten in een afzonderlijke onderliggende profiel. Het onderliggende profiel wordt toegevoegd aan de bovenliggende als de enige eindpunt in West-Europa. De instellingen op het onderliggende profiel kunnen u de distributie van verkeer met West-Europa beheren door in te schakelen op basis van prioriteit of gewogen verkeersroutering binnen deze regio.

!['Prestaties' verkeer met aangepaste regionale verkeersdistributie][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Voorbeeld 5: Per eindpunt controle-instellingen

Stel dat u Traffic Manager gebruikt voor het probleemloos migreren verkeer van een verouderde on-premises website naar een nieuwe Cloud-gebaseerde versie die wordt gehost in Azure. Voor de oude site, die u wilt de startpagina URI gebruiken voor het bewaken van de gezondheid van de site. Maar voor de nieuwe Cloud-gebaseerde versie, implementeert u een aangepaste pagina (pad ' / monitor.aspx') die aanvullende controles bevat.

![Traffic Manager-eindpunt bewaking (standaardinstelling)][9]

De controle-instellingen in een Traffic Manager-profiel van toepassing op alle eindpunten binnen één profiel. Bij geneste profielen gebruikt u een profiel van de verschillende onderliggende per site voor het definiëren van verschillende instellingen voor controle.

![Bewaking met instellingen voor de per-eindpunten Traffic Manager-eindpunt][10]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Traffic Manager-profielen](traffic-manager-overview.md)

Meer informatie over het [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

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
