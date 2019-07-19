---
title: Azure Standard Load Balancer en Beschikbaarheidszones
titlesuffix: Azure Load Balancer
description: Load Balancer van het type Standard en beschikbaarheidszones
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: allensu
ms.openlocfilehash: 5ef7de148d5ef4727602b8287164f2aff9ccf822
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274502"
---
# <a name="standard-load-balancer-and-availability-zones"></a>Load Balancer van het type Standard en beschikbaarheidszones

De standaard-SKU van Azure Load Balancer ondersteunt [Beschikbaarheidszones](../availability-zones/az-overview.md) -scenario's. Er zijn verschillende nieuwe concepten beschikbaar met Standard Load Balancer, waarmee u de beschik baarheid in uw end-to-end-scenario kunt optimaliseren door resources te uitlijnen met zones en deze te verdelen over zones.  Bekijk [Beschikbaarheidszones](../availability-zones/az-overview.md) voor hulp bij wat Beschikbaarheidszones zijn, welke regio's momenteel ondersteuning bieden voor Beschikbaarheidszones en andere gerelateerde concepten en producten. Beschikbaarheidszones in combi natie met Standard Load Balancer zijn een expansieve, en een flexibele set functies die veel verschillende scenario's kunnen maken.  Lees dit document voor meer informatie over deze [concepten](#concepts) en [ontwerp richtlijnen](#design)voor het basis scenario.

>[!IMPORTANT]
>Bekijk [Beschikbaarheidszones](../availability-zones/az-overview.md) voor Verwante onderwerpen, inclusief alle specifieke informatie over de regio.

## <a name="concepts"></a>Beschikbaarheidszones concepten toegepast op Load Balancer

Er is geen rechtstreekse relatie tussen Load Balancer resources en de werkelijke infra structuur. Als u een Load Balancer maakt, wordt er geen exemplaar gemaakt. Load Balancer resources zijn objecten waarin u kunt zien hoe Azure de vooraf gebouwde multi tenant-infra structuur moet Program meren om het scenario dat u wilt maken te verhalen.  Dit is belang rijk in de context van Beschikbaarheidszones omdat één Load Balancer resource de programmering van de infra structuur in meerdere Beschikbaarheidszones kan beheren terwijl een zone-redundante service als één resource wordt weer gegeven vanuit het oogpunt van de klant.

De functies van een Load Balancer resource worden uitgedrukt als een front-end, een regel, een status test en een back-end-groeps definitie.

In de context van Beschikbaarheidszones worden het gedrag en de eigenschappen van een Load Balancer resource beschreven als zone-redundante of zonegebonden.  Zone-redundante en zonegebonden beschrijven de zonality van een eigenschap.  In de context van Load Balancer betekent zone-redundantie altijd dat *meerdere zones* en zonegebonden betekent dat de service wordt geïsoleerd voor *één zone*.

Zowel de open bare als de interne Load Balancer ondersteunings zone-redundante en zonegebonden-scenario's en beide kunnen verkeer omleiden naar alle zones, indien nodig (*Kruis zone taak verdeling*).

Een Load Balancer resource zelf is regionaal en nooit zonegebonden.  En een VNet en subnet zijn altijd regionaal en nooit zonegebonden.

### <a name="frontend"></a>Front

Een Load Balancer frontend is een front-end-IP-configuratie die verwijst naar een open bare IP-adres of een privé-IP-adres binnen het subnet van een virtuele netwerk bron.  Het vormt het eind punt met gelijke taak verdeling waar uw service wordt weer gegeven.

Een Load Balancer resource kan gelijktijdig front-zonegebonden en zone-redundante front-ends bevatten. 

Wanneer een open bare IP-bron is gegarandeerd voor een zone, is het zonality (of het ontbreken daarvan) niet van onveranderbaar.  Als u de zonality van een openbaar IP-frontend wilt wijzigen of weglaten, moet u het open bare IP-adres opnieuw in de juiste zone maken.  

U kunt de zonality van een front-end van een interne Load Balancer wijzigen door de front-end te verwijderen en opnieuw te maken, door de zonality te wijzigen of weg te zetten.

Als u meerdere front-ends gebruikt, controleert u [meerdere frontends voor Load Balancer](load-balancer-multivip-overview.md) op belang rijke overwegingen.

