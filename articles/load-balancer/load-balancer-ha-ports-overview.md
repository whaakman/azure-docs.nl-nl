---
title: Overzicht van poorten voor hoge beschikbaarheid in Azure
titlesuffix: Azure Load Balancer
description: Meer informatie over hoge beschikbaarheid poorten taakverdeling op een interne load balancer.
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/07/2018
ms.author: kumud
ms.openlocfilehash: e37b127b112768cd09989e1a4b5edf99ca101983
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53141863"
---
# <a name="high-availability-ports-overview"></a>Overzicht van poorten voor hoge beschikbaarheid

Azure Standard Load Balancer kunt u taakverdeling TCP en UDP-stromen op alle poorten tegelijkertijd wanneer u een interne load balancer. 

Een regel voor hoge beschikbaarheid (HA)-poorten is een variant van een regel voor taakverdeling, op een interne standaardversie van Load Balancer geconfigureerd. U kunt het gebruik van een load balancer vereenvoudigen door te geven van een regel voor het verdelen van alle TCP en UDP-stromen die op alle poorten van een interne standaardversie van Load Balancer binnenkomen. De beslissing load balancing is per stroom gemaakt. Deze actie is gebaseerd op de volgende 5-tuple-verbinding: bron-IP-adres, bronpoort, doel-IP-adres, bestemmingspoort en protocol.

De functie voor HA-poorten helpt u bij belangrijke scenario's, zoals hoge beschikbaarheid en schaal voor de virtuele netwerkapparaten (NVA's) in virtuele netwerken. De functie kan ook helpen bij een groot aantal poorten verdeeld worden moet. 

De functie voor HA-poorten is geconfigureerd wanneer u de front-end en back-end-poorten ingesteld op **0** en het protocol **alle**. De interne load balancer-resource verdeelt vervolgens alle TCP en UDP-stromen, ongeacht het poortnummer.

## <a name="why-use-ha-ports"></a>Waarom HA-poorten gebruiken?

### <a name="nva"></a>Virtuele netwerkapparaten

U kunt NVA's gebruiken om u te helpen beveiligen uw Azure-workload tegen meerdere soorten beveiligingsrisico's. Wanneer u de NVA's in deze scenario's gebruikt, moeten ze betrouwbaar en maximaal beschikbare en ze moeten uitbreiden voor de vraag.

U kunt deze doelen bereiken door eenvoudig NVA exemplaren toe te voegen aan de back-endpool van de interne load balancer en load balancer-regel voor het configureren van een HA-poorten.

Voor NVA HA scenario's bieden HA-poorten aan de volgende voordelen:
- Snelle failover in orde-exemplaren voorzien van statuscontroles per exemplaar
- Hogere prestaties met scale-out aan *n*-actieve exemplaren
- Geef *n*-actieve en actief-passief-scenario's
- Voorkom de noodzaak voor complexe oplossingen, zoals Apache ZooKeeper-knooppunten voor het bewaken van apparaten

Het volgende diagram toont een virtueel netwerk hub en spoke-implementatie. Geforceerde tunneling in de knooppunten hun verkeer naar de virtuele hub-netwerk en via de NVA, voordat u de vertrouwde ruimte. De NVA's zich achter een interne Standard Load Balancer met de configuratie van een HA-poorten. Al het verkeer kan worden verwerkt en dienovereenkomstig doorgestuurd.

