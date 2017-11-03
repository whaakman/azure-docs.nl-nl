---
title: Meerdere VIP's voor Azure Load Balancer | Microsoft Docs
description: Overzicht van meerdere VIP's op Azure Load Balancer
services: load-balancer
documentationcenter: na
author: chkuhtz
manager: narayan
editor: 
ms.assetid: 748e50cd-3087-4c2e-a9e1-ac0ecce4f869
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: chkuhtz
ms.openlocfilehash: 1045a18f5fd9739a6028198deea129e9e621f127
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="multiple-vips-for-azure-load-balancer"></a>Meerdere VIP's voor Azure Load Balancer

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure Load Balancer kunt u load balance services op meerdere poorten of meerdere IP-adressen. Laden van stromen verdelen over een set van virtuele machines kunt u openbare en interne load balancer definities.

In dit artikel beschrijft de grondbeginselen van deze mogelijkheid, belangrijke concepten en beperkingen. Als u alleen weergeven van services op één IP-adres wilt, kunt u vinden vereenvoudigde instructies voor het [openbare](load-balancer-get-started-internet-portal.md) of [interne](load-balancer-get-started-ilb-arm-portal.md) load balancer-configuraties. Het toevoegen van meerdere VIP's is incrementele naar een configuratie met één VIP. Met behulp van de concepten in dit artikel, kunt u een vereenvoudigde configuratie op elk gewenst moment uitbreiden.

Wanneer u een Azure Load Balancer definieert, worden een front-end- en een back-endconfiguratie zijn verbonden met regels. De health test waarnaar wordt verwezen door de regel wordt gebruikt om te bepalen hoe nieuwe stromen worden verzonden naar een knooppunt in de back-endpool. De frontend wordt gedefinieerd door een virtuele IP-adres (VIP), dit een 3-tuple bestaat uit een IP-adres (openbare of interne), een transportprotocol (UDP of TCP) en een poortnummer is. Een DIP is een IP-adres op een Azure virtuele NIC die is gekoppeld aan een virtuele machine in de back-endpool.

De volgende tabel bevat enkele voorbeeld-frontend-configuraties:

| VIP | IP-adres | Protocol | poort |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

De tabel bevat vier verschillende frontends. Frontends #1, #2 en &#3; zijn een enkele VIP met meerdere regels. Hetzelfde IP-adres wordt gebruikt, maar de poort of het protocol is verschillend voor elke frontend. Frontends #1 en #4 zijn een voorbeeld van meerdere VIP's, waarbij het dezelfde frontend-protocol en poort opnieuw worden gebruikt tussen meerdere VIP's.

Azure Load Balancer biedt flexibiliteit voor het definiëren van de load-balancingregels. Een regel verklaart hoe een adres en poort op de frontend is toegewezen aan het adres van het doel en de poort op de back-end. Of back-end-poorten zijn opnieuw gebruikt verschillende regels, is afhankelijk van het type van de regel. Elk type regel heeft bepaalde vereisten die invloed kunnen zijn op de host-configuratie en test ontwerp. Er zijn twee typen regels:

1. De standaardregel met geen back-end-poort opnieuw gebruiken
2. De regel zwevend IP is waar back-end-poorten worden hergebruikt

Azure Load Balancer kunt u mix van beide regeltypen op de dezelfde load balancer-configuratie. De load balancer kunt gebruiken ze tegelijkertijd voor een bepaalde virtuele machine of een combinatie hiervan, zolang u de beperkingen van de regel naleeft. Welke regeltype dat u kiest, is afhankelijk van de vereisten van uw toepassing en de complexiteit van de ondersteuning van dat de configuratie. U moet evalueren welke regeltypen zijn het meest geschikt is voor uw scenario.

We verkennen deze scenario's verder door te beginnen met het standaardgedrag.

## <a name="rule-type-1-no-backend-port-reuse"></a>Regeltype #1: Er is geen back-end-poort opnieuw gebruiken

