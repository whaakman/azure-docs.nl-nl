---
title: Azure Standard Load Balancer en beschikbaarheid Zones | Microsoft Docs
description: Standaard Load Balancer en beschikbaarheid Zones
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2018
ms.author: kumud
ms.openlocfilehash: 70b39b854a3b7cb28716d3cb290998690dbeb549
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="standard-load-balancer-and-availability-zones"></a>Standaard Load Balancer en beschikbaarheid Zones

Azure Load Balancer standaard SKU ondersteunt [beschikbaarheid Zones](../availability-zones/az-overview.md) scenario's. Verschillende nieuwe concepten zijn beschikbaar met standaard Load Balancer, waarmee u kunt de beschikbaarheid in uw scenario end-to-end optimaliseren door het uitlijnen van resources met zones, evenals deze verdelen over zones.  Bekijk [beschikbaarheid Zones](../availability-zones/az-overview.md) voor hulp bij het wat beschikbaarheid Zones zijn, welke regio's wordt momenteel ondersteund beschikbaarheid Zones en andere verwante concepten en producten. Beschikbaarheid Zones in combinatie met standaard Load Balancer is een uitgebreide en flexibele functieset die veel verschillende scenario's kunt maken.  Lees dit document te begrijpen [concepten](#concepts) en fundamentele scenario [richtlijnen ontwerpen](#design).

