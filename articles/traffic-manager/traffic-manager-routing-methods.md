---
title: Met Azure Traffic Manager - verkeersrouteringsmethoden | Microsoft Docs
description: Hiermee geeft u inzicht in de verschillende verkeersrouteringsmethoden van Traffic Manager gebruikt de artikelen
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: timlt
editor: ''
ms.assetid: db1efbf6-6762-4c7a-ac99-675d4eeb54d0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: kumud
ms.openlocfilehash: 03f1cc3a34fa8a472dcab9654b65cc97b8473993
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398614"
---
# <a name="traffic-manager-routing-methods"></a>Methoden voor het doorsturen van Traffic Manager

Met Azure Traffic Manager biedt ondersteuning voor vier methoden voor routering van verkeer om te bepalen hoe u netwerkverkeer omgeleid naar de verschillende service-eindpunten. Traffic Manager geldt de verkeersrouteringsmethode voor elke DNS-query die wordt ontvangen. De verkeersrouteringsmethode bepaalt welk eindpunt in het DNS-antwoord geretourneerd.

Er zijn vier methoden voor het doorsturen van verkeer beschikbaar in Traffic Manager:

* **[Prioriteit](#priority):** Selecteer **prioriteit** wanneer u wilt gebruiken van een primaire service-eindpunt voor al het verkeer en geef de back-ups in geval de primaire of de back-eindpunten zijn niet beschikbaar.
* **[Gewogen](#weighted):** Selecteer **gewogen** wanneer u wilt dat verkeer distribueert naar een set met eindpunten, hetzij gelijkmatig of op basis van gewicht, dat u definieert.
* **[Prestaties](#performance):** Selecteer **prestaties** wanneer u eindpunten in verschillende geografische locaties en eindgebruikers om de "dichtstbijzijnde" eindpunt in termen van de laagste netwerklatentie te gebruiken.
* **[Geografische](#geographic):** Selecteer **geografische** zodat gebruikers worden omgeleid naar specifieke eindpunten (Azure, externe of geneste) op basis van welke geografische locatie hun DNS-query is afkomstig uit. Dit biedt uitgebreide mogelijkheden met Traffic Manager hebben klanten om in te schakelen van scenario's waarbij de geografische regio van de gebruiker weten en routering toe op basis van die belangrijk is. Voorbeelden zijn onder meer gegevens soevereiniteit mandaten, lokalisatie van content & gebruiker ervaring naleven en verkeer vanuit verschillende regio's te meten.

Alle Traffic Manager-profielen bevatten bewaking van de gezondheid van het eindpunt en automatische eindpunt-failover. Zie voor meer informatie, [Traffic Manager-eindpunt bewaking](traffic-manager-monitoring.md). Een enkele Traffic Manager-profiel kan slechts één methode voor verkeersroutering kunt gebruiken. U kunt een andere methode voor verkeersroutering selecteren voor uw profiel op elk gewenst moment. Wijzigingen worden toegepast binnen een minuut en zonder uitvaltijd worden in rekening gebracht. Routeringsmethoden voor verkeer kunnen worden gecombineerd met behulp van geneste Traffic Manager-profielen. Nesten kunt geavanceerde en flexibele routering van verkeer configuraties die voldoen aan de behoeften van grotere, complexe toepassingen. Zie voor meer informatie, [geneste Traffic Manager-profielen](traffic-manager-nested-profiles.md).

## <a name = "priority"></a>Verkeersrouteringsmethode van prioriteit

Vaak wil een organisatie voor de betrouwbaarheid van de services door het implementeren van een of meer back-services als hun primaire service uitvalt. De methode 'Prioriteit' Routering van verkeer kan Azure-klanten eenvoudig dit patroon wilt implementeren failover.

![Met Azure Traffic Manager 'Prioriteit' verkeersrouteringsmethode][1]

Traffic Manager-profiel bevat een geprioriteerde lijst met service-eindpunten. Standaard verzendt Traffic Manager alle verkeer naar het primaire eindpunt van de (hoogste prioriteit). Als het primaire eindpunt niet beschikbaar is, stuurt Traffic Manager het verkeer naar het tweede eindpunt. Als de primaire en secundaire eindpunten niet beschikbaar zijn, wordt het verkeer wordt gerouteerd naar de derde, enzovoort. Beschikbaarheid van het eindpunt is gebaseerd op de geconfigureerde status (ingeschakeld of uitgeschakeld) en de voortdurende eindpuntbewaking.

### <a name="configuring-endpoints"></a>Eindpunten configureren

Met Azure Resource Manager kunt u de eindpuntprioriteit expliciet het gebruik van de eigenschap 'prioriteit' voor elk eindpunt configureren. Deze eigenschap is een waarde tussen 1 en 1000. Lagere waarden geven een hogere prioriteit. Eindpunten kunnen geen prioriteitswaarden worden gedeeld. Als u de eigenschap is optioneel. Wanneer u dit weglaat, wordt een prioriteit is standaard op basis van de volgorde van het eindpunt wordt gebruikt.

##<a name = "weighted"></a>Gewogen verkeersrouteringsmethode
De 'Gewogen' verkeersrouteringsmethode kunt u verkeer gelijkmatig verdelen of een vooraf gedefinieerde weging gebruiken.

![Azure Traffic Manager 'Gewogen' verkeersrouteringsmethode][2]

In de routering van verkeer gewogen methode, kunt u een gewicht toewijzen aan elk eindpunt in de profielconfiguratie van Traffic Manager. Het gewicht is een geheel getal tussen 1 en 1000. Deze parameter is optioneel. Als u dit weglaat, wordt in Traffic Managers maakt gebruik van een standaardgewicht van '1'. De hoger gewicht, hoe hoger de prioriteit.

Voor elke DNS-query wordt ontvangen, wordt geen beschikbaar eindpunt willekeurig gekozen door Traffic Manager. De kans van het kiezen van een eindpunt is gebaseerd op het gewicht dat is toegewezen aan alle beschikbare eindpunten. Met behulp van hetzelfde gewicht over alle eindpunten resultaten in een zelfs verkeersdistributie. Met behulp van hogere of lagere gewichten op de specifieke eindpunten zorgt ervoor dat deze eindpunten moet meer of minder vaak worden geretourneerd in de DNS-antwoorden.

De gewogen methode kunt enkele handige scenario's:

* Geleidelijke toepassingsupgrade: een percentage van het verkeer routeren naar een nieuw eindpunt toe te wijzen en het verkeer, geleidelijk na verloop van tijd tot 100% te verhogen.
* Toepassingen migreren naar Azure: een profiel maken met Azure en externe eindpunten. Het gewicht van de eindpunten om de voorkeur van de nieuwe eindpunten te wijzigen.
* Cloudbursting voor extra capaciteit: snelle uitbreiding van een on-premises implementatie naar de cloud door de gegevens achter een Traffic Manager-profiel. Als u extra capaciteit in de cloud nodig hebt, kunt u toevoegen of meer eindpunten inschakelen en opgeven welk deel van het verkeer wordt gerouteerd naar elk eindpunt.

De Azure Resource Manager-portal ondersteunt de configuratie van de routering van verkeer gewogen.  U kunt het gewicht met behulp van de Resource Manager-versies van Azure PowerShell, CLI en de REST API's configureren.

Het is belangrijk om te begrijpen dat DNS-antwoorden worden opgeslagen in de cache door clients en de recursieve DNS-servers die clients gebruiken om op te lossen van DNS-namen. Deze cache, kan een invloed hebben op gewogen verkeer distributies. Wanneer het aantal clients en de recursieve DNS-servers is, wordt verkeersdistributie werkt zoals verwacht. Echter, als het aantal clients of recursieve DNS-servers klein is, caching kan aanzienlijk scheeftrekken de distributie van verkeer.

Veelgebruikte toepassingsgebieden zijn:

* Ontwikkel- en testomgevingen
* Communicatie toepassing
* Toepassingen die zijn gericht op een smal-gebruikersgroep die delen een gemeenschappelijke recursieve DNS-infrastructuur (bijvoorbeeld medewerkers van bedrijf verbinding te maken via een proxy)

Deze DNS-cache-effecten gelden voor alle DNS-verkeer routeren systemen, niet alleen Azure Traffic Manager. In sommige gevallen, expliciet de DNS-cache wissen bieden mogelijk een tijdelijke oplossing. In andere gevallen is een alternatieve methode om routering van verkeer mogelijk meer geschikt.

## <a name = "performance"></a>Verkeersrouteringsmethode van prestaties

Implementeren van eindpunten in twee of meer locaties over de hele wereld, kunt de reactietijd van veel toepassingen verbeteren door het routeren van verkeer naar de locatie die zich het dichtst bij u. De methode 'Prestaties' Routering van verkeer biedt deze mogelijkheid.

![Met Azure Traffic Manager 'Prestaties' verkeersrouteringsmethode][3]

Het eindpunt van de 'dichtstbijzijnde' is niet noodzakelijkerwijs dichtstbijzijnde wordt gemeten door geografische afstand. De methode 'Prestaties' Routering van verkeer bepaalt in plaats daarvan het eindpunt van de dichtstbijzijnde netwerklatentie meten. Traffic Manager onderhoudt een tabel Internet latentie voor het volgen van de retourtijd tussen IP-adresbereiken en elke Azure-datacenter.

Traffic Manager, zoekt u de bron-IP-adres van de inkomende DNS-aanvraag in de tabel van de latentie van Internet. Traffic Manager kiest een eindpunt beschikbaar in de Azure-datacenter die heeft de laagste latentie voor dat IP-adresbereik, en vervolgens dat eindpunt in het DNS-antwoord geretourneerd.

Zoals uitgelegd in [hoe Traffic Manager werkt](traffic-manager-how-it-works.md), Traffic Manager geen DNS-query's rechtstreeks van clients ontvangt. In plaats daarvan DNS-query's zijn afkomstig uit de recursieve DNS-service dat de clients zijn geconfigureerd voor gebruik. Daarom het IP-adres gebruikt om te bepalen van het eindpunt van de 'dichtstbijzijnde' is geen IP-adres van de client, maar het is het IP-adres van de recursieve DNS-service. Dit IP-adres is in de praktijk moet een goede proxy voor de client.


Traffic Manager wordt regelmatig bijgewerkt in de tabel Internet latentie rekening gehouden met wijzigingen in de globale Internet en de nieuwe Azure-regio's. Echter, de prestaties van toepassingen is afhankelijk van realtime variaties in de belasting via Internet. Verkeer routeren bewaakt belasting van een opgegeven service-eindpunt niet. Echter, als een eindpunt beschikbaar is, Traffic Manager bevat geen deze in de reacties van DNS-query's.


Die u moet weten:

* Als uw profiel meerdere eindpunten in dezelfde Azure-regio bevat, klikt u vervolgens verdeelt Traffic Manager verkeer gelijkmatig over de beschikbare eindpunten in deze regio. Als u liever een ander verkeersdistributie binnen een regio, kunt u [geneste Traffic Manager-profielen](traffic-manager-nested-profiles.md).
* Als alle ingeschakelde eindpunten in de dichtstbijzijnde Azure-regio worden gedegradeerd, wordt verkeer in Traffic Manager verplaatst naar de eindpunten in de volgende dichtstbijzijnde Azure-regio. Als u een gewenste failover-volgorde definiëren wilt, gebruikt u [geneste Traffic Manager-profielen](traffic-manager-nested-profiles.md).
* Als u de methode voor verkeersroutering voor prestaties voor externe eindpunten of geneste eindpunten, moet u de locatie opgeven van deze eindpunten. Kies de Azure-regio die het dichtst bij uw implementatie. Deze locaties zijn de waarden die door de tabel van de latentie van Internet worden ondersteund.
* De algoritme die u kiest voor het eindpunt is deterministisch. Herhaalde DNS-query's van dezelfde client wordt omgeleid naar hetzelfde eindpunt. Normaal gesproken clients verschillende recursieve DNS-servers gebruiken als u onderweg bent. De client kan worden doorgestuurd naar een ander eindpunt. Routering kan ook worden beïnvloed door wijzigingen in de tabel van de latentie van Internet. De verkeersrouteringsmethode van prestaties is daarom geen garantie dat een client altijd naar hetzelfde eindpunt wordt gerouteerd.
* Wanneer de tabel van de latentie van Internet wordt gewijzigd, merkt u wellicht dat sommige clients worden omgeleid naar een ander eindpunt. Deze wijziging routering is meer nauwkeurige op basis van huidige latentiegegevens. Deze updates zijn essentieel voor het onderhouden van de nauwkeurigheid van het verkeer routeren, zoals het Internet voortdurend zich verder ontwikkelt.

## <a name = "geographic"></a>Geografische verkeersrouteringsmethode

Traffic Manager-profielen kunnen worden geconfigureerd voor het gebruik van de geografische routering methode zodat gebruikers worden omgeleid naar specifieke eindpunten (Azure, externe of geneste) op basis van op welke geografische locatie hun DNS-query is afkomstig uit. Dit biedt uitgebreide mogelijkheden met Traffic Manager hebben klanten om in te schakelen van scenario's waarbij de geografische regio van de gebruiker weten en routering toe op basis van die belangrijk is. Voorbeelden zijn onder meer gegevens soevereiniteit mandaten, lokalisatie van content & gebruiker ervaring naleven en verkeer vanuit verschillende regio's te meten.
Wanneer een profiel is geconfigureerd voor de geografische routering, elk eindpunt die zijn gekoppeld aan dat profiel moet beschikken over een reeks geografische regio's zijn toegewezen. Een geografische regio kan zijn op de volgende detailniveaus 
- World – andere regio's
- Regionale groepering – bijvoorbeeld Afrika, Midden-Oosten, Australië/stille enzovoort. 
- Land/regio, bijvoorbeeld, Ierland, Peru, Hong Kong SAR enzovoort. 
- Staat/provincie – bijvoorbeeld VS-Californië, Australië-Queensland, Canada-Alberta enz. (Opmerking: deze granulariteitsniveau wordt alleen ondersteund voor staten / provincies in Australië, Canada, groot-Brittannië en Verenigde Staten).

Wanneer een regio of een set van regio's is toegewezen aan een eindpunt, worden alle aanvragen van deze regio's alleen naar dit eindpunt wordt geleid. Traffic Manager maakt gebruik van de bron-IP-adres van de DNS-query om te bepalen van de regio van waaruit is van een gebruiker uitvoeren van query's: dit is doorgaans het IP-adres van de lokale DNS-resolver uitvoeren van de query namens de gebruiker.  

![Azure Traffic Manager 'Geografische' verkeersrouteringsmethode](./media/traffic-manager-routing-methods/geographic.png)

Traffic Manager leest de bron-IP-adres van de DNS-query en besluit welke geografische regio die afkomstig van zijn is. Vervolgens wordt gezocht om te zien of er een eindpunt waarvoor deze geografische regio aan is toegewezen. Deze zoekactie wordt gestart op het laagste niveau van granulatie (staat/provincie waar deze wordt ondersteund, anders op het niveau van het land/regio) tot het hoogste niveau, is deze **World**. De eerste overeenkomst gevonden met behulp van deze verandering is aangewezen als het eindpunt te retourneren in de query-antwoord. Wanneer die overeenkomt met een geneste type eindpunt, een eindpunt binnen dit profiel onderliggende wordt geretourneerd, gebaseerd op de routeringsmethode. De volgende punten zijn van toepassing zijn op dit gedrag:

- Een geografische regio kan worden toegewezen aan één eindpunt in een Traffic Manager-profiel alleen wanneer het routeringstype geografische routering is. Dit zorgt ervoor dat routering van gebruikers deterministisch is, en klanten scenario's waarin ondubbelzinnige geografische grenzen kunnen inschakelen.
- Als een gebruiker de regio met twee verschillende eindpunten geografische toewijzing komt, wordt in Traffic Manager selecteert u het eindpunt met de laagste granulariteit en houdt geen rekening met de routering van aanvragen van die regio naar het andere eindpunt. Neem bijvoorbeeld een profiel voor de geografische routering van type met twee eindpunten - 1 en Endpoint2. 1 is geconfigureerd voor het ontvangen verkeer vanuit Ierland en Endpoint2 is geconfigureerd voor het ontvangen van verkeer van Europa. Als een verzoek afkomstig uit Ierland is, is deze altijd doorgestuurd naar 1.
- Omdat een regio kan worden toegewezen slechts aan één eindpunt, wordt het in Traffic Manager retourneert, ongeacht of het eindpunt in orde of niet is.

    >[!IMPORTANT]
    >Het is raadzaam dat klanten die gebruikmaken van de geografische routeringsmethode koppelen aan de geneste type-eindpunten die onderliggende profielen met ten minste twee eindpunten binnen elk.
- Als een eindpunt-overeenkomst wordt gevonden en het eindpunt is in de **gestopt** staat, Traffic Manager een NODATA-antwoord geretourneerd. Er wordt in dit geval geen verdere zoekacties in de hiërarchie geografische regio hoger uitgevoerd. Dit gedrag is ook van toepassing voor geneste eindpunttypen wanneer het onderliggende profiel in de **gestopt** of **uitgeschakelde** staat.
- Als een eindpunt wordt weergegeven een **uitgeschakelde** status, deze niet opgenomen in de regio die overeenkomt met proces. Dit gedrag is ook van toepassing voor geneste eindpunttypen wanneer het eindpunt is in de **uitgeschakelde** staat.
- Als een query afkomstig is van een geografische regio die niet toegewezen in dit profiel is, wordt in Traffic Manager een NODATA-antwoord retourneert. Daarom het wordt sterk aanbevolen dat klanten gebruiken de geografische routering met één eindpunt, in het ideale geval van het type geneste met ten minste twee eindpunten in het onderliggende-profiel met de regio **World** zijn toegewezen. Dit zorgt er ook voor dat alle IP-adressen die niet zijn toegewezen aan een regio worden verwerkt.

Zoals uitgelegd in [hoe Traffic Manager werkt](traffic-manager-how-it-works.md), Traffic Manager geen DNS-query's rechtstreeks van clients ontvangt. In plaats daarvan DNS-query's zijn afkomstig uit de recursieve DNS-service dat de clients zijn geconfigureerd voor gebruik. Daarom het IP-adres gebruikt om te bepalen van de regio is niet van de client-IP-adres, maar het is het IP-adres van de recursieve DNS-service. Dit IP-adres is in de praktijk moet een goede proxy voor de client.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen van toepassingen van hoge beschikbaarheid met [eindpuntcontrole van Traffic Manager](traffic-manager-monitoring.md)

Meer informatie over het [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png