#### <a name="zone-redundant-by-default"></a>Zone-redundant standaard

>[!IMPORTANT]
>Bekijk [Beschikbaarheidszones](../availability-zones/az-overview.md) voor Verwante onderwerpen, inclusief alle specifieke informatie over de regio.

In een regio met Beschikbaarheidszones is een Standard Load Balancer front-front-end standaard zone-redundant.  Eén frontend-IP-adres kan onterecht zone storingen en kan worden gebruikt om alle leden van de back-endadresgroep onafhankelijk van de zone te bereiken. Dit betekent niet het Hitless, maar eventuele nieuwe pogingen of het opnieuw instellen van het bestand is geslaagd. DNS-redundantie schema's zijn niet vereist. Het enkelvoudige IP-adres van de frontend wordt gelijktijdig bediend door meerdere onafhankelijke infrastructuur implementaties in meerdere Beschikbaarheidszones.  Zone-redundant houdt in dat alle binnenkomende of uitgaande stromen door meerdere Beschikbaarheidszones in een regio tegelijk worden bediend met één IP-adres.

Een of meer Beschikbaarheidszones kunnen mislukken en het gegevenspad blijft in de tijd dat de ene zone in de regio in orde is. Zone-redundante configuratie is de standaard instelling en vereist geen extra acties.  

Gebruik het volgende script om een zone-redundant openbaar IP-adres te maken voor uw interne Standard Load Balancer. Als u bestaande resource manager-sjablonen in uw configuratie gebruikt, voegt u de sectie **SKU** toe aan deze sjablonen.

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

Gebruik het volgende script om een zone-redundant frontend-IP-adres te maken voor uw interne Standard Load Balancer. Als u bestaande resource manager-sjablonen in uw configuratie gebruikt, voegt u de sectie **SKU** toe aan deze sjablonen.

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

#### <a name="optional-zone-isolation"></a>Optionele zone isolatie

U kunt ervoor kiezen een front-end te garanderen voor één zone, die wordt aangeduid als een zonegebonden-front- *End*.  Dit betekent dat elke binnenkomende of uitgaande stroom wordt bediend door één zone in een regio.  Uw front-end-shares worden meegezonden met de status van de zone.  Het gegevenspad wordt niet beïnvloed door storingen in andere zones dan waar het is gegarandeerd. U kunt zonegebonden-frontends gebruiken om een IP-adres per beschikbaarheids zone beschikbaar te maken.  Daarnaast kunt u zonegebonden-front-ends rechtstreeks gebruiken of, wanneer de front-end bestaat uit open bare IP-adressen, deze integreren met een product met een DNS-taak verdeling, zoals [Traffic Manager](../traffic-manager/traffic-manager-overview.md) en één DNS-naam gebruiken, die door een client wordt omgezet naar meerdere zonegebonden IP-adressen. .  U kunt dit ook gebruiken voor het weer geven van eind punten met gelijke taak verdeling voor elke zone afzonderlijk te bewaken.  Als u deze concepten (zone-redundante en zonegebonden voor dezelfde back-end) wilt laten door lopen, bekijkt u [meerdere frontends voor Azure Load Balancer](load-balancer-multivip-overview.md).

Voor een open bare Load Balancer frontend voegt u een para meter *zones* toe aan de open bare IP waarnaar wordt verwezen door de frontend-IP-configuratie.  

Voor een interne Load Balancer frontend voegt u een para meter *zones* toe aan de interne Load Balancer frontend-IP-configuratie. De zonegebonden-frontend zorgt ervoor dat de Load Balancer een IP-adres in een subnet naar een specifieke zone te garanderen.

Gebruik het volgende script om een openbaar IP-adres voor zonegebonden-standaard te maken in Beschik baarheid Zone 1. Als u bestaande resource manager-sjablonen in uw configuratie gebruikt, voegt u de sectie **SKU** toe aan deze sjablonen.

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

Gebruik het volgende script om een interne Standard Load Balancer front-end in Beschik baarheid te maken Zone 1.

Als u bestaande resource manager-sjablonen in uw configuratie gebruikt, voegt u de sectie **SKU** toe aan deze sjablonen. Definieer ook de **zone** -eigenschap in de front-end-IP-configuratie voor de onderliggende bron.

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