![MultiVIP-afbeelding](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

In dit scenario worden de frontend-VIP's als volgt geconfigureerd:

| VIP | IP-adres | Protocol | poort |
| --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

Het DIP is de bestemming van de binnenkomende stroom. Elke virtuele machine wordt in de endadresgroep back-van de gewenste service op een unieke poort op een DIP. Deze service is gekoppeld aan de frontend via de regeldefinitie van een.

We definiëren twee regels:

| Regel | Frontend toewijzen | Voor back-endpool |
| --- | --- | --- |
| 1 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

Het volledige toewijzen in Azure Load Balancer is nu als volgt:

| Regel | VIP-IP-adres | Protocol | poort | Doel | poort |
| --- | --- | --- | --- | --- | --- |
| ![Regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |DIP IP-adres |80 |
| ![Regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |DIP IP-adres |81 |

Elke regel moet een stroom met een unieke combinatie van IP-adres en de doelpoort opleveren. De doelpoort van de stroom te variëren, kunnen meerdere regels stromen leveren aan de dezelfde DIP op verschillende poorten.

Statuscontroles altijd omgeleid naar het DIP van een virtuele machine. U moet u zorgen dat uw test de status van de virtuele machine weerspiegelt.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Regeltype #2: back-end-poort opnieuw gebruiken met behulp van zwevend IP

Azure Load Balancer biedt de flexibiliteit om te gebruiken als de frontend-poort over meerdere VIP's, ongeacht het regeltype dat u gebruikt. Bovendien worden sommige toepassingsscenario's liever of moeten u dezelfde poort moet worden gebruikt door meerdere exemplaren van een toepassing op een enkele virtuele machine in de back-endpool. Algemene voorbeelden poort hergebruik van omvatten clustering voor hoge beschikbaarheid, virtuele apparaten en het blootstellen van meerdere TLS eindpunten zonder opnieuw versleutelen.

Als u gebruiken als de back-endpoort over meerdere regels wilt, moet u zwevend IP inschakelen in de regeldefinitie van de.

Zwevend IP is een deel van wat staat bekend als Direct Server retourneren (DSR). DSR bestaat uit twee delen: een stroom-topologie en een IP-schema. Op een niveau platform werkt Azure Load Balancer altijd in een topologie van de stroom DSR ongeacht of zwevend IP is ingeschakeld of niet. Dit betekent dat het uitgaande deel van een stroom altijd correct wordt herschreven om rechtstreeks stromen terug naar de oorsprong.

Met het regeltype standaard beschrijft Azure een traditionele load balancing-toewijzingsschema voor IP-adres voor eenvoudig te gebruiken. Zwevend IP inschakelen, wijzigt het schema voor het IP-van-adres om toe te staan voor de extra flexibiliteit, zoals hieronder wordt uitgelegd.

Het volgende diagram ziet u deze configuratie:

![MultiVIP-afbeelding](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

In dit scenario heeft elke virtuele machine in de back-endpool drie netwerkinterfaces:

* DIP: een virtuele NIC die is gekoppeld aan de virtuele machine (IP-configuratie van Azure NIC resource)
* VIP1: een loopback-interface in het gastbesturingssysteem dat is geconfigureerd met IP-adres van VIP1
* VIP2: een loopback-interface in het gastbesturingssysteem dat is geconfigureerd met IP-adres van VIP2

> [!IMPORTANT]
> De configuratie van de logische interfaces wordt uitgevoerd binnen het gastbesturingssysteem. Deze configuratie is niet uitgevoerd of worden beheerd door Azure. Zonder deze configuratie zijn werken de regels niet. Health test definities gebruiken het DIP van de virtuele machine in plaats van de logische VIP. Daarom moet uw service test antwoorden opgeven voor een DIP-poort die overeenkomen met de status van de service die wordt aangeboden op het logische VIP.

Stel de dezelfde frontend-configuratie zoals in het voorgaande scenario:

| VIP | IP-adres | Protocol | poort |
| --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

We definiëren twee regels:

| Regel | Frontend toewijzen | Voor back-endpool |
| --- | --- | --- |
| 1 |![Regel](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (in VM1 en VM2) |
| 2 |![Regel](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![back-end](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (in VM1 en VM2) |

De volgende tabel ziet u de volledige toewijzing in de load balancer:

| Regel | VIP-IP-adres | Protocol | poort | Doel | poort |
| --- | --- | --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |hetzelfde als het VIP (65.52.0.1) |hetzelfde als het VIP (80) |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |hetzelfde als het VIP (65.52.0.2) |hetzelfde als het VIP (80) |

Het doel van de binnenkomende stroom is het VIP-adres op de loopback-interface in de virtuele machine. Elke regel moet een stroom met een unieke combinatie van IP-adres en de doelpoort opleveren. Hergebruik van de poort is door te variëren van het doel-IP-adres van de stroom, mogelijk op dezelfde virtuele machine. Uw service wordt door deze te binden aan de IP-adres en poort van de respectieve loopback-interface van de VIP blootgesteld aan de load balancer.

Merk op dat in dit voorbeeld de doelpoort niet wijzigt. Hoewel dit een zwevend IP-scenario is, ondersteunt Load Balancer van Azure ook het definiëren van een regel te herschrijven van de back-end-doelpoort en zodat deze verschilt van de frontend-doelpoort.

Het regeltype zwevend IP vormt de basis van verschillende patronen van load balancer-configuratie. Een voorbeeld dat beschikbaar is de [SQL AlwaysOn met meerdere Listeners](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) configuratie. Na verloop van tijd documenteren we meer van deze scenario's.

## <a name="limitations"></a>Beperkingen

* Configuraties met meerdere VIP worden alleen ondersteund met IaaS VM's.
* Met de regel zwevend IP moet uw toepassing het DIP gebruiken voor uitgaande stromen. Als uw toepassing verbinding heeft met het VIP-adres geconfigureerd op de loopback-interface in het gastbesturingssysteem, vervolgens snat omzetten is niet beschikbaar voor het herschrijven van de uitgaande stroom en de stroom is mislukt.
* Openbare IP-adressen hebben een invloed op de facturering. Zie voor meer informatie [prijzen van IP-adres](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Abonnement-beperkingen gelden. Zie voor meer informatie [Servicelimieten](../azure-subscription-service-limits.md#networking-limits) voor meer informatie.