![Diagram van de hub en spoke-virtueel netwerk, met NVA's in de modus HA geïmplementeerd](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Als u met behulp van NVA's, controleert u of met hun leveranciers van het best gebruik HA-poorten en voor meer informatie over welke scenario's worden ondersteund.

### <a name="load-balancing-large-numbers-of-ports"></a>Load balancing groot aantal poorten

U kunt ook de HA-poorten gebruiken voor toepassingen waarvoor de taakverdeling van een groot aantal poorten. U kunt deze scenario's vereenvoudigen met behulp van een interne [Standard Load Balancer](load-balancer-standard-overview.md) met HA-poorten. Een enkele load balancing-regel vervangt meerdere afzonderlijke-taakverdelingsregels, één voor elke poort.

## <a name="region-availability"></a>Beschikbaarheid in regio’s

De HA-poorten-functie is beschikbaar in alle globale Azure-regio's.

## <a name="supported-configurations"></a>Ondersteunde configuraties

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Een configuratie met één en niet-zwevend IP (niet - Direct Server Return) HA-poorten op een interne Standard Load Balancer

Deze configuratie is een basisconfiguratie van de HA-poorten. U kunt configureren dat een HA poorten taakverdeling regel op één front-end-IP-adres door het volgende te doen:
1. Tijdens het configureren van Standard Load Balancer, selecteer de **HA-poorten** selectievakje in de configuratie van de Load Balancer-regel.
2. Voor **zwevend IP**, selecteer **uitgeschakelde**.

Deze configuratie is niet toegestaan voor een andere load balancing regelconfiguratie op de huidige load balancer-resource. Daarnaast kunt u geen andere interne load balancer-resource-configuratie voor de gegeven reeks back-end-exemplaren.

U kunt echter een openbare Standard Load Balancer voor de back-endexemplaren naast deze regel HA-poorten configureren.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Een configuratie met één en zwevend IP (Direct Server Return) HA-poorten op een interne Standard Load Balancer

Op deze manier kunt u de load balancer voor het gebruik van een regel voor taakverdeling met configureren **HA poort** met een één front-end door in te stellen de **zwevend IP** naar **ingeschakeld**. 

U kunt meer zwevende IP-taakverdelingsregels en/of een openbare load balancer toevoegen met behulp van deze configuratie. U kunt geen echter een niet-zwevend IP-adres, HA-poorten gebruiken load balancing-configuratie op deze configuratie.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>Configuraties met meerdere HA-poorten op een interne Standard Load Balancer

Als uw scenario vereist dat u meer dan één HA-poort-front-end voor de dezelfde back-end-pool configureren, kunt u het volgende doen: 
- Meer dan één front-end privé IP-adres configureren voor een enkele interne Standard Load Balancer-resource.
- Configureer meerdere load balancing regels, waarbij elke regel één unieke front-end-IP-adres geselecteerd heeft.
- Selecteer de **HA-poorten** optie en stel vervolgens **zwevend IP** naar **ingeschakeld** voor alle load balancing-regels.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>Een interne load balancer met HA-poorten en een openbare load balancer op hetzelfde exemplaar van de back-end

U kunt configureren *één* openbare Standard Load Balancer-resource voor de back-end-resources, samen met een enkele interne Standard Load Balancer met HA-poorten.

>[!NOTE]
>Deze mogelijkheid is momenteel beschikbaar zijn via Azure Resource Manager-sjablonen, maar het is niet beschikbaar via de Azure-portal.

## <a name="limitations"></a>Beperkingen

- Maximaal beschikbare is poortconfiguratie alleen beschikbaar voor interne load balancers. Het is niet beschikbaar voor openbare load balancers.

- De combinatie van een HA-poorten taakverdelingsregel en een niet-HA-poorten taakverdelingsregel wordt niet ondersteund.

- De functie voor HA-poorten is niet beschikbaar voor IPv6.

- Stroom symmetrie voor NVA's wordt met één NIC alleen ondersteund. Zie de beschrijving en diagram voor [virtuele netwerkapparaten](#nva). Echter, als een doel NAT voor uw scenario werkt kan, kan u deze om te controleren of dat de interne load balancer wordt het retourverkeer verzonden naar de dezelfde NVA.


## <a name="next-steps"></a>Volgende stappen

- [HA-poorten op een interne Standard Load Balancer configureren](load-balancer-configure-ha-ports.md)
- [Meer informatie over de standaardversie van Load Balancer](load-balancer-standard-overview.md)