### <a name="cross-zone-load-balancing"></a>Taak verdeling in meerdere zones

Taak verdeling tussen meerdere zones is de mogelijkheid van Load Balancer om een back-end-eind punt in een zone te bereiken en is onafhankelijk van front-end en de bijbehorende zonality.

Als u uw implementatie binnen één zone wilt afstemmen en garanderen, moet u zonegebonden front-end-en zonegebonden-back-end-bronnen in dezelfde zone uitlijnen. Er is geen verdere actie vereist.

### <a name="backend"></a>Back-end

Load Balancer werkt met Virtual Machines.  Elke virtuele machine in één VNet kan deel uitmaken van de back-end-groep, ongeacht of deze al dan niet is gegarandeerd voor een zone of waarop de zone is gegarandeerd.

Als u uw front-end en back-end met één zone wilt afstemmen en garanderen, plaatst u alleen Vm's binnen dezelfde zone in de bijbehorende back-end-pool.

Als u virtuele machines wilt adresseren over meerdere zones, plaatst u Vm's van meerdere zones in dezelfde back-endadresgroep.  Wanneer u schaal sets voor virtuele machines gebruikt, kunt u een of meer virtuele-machine schaal sets in dezelfde back-end-groep plaatsen.  En elk van deze schaal sets voor virtuele machines kunnen zich in één of meerdere zones bevinden.

### <a name="outbound-connections"></a>Uitgaande verbindingen

[Uitgaande verbindingen](load-balancer-outbound-connections.md) worden door alle zones bediend en worden automatisch zone redundant in een regio met Beschikbaarheidszones wanneer een virtuele machine is gekoppeld aan een open bare Load Balancer en een zone-redundante front-end.  Binnenkomende poort toewijzingen voor verbinding met SNAT blijven zone storingen.  

Als de virtuele machine is gekoppeld aan een openbaar Load Balancer en een zonegebonden-front-end, worden uitgaande verbindingen gegarandeerd door één zone.  Uitgaande verbindingen delen verspreiding met de status van de respectieve zone.

De toewijzings-en-algoritme voor de SNAT-poort is hetzelfde als met of zonder zones.

### <a name="health-probes"></a>Statuscontroles

De bestaande definities voor de Health-tests blijven behouden, zonder Beschikbaarheidszones.  Maar we hebben het status model op een infrastructuur niveau uitgebreid. 

Wanneer u gebruikmaakt van zone-redundante front-ends, wordt Load Balancer uitbrei ding van het interne status model om de bereik baarheid van een virtuele machine onafhankelijk te testen vanuit elke beschikbaarheids zone en om de paden af te sluiten in zones die mogelijk zijn mislukt zonder tussen komst van de klant.  Als een gegeven pad niet beschikbaar is in de Load Balancer-infra structuur van een zone naar een virtuele machine in een andere zone, kan Load Balancer deze fout detecteren en voor komen. Andere zones die deze VM kunnen bereiken, kunnen de virtuele machine blijven gebruiken vanaf hun respectieve frontends.  Als gevolg hiervan is het mogelijk dat tijdens de fout gebeurtenissen elke zone enigszins verschillende stroom distributies heeft en de algehele status van uw end-to-end-service beveiligt.

## <a name="design"></a>Ontwerp overwegingen

Load Balancer is in de context van Beschikbaarheidszones doel bewust flexibel. U kunt uitlijnen op zones of u kunt kiezen om zone-redundant te maken.  Verhoogde Beschik baarheid is de prijs van verhoogde complexiteit en u moet ontwerpen voor de beschik baarheid van optimale prestaties.  Laten we eens kijken naar enkele belang rijke aandachtspunten voor het ontwerp.

### <a name="automatic-zone-redundancy"></a>Automatische zone-redundantie

Load Balancer maakt het eenvoudig om één IP-adres te hebben als een zone-redundante front-end. Een zone-redundant IP-adres kan veilig worden gebruikt voor een zonegebonden-bron in elke zone en er kunnen een of meer zone fouten optreden, zolang de ene zone in orde blijft binnen de regio. Daarentegen is een zonegebonden-front-end een verlaging van de service naar een enkele zone en deelt het verspreidings niveau met de betreffende zone.

