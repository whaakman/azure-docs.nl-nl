---
title: Meerdere front-ends voor Azure Load Balancer | Microsoft Docs
description: Overzicht van meerdere front-ends op Azure Load Balancer
services: load-balancer
documentationcenter: na
author: chkuhtz
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: chkuhtz
ms.openlocfilehash: d435d2c491cf17356e96f7bbb05b1e22c8e04aca
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219355"
---
# <a name="multiple-frontends-for-azure-load-balancer"></a>Meerdere front-ends voor Azure Load Balancer

Azure Load Balancer kunt u saldo-services op meerdere poorten, meerdere IP-adressen of beide. Definities van openbare en interne load balancer kunt u saldo stromen laden in een set van virtuele machines.

Dit artikel beschrijft de grondbeginselen van deze mogelijkheid, belangrijke concepten en beperkingen. Als u alleen weergeven van services op één IP-adres wilt, kunt u eenvoudige instructies voor het vinden [openbare](load-balancer-get-started-internet-portal.md) of [interne](load-balancer-get-started-ilb-arm-portal.md) load balancer-configuraties. Het toevoegen van meerdere front-ends is incrementeel ten opzichte van een enkele front-end-configuratie. Met behulp van de concepten in dit artikel, kunt u een vereenvoudigde configuratie op elk gewenst moment uitbreiden.

Wanneer u een Azure Load Balancer definieert, worden een front-end- en een back-end-groep-configuratie zijn verbonden met regels. De statustest waarnaar wordt verwezen door de regel wordt gebruikt om te bepalen hoe u met nieuwe stromen worden verzonden naar een knooppunt in de back-endpool. De front-end (ook wel VIP) wordt gedefinieerd door een 3-tuple bestaat uit een IP-adres (openbare of interne), een transportprotocol (UDP of TCP) en een poortnummer van de load balancer-regel. De back endpool is een verzameling van virtuele Machine IP-configuraties (onderdeel van de NIC-bron) die verwijzen naar de Load Balancer-back-endadresgroep.

De volgende tabel bevat enkele frontend-voorbeeld-configuraties:

| Front-end | IP-adres | protocol | poort |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

De tabel bevat vier verschillende front-ends. Front-ends #1, #2 en 3 # zijn een één front met meerdere regels. Hetzelfde IP-adres wordt gebruikt, maar de poort of het protocol verschilt voor elk front-end. Front-ends #1 en #4 zijn een voorbeeld van meerdere front-ends, waarbij het dezelfde frontend-protocol en poort worden hergebruikt voor meerdere front-ends.

Azure Load Balancer biedt flexibiliteit voor het definiëren van de load balancer-regels. Een regel verklaart hoe een adres en poort op het front-end is toegewezen aan de doel-adres en poort op de back-end. Wel of niet-back-end-poorten worden hergebruikt voor regels, is afhankelijk van het type van de regel. Elk type regel heeft specifieke vereisten die invloed kunnen zijn op de host-configuratie en test-ontwerp. Er zijn twee soorten regels:

1. De standaardregel met geen back-end-poort niet opnieuw gebruiken
2. De regel zwevend IP is waar de back-end-poorten worden hergebruikt

Azure Load Balancer kunt u beide typen op de dezelfde load balancer-configuratie te combineren. De load balancer kunt ze tegelijkertijd gebruiken voor een bepaalde virtuele machine of een combinatie hiervan, zolang u houdt zich aan de beperkingen van de regel. Welke regeltype dat u kiest, is afhankelijk van de vereisten van uw toepassing en de complexiteit die configuratie ondersteunen. U moet evalueren welke regeltypen zijn vooral geschikt voor uw scenario.

Deze scenario's verder verkennen we door te beginnen met het standaardgedrag.

## <a name="rule-type-1-no-backend-port-reuse"></a>Regeltype #1: Er is geen back-end-poort niet opnieuw gebruiken

