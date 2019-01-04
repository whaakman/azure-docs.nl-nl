---
title: Azure Standard Load Balancer en Beschikbaarheidszones
titlesuffix: Azure Load Balancer
description: Load Balancer van het type Standard en beschikbaarheidszones
services: load-balancer
documentationcenter: na
author: KumudD
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/08/2018
ms.author: kumud
ms.openlocfilehash: 2c4503b6ff065e98c49fe3f4e06b63cbeb7d1770
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53652741"
---
# <a name="standard-load-balancer-and-availability-zones"></a>Load Balancer van het type Standard en beschikbaarheidszones

Biedt ondersteuning voor Azure Load Balancer standaard-SKU [Beschikbaarheidszones](../availability-zones/az-overview.md) scenario's. Verscheidene nieuwe concepten zijn beschikbaar in Standard Load Balancer, waarmee u voor een optimale beschikbaarheid in uw end-to-end-scenario door resources met zones uitlijnen en verdelen ze in zones.  Beoordeling [Beschikbaarheidszones](../availability-zones/az-overview.md) voor meer informatie over wat Beschikbaarheidszones zijn in welke regio's momenteel ondersteuning voor Beschikbaarheidszones en andere verwante concepten en producten. Beschikbaarheidszones in combinatie met de standaardversie van Load Balancer zijn een functie voor uitbreidbare en flexibele set die veel verschillende scenario's kunt maken.  Raadpleeg dit document voor meer informatie over deze [concepten](#concepts) en fundamentele scenario [ontwerprichtlijnen](#design).