Zone-redundantie impliceert niet Hitless DataPath of Control-vlak;  het is uitdrukkelijk gegevens vlak. Zone-redundante stromen kunnen alle zones en de stromen van een klant gebruiken om alle zones in een regio te gebruiken. In het geval van een zone storing worden de verkeers stromen die op dat moment in goede zones worden gebruikt, niet beïnvloed.  Verkeers stromen die gebruikmaken van een zone op het moment van de zone storing, kunnen worden beïnvloed, maar toepassingen kunnen worden hersteld. Deze stromen kunnen in de resterende gezonde zones binnen de regio worden voortgezet na het opnieuw verzenden of opnieuw instellen, zodra Azure is geconvergeerd rond de zone storing.

### <a name="xzonedesign"></a>Grenzen van meerdere zones

Het is belang rijk om te begrijpen dat elke keer dat een end-to-end-service zones overschrijdt, u verspreiding deelt met niet één zone, maar mogelijk meerdere zones.  Als gevolg hiervan is het mogelijk dat uw end-to-end-service geen Beschik baarheid heeft verkregen via niet-zonegebonden-implementaties.

Vermijd onbedoelde afhankelijkheden voor meerdere zones, waardoor de beschik baarheid wordt gerealiseerd bij het gebruik van Beschikbaarheidszones.  Als uw toepassing bestaat uit meerdere onderdelen en u zich wilt voordoen aan een zone storing, moet u ervoor zorgen dat u voldoende essentiële onderdelen in het geval van een mislukte zone overhoudt.  Eén belang rijk onderdeel van uw toepassing kan bijvoorbeeld van invloed zijn op uw hele toepassing als deze alleen voor komt in een andere zone dan de nagelaten zone (s).  Daarnaast moet u ook rekening houden met het herstel van de zone en hoe uw toepassing convergeert. Laten we eens kijken naar enkele belang rijke punten en gebruiken ze als inspiratie punt voor vragen zoals u in uw specifieke scenario kunt zien.

- Als uw toepassing twee onderdelen heeft, zoals een IP-adres en een virtuele machine met beheerde schijf, en deze in zone 1 en zone 2 worden gegarandeerd, wordt de end-to-end-service niet overstaan wanneer zone 1 mislukt.  U hoeft geen zones te maken, tenzij u volledig begrijpt dat u een potentieel gevaarlijke fout modus maakt.

- Als uw toepassing twee onderdelen heeft, zoals een IP-adres en een virtuele machine met een beheerde schijf, en deze zijn gegarandeerd zone-redundante en zone 1, blijft uw end-to-end-service zone storingen van zone 2, zone 3, of beide, tenzij zone 1 is mislukt.  U verliest echter een mogelijke reden om de status van uw service te controleren als u er zeker van bent dat u de beschik baarheid van de front-end kunt bereiken.  Overweeg het ontwikkelen van een uitgebreidere status-en capaciteits model.  U kunt zone-redundante en zonegebonden-concepten samen gebruiken om inzicht en beheer baarheid uit te breiden.

- Als uw toepassing twee onderdelen heeft, zoals een zone-redundante Load Balancer frontend en een schaalset in meerdere zones in drie zones, zijn uw resources in zones die niet van invloed zijn op de fout, beschikbaar, maar kan uw end-to-end-service capaciteit worden verslechterd tijdens een zone fout. In het perspectief van een infra structuur kan uw implementatie een of meer zone storingen overleven, en dit leidt tot de volgende vragen:
  - Begrijpt u hoe de oorzaken van uw toepassingen van toepassing zijn op dergelijke fouten en de capaciteit van het bedrijf?
  - Moet u in uw service veiligheids maatregelen hebben om zo nodig een failover naar een regio paar af te dwingen?
  - Hoe kunt u een dergelijk scenario bewaken, detecteren en beperken? U kunt Standard Load Balancer diagnostische gegevens gebruiken om de bewaking van uw end-to-end-service prestaties te verbeteren. Bedenk wat er beschikbaar is en wat mogelijk moet worden uitgebreid voor een volledige foto.

