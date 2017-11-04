---
title: Azure Traffic Manager - verkeersrouteringsmethoden | Microsoft Docs
description: Hiermee artikelen geeft die u inzicht in de verschillende verkeersrouteringsmethoden die wordt gebruikt door Traffic Manager
services: traffic-manager
documentationcenter: 
author: KumudD
manager: timlt
editor: 
ms.assetid: db1efbf6-6762-4c7a-ac99-675d4eeb54d0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: kumud
ms.openlocfilehash: fe776e24a4f78b389c6096694055b38befa3c419
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="traffic-manager-routing-methods"></a>Methoden voor het doorsturen van Traffic Manager

Azure Traffic Manager ondersteunt vier verkeersroutering methoden om het netwerkverkeer omgeleid naar de verschillende service-eindpunten vast te stellen. De methode voor verkeersroutering geldt Traffic Manager voor elke DNS-query die wordt ontvangen. De methode traffic routing bepaalt welk eindpunt in het DNS-antwoord geretourneerd.

Er zijn vier methoden voor het doorsturen van verkeer beschikbaar in Traffic Manager:

* **[Prioriteit](#priority):** Selecteer **prioriteit** wanneer u wilt gebruiken van een primaire service-eindpunt voor al het verkeer en geef de back-ups in het geval de primaire of de back-eindpunten zijn niet beschikbaar.
* **[Gewogen](#weighted):** Selecteer **gewogen** als u wilt voor de distributie van verkeer over een set eindpunten, gelijkmatig of op basis van gewicht, die u definieert.
* **[Prestaties](#performance):** Selecteer **prestaties** wanneer u eindpunten op verschillende geografische locaties en u wilt dat eindgebruikers het eindpunt 'dichtstbijzijnde' in de laagste netwerklatentie termen te gebruiken.
* **[Geografische](#geographic):** Selecteer **geografische** zodat gebruikers worden omgeleid naar specifieke eindpunten (Azure, extern of geneste) op basis van welke geografische locatie hun DNS-query is afkomstig uit. Hiermee machtigt Traffic Manager-klanten scenario's waar dat bekend is van een gebruiker geografische regio en routering toe op basis van die is belangrijk te maken. Voorbeelden zijn die voldoet aan gegevens onafhankelijkheid vereist, lokalisatie van inhoud & gebruiker ervaring en verkeer vanuit verschillende regio's te meten.

Alle Traffic Manager-profielen bevatten bewaking van endpoint-status- en eindpunt automatische failover. Zie voor meer informatie [Traffic Manager-eindpunt bewaking](traffic-manager-monitoring.md). Een enkele Traffic Manager-profiel kunt slechts één methode voor het doorsturen van verkeer. U kunt een andere verkeersrouteringsmethode selecteren voor uw profiel op elk gewenst moment. Wijzigingen worden toegepast binnen één minuut en er geen uitvaltijd is ontstaan. Voor verkeersroutering methoden kunnen worden gecombineerd met behulp van geneste Traffic Manager-profielen. Geneste kunt geavanceerde en flexibele verkeersroutering configuraties die voldoen aan de behoeften van grotere, complexe toepassingen. Zie voor meer informatie [Traffic Manager-profielen genest](traffic-manager-nested-profiles.md).

## <a name = "priority"></a>Prioriteit voor verkeersroutering methode

Een organisatie wil vaak betrouwbaarheid bevatten voor de services die door het implementeren van een of meer back-services als hun primaire service uitvalt. De methode 'Prioriteit' traffic routing kan Azure klanten op eenvoudige wijze dit patroon van een failover.

![Azure Traffic Manager 'Prioriteit' verkeersroutering methode][1]

Traffic Manager-profiel bevat een geprioriteerde lijst van de service-eindpunten. Traffic Manager verzendt standaard al het verkeer naar de primaire (hoogste prioriteit)-eindpunt. Als het primaire eindpunt niet beschikbaar is, wordt in Traffic Manager het verkeer naar het tweede eindpunt gestuurd. Als de primaire en secundaire eindpunten niet beschikbaar zijn, wordt het verkeer gaat naar de andere, enzovoort. Beschikbaarheid van het eindpunt is gebaseerd op de geconfigureerde status (ingeschakeld of uitgeschakeld) en de lopende eindpuntcontrole.

### <a name="configuring-endpoints"></a>Eindpunten configureren

Met Azure Resource Manager, configureert u de eindpuntprioriteit expliciet met behulp van de prioriteitseigenschap voor elk eindpunt. Deze eigenschap is een waarde tussen 1 en 1000. Lagere waarden vertegenwoordigen een hogere prioriteit. Eindpunten kunnen geen prioriteitswaarden delen. De eigenschap is optioneel. Wanneer u dit weglaat, wordt een standaardprioriteit op basis van de endpoint-volgorde wordt gebruikt.

##<a name = "weighted"></a>Gewogen verkeersroutering methode
De 'Gewogen' verkeersroutering methode kunt u verkeer gelijkmatig verdelen of een vooraf gedefinieerde weging gebruiken.

![Azure Traffic Manager-'Gewogen' methode verkeer routering][2]

In de gewogen verkeersroutering methode kunt u een gewicht toewijzen aan elk eindpunt in de profielconfiguratie van Traffic Manager. Het gewicht is een geheel getal van 1 tot en met 1000. Deze parameter is optioneel. Als u dit weglaat, wordt verkeer Managers een standaardgewicht van '1'.

Voor elke DNS-query heeft ontvangen, kiest de Traffic Manager willekeurig een beschikbare eindpunt. De kans van de keuze van een eindpunt is gebaseerd op het gewicht is toegewezen aan alle beschikbare eindpunten. Met behulp van hetzelfde gewicht over alle eindpunten resulteert in een zelfs verkeer distributie. Met behulp van hogere of lagere gewichten op specifieke eindpunten zorgt ervoor dat deze eindpunten moet meer of minder vaak worden geretourneerd in de DNS-antwoorden.

De gewogen maakt een nuttig scenario's:

* Upgrade van de toepassing geleidelijk: een percentage van het verkeer te routeren naar een nieuw eindpunt toewijzen en het verkeer geleidelijk te verhogen na verloop van tijd tot 100%.
* Toepassingen migreren naar Azure: een profiel maken met Azure en externe eindpunten. Het gewicht van de eindpunten van de nieuwe eindpunten liever aanpassen.
* Cloud sturen voor extra capaciteit: snel een on-premises implementatie naar de cloud uitbreiden door de gegevens achter een Traffic Manager-profiel. Wanneer u extra capaciteit in de cloud, kunt u toevoegen of meer eindpunten inschakelen en geef op welk gedeelte van het verkeer naar elk eindpunt gaat.

De Azure Resource Manager-portal ondersteunt de configuratie van gewogen voor verkeersroutering.  U kunt met behulp van de Resource Manager-versies van Azure PowerShell, CLI en de REST API's gewichten configureren.

Het is belangrijk te weten dat DNS-antwoorden in de cache opgeslagen door clients en de recursieve DNS-servers die clients gebruiken voor het omzetten van DNS-namen. Deze cache, kan een invloed hebben op gewogen verkeer distributies. Wanneer het aantal clients en de recursieve DNS-servers groot is, wordt verkeer distributie werkt zoals verwacht. Echter, wanneer het aantal clients of recursieve DNS-servers klein is, cachebewerkingen kan aanzienlijk laten uitvallen de distributie van verkeer.

Algemene gebruiksvoorbeelden zijn onder andere:

* Ontwikkeling en testomgevingen
* Toepassingen communicatie
* Toepassingen die gericht is op een beperkte-gebruikersgroep die delen een gemeenschappelijke recursieve DNS-infrastructuur (bijvoorbeeld medewerkers van de bedrijfsportal verbinding maken via een proxy)

Deze DNS-cache in gevolgen voor alle DNS-verkeer routeren systemen, niet alleen Azure Traffic Manager gelden. In sommige gevallen kan expliciet de DNS-cache wissen bieden een tijdelijke oplossing. In andere gevallen is zijn een alternatieve methode voor verkeersroutering geschikter.

## <a name = "performance"></a>Prestaties voor verkeersroutering methode

Eindpunten in twee of meer locaties wereldwijd implementeren, kunt de reactietijd van de vele toepassingen verbeteren door routeren van verkeer naar de locatie die het dichtst bij u is. De methode 'Prestaties' traffic routing biedt deze mogelijkheid.

![Azure Traffic Manager 'Prestaties' verkeersroutering methode][3]

Het eindpunt 'dichtstbijzijnde' is niet noodzakelijkerwijs dichtstbijzijnde door geografische afstand wordt gemeten. In plaats daarvan bepaalt de methode 'Prestaties' traffic routing het dichtstbijzijnde eindpunt door te meten netwerklatentie. Traffic Manager onderhoudt een tabel van de latentie Internet om bij te houden de round trip-tijd tussen het IP-adresbereiken en elke Azure-datacenter.

Traffic Manager zoekt naar het IP-bronadres van de inkomende DNS-aanvraag in de tabel van de latentie Internet. Traffic Manager kiest een eindpunt beschikbaar in het datacenter Azure die de laagste latentie voor deze IP-adresbereik is, en vervolgens dat eindpunt in de DNS-antwoord geretourneerd.

Zoals uitgelegd in [hoe Traffic Manager werkt](traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager ontvangt geen DNS-query's rechtstreeks van clients. In plaats daarvan DNS-query's zijn afkomstig van de recursieve DNS-service dat de clients zijn geconfigureerd voor gebruik. Daarom het IP-adres gebruikt om te bepalen van het eindpunt 'dichtstbijzijnde' is geen IP-adres van de client, maar het is het IP-adres van de recursieve DNS-service. Dit IP-adres is in de praktijk kan een goede proxy voor de client.


Traffic Manager wordt regelmatig bijgewerkt in de tabel Internet latentie vanwege wijzigingen in de globale Internet en de nieuwe Azure-regio's. Echter de prestaties van toepassingen is afhankelijk van realtime variaties in load via Internet. Verkeer routeren bewaakt belasting van een bepaalde service-eindpunt niet. Echter, als een eindpunt niet beschikbaar is, Traffic Manager biedt niet opnemen in reacties op DNS-query's.


Verwijst naar Let op:

* Als uw profiel meerdere eindpunten in dezelfde Azure-regio bevat, klikt u vervolgens wordt Traffic Manager verkeer gelijkmatig over het aantal beschikbare eindpunten in deze regio. Als u liever een ander verkeer distributie binnen een regio, kunt u [Traffic Manager-profielen genest](traffic-manager-nested-profiles.md).
* Als alle ingeschakelde eindpunten in de dichtstbijzijnde Azure-regio zijn gedegradeerd, wordt het Traffic Manager verkeer verplaatst naar de eindpunten in de volgende dichtstbijzijnde Azure-regio. Als u een volgorde van voorkeur failover definiëren wilt, gebruikt u [Traffic Manager-profielen genest](traffic-manager-nested-profiles.md).
* Als u de verkeersrouteringsmethode prestaties met externe eindpunten of geneste eindpunten, moet u de locatie van deze eindpunten opgeven. Kies de Azure-regio die het dichtst bij uw implementatie. Deze locaties zijn de waarden die worden ondersteund door de latentie Internet tabel.
* De algoritme die kiest van het eindpunt is deterministisch. Herhaalde DNS-query's uit dezelfde client omgeleid naar hetzelfde eindpunt. Clients gebruiken doorgaans verschillende recursieve DNS-servers als onderweg. De client kan worden gerouteerd naar een ander eindpunt. Routering kan ook worden beïnvloed door wijzigingen in de tabel van de latentie Internet. De prestaties voor verkeersroutering methode is daarom geen garantie dat een client altijd worden omgeleid naar hetzelfde eindpunt.
* Wanneer de Internet-latentie-tabel is gewijzigd, is het mogelijk dat sommige clients worden omgeleid naar een ander eindpunt. Deze wijziging routering is nauwkeuriger op basis van latentiegegevens van de huidige. Deze updates zijn essentieel voor het onderhouden van de nauwkeurigheid van het verkeer routeren naarmate Internet voortdurend zich verder ontwikkelen.

## <a name = "geographic"></a>Geografische verkeersroutering methode

Traffic Manager-profielen kunnen worden geconfigureerd voor het gebruik van de geografische routeringsmethode zodat gebruikers worden omgeleid naar specifieke eindpunten (Azure, extern of geneste) op basis van welke geografische locatie hun DNS-query is afkomstig uit. Hiermee machtigt Traffic Manager-klanten scenario's waar dat bekend is van een gebruiker geografische regio en routering toe op basis van die is belangrijk te maken. Voorbeelden zijn die voldoet aan gegevens onafhankelijkheid vereist, lokalisatie van inhoud & gebruiker ervaring en verkeer vanuit verschillende regio's te meten.
Wanneer een profiel is geconfigureerd voor het doorsturen van geografische, elk eindpunt gekoppeld dat profiel moet beschikken over een reeks geografische regio's die zijn toegewezen. Een geografische regio kan zich op de volgende detailniveaus 
- World – elke regio
- Regionale groepering – bijvoorbeeld Afrika Midden-Oosten, Australië/Pacific enzovoort. 
- Land/regio – bijvoorbeeld Ierland Peru, Hongkong SAR enzovoort. 
- Staat/provincie – bijvoorbeeld Verenigde Staten Californië, Australië-Queensland, Canada Alberta enz. (Opmerking: deze granulariteitsniveau wordt alleen ondersteund voor staten / provincies in Australië, Canada VK en Verenigde Staten).

Wanneer een regio of een set van regio's is toegewezen aan een eindpunt, wordt alle aanvragen van deze gebieden gerouteerd alleen naar dat eindpunt. Traffic Manager maakt gebruik van het bron-IP-adres van de DNS-query om te bepalen van de regio van waaruit het opvragen van een gebruiker uit – meestal is dit het IP-adres van de lokale DNS-resolver tijdens het doorzoeken van de query namens de gebruiker.  

![Azure Traffic Manager-'Geografische' methode verkeer routering](./media/traffic-manager-routing-methods/geographic.png)

Traffic Manager leest het bron-IP-adres van de DNS-query en besluit welke geografische regio die afkomstig van zijn is. Het lijkt erop klikt om te zien of er een eindpunt dat deze geografische regio toegewezen heeft. Deze zoekactie wordt gestart op het laagste niveau van granulatie (staat/provincie waar het wordt ondersteund, anders op het niveau van het land/regio) en probeert het helemaal tot aan het hoogste niveau, **World**. Het eerste resultaat gevonden met behulp van deze verandering is aangewezen als het eindpunt te retourneren in de query-antwoord. Als die overeenkomt met een eindpunt binnen die onderliggende profiel van een geneste type eindpunt wordt geretourneerd, gebaseerd op de routeringsmethode voor. De volgende punten van toepassing zijn op dit gedrag:

- Een geografische regio kan alleen worden toegewezen aan één eindpunt in een Traffic Manager-profiel als het type routering geografische routering. Dit zorgt ervoor dat gebruikers routering deterministisch is en klanten scenario's waarvoor ondubbelzinnig geografische grenzen kunnen inschakelen.
- De regio van een gebruiker afkomstig zijn onder twee verschillende eindpunten geografische toewijzing, Traffic Manager selecteert het eindpunt met de laagste granulatie als u routering aanvragen van die regio aan het andere eindpunt niet. Neem bijvoorbeeld een type routering voor geografische profiel met twee eindpunten - 1 en Endpoint2. 1 is geconfigureerd voor het ontvangen verkeer van Ierland en Endpoint2 is geconfigureerd voor het ontvangen verkeer van Europa. Als een aanvraag afkomstig uit Ierland is, wordt het altijd naar 1 gerouteerd.
- Omdat een regio kan alleen worden toegewezen aan één eindpunt, wordt het in Traffic Manager retourneert ongeacht of het eindpunt al dan niet in orde is.

    >[!IMPORTANT]
    >Het is raadzaam dat klanten die gebruikmaken van de geografische routeringsmethode koppelen aan de geneste type-eindpunten die onderliggende profielen met ten minste twee eindpunten binnen elk heeft.
- Als een eindpunt overeenkomst is gevonden en dat eindpunt in de **gestopt** staat, Traffic Manager een GEENGEGEVENS antwoord geretourneerd. In dit geval geen verdere zoekacties bestaat hoger staan in de hiërarchie geografische regio. Dit gedrag is ook van toepassing voor geneste eindpunttypen wanneer het onderliggende profiel in de **gestopt** of **uitgeschakelde** status.
- Als een eindpunt wordt weergegeven in een **uitgeschakelde** status, het niet opgenomen in de regio die overeenkomt met de proces. Dit gedrag is ook van toepassing voor geneste eindpunttypen wanneer het eindpunt is in de **uitgeschakelde** status.
- Als een query afkomstig is van een geografische regio die niet toegewezen in dit profiel is, retourneert Traffic Manager een antwoord GEENGEGEVENS. Daarom is het raadzaam dat klanten gebruiken geografische routering met één eindpunt, in het ideale geval van het type geneste met ten minste twee eindpunten in het onderliggende-profiel met de regio **World** toegewezen. Dit zorgt er ook voor dat alle IP-adressen die niet zijn toegewezen aan een regio worden verwerkt.

Zoals uitgelegd in [hoe Traffic Manager werkt](traffic-manager-how-traffic-manager-works.md), Traffic Manager ontvangt geen DNS-query's rechtstreeks van clients. In plaats daarvan DNS-query's zijn afkomstig van de recursieve DNS-service dat de clients zijn geconfigureerd voor gebruik. Daarom het IP-adres gebruikt om te bepalen van de regio is niet van de client-IP-adres, maar het is het IP-adres van de recursieve DNS-service. Dit IP-adres is in de praktijk kan een goede proxy voor de client.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen van hoge beschikbaarheid van toepassingen via [eindpuntcontrole Traffic Manager](traffic-manager-monitoring.md)

Meer informatie over hoe [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png