![Meerdere front-afbeelding met groene en paarse frontend](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

In dit scenario worden de front-ends als volgt geconfigureerd:

| Front-end | IP-adres | protocol | poort |
| --- | --- | --- | --- |
| ![groen frontend](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![paarse frontend](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

Het DIP is het doel van de inkomende stroom. Elke virtuele machine wordt aangegeven dat de gewenste service op een unieke poort op een DIP in de back-endadresgroep. Deze service is gekoppeld aan de front-end via de regeldefinitie van een.

We definiëren twee regels:

| Regel | Kaart frontend | Aan de back-endpool |
| --- | --- | --- |
| 1 |![groen frontend](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 |![Back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![Back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 |![Back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![Back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

De toewijzing van het volledige in Azure Load Balancer is nu als volgt:

| Regel | Frontend-IP-adres | protocol | poort | Doel | poort |
| --- | --- | --- | --- | --- | --- |
| ![groen regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |IP-adres |80 |
| ![paarse regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |IP-adres |81 |

Elke regel moet worden gegenereerd, een stroom met een unieke combinatie van de doel-IP-adres en doelpoort. De doelpoort van de stroom te variëren, kunnen meerdere regels stromen leveren aan de dezelfde DIP op verschillende poorten.

Statuscontroles zijn altijd worden omgeleid naar het DIP van een virtuele machine. U moet u zorgen dat uw test de status van de virtuele machine weerspiegelt.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Regeltype #2: back-end-poort opnieuw worden gebruikt met behulp van zwevend IP

Azure Load Balancer biedt de flexibiliteit om te opnieuw gebruiken van de front-endpoort via meerdere front-ends, ongeacht het type van de regel die wordt gebruikt. Bovendien sommige toepassingsscenario de voorkeur geeft aan of vereisen dat dezelfde poort moet worden gebruikt door meerdere exemplaren van een toepassing op een enkele virtuele machine in de back-endpool. Algemene voorbeelden van poort hergebruik omvatten clustering voor hoge beschikbaarheid, virtuele apparaten en meerdere TLS-eindpunten zonder opnieuw te versleutelen om vrij te geven.

Als u de back-endpoort over meerdere regels gebruiken wilt, moet u zwevend IP inschakelen in het regeldefinitie van de.

'Zwevende IP-adres' is van het Azure-terminologie voor een gedeelte van wat staat bekend als directe Server retourneren (DSR). DSR bestaat uit twee delen: een stroom-topologie en een IP-schema. Op het niveau van een platform werkt de Azure Load Balancer altijd in een DSR-flow-topologie, ongeacht of zwevend IP is ingeschakeld of niet. Dit betekent dat het uitgaande deel van een stroom is altijd goed herschreven om Microsoft flow rechtstreeks naar de oorsprong.

Azure wordt aangegeven dat een traditionele load balancing-IP-adres toewijzingsschema voor het gebruiksgemak van het type van de regel standaard. Zwevend IP inschakelen, wijzigt het IP-adres toewijzingsschema om toe te staan voor meer flexibiliteit, zoals hieronder wordt uitgelegd.

Het volgende diagram illustreert deze configuratie:

![Meerdere front-afbeelding met groene en paarse frontend met DSR](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

Voor dit scenario heeft elke virtuele machine in de back-endpool drie netwerkinterfaces:

* DIP: een virtuele NIC die is gekoppeld aan de virtuele machine (IP-configuratie van NIC-bron van Azure)
* Frontend 1: een loopback-interface in het gastbesturingssysteem dat is geconfigureerd met IP-adres van front-end 1
* Frontend 2: een loopback-interface in het gastbesturingssysteem dat is geconfigureerd met IP-adres van front-end 2

> [!IMPORTANT]
> De configuratie van de loopback-interfaces wordt uitgevoerd binnen het gastbesturingssysteem te installeren. Deze configuratie is niet uitgevoerd of worden beheerd door Azure. Zonder deze configuratie worden werkt de regels niet. Health test definities gebruiken het DIP van de virtuele machine in plaats van de loopback-interface voor de DSR-Frontend. Uw service moet daarom testrespons opgeven voor een DIP-poort die overeenkomen met de status van de service die wordt aangeboden op de loopback-interface voor de DSR-Frontend.

Stel de dezelfde front-end-configuratie zoals in het voorgaande scenario:

| Front-end | IP-adres | protocol | poort |
| --- | --- | --- | --- |
| ![groen frontend](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![paarse frontend](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

We definiëren twee regels:

| Regel | Front-end | Toewijzen aan de back-endpool |
| --- | --- | --- |
| 1 |![regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 |![Back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) Frontend1:80 (in VM1 en VM2) |
| 2 |![regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 |![Back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) Frontend2:80 (in VM1 en VM2) |

De volgende tabel ziet u de volledige toewijzing in de load balancer:

| Regel | Frontend-IP-adres | protocol | poort | Doel | poort |
| --- | --- | --- | --- | --- | --- |
| ![groen regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |hetzelfde als de front-end (65.52.0.1) |hetzelfde als de front-end (80) |
| ![paarse regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |hetzelfde als de front-end (65.52.0.2) |hetzelfde als de front-end (80) |

Het doel van de inkomende stroom is de front-end-IP-adres op de loopback-interface op de virtuele machine. Elke regel moet worden gegenereerd, een stroom met een unieke combinatie van de doel-IP-adres en doelpoort. Hergebruik van poort is door het doel-IP-adres van de stroom te variëren, mogelijk op dezelfde virtuele machine. Uw service wordt blootgesteld aan de load balancer door deze te binden aan van de front-end-IP-adres en poort van de respectieve loopback-interface.

U ziet in dit voorbeeld verandert niet de doelpoort. Hoewel dit een zwevend IP-scenario is, ondersteunt Azure Load Balancer ook definiëren van een regel te herschrijven van de back-end-doelpoort en gemakkelijk af van de frontend-doelpoort.

Het regeltype zwevend IP vormt de basis van verschillende patronen van load balancer-configuratie. Een voorbeeld dat beschikbaar is op dit moment is de [SQL AlwaysOn met meerdere Listeners](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) configuratie. Na verloop van tijd, zullen we documenteren meer van deze scenario's.

## <a name="limitations"></a>Beperkingen

* Meerdere front-configuraties worden alleen ondersteund met IaaS-VM's.
* Aan de regel zwevend IP moet uw toepassing de primaire IP-configuratie gebruiken voor uitgaande stromen. Als uw toepassing wordt gebonden aan de front-end-IP-adres geconfigureerd op de loopback-interface in het gastbesturingssysteem te installeren, Azure van SNAT is niet beschikbaar voor het herschrijven van de uitgaande stroom en de stroom is mislukt.
* Openbare IP-adressen zijn van invloed op de facturering. Zie voor meer informatie, [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Limieten voor een abonnement van toepassing. Zie voor meer informatie, [Servicelimieten](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- Beoordeling [uitgaande verbindingen](load-balancer-outbound-connections.md) om te begrijpen van de impact van meerdere front-ends op het gedrag van uitgaande verbindingen.