- Zones kunnen fouten gemakkelijker te begrijpen en bevatten.  Zones mislukken echter niet anders dan andere fouten wanneer het gaat om concepten zoals time-outs, nieuwe pogingen en uitstel-algoritmen. Hoewel Azure Load Balancer voorziet in zone-redundante paden en probeert snel, op pakket niveau in realtime te herstellen, kunnen er tijdens het begin van een storing opnieuw worden verzonden of hersteld. het is belang rijk om te begrijpen hoe uw toepassing zich kan richten op Masters. Uw taakverdelings schema blijft bewaard, maar u moet het volgende plannen:
  - Als er een storing optreedt in een zone, begrijpt uw end-to-end-service dit en als de status verloren gaat, hoe kunt u deze herstellen?
  - Wanneer een zone wordt geretourneerd, weet uw toepassing hoe u veilig kunt convergeren?

### <a name="zonalityguidance"></a>Zone-redundant versus zonegebonden

>[!IMPORTANT]
>Bekijk [Beschikbaarheidszones](../availability-zones/az-overview.md) voor Verwante onderwerpen, inclusief alle specifieke informatie over de regio.

Zone-redundante kan een zone-neutraal bieden en tegelijkertijd de optie met één IP-adres voor de service.  Dit kan de complexiteit van de beurt verminderen.  Zone-redundante heeft ook mobiliteit over zones en kan veilig worden gebruikt voor bronnen in elke zone.  Het is ook een toekomstige proef versie in regio's zonder Beschikbaarheidszones, waardoor de vereiste wijzigingen kunnen worden beperkt zodra een regio Beschikbaarheidszones.  De configuratie syntaxis voor een zone-redundant IP-adres of frontend slaagt in een wille keurige regio, inclusief die zonder Beschikbaarheidszones.

Zonegebonden kan een expliciete garantie voor een zone bieden, het delen van verspreiding met de status van de zone. Het koppelen van een zonegebonden IP-adres of zonegebonden Load Balancer frontend kan een wenselijk of redelijk kenmerk zijn, met name als uw gekoppelde bron een zonegebonden-VM in dezelfde zone is.  Het is ook mogelijk dat uw toepassing expliciete kennis vereist over de zone waarin een resource zich bevindt en dat u de beschik baarheid in afzonderlijke zones expliciet wilt controleren.  U kunt ervoor kiezen om meerdere zonegebonden-frontends beschikbaar te maken voor een end-to-end-service die wordt gedistribueerd over zones (dat wil zeggen, per zone zonegebonden-front-end voor meerdere zonegebonden virtuele-machine schaal sets).  En als uw zonegebonden-front-end een openbaar IP-adres zijn, kunt u deze meerdere zonegebonden-front-ends gebruiken om uw service met [Traffic Manager](../traffic-manager/traffic-manager-overview.md)aan te bieden.  U kunt ook meerdere zonegebonden-frontends gebruiken voor het verkrijgen van informatie over zone status en prestaties met behulp van bewakings oplossingen van derden en de algehele service bieden met een zone-redundante front-end. U moet alleen zonegebonden-resources met zonegebonden-front-end-toepassingen die zijn afgestemd op dezelfde zone, en potentieel schadelijke cross-zone scenario's voor zonegebonden bronnen vermijden.  Zonegebonden-resources bestaan alleen in regio's waar Beschikbaarheidszones bestaan.

Er zijn geen algemene richt lijnen die een betere keuze is dan de andere, zonder de service architectuur te weten.

## <a name="limitations"></a>Beperkingen

- Terwijl het gegevens vlak volledig zone-redundant is (tenzij zonegebonden guarantee is opgegeven), zijn besturings vlak bewerkingen niet volledig zone-redundant.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Beschikbaarheidszones](../availability-zones/az-overview.md)
- Meer informatie over [Standard Load Balancer](load-balancer-standard-overview.md)
- Meer informatie over het [laden van virtuele machines binnen een zone met behulp van een Standard Load Balancer met een zonegebonden](load-balancer-standard-public-zonal-cli.md) -front-end
- Meer informatie over het verdelen [van virtuele machines over zones met behulp van een Standard Load Balancer met een zone-redundante front-end](load-balancer-standard-public-zone-redundant-cli.md)