>[!NOTE]
>Beoordeling [Beschikbaarheidszones](https://aka.ms/availabilityzones) voor andere verwante onderwerpen. 

## <a name="concepts"></a> Beschikbaarheidszones concepten die worden toegepast op de Load Balancer

Er is geen directe relatie tussen resources voor Load Balancer en de werkelijke infrastructuur; het maken van een Load Balancer biedt geen exemplaar maken. Resources voor Load Balancer zijn waarin u hoe de vooraf gedefinieerde multitenant-infrastructuur voor het scenario dat u wilt maken-Azure moet programma kunt express-objecten.  Dit is van belang is in de context van de Beschikbaarheidszones omdat één Load Balancer-resource programmering van infrastructuur in meerdere Beschikbaarheidszones beheren kunt, terwijl een zone-redundante service wordt weergegeven als één resource vanuit het oogpunt van een klant.

Functies van een Load Balancer-resource worden uitgedrukt als een frontend, een regel, een statustest en de definitie van een back-end-pool.

Het gedrag en de eigenschappen van een Load Balancer-resource in de context van de Beschikbaarheidszones, worden als zone-redundante of zonegebonden beschreven.  Zone-redundante en zonegebonden beschrijven de zonality van een eigenschap.  In de context van de Load Balancer, zone-redundante altijd betekent *alle zones* en zonegebonden betekent dat de service om te garanderen een *één zone*.

Zone-redundante en zonegebonden scenario's voor ondersteuning van openbare en interne Load Balancer en beide kunnen instellen dat verkeer in zones behoefte (*zoneoverschrijdende load balancing*).

Een Load Balancer-resource zelf is regionale en nooit zonegebonden.  En een VNet en subnet zijn altijd regionale en nooit zonegebonden.

### <a name="frontend"></a>Front-end

Een Load Balancer-frontend is een Frontend-IP-configuratie verwijst naar een openbare IP-adresresource of een privé IP-adres binnen het subnet van een virtueel netwerk-resource.  Dit vormt het eindpunt met gelijke waar uw service beschikbaar is gemaakt.

Een Load Balancer-resource kan tegelijk zonegebonden en zone-redundante front-ends bevatten. 

Als een openbaar IP-adresresource is aan een zone is gegarandeerd, niet de zonality (of het gebrek daaraan) veranderlijke.  Als u wilt wijzigen of laat de zonality van een openbare IP-front, moet u het openbare IP-adres in de juiste zone opnieuw maken.  

U kunt de zonality van een front-end van een interne Load Balancer wijzigen door te verwijderen en opnieuw maken van de front-end, wijzigen of de zonality weglaten.

Wanneer u meerdere front-ends, Bekijk [meerdere front-ends voor Load Balancer](load-balancer-multivip-overview.md) voor belangrijke overwegingen.

#### <a name="zone-redundant-by-default"></a>Zone-redundant standaard

Een Standard Load Balancer-frontend is in een regio met Availability Zones, zone-redundante standaard.  Een enkele frontend-IP-adres zone kunt storingen en kan worden gebruikt om alle leden van back-end, ongeacht de zone bereiken. Dit betekent dat niet hitless gegevenspad, maar geen nieuwe pogingen of reestablishment slaagt. DNS--redundantie schema's zijn niet vereist. Enkele van de front-end-IP-adres wordt tegelijkertijd worden bediend door meerdere onafhankelijke infrastructuur implementaties in meerdere Beschikbaarheidszones.  Zone-redundante betekent dat alle binnenkomende of uitgaande stromen worden bediend door meerdere Beschikbaarheidszones in een regio tegelijkertijd met behulp van één IP-adres.

Een of meer Beschikbaarheidszones kan mislukken en het gegevenspad blijft zo lang als één zone in de regio blijven in orde is. Zone-redundante configuratie is de standaardinstelling en is geen verdere actie vereist.  Wanneer een regio de mogelijkheid krijgt voor ondersteuning voor Beschikbaarheidszones, wordt een bestaande frontend zone-redundante automatisch.

Het volgende script gebruiken om te maken van een zone-redundante openbare IP-adres voor uw interne Standard Load Balancer. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie om deze sjablonen.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

Het volgende script gebruiken om te maken van een zone-redundante frontend-IP-adres voor uw interne Standard Load Balancer. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie om deze sjablonen.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zone_redundant_frontend",
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

#### <a name="optional-zone-guarantee"></a>Optionele zone garantie

U kunt ervoor kiezen een frontend gegarandeerd een enkele zone die wordt ook wel een *zonegebonden frontend*.  Dit betekent dat binnenkomende of uitgaande stromen worden geleverd door een enkele zone in een regio.  Uw frontend deelt gedrag met de status van de zone.  Het gegevenspad wordt niet beïnvloed door fouten in zones dan wanneer deze is gegarandeerd. U kunt zonegebonden front-ends gebruiken om een IP-adres per binnen een Beschikbaarheidszone zichtbaar te maken.  Bovendien kunt u rechtstreeks zonegebonden front-ends gebruiken of, wanneer de front-end uit openbare IP-adressen bestaat, moet u deze integreren in een DNS-taakverdeling product, zoals [Traffic Manager](../traffic-manager/traffic-manager-overview.md) en één DNS-naam, die een client wordt omgezet in gebruik meerdere zonegebonden IP-adressen.  U kunt dit ook gebruiken om beschikbaar te stellen per zone taakverdeling eindpunten voor het bewaken van afzonderlijk elke zone.  Als u wilt combineren van deze concepten (zone-redundante en zonegebonden voor dezelfde back-end), raadpleegt u [meerdere front-ends voor Azure Load Balancer](load-balancer-multivip-overview.md).

Voor de front-end van een openbare Load Balancer, die u toevoegt een *zones* parameter voor het openbare IP-adres waarnaar wordt verwezen door de front-end-IP-configuratie.  

Voor een interne Load Balancer-frontend, voegt u toe een *zones* parameter voor de interne Load Balancer front-end-IP-configuratie. De frontend van de zonegebonden zorgt ervoor dat de Load Balancer om te waarborgen van een IP-adres in een subnet naar een specifieke zone.

Het volgende script gebruiken voor het maken van een zonegebonden standaard, openbaar IP-adres in beschikbaarheid Zone 1. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie om deze sjablonen.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "zones": [ "1" ],
            "sku":
            {
                "name": "Standard"
            },
```

Het volgende script gebruiken om te maken van een interne Standard Load Balancer-front-end in de beschikbaarheid van Zone 1.

Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie om deze sjablonen. Ook, definiëren de **zones** eigenschap in de front-end-IP-configuratie voor de onderliggende resource.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zonal_frontend_in_az1",
                        "zones": [ "1" ],
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

### <a name="cross-zone-load-balancing"></a>Zoneoverschrijdende Load-Balancing

Zoneoverschrijdende load balancing is het vermogen van de Load Balancer een back endeindpunt in een andere zone bereiken en is onafhankelijk van de front-end- en de zonality.

Als u wilt uitlijnen en garanderen van uw implementatie binnen een enkele zone, zonegebonden front-end en back-van zonegebonden endresources aan de zone met dezelfde uitlijnen. Er is geen verdere actie vereist.

### <a name="backend"></a>Back-end

Load Balancer werkt met virtuele Machines.  Alle virtuele machines in een enkel VNet kan deel uitmaken van de back-endpool, ongeacht of deze aan een zone is gegarandeerd of welke zone is gegarandeerd.

Als u wilt uitlijnen en garanderen van de front-end en back-end met een enkele zone, plaats alleen voor VM's binnen dezelfde regio in de desbetreffende back-endpool.

Als u adres VM's in meerdere zones wilt, plaats gewoon voor virtuele machines in meerdere zones in de dezelfde back-endpool.  Wanneer u schaalsets van virtuele machine, kunt u een of meer virtuele-machineschaalsets aan dezelfde back endadresgroep plaatsen.  En elk van deze virtuele-machineschaalsets kan zich in een of meerdere zones.

### <a name="outbound-connections"></a>Uitgaande verbindingen

[Uitgaande verbindingen](load-balancer-outbound-connections.md) door alle zones worden verwerkt en in een regio met Beschikbaarheidszones automatisch zone-redundante zijn wanneer een virtuele machine gekoppeld aan een openbare Load Balancer en een zone-redundante front is.  Uitgaande verbindingen SNAT poort toewijzingen overleven zone fouten.  

Op zijn beurt als de virtuele machine gekoppeld aan een openbare Load Balancer en een zonegebonden front is, worden uitgaande verbindingen gegarandeerd door een enkele zone worden geleverd.  Uitgaande verbindingen delen gedrag met de status van de respectieve zone.

De voorafgaande toewijzing SNAT-poort en de algoritme is hetzelfde met of zonder zones.

### <a name="health-probes"></a>Statuscontroles

Uw bestaande health test definities ongewijzigd blijven zonder Beschikbaarheidszones.  Maar we hebben het statusmodel op het niveau van een infrastructuur uitgebreid. 

Bij het gebruik van zone-redundante uitbreiding front-ends, Load Balancer van de interne statusmodel onafhankelijk de bereikbaarheid van een virtuele machine van elke Beschikbaarheidszone-test en paden afsluiten in zones die mogelijk is mislukt zonder tussenkomst van de klant.  Als een opgegeven pad niet beschikbaar vanaf de infrastructuur van de Load Balancer van een zone voor een virtuele machine in een andere zone is, kan de Load Balancer detecteren en te voorkomen dat deze fout. Andere zones die deze virtuele machine kunnen bereiken kunnen blijven voldoen aan de VM op basis van hun respectieve front-ends.  Als gevolg hiervan is het mogelijk dat tijdens de gebeurtenissen die betrekking hebben, elke zone mogelijk enigszins stroom distributies bij het beveiligen van de algemene status van uw end-to-end-service.

## <a name="design"></a> Overwegingen bij het ontwerpen

Load Balancer is opzettelijk flexibel in de context van de Beschikbaarheidszones. U kunt kiezen om uitgelijnd met zones of kunt u zone-redundante worden.  Grotere beschikbaarheid kunt komen voor de prijs van een grotere complexiteit en moet u ontwerpen voor beschikbaarheid voor optimale prestaties.  Laten we eens enkele belangrijke ontwerpoverwegingen.

### <a name="automatic-zone-redundancy"></a>Automatische zone-redundantie

Load Balancer kunt u eenvoudig een enkel IP-adres als een zone-redundante front hebben. Een zone-redundante IP-adres kan veilig geen zonegebonden resource in een zone fungeren en kan een of meer zone fouten overleven als één zone in de regio in orde is blijft. Een zonegebonden front is echter een vermindering van de service naar een enkele zone en -shares gedrag met de bijbehorende zone.

Zone-redundantie impliceert niet hitless gegevenspad of controlelaag;  het is uitdrukkelijk gegevenslaag. Zone-redundante stromen kunnen zones en stromen van de klant wordt alle in orde zones gebruiken in een regio. In het geval van een fout opgetreden in een zone verkeersstromen, gezonde zones met op dat punt in tijd niet beïnvloed.  Verkeersstromen met behulp van een zone op het moment van de zone fout wordt mogelijk beïnvloed, maar toepassingen kunnen herstellen en deze stromen blijven in de resterende in orde zones in het gebied van doorgifte of reestablishment zodra Azure geconvergeerd rond de zone-fout.

### <a name="xzonedesign"></a> Cross-zone grenzen

Het is belangrijk om te begrijpen dat elk gewenst moment een end-to-end-service snijdt zones, u gedrag delen met niet één zone, maar mogelijk meerdere zones.  Als gevolg hiervan uw end-to-end-service mogelijk niet hebben opgedaan met beschikbaarheid via niet-zonegebonden implementaties.

Vermijd de introductie van onbedoelde zoneoverschrijdende afhankelijkheden, die wordt ongeldig verklaard beschikbaarheid winsten bij het gebruik van Beschikbaarheidszones.  Wanneer uw toepassing uit meerdere onderdelen bestaat en u wilt worden bestand is tegen fouten van de zone, moet u ervoor zorgen dat dat de voldoende belangrijke onderdelen in het geval van een zone mislukken.  Bijvoorbeeld één kritieke onderdeel voor uw toepassing kan van invloed zijn op de gehele toepassing als deze alleen in een zone dan de overgebleven zone (s bestaat).  Bovendien ook rekening houden met de herstelbewerking zone en hoe uw toepassing worden geconvergeerd. Laten we bekijken enkele belangrijke punten en deze gebruiken als inspiratie voor vragen als u denkt uw specifieke scenario dat.

- Als uw toepassing bestaat uit twee onderdelen, zoals een IP-adres en een virtuele machine met een beheerde schijf, en ze worden gegarandeerd in zone 1 en zone 2, wanneer de zone 1, mislukt de end-to-end-service niet overleeft wanneer zone 1 is mislukt.  Niet overschreden door de zones, tenzij u beter begrijpen die u een potentieel schadelijke foutmodus maakt.

- Als uw toepassing bestaat uit twee onderdelen, zoals een IP-adres en een VM met beheerde schijf, en ze worden zone-redundante en zone 1 respectievelijk worden gegarandeerd, uw end-to-end-service blijven geldig na het mislukken van de zone van zone 2, zone 3, of beide, tenzij de zone 1 is mislukt.  Echter, verliest u enkele mogelijkheid tot reden over de status van uw service als uw probleem, is de bereikbaarheid van de front-end.  Houd rekening met een meer uitgebreide status en capaciteit analysemodel.  U kunt de zone-redundante en zonegebonden concepten samen gebruiken om uit te breiden inzicht en beheerbaarheid.

- Als uw toepassing bestaat uit twee onderdelen, zoals een zone-redundante Load Balancer-frontend en een zoneoverschrijdende virtuele-machineschaalset in drie zones, uw resources in zones niet beïnvloed door de fout is beschikbaar, maar de servicecapaciteit van de end-to-end-is mogelijk gedegradeerd tijdens de zone is mislukt. Uw implementatie kan een of meer zone fouten overleven vanuit het perspectief van een infrastructuur, en dit genereert de volgende vragen:
  - Begrijpt u hoe uw toepassing redenen over deze fouten en verminderde capaciteit?
  - Moet u beveiliging in uw service, zodat een failover afgedwongen op een paar regio indien nodig hebt?
  - Hoe wordt u bewaken, opsporen en verhelpen dergelijk scenario? Het is mogelijk dat er diagnostische gegevens van Standard Load Balancer gebruiken voor het verbeteren van de bewaking van de serviceprestaties van uw end-to-end. Bedenk wat er beschikbaar is en wat kan uitdiepen nodig hebt om een volledig overzicht.

- Zones kunnen u problemen eenvoudiger begrepen en die zijn opgenomen.  Zone-fout is echter niet anders dan andere fouten als het gaat om concepten zoals time-outs, nieuwe pogingen en algoritmen voor uitstel. Hoewel Azure Load Balancer zone-redundante paden biedt en er wordt geprobeerd om te snel herstellen op het pakketniveau van een in realtime, herhaald verzenden of reestablishments zich kunnen voordoen tijdens het begin van een fout en is het belangrijk om te begrijpen hoe uw toepassing met copes fouten. Het schema voor de taakverdeling overleeft, maar moet u plannen voor het volgende:
  - Als een zone is mislukt, uw end-to-end-service te begrijpen dit en als de status van de verloren gegaan is, hoe wordt u herstellen?
  - Wanneer een zone worden geretourneerd, uw toepassing te begrijpen hoe u veilig worden geconvergeerd?

### <a name="zonalityguidance"></a> Zone-redundante versus zonegebonden

Zone-redundante biedt een zone-neutraal en op dezelfde tijd flexibele optie met een enkel IP-adres voor de service.  Deze kan op zijn beurt complexiteit verminderen.  Zone-redundante ook heeft mobility in zones en veilig kan worden gebruikt voor bronnen in elke zone.  Het is ook de oplossing Toekomstbestendig in regio's zonder Beschikbaarheidszones, die wijzigingen die vereist zijn wanneer een regio toegang tot Beschikbaarheidszones kunt beperken.  De syntaxis van de configuratie voor een zone-redundante IP-adres of een frontend slaagt in elke regio inclusief gebruikers met Beschikbaarheidszones.

Zonegebonden krijgt u een expliciete garantie voor een zone terechtkomt delen met de status van de zone. Een zonegebonden IP-adres koppelen is adres of de zonegebonden Load Balancer-frontend een kenmerk wenselijk of redelijke met name als de gekoppelde resource een zonegebonden VM in dezelfde zone is.  Of misschien uw toepassing vereist expliciete kennis over welke zone een resource zich bevindt in en u wilt expliciet reden over beschikbaarheid in afzonderlijke zones.  U kunt kiezen om meerdere zonegebonden front-ends voor een end-to-end-service die verdeeld over zones zichtbaar te maken (dat wil zeggen, per zone zonegebonden frontends voor meerdere zonegebonden virtuele-machineschaalsets wordt ingesteld).  En als uw zonegebonden front-ends openbare IP-adressen zijn, kunt u deze meerdere zonegebonden front-ends voor de weergave van uw service met [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Of u kunt meerdere zonegebonden front-ends gebruiken om te krijgen per zone status en prestaties inzichten via externe bewakingsoplossingen en de algehele service met een zone-redundante front zichtbaar maken. U moet alleen zonegebonden resources met zonegebonden front-ends uitgelijnd op dezelfde zone fungeren en te voorkomen dat mogelijk schadelijke zoneoverschrijdende-scenario's voor zonegebonden resources.  Verwijzing voor zonegebonden resources alleen aanwezig in regio's waar Beschikbaarheidszones bestaan.

Er is geen algemene richtlijnen die een is een betere keuze dan de andere zonder te weten van de servicearchitectuur.

## <a name="limitations"></a>Beperkingen

- Terwijl gegevens vlak is volledig zone-redundante (tenzij zonegebonden garantie is opgegeven), bewerkingen voor de controlelaag zijn niet volledig zone-redundante.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Beschikbaarheidszones](../availability-zones/az-overview.md)
- Meer informatie over [Standard Load Balancer](load-balancer-standard-overview.md)
- Meer informatie over het [taakverdeling instellen voor virtuele machines binnen een zone met behulp van een Standard Load Balancer met een zonegebonden frontend.](load-balancer-standard-public-zonal-cli.md)
- Meer informatie over het [taakverdeling instellen voor virtuele machines in zones met behulp van een Standard Load Balancer met een zone-redundante front](load-balancer-standard-public-zone-redundant-cli.md)