>[!NOTE]
> De Load Balancer standaard SKU is momenteel in preview. Tijdens de preview, de functie hebben mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie. Gebruik de algemeen beschikbaar [Load Balancer basis-SKU](load-balancer-overview.md) voor uw productie-services. Om te gebruiken [beschikbaarheid Zones Preview](https://aka.ms/availabilityzones) met deze Preview vereist een [afzonderlijke aanmelding](https://aka.ms/availabilityzones), naast het aanmelden voor een Load Balancer [standaard preview](#preview-sign-up).

## <a name="concepts"></a> Beschikbaarheid Zones concepten die worden toegepast met Load Balancer

Er is geen directe relatie tussen resources van de Load Balancer en werkelijke infrastructuur; maken van een Load Balancer maken geen exemplaar. Load Balancer resources zijn waarbinnen u hoe de vooraf gedefinieerde multitenant-infrastructuur voor het scenario dat u wilt maken op Azure moet programma kunt express-objecten.  Dit is van belang is in de context van beschikbaarheid Zones omdat één Load Balancer resource programmeren van infrastructuur in meerdere Zones van de beschikbaarheid bepalen kunt tijdens een zone-redundante service wordt weergegeven als één resource vanuit het oogpunt van een klant.

Functies van de bron van een Load Balancer worden uitgedrukt als een frontend, een regel, een health test en de definitie van een back-end-adresgroep.

Het gedrag en de eigenschappen van een Load Balancer-resource in de context van beschikbaarheid Zones worden als zone-redundante of zonal beschreven.  Zone-redundante beschrijft zonal de zonality van een eigenschap.  In de context van de Load Balancer, zone-redundante altijd betekent *alle zones* en zonal middelen garanderen van de service een *enkele zone*.

Openbare en interne Load Balancer zone-redundante en zonal scenario's ondersteunen en beide kunnen verkeer omleiden via zones naar behoefte (*cross-zone taakverdeling*).

Een Load Balancer-resource zelf is regionale en nooit zonal.  En een VNet en het subnet zijn altijd regionale en nooit zonal.

### <a name="frontend"></a>Front-end

Een Load Balancer-frontend is een Frontend-IP-adresconfiguratie verwijst naar een openbare IP-adres resource of een particulier IP-adres binnen het subnet van een resource van het virtuele netwerk.  Vormt het eindpunt taakverdeling waar uw service wordt weergegeven.

Een Load Balancer-resource kan tegelijk zonal en zone-redundante frontends bevatten.

Wanneer een openbare IP-resource heeft is gegarandeerd aan een zone, is de zonality (of ontbreken daarvan) geen veranderlijke.  Als u wilt wijzigen of laat de zonality van een openbare IP-frontend, moet u het openbare IP-adres in de juiste zone opnieuw.  

U kunt de zonality van een frontend van een interne Load Balancer wijzigen door te verwijderen en opnieuw maken van de frontend, wijzigen of als de zonality wordt weggelaten.

Wanneer u meerdere frontends, Bekijk [meerdere frontends voor de Load Balancer](load-balancer-multivip-overview.md) voor belangrijke tips hierbij.

#### <a name="zone-redundant-by-default"></a>Zoneredundant standaard

Een standaard Load Balancer-frontend is in een regio met beschikbaarheid Zones, zone-redundante standaard.  Een enkele frontend-IP-adres zone kan storingen en kan worden gebruikt voor het bereiken van alle back-end-groepsleden ongeacht de zone. Dit betekent niet hitless gegevenspad, maar alle nieuwe pogingen of reestablishment slaagt. DNS-redundantie schema's zijn niet vereist. De frontend één IP-adres wordt tegelijkertijd door onafhankelijke infrastructuur implementaties in elke Zone beschikbaarheid behandeld.  Zone-redundante betekent dat alle binnenkomende of uitgaande stromen door alle Zones voor beschikbaarheid in een regio die gelijktijdig met één IP-adres worden behandeld.

Een of meer beschikbaarheid Zones kan mislukken en het gegevenspad blijft zo lang als één zone in de regio blijft in orde is. Zone-redundante configuratie is de standaardinstelling en vereist geen aanvullende acties.  Wanneer een regio de mogelijkheid krijgt te ondersteunen van de Zones van de beschikbaarheid, wordt een bestaande frontend zone-redundante automatisch.

Gebruik het volgende script te maken van een zone-redundante openbare IP-adres voor uw interne standaard Load Balancer. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie tot deze sjablonen.

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

Gebruik het volgende script te maken van een zone-redundante frontend-IP-adres voor uw interne standaard Load Balancer. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie tot deze sjablonen.

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

U kunt een frontend gegarandeerd dat een enkele zone hebt waarvan bekend is als een *zonal frontend*.  Dit betekent dat elk binnenkomend of uitgaand stroom wordt geleverd door een enkele zone in een regio.  Uw frontend deelt gedrag met de status van de zone.  Het gegevenspad wordt niet beïnvloed door fouten in zones dan waar deze is gegarandeerd. U kunt zonal frontends gebruiken om een IP-adres per beschikbaarheid Zone weer te geven.  Bovendien kunt u rechtstreeks zonal frontends gebruiken of, wanneer de frontend uit openbare IP-adressen bestaat, moet u ze integreren in een DNS-taakverdeling product zoals [Traffic Manager](../traffic-manager/traffic-manager-overview.md) en gebruik één DNS-naam, die een client wordt omgezet in meerdere zonal IP-adressen.  U kunt dit ook gebruiken om per zone taakverdeling eindpunten voor het bewaken van elke zone afzonderlijk weer te geven.  Als u wilt overgaan van deze concepten (zone-redundante en zonal voor dezelfde back-end), raadpleegt u [meerdere frontends voor Azure Load Balancer](/load-balancer-multivip-overview.md).

Een openbare Load Balancer-frontend, voegt u toe een *zones* -parameter voor het openbare IP-adres waarnaar wordt verwezen door de frontend-IP-configuratie.  

Voor een interne Load Balancer-frontend, voegt u een *zones* -parameter voor de interne Load Balancer frontend-IP-configuratie. De zonal frontend zorgt ervoor dat de Load Balancer garanderen van een IP-adres in een subnet aan een specifieke zone.

Gebruik het volgende script te maken van een zonal standaard openbare IP-adres in beschikbaarheid Zone 1. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie tot deze sjablonen.

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

Gebruik het volgende script te maken van een interne standaard Load Balancer-front-end in beschikbaarheid Zone 1.

Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie tot deze sjablonen. Bepaal ook de **zones** eigenschap in de frontend-IP-configuratie voor de onderliggende resource.

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

### <a name="cross-zone-load-balancing"></a>Cross-zone voor taakverdeling

Taakverdeling cross-zone, is de mogelijkheid van de Load Balancer een back-end-eindpunt in een zone te bereiken en is onafhankelijk van de front-end- en de zonality.

Als u wilt uitlijnen en garanderen van uw implementatie binnen een enkele zone, uitlijnen zonal frontend en backend zonal resources aan de zone met dezelfde. Er is geen verdere actie vereist.

### <a name="backend"></a>Back-end

Load Balancer werkt met virtuele Machines.  Een virtuele machine in een enkel VNet kan deel uitmaken van de back-endpool ongeacht of deze is gegarandeerd aan een zone of welke zone is gegarandeerd.

Als u wilt uitlijnen en garanderen van uw frontend en backend met een enkele zone, plaats alleen voor virtuele machines binnen dezelfde zone in de betreffende back-endpool.

Als u te adres VM's via meerdere zones wenst, plaatsen gewoon voor virtuele machines in meerdere zones aan dezelfde back endpool.  Wanneer met behulp van de virtuele-machineschaalset wordt ingesteld, kunt u een of meer virtuele-machineschaalsets plaatsen in dezelfde endadresgroep back.  En mag elk van deze virtuele-machineschaalsets in één of meerdere zones.

### <a name="outbound-connections"></a>Uitgaande verbindingen

[Uitgaande verbindingen](load-balancer-outbound-connections.md) door alle zones worden behandeld en in een regio met beschikbaarheid Zones automatisch zone-redundante zijn wanneer een virtuele machine gekoppeld aan een openbare Load Balancer en een zone-redundante frontend is.  Uitgaande verbinding snat omzetten poort toewijzingen overleven zone fouten.  

Als de virtuele machine gekoppeld aan een openbare Load Balancer en een zonal frontend is, zijn op zijn beurt uitgaande verbindingen om te worden geleverd door een enkele zone gegarandeerd.  Uitgaande verbindingen delen gedrag met de status van de desbetreffende zone.

Het voorafgaande toewijzing snat omzetten-poort en de algoritme is dezelfde met of zonder zones.

### <a name="health-probes"></a>Statuscontroles

Uw bestaande health test definities ongewijzigd blijven zonder beschikbaarheid Zones.  Maar we het statusmodel op het niveau van infrastructuur hebt uitgebreid. 

Wanneer u zone-redundante uitbreiding frontends, Load Balancer van de interne statusmodel onafhankelijk test de bereikbaarheid van een virtuele machine uit elke Zone beschikbaarheid en paden afsluiten in zones die mogelijk zonder tussenkomst van de klant is mislukt.  Als een opgegeven pad niet beschikbaar is via de Load Balancer-infrastructuur van een zone voor een virtuele machine in een andere zone is, Load Balancer detecteren en te voorkomen dat deze fout. Andere zones die toegang heeft tot deze virtuele machine kunnen blijven voldoen aan de VM van hun respectieve frontends.  Als gevolg hiervan is het mogelijk dat tijdens de mislukte gebeurtenissen, elke zone mogelijk enigszins andere stroom distributies bij het beveiligen van de algemene status van uw service end-to-end.

## <a name="design"></a> Overwegingen bij het ontwerpen

Load Balancer is opzettelijk flexibel in de context van beschikbaarheid Zones. U kunt uitlijnen met zones of kunt u zone-redundante worden.  Grotere beschikbaarheid kan worden geleverd voor de prijs van grotere complexiteit en moet u ontwerpen voor beschikbaarheid voor optimale prestaties.  Laten we op een aantal belangrijke ontwerpoverwegingen.

### <a name="automatic-zone-redundancy"></a>Automatische zone-redundantie

Load Balancer kunt u eenvoudig een enkel IP-adres als een zone-redundante frontend hebben. Een zone-redundante IP-adres een zonal resource in een zone kan veilig worden voorzien en kan een of meer zone fouten overleven zolang één zone in orde binnen de regio blijft. Als u daarentegen is een zonal frontend een vermindering van de service naar een enkele zone en shares gedrag met de desbetreffende zone.

Zone-redundantie betekent niet dat hitless gegevenspad of besturingselement vlak;  het is uitdrukkelijk vlak van gegevens. Zone-redundante stromen kunnen geen zones gebruiken en het gebruik van een klant stromen alle orde zones in een regio. In geval van storing van de zone, worden verkeersstromen, met goede zones op dat moment niet beïnvloed.  Verkeersstromen met behulp van een zone op het moment van de zone fout wordt mogelijk beïnvloed maar toepassingen kunnen herstellen en deze stromen in de resterende orde zones in het gebied van herverzending of reestablishment kunnen worden voortgezet wanneer Azure is geconvergeerd rond de zone-fout.

### <a name="xzonedesign"></a> Cross-zone grenzen

Het is belangrijk te weten dat elk gewenst moment een end-to-end-service bestrijkt zones, u gedrag met niet één zone, maar mogelijk meerdere zones delen.  Als gevolg hiervan uw end-to-end-service mogelijk niet hebben opgedaan met andere beschikbaarheid via niet-zonal implementaties.

Vermijd introducing onbedoelde cross-zone-afhankelijkheden die wordt ongeldig verklaard beschikbaarheid krijgt wanneer u de beschikbaarheid van Zones.  Wanneer uw toepassing bestaat uit meerdere onderdelen en u wilt netwerkfouten zone mislukt, moet u om te controleren of het overleven van voldoende belangrijke onderdelen in het geval van een zone mislukken behandelen.  Kritieke één component voor uw toepassing kan bijvoorbeeld gevolgen voor uw hele toepassing als deze alleen in een zone dan de overgebleven zones bestaat.  Bovendien ook rekening houden met het herstel van de zone en hoe doorgegeven aan uw toepassing wordt uitgevoerd. Laten we enkele belangrijke punten bekijken en ze gebruiken als inspiratie voor vragen als u denkt via uw specifieke scenario dat.

- Als uw toepassing bestaat uit twee onderdelen, zoals een IP-adres en een virtuele machine met beheerde schijf en ze zijn gegarandeerd in zone 1 en zone 2, wanneer de zone 1 mislukt uw end-to-end-service wordt niet blijven bewaard als zone 1 is mislukt.  Cross-zones niet als u volledig begrijpt dat u een potentieel gevaarlijke fout-modus maakt.

- Als uw toepassing twee onderdelen, heeft zoals een IP-adres en een virtuele machine met de schijf, beheerd en ze zijn gegarandeerd zone-redundante en 1 respectievelijk zone, uw end-to-end-service wordt storingen zone van de zone 2, zone 3 of beide tenzij zone 1 is mislukt.  Echter, verliest u enige mogelijkheid tot reden over de status van uw service als u worden geobserveerd de bereikbaarheid van de frontend.  U kunt een uitgebreidere gezondheid en capaciteit model te ontwikkelen.  U kunt mogelijk zone-redundante en zonal concepten samen gebruiken om uit te breiden inzicht en beheerbaarheid.

- Als uw toepassing bestaat uit twee onderdelen, zoals een zone-redundante Load Balancer-front- en een cross-zone virtuele-machineschaalset in drie zones, uw resources in zones niet beïnvloed door de fout is beschikbaar, maar uw end-to-end-service mogelijk een slechtere in termen van capaciteit tijdens zone-fout. Uw implementatie kan een of meer zone fouten overleven vanuit het perspectief van een infrastructuur, en dit genereert de volgende vragen:
  - Begrijpt u hoe uw toepassing redenen over deze fouten en verslechterde capaciteit?
  - Moet u de beveiliging van een failover naar een paar regio indien nodig in uw service om af te dwingen hebben?
  - Hoe wordt u bewaken, detecteren en het verminderen van dergelijk scenario? U kunt mogelijk met standaard Load Balancer diagnostics standaardverificatie bewaking van de serviceprestaties van uw end-to-end. Overweeg wat er beschikbaar is en wat kan uitdiepen nodig voor een volledig overzicht.

- Zones kunnen zorgen dat problemen eenvoudiger begrepen en opgenomen.  Zone-fout is echter niet anders dan andere fouten bij het concepten zoals time-outs, nieuwe pogingen en backoff algoritmen. Hoewel Azure Load Balancer zone-redundante paden biedt en probeert te herstellen snel op het pakketniveau van een in realtime herhaald verzenden of reestablishments zich kunnen voordoen tijdens het begin van een fout en is het belangrijk om te begrijpen hoe uw toepassing met copes fouten. Uw schema voor de taakverdeling wordt overleven, maar u moet plannen voor het volgende:
  - Wanneer een zone mislukt, uw service end-to-end Hiermee instemt en als de status verbroken wordt, hoe moet u herstellen?
  - Wanneer een zone wordt geactiveerd, uw toepassing te begrijpen hoe veilig convergeren?

### <a name="zonalityguidance"></a> Zone-redundante versus zonal

Zone-redundante bieden een zone-networkdirect en op dezelfde tijd robuuste optie met een enkel IP-adres voor de service.  Dit kan op zijn beurt complexiteit verminderen.  Zone-redundante ook heeft mobility diverse zones en veilig kan worden gebruikt voor bronnen in een zone.  Het is ook toekomstige bewijs in regio's zonder beschikbaarheid Zones die wijzigingen vereist wanneer een regio beschikbaarheid Zones krijgen kunt beperken.  De syntaxis van de configuratie voor een zone-redundante IP-adres of frontend slaagt in elke regio inclusief gebruikers met beschikbaarheid Zones.

Zonal kan bieden een expliciete garantie aan een zone gedrag delen met de status van de zone. Koppelen van een zonal IP kan-adres of zonal Load Balancer-frontend worden een kenmerk wenselijk of redelijke met name als uw gekoppelde resource een zonal VM in dezelfde regio is.  Of misschien expliciete kennis over welke zone een resource bevindt zich in uw toepassing is vereist en u expliciet reden over beschikbaarheid in afzonderlijke zones.  U kunt kiezen om meerdere zonal frontends voor een end-to-end-service verdeeld over zones weer te geven (dat wil zeggen, per zone zonal frontends voor meerdere zonal virtuele-machineschaalset stelt).  Als uw zonal frontends openbare IP-adressen, kunt u deze meerdere zonal frontends gebruiken voor het blootstellen van uw service met [Traffic Manager](../traffic-manager/traffic-manager-overview.md).  Of u kunt meerdere zonal frontends per zone status en prestaties insights via van derden bewakingsoplossingen krijgen en de algehele service met een zone-redundante frontend weer te geven. U moet alleen zonal resources met zonal frontends uitgelijnd op dezelfde zone leveren en te voorkomen dat schadelijke cross-zone-scenario's voor zonal bronnen.  Er bestaan alleen zonal resources in regio's waar beschikbaarheid Zones bestaan.

Er is geen algemene richtlijnen dat een een betere keuze dan de andere is zonder te weten van de service-architectuur.

## <a name="limitations"></a>Beperkingen

- Gegevens terwijl vlak is volledig zone-redundante (tenzij zonal garantie is opgegeven), vlak beheerbewerkingen zijn niet volledig zone-redundante.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [beschikbaarheid Zones](../availability-zones/az-overview.md)
- Meer informatie over [standaard Load Balancer](load-balancer-standard-overview.md)
