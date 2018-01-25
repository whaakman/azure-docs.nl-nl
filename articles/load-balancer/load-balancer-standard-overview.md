---
title: Overzicht van Azure Load Balancer standaard | Microsoft Docs
description: Overzicht van Azure Load Balancer Standard-functies
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/28/2017
ms.author: kumud
ms.openlocfilehash: ddcbe895bdaa6eaa49e8ed129fe92b415f2600ef
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="azure-load-balancer-standard-overview-preview"></a>Overzicht van Azure Load Balancer-standaard (preview)

De Azure Load Balancer standaard SKU en openbare IP-standaard SKU kunt samen u voor het bouwen van uiterst schaalbare en betrouwbare architecturen. Toepassingen die gebruikmaken van Load Balancer Standard kunnen profiteren van nieuwe mogelijkheden. Lage latentie en hoge doorvoersnelheid schaal zijn beschikbaar voor miljoenen stromen voor alle TCP en UDP-toepassingen.

>[!NOTE]
> De Load Balancer standaard SKU is momenteel in preview. Tijdens de preview, de functie hebben mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie. Gebruik de algemeen beschikbaar [Load Balancer basis-SKU](load-balancer-overview.md) voor uw productie-services. De functies die gekoppeld aan deze preview zijn [beschikbaarheid Zones](https://aka.ms/availabilityzones), en [HA poorten](https://aka.ms/haports), afzonderlijke aanmelding op dit moment vereisen. Volg de instructies respectieve voor registratie van deze functies, naast het aanmelden voor een Load Balancer [standaard preview](#preview-sign-up).

## <a name="why-use-load-balancer-standard"></a>Waarom gebruiken Load Balancer standaard?

U kunt het Load Balancer standaard gebruiken voor het volledige bereik van virtuele-datacenters. Op kleine schaal implementaties voor grotere, complexe meerdere zone architecturen, u kunt vanaf Load Balancer standaard om te profiteren van de volgende mogelijkheden:

- [Enterprise scale](#enterprisescale) met Load Balancer Standard kan worden bereikt. Deze functie kan worden gebruikt met een willekeurig exemplaar van de virtuele machine (VM) binnen een virtueel netwerk, maximaal 1000 VM-exemplaren.

- [Nieuwe diagnostische insights](#diagnosticinsights) zijn beschikbaar om te begrijpen en oplossen van dit essentieel onderdeel van uw virtuele Datacenter beheren. Met Azure-Monitor (preview) kunt weergeven, gefilterd en gegroepeerd nieuwe multidimensionale metrische gegevens voor de doorlopende gegevens pad health metingen. Uw gegevens uit een front-virtuele machine, eindpunt statuscontroles, voor TCP-verbindingspogingen, en uitgaande verbindingen bewaken.

- [Netwerkbeveiligingsgroepen](#nsg) zijn nu vereist voor een VM-instantie die is gekoppeld aan de Load Balancer standaard SKU's of openbare IP-standaard SKU's. Netwerkbeveiligingsgroepen (nsg's) een betere beveiliging voor uw scenario.

- [Hoge beschikbaarheid (HA)-poorten bieden hoge betrouwbaarheid](#highreliability) en schaal voor de virtuele netwerkapparaten (NVAs) en andere scenario's van toepassing. HA poorten taakverdeling maken voor alle poorten op een Azure interne Load Balancer (ILB) front-end voor een pool van VM-exemplaren.

- [Uitgaande verbindingen](#outboundconnections) nu een nieuwe bron Network Address Translation (snat omzetten) poort toewijzing model waarmee groter tolerantie en schaal gebruiken.

- [Load Balancer Standard met beschikbaarheid Zones](#availabilityzones) kan worden gebruikt voor het maken van de zone-redundante en zonal architecturen. Beide deze architecturen kunnen opnemen taakverdeling cross-zone. U kunt bereiken zone redundantie zonder afhankelijkheid van DNS-records. Er is een enkel IP-adres zone-redundante standaard.  Een enkel IP-adres kan een virtuele machine in een virtueel netwerk in een regio die op alle Zones voor beschikbaarheid bereiken.


U kunt Load Balancer standaard een gebruiken in een openbare of interne configuratie ter ondersteuning van de volgende belangrijke scenario's:

- Taakverdeling maken voor binnenkomend verkeer naar goede back-end-exemplaren.
- Poort forward binnenkomend verkeer op slechts één exemplaar van de back-end.
- Uitgaand verkeer van een particuliere IP-adres in het virtuele netwerk naar een openbare IP-adres vertalen.

### <a name = "enterprisescale"></a>Schaal van de onderneming

 Load Balancer standaard gebruiken voor het ontwerpen van uw virtuele datacenter met hoge prestaties en ondersteuning voor elke TCP of UDP-toepassing. Gebruik zelfstandige VM-exemplaren of maximaal 1000 exemplaren van virtuele-machineschaalset die is ingesteld in een back-end-pool. Blijven doorsturen van de lage latentie, hoge doorvoersnelheid prestaties en schalen naar miljoenen stromen voor een volledig beheerde Azure-service gebruiken.
 
Load Balancer standaard kan verkeer naar een VM-instantie in een virtueel netwerk in een regio doorsturen. Grootten van de back-end-pool kunnen maximaal 1000 exemplaren met elke combinatie van de volgende scenario's voor VM zijn:

- Zelfstandige virtuele machines zonder beschikbaarheidssets
- Zelfstandige virtuele machines met beschikbaarheidssets
- Virtuele-machineschaalset wordt ingesteld, maximaal 1000 exemplaren
- meerdere virtuele-machineschaalsets
- Mengsels van VM's en virtuele-machineschaalsets

Er is niet langer een vereiste voor beschikbaarheidssets. U kunt met beschikbaarheidssets voor andere voordelen die ze bieden.

### <a name = "diagnosticinsights"></a>Diagnostische insights

Load Balancer Standard biedt nieuwe mogelijkheden voor meerdere dimensies diagnostische voor openbare en interne Load Balancer-configuraties. Deze nieuwe metrische gegevens worden geleverd via Azure Monitor (preview) en alle gerelateerde mogelijkheden, inclusief de mogelijkheid te integreren met downstream consumenten gebruiken.

| Gegevens | Beschrijving |
| --- | --- |
| VIP-beschikbaarheid | Load Balancer standaard oefent continu het gegevenspad van binnen een regio aan de Load Balancer-front-helemaal tot aan de stack SDN die ondersteuning biedt voor uw virtuele machine. Als in orde exemplaren blijft, volgt de meting hetzelfde pad als uw toepassing taakverdeling verkeer. Het gegevenspad die wordt gebruikt door uw klanten ook wordt gevalideerd. De meting is onzichtbaar voor uw toepassing en niet van invloed op een andere bewerkingen.|
| Beschikbaarheid van de DIP | Load Balancer standaard maakt gebruik van een gedistribueerde health-service die u de status van het toepassingseindpunt van uw volgens de configuratie-instellingen bewaakt te zoeken. Met deze metriek zorgt voor een statistische functie of groep per eindpunt gefilterd-weergave van elk eindpunt afzonderlijke exemplaar in de Load Balancer.  U kunt zien hoe de Load Balancer de status van uw toepassing, zoals aangegeven door uw configuratie van health test bekijkt.
| SYN-pakketten | Load Balancer standaard niet beëindigen TCP-verbindingen of interactie met TCP of UDP pakketstromen. Stromen en hun handshakes zijn altijd tussen de bron en de VM-instantie. Als u wilt uw TCP-protocol scenario's op te lossen, kunt u het gebruik van SYN pakketten items om te begrijpen hoeveel TCP-verbinding is geprobeerd. De metriek rapporteert het aantal TCP SYN-pakketten die zijn ontvangen.|
| Verbindingen met snat omzetten | Load Balancer standaard rapporteert het aantal uitgaande stromen die op de front-openbare IP-adres zijn masqueraded. Snat omzetten poorten zijn een onuitputtelijk resource. Met deze metriek kunt geven een indicatie van hoe zwaar uw toepassing is vertrouwen op snat omzetten voor uitgaande oorsprong stromen.  Tellers voor geslaagde en mislukte uitgaande snat omzetten stromen worden gerapporteerd, en kunnen worden gebruikt om problemen op te begrijpen van de status van uw uitgaande stromen.|
| Byte-prestatiemeteritems | Load Balancer standaard rapporten de gegevens per front-end verwerkt.|
| Pakket-prestatiemeteritems | Load Balancer standaard rapporteert de pakketten dat per front-end verwerkt.|

### <a name = "highreliability"></a>Hoge betrouwbaarheid

Configureer regels voor het maken van de schaal van uw toepassing en maximaal betrouwbaar voor taakverdeling. U kunt regels voor afzonderlijke poorten configureren of kunt u HA poorten gebruiken om alle verkeer, ongeacht het TCP- of UDP-poortnummer in balans.  

U kunt de nieuwe functie van de HA-poorten gebruiken voor het ontgrendelen van allerlei scenario's voor hoge beschikbaarheid en schaalbaarheid voor interne NVAs. De functie is handig voor andere scenario's waarin het niet handig of ongewenste als afzonderlijke poorten wilt opgeven. HA poorten bieden redundantie en schaal doordat zoveel exemplaren als u nodig hebt. De configuratie is niet langer beperkt tot actief/passief-scenario's. Uw service beveiligen uw health test configuraties door het doorsturen van verkeer alleen op in orde exemplaren.

NVA leveranciers bieden volledig door de leverancier ondersteund, robuuste scenario's voor hun klanten. De storingspunt wordt verwijderd en meerdere actieve exemplaren worden ondersteund voor schaal. U kunt schalen op twee of meer exemplaren, afhankelijk van de mogelijkheden van uw apparaat. Neem contact op met de leverancier van uw NVA voor meer informatie over deze scenario's.

### <a name = "availabilityzones"></a>Beschikbaarheid zones

[!INCLUDE [availability-zones-preview-statement](../../includes/availability-zones-preview-statement.md)]

Gaan de tolerantie van uw toepassing met het gebruik van beschikbaarheid Zones in ondersteunde regio's. Beschikbaarheid Zones zijn momenteel in preview in specifieke gebieden en aanvullende opt-in vereist.

### <a name="automatic-zone-redundancy"></a>Automatische zone-redundantie

U kunt kiezen of Load Balancer een zone-redundante of zonal front-voor elke toepassing leveren moet. Het is gemakkelijk zone-om redundantie tot stand met Load Balancer Standard. Er is één front-end-IP-adres automatisch zone-redundante. Een zone-redundante front-end wordt geleverd door alle zones voor beschikbaarheid in een regio tegelijkertijd. Een zone-redundante gegevenspad wordt voor binnenkomende en uitgaande verbindingen gemaakt. Zone-redundantie in Azure vereist meerdere IP-adressen en DNS-records niet. 

Redundantie van de zone is beschikbaar voor de front-ends van openbare of interne. Uw openbare IP-adres en de front-privé-IP voor de interne Load Balancer mag zone-redundante.

Het volgende script gebruiken voor het maken van een zone-redundante openbare IP-adres voor de interne Load Balancer. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie tot deze sjablonen.

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

Het volgende script gebruiken voor het maken van een zone-redundante front-end-IP-adres voor de interne Load Balancer. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie tot deze sjablonen.

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

Als uw openbare IP-adres front-zone-redundante is, worden uitgaande verbindingen die afkomstig zijn van de VM-exemplaren automatisch zone-redundante. De front-end wordt beschermd tegen storingen van de zone. Uw poorttoewijzing snat omzetten blijft ook zone-fout.

#### <a name="cross-zone-load-balancing"></a>Cross-zone voor taakverdeling

Cross-zone voor de load balancer is beschikbaar in een regio voor de back-end-pool en biedt flexibiliteit van uw VM-exemplaren. Een front-biedt stromen voor een VM in het virtuele netwerk, ongeacht de Zone van de beschikbaarheid van de VM-instantie.

U kunt ook een bepaald gebied voor uw exemplaren front-end en back-end uw gegevenspad en resources met een specifieke zone op één lijn opgeven.

Virtuele netwerken en subnetten worden nooit beperkt door een zone. Definieer alleen een back-end-pool met de gewenste VM-exemplaren en de configuratie is voltooid.

#### <a name="zonal-deployments"></a>Zonal implementaties

Als een optie kunt u de load balancer front-end voor een specifieke zone uitlijnen met het definiëren van een zonal front-end. Een zonal front-door de aangewezen één beschikbaarheid Zone alleen wordt behandeld. Als de front-end wordt gecombineerd met zonal VM-instanties, kunt u bronnen voor specifieke zones uitlijnen.

Er bestaat een openbaar IP-adres dat is gemaakt in een specifieke zone altijd in de zone. Het is niet mogelijk om te wijzigen van de zone van een openbaar IP-adres. Maak een zone-redundante openbare IP-adres in plaats daarvan voor een openbaar IP-adres die kan worden gekoppeld aan resources in meerdere zones.

Gebruik het volgende script te maken van een zonal openbare IP-adres in beschikbaarheid Zone 1. Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie tot deze sjablonen.

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

Gebruik het volgende script maken van een interne Load Balancer front-end in beschikbaarheid Zone 1.

Als u bestaande Resource Manager-sjablonen in uw configuratie gebruikt, voegt de **sku** sectie tot deze sjablonen. Bepaal ook de **zones** eigenschap in de front-end-IP-configuratie voor de onderliggende resource.

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

Cross-zone taakverdeling voor uw back-end-pool door de gegevens van uw VM-exemplaren die zich in een virtueel netwerk in de groep toevoegen.

De Load Balancer standaard bron is altijd regionale en zone-redundante waar beschikbaarheid Zones worden ondersteund. U kunt implementeren op een openbaar IP-adres of een interne Load Balancer front-end dat geen een toegewezen zone in elke regio. Ondersteuning voor beschikbaarheid Zones heeft geen invloed op de mogelijkheid tot implementatie. Als een regio later beschikbaarheid Zones krijgt, eerder geïmplementeerd openbare IP-adressen of interne Load Balancer front-ends automatisch worden zone-redundante. Een zone-redundante gegevenspad betekent niet dat pakketverlies 0%.

### <a name = "nsg"></a>Netwerkbeveiligingsgroepen

Load Balancer standaard- en openbare IP-volledig vrijgeven aan het virtuele netwerk waarvoor het gebruik van Netwerkbeveiligingsgroepen (nsg's). Nsg's maken het mogelijk voor netwerkverkeer goedgekeurde IP-adressen. Nsg's kunt u volledige controle krijgen over verkeer voor uw implementatie. U hoeft niet langer te wachten op andere verkeersstromen zodanig te voltooien.

Nsg's koppelen aan subnetten of de netwerkinterfaces (NIC's) van de VM-exemplaren in de groep back-end. Deze configuratie gebruiken met Load Balancer Standard en openbare IP-standaard, wanneer deze wordt gebruikt als een openbaar IP op exemplaarniveau. Het NSG moet expliciet geaccepteerde het verkeer dat u toestaan wilt, zodat die verkeer.

Zie voor meer informatie over nsg's en hoe u deze wilt toepassen voor uw scenario, [Netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md).

### <a name ="outboundconnections"></a>Uitgaande verbindingen

Load Balancer Standard biedt uitgaande verbindingen voor virtuele machines die zich binnen het virtuele netwerk wanneer een load balancer wordt gebruikt zich voordoet op poort snat omzetten. Het algoritme snat omzetten zich voordoet op poort biedt verbeterde stabiliteit en schalen.

Wanneer een resource met openbare Load Balancer gekoppeld aan VM-instanties is, wordt elke bron uitgaande verbinding herschreven. De bron wordt herschreven van het virtuele netwerk persoonlijke IP-adresruimte in het front-openbare IP-adres van de load balancer.

Wanneer de uitgaande verbindingen worden gebruikt met een zone-redundante front-end, de verbindingen zijn ook zone-redundante en snat omzetten poort toewijzingen zone storingen.

Het nieuwe algoritme in de Load Balancer standaard preallocates snat omzetten poorten op de Netwerkinterfacekaart van elke virtuele machine. Wanneer een NIC wordt toegevoegd aan de groep, worden de poorten snat omzetten vooraf op basis van de grootte van de groep toegewezen. De volgende tabel ziet u de poort preallocations voor zes lagen van de back-end-pool grootten:

| Poolgrootte (VM-exemplaren) | Vooraf toegewezen aantal snat omzetten poorten |
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1,000 | 32 |

Snat omzetten poorten vertalen niet rechtstreeks aan het aantal uitgaande verbindingen. Een poort snat omzetten kan opnieuw worden gebruikt voor meerdere unieke bestemmingen. Raadpleeg voor meer informatie de [uitgaande verbindingen](load-balancer-outbound-connections.md) artikel.

Als de back-end-poolgrootte verhoogt en in een hogere laag overgangen, worden de helft van de toegewezen poorten vrijgemaakt. Verbindingen die zijn gekoppeld aan een time-out geregenereerde poort en moeten opnieuw worden gemaakt. Nieuwe verbindingspogingen voltooid onmiddellijk. Als de back-end-poolgrootte afneemt en in een lagere laag overgangen, verhoogt het aantal beschikbare poorten met snat omzetten. Bestaande verbindingen worden niet getroffen.

Load Balancer standaard heeft een aanvullende configuratie-optie die kan worden gebruikt op basis van de afzonderlijke regels. U kunt bepalen dat front-end voor snat omzetten zich voordoet op poort wordt gebruikt wanneer meerdere front-ends beschikbaar zijn.

Wanneer alleen Load Balancer standaard VM-instanties fungeert, uitgaande verbindingen met snat omzetten niet beschikbaar. U kunt deze mogelijkheid expliciet herstellen door het VM-exemplaren ook toewijzen aan een openbare load balancer. U kunt ook rechtstreeks openbare IP-adressen toewijzen als openbare IP's op exemplaarniveau elk VM-exemplaar. Het is mogelijk dat deze configuratieoptie vereist voor sommige besturingssystemen en toepassingsscenario's. 

### <a name="port-forwarding"></a>Poort doorsturen

Basic en Standard Load Balancers bieden de mogelijkheid voor het configureren van inkomende NAT-regels voor het toewijzen van een front-endpoort aan een afzonderlijke back-end-exemplaar. Door deze regels te configureren, kunt u Remote Desktop Protocol-eindpunten en SSH-eindpunten beschikbaar of andere toepassingsscenario's uitvoeren.

Load Balancer standaard blijft bieden de mogelijkheid poort doorsturen via de binnenkomende NAT-regels. Wanneer gebruikt in combinatie met zone-redundante front-ends, inkomende NAT-regels zone-redundante en storingen van de zone.

### <a name="multiple-front-ends"></a>Meerdere front-ends

Configureer meerdere front-ends voor flexibiliteit bij het ontwerpen wanneer meerdere afzonderlijke IP-adressen worden weergegeven, zoals het TLS-websites of de SQL AlwaysOn-beschikbaarheidsgroep eindpunten voor toepassingen vereist. 

Load Balancer standaard nog steeds meerdere front-ends waarin u wilt weergeven van een specifieke toepassingseindpunt op een uniek IP-adres.

Zie voor meer informatie over het configureren van meerdere front-end-IP-adressen [meerdere IP-configuratie](load-balancer-multivip-overview.md).

## <a name = "sku"></a>Over SKU 's

SKU's zijn alleen beschikbaar in het Azure Resource Manager-implementatiemodel. Deze preview introduceert twee SKU's voor Load Balancer en openbare IP-resources: Basic en Standard. De SKU's verschillen in mogelijkheden, prestatiekenmerken, beperkingen en een aantal ingebouwde gedrag. Virtuele machines kunnen worden gebruikt met beide SKU. Voor zowel de Load Balancer en de openbare IP-resources blijven SKU's optionele kenmerken. Wanneer de SKU's worden weggelaten in de Scenariodefinitie van een, wordt de configuratie van de standaard voor het gebruik van de basis-SKU.

>[!IMPORTANT]
>De SKU van een bron is niet veranderlijke. U kunt de SKU van een bestaande resource niet wijzigen.  

### <a name="load-balancer"></a>Load Balancer

De [bestaande Load Balancer-resource](load-balancer-overview.md) wordt de basis-SKU en blijft in het algemeen beschikbaar is en niet gewijzigd.

Standaard SKU van Load Balancer is nieuw en zijn momenteel in preview. 1 augustus 2017 API-versie voor Microsoft.Network/loadBalancers voegt de **sku** eigenschap in op de resource-uitbreiding:

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```
Load Balancer standaard is automatisch zone robuuste in regio's die worden geboden door beschikbaarheid Zones. Als de Load Balancer is gedeclareerd zonal, is deze niet automatisch zone Refs.

### <a name="public-ip"></a>Openbare IP

De [bestaande openbare IP-resource](../virtual-network/virtual-network-ip-addresses-overview-arm.md) wordt de basis-SKU en blijft in het algemeen beschikbaar zijn met alle van de mogelijkheden, prestatiekenmerken en beperkingen.

Openbare IP-standaard SKU is nieuw en zijn momenteel in preview. 1 augustus 2017 API-versie voor Microsoft.Network/publicIPAddresses voegt de **sku** eigenschap in op de resource-uitbreiding:

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

In tegenstelling tot openbare IP-Basic meerdere toewijzingsmethoden biedt, openbare IP-standaard gebruikt altijd statische toewijzing.

Openbare IP-standaard is automatisch zone robuuste in regio's die worden geboden door beschikbaarheid Zones. Als het openbare IP-adres is gedeclareerd zonal, is deze niet automatisch zone Refs. Een zonal openbare IP-adres kan niet worden gewijzigd van een zone naar de andere.

## <a name="migration-between-skus"></a>Migratie tussen SKU 's

Er zijn geen veranderlijke SKU's. Volg de stappen in deze sectie voor het verplaatsen van één resource SKU naar een andere.

### <a name="migrate-from-basic-to-standard-sku"></a>Migreren van Basic naar standaard SKU

1. Maak een nieuwe standaard resource (Load Balancer en openbare IP-adressen, indien nodig). Maak uw regels en definities-test.

2. De basis-SKU-resources (Load Balancer en openbare IP-adressen, indien van toepassing) verwijderen van alle VM-instanties. Zorg ervoor dat alle VM-instanties van een beschikbaarheidsset ook verwijderd.

3. Alle exemplaren van de VM koppelen aan de nieuwe standaard SKU-resources.

### <a name="migrate-from-standard-to-basic-sku"></a>Migreren van standaard naar de basis-SKU

1. Maak een nieuwe Basic resource (Load Balancer en openbare IP-adressen, indien nodig). Maak uw regels en definities-test. 

2. De standaard SKU-resources (Load Balancer en openbare IP-adressen, indien van toepassing) verwijderen van alle VM-exemplaren. Zorg ervoor dat alle VM-instanties van een beschikbaarheidsset ook verwijderd.

3. Alle exemplaren van de VM koppelen aan de nieuwe basis-SKU-resources.

>[!IMPORTANT]
>
>Zijn er beperkingen met betrekking tot gebruik van de basis en standaard SKU's.
>
>HA zijn poorten en diagnostische gegevens van de standaard SKU alleen beschikbaar in de standaard SKU. U kunt geen migreren van de standaard SKU naar de basis-SKU of deze functies ook behouden.
>
>Overeenkomende SKU's moet worden gebruikt voor Load Balancer en openbare IP-bronnen. Een combinatie van basis-SKU-bronnen en bronnen van de standaard SKU is niet mogelijk. Een virtuele machine, VM's in een Beschikbaarheidsset kan niet worden gekoppeld of een virtuele-machineschaalset ingesteld op beide SKU's tegelijk.
>

## <a name="region-availability"></a>Beschikbaarheid in regio’s

Load Balancer standaard is momenteel beschikbaar in deze regio's:
- VS - oost 2
- VS - midden
- Noord-Europa
- West-centraal VS
- West-Europa
- Zuidoost-Azië

## <a name="sku-service-limits-and-abilities"></a>SKU-Servicelimieten en mogelijkheden

Azure [Servicelimieten voor netwerken](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits) toepassen per regio per abonnement. 

De volgende tabel vergelijkt de beperkingen en mogelijkheden van de Load Balancer Basic en Standard-SKU's:

| Load Balancer | Basic | Standard |
| --- | --- | --- |
| Grootte van de back-end | maximaal 100 | maximaal 1000 |
| Back-end-pool grens | Beschikbaarheidsset | virtueel netwerk, regio |
| Ontwerp van de back-end-pool | Virtuele machines in de Beschikbaarheidsset, virtuele-machineschaalset in Beschikbaarheidsset instellen | Een VM-instantie in het virtuele netwerk |
| HA poorten | Niet ondersteund | Beschikbaar |
| Diagnostiek | Beperkt, openbare alleen | Beschikbaar |
| VIP-beschikbaarheid  | Niet ondersteund | Beschikbaar |
| Snelle IP-mobiliteit | Niet ondersteund | Beschikbaar |
|Beschikbaarheid Zones scenario 's | Alleen Zonal | Zonal, Zone-redundante Cross-zone-taakverdeling |
| Uitgaande snat omzetten algoritme | Op aanvraag | Vooraf toegewezen |
| Uitgaande snat omzetten front-selectie | Niet-configureerbare meerdere kandidaten | Optionele configuratie te verminderen kandidaten |
| Netwerkbeveiligingsgroep | Optionele op NIC/subnet | Vereist |

De volgende tabel vergelijkt de beperkingen en mogelijkheden van het openbare IP-basis en standaard SKU's:

| Openbare IP | Basic | Standard |
| --- | --- | --- |
| Beschikbaarheid Zones scenario 's | Alleen Zonal | Zone-redundante (standaard), zonal (optioneel) | 
| Snelle IP-mobiliteit | Niet ondersteund | Beschikbaar |
| VIP-beschikbaarheid | Niet ondersteund | Beschikbaar |
| Prestatiemeteritems | Niet ondersteund | Beschikbaar |
| Netwerkbeveiligingsgroep | Optionele op NIC | Vereist |


## <a name="preview-sign-up"></a>Preview-registratie

Als u wilt deelnemen aan de preview voor Load Balancer standaard SKU en de bijbehorende openbare IP-standaard SKU Registreer uw abonnement.  Uw abonnement biedt die u toegang via PowerShell of Azure CLI 2.0 tot wordt geregistreerd. Als u wilt registreren, moet u de volgende stappen uitvoeren:

>[!NOTE]
>Registratie van de Load Balancer standaard functie kan een uur duren globally kracht worden. Als u wilt gebruiken Load Balancer Standard met [beschikbaarheid Zones](https://aka.ms/availabilityzones) en [HA poorten](https://aka.ms/haports), een afzonderlijke registratie is vereist voor deze voorbeelden. Volg de instructies respectieve voor registratie van deze functies.

### <a name="sign-up-by-using-azure-cli-20"></a>Aanmelden met behulp van Azure CLI 2.0

1. Registreer het onderdeel met de provider:

    ```cli
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    ```
    
2. De bewerking kan maximaal 10 minuten duren. U kunt de status van de bewerking met de volgende opdracht controleren:

    ```cli
    az feature show --name AllowLBPreview --namespace Microsoft.Network
    ```
    
    Doorgaan met de volgende stap wanneer de status van het onderdeel registratie retourneert 'Geregistreerde':
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowLBPreview",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
3. Voer de registratie preview door uw abonnement met de resourceprovider opnieuw te registreren:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    
### <a name="sign-up-by-using-powershell"></a>Aanmelden met behulp van PowerShell

1. Registreer het onderdeel met de provider:

    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```
    
2. De bewerking kan maximaal 10 minuten duren. U kunt de status van de bewerking met de volgende opdracht controleren:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

    Doorgaan met de volgende stap wanneer de status van het onderdeel registratie retourneert 'Geregistreerde':
   
    ```
    FeatureName      ProviderName        RegistrationState
    -----------      ------------        -----------------
    AllowLBPreview   Microsoft.Network   Registered
    ```
    
3. Voer de registratie preview door uw abonnement met de resourceprovider opnieuw te registreren:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```
 
## <a name="pricing"></a>Prijzen

Load Balancer standaard SKU facturering is gebaseerd op de geconfigureerde regels en verwerkte gegevens. Er zijn geen kosten verbonden zijn tijdens de preview-periode. Raadpleeg voor meer informatie de [Load Balancer](https://aka.ms/lbpreviewpricing) en [openbare IP-adres](https://aka.ms/lbpreviewpippricing) prijzen van pagina's.

Klanten blijven genieten van Load Balancer basis-SKU zonder kosten.

## <a name="limitations"></a>Beperkingen

De volgende beperkingen toepassen op het moment van preview en nog worden gewijzigd:

- Back-end-exemplaren van Load Balancer kunnen niet worden gevonden in virtuele netwerken peer is ingesteld op dit moment. Alle exemplaren van de back-end moeten zich in dezelfde regio.
- Er zijn geen veranderlijke SKU's. U kunt de SKU van een bestaande resource niet wijzigen.
- Beide SKU's kunnen worden gebruikt met een zelfstandige virtuele machine, VM-exemplaren in een Beschikbaarheidsset of een virtuele-machineschaalset ingesteld. Combinaties van de virtuele machine kunnen niet worden gebruikt met beide SKU's tegelijk. Een configuratie met een combinatie van SKU's is niet toegestaan.
- met behulp van een interne Load Balancer Standard met een VM-instantie (of een deel van een Beschikbaarheidsset) schakelt [snat omzetten uitgaande verbindingen standaard](load-balancer-outbound-connections.md). U kunt deze mogelijkheid naar een zelfstandige virtuele machine, VM-exemplaren in een Beschikbaarheidsset of een virtuele-machineschaalset herstellen. U kunt ook de mogelijkheid om u te maken van uitgaande verbindingen herstellen. Tegelijkertijd de toewijzen een openbare Load Balancer Standard of openbare IP-standaard als een instantieniveau openbare IP-adres, hetzelfde VM-exemplaar voor het herstellen van deze mogelijkheden. Nadat de toewijzing voltooid is, is opnieuw zich voordoet op poort snat omzetten naar een openbaar IP-adres opgegeven.
- VM-exemplaren moet mogelijk worden gegroepeerd in beschikbaarheidssets voor volledige back-end-pool schaal. Maximaal 150 beschikbaarheid worden sets en zelfstandige virtuele machines geplaatst in een enkele back-end-pool.
- IPv6 wordt niet ondersteund.
- In de context van beschikbaarheid Zones is front-end niet van zonal veranderlijke naar zone-redundante of vice versa. Nadat een front-wordt gemaakt als zone-redundante, blijft deze zone-redundante. Nadat een front-wordt gemaakt als zonal, blijft het zonal.
- In de context van beschikbaarheid Zones, kan niet een zonal openbare IP-adres van een zone worden verplaatst naar een andere.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Load Balancer Basic](load-balancer-overview.md).
- Meer informatie over [beschikbaarheid Zones](../availability-zones/az-overview.md).
- Informatie over een aantal van de andere sleutel [netwerkmogelijkheden](../networking/networking-overview.md) in Azure.

