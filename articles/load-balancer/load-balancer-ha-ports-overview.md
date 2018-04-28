---
title: Overzicht van hoge beschikbaarheid poorten in Azure | Microsoft Docs
description: Meer informatie over hoge beschikbaarheid poorten taakverdeling op een interne load balancer.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: kumud
ms.openlocfilehash: 53e09498324f802ce1839d262999657d37ee973b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="high-availability-ports-overview"></a>Overzicht van hoge beschikbaarheid-poorten

Azure Load Balancer standaard kunt u taakverdeling TCP en UDP-overdrachten op alle poorten tegelijkertijd wanneer u een interne load balancer. 

De regel voor een hoge beschikbaarheid (HA)-poorten is een variant van een regel voor load balancing, geconfigureerd op een interne standaard Load Balancer. U kunt het gebruik van een load balancer vereenvoudigen door een enkele regel te verdelen alle TCP en UDP-stromen die op alle poorten van een interne standaard Load Balancer binnenkomen. De beslissing taakverdeling wordt per overdracht uitgevoerd. Deze actie is gebaseerd op de volgende 5-tuple verbinding: bron-IP-adres, bronpoort, IP-doeladres, doelpoort en -protocol.

De functie HA-poorten kunt u met essentiële scenario's, zoals hoge beschikbaarheid en schaal voor de virtuele netwerkapparaten (NVAs) in virtuele netwerken. De functie kan ook helpen bij een groot aantal poorten verdeeld worden moet. 

De functie van de poorten HA is geconfigureerd als u de front-end en back-end-poorten ingesteld op **0** en het protocol voor **alle**. De interne load balancer-resource vervolgens een compromis tussen alle TCP en UDP-stromen, ongeacht het poortnummer.

## <a name="why-use-ha-ports"></a>Waarom HA poorten gebruiken?

### <a name="nva"></a>Virtuele netwerkapparaten

U kunt NVAs gebruiken om u te helpen beveiligen uw Azure-workload uit meerdere typen beveiligingsrisico's. Wanneer u NVAs in deze scenario's gebruikt, moeten ze betrouwbaar en maximaal beschikbare, en ze moeten worden uitgebreid voor de vraag.

U kunt deze doelstellingen kunt bereiken door NVA exemplaren toe te voegen aan de back-end-pool van de interne load balancer en load balancer-regel voor het configureren van een HA-poorten.

HA-poorten bieden voor NVA HA scenario's met de volgende voordelen:
- Snelle failover op in orde exemplaren voorzien van statuscontroles per exemplaar
- Hogere prestaties met scale-out voor *n*-actieve exemplaren
- Geef *n*-actief en actief / passief-scenario's
- Overbodig complexe oplossingen, zoals Apache ZooKeeper-knooppunten voor de bewaking van apparaten

Het volgende diagram toont een virtueel netwerk hub en spoke-implementatie. Geforceerde tunneling in de spaken hun verkeer van het virtuele netwerk hub en via de NVA, voordat u de vertrouwde ruimte verlaat. De NVAs zich achter een interne standaard Load Balancer is met de configuratie van een HA-poorten. Al het verkeer worden verwerkt en dienovereenkomstig doorgestuurd.

![Diagram van hub en spoke-virtueel netwerk met NVAs geïmplementeerd in de HA-modus](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Als u NVAs gebruikt, controleert u met hun providers het beste HA-poorten te gebruiken en voor meer informatie over welke scenario's worden ondersteund.

### <a name="load-balancing-large-numbers-of-ports"></a>Taakverdeling grote aantallen poorten

U kunt ook de HA-poorten gebruiken voor toepassingen waarvoor taakverdeling van grote aantallen poorten. U kunt deze scenario's met behulp van een interne vereenvoudigen [standaard Load Balancer](load-balancer-standard-overview.md) met HA-poorten. Een enkele regel voor load balancing vervangt meerdere afzonderlijke taakverdeling regels, één voor elke poort.

## <a name="region-availability"></a>Beschikbaarheid in regio’s

De HA-poorten-functie is beschikbaar in alle globale Azure-regio's.

## <a name="supported-configurations"></a>Ondersteunde configuraties

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Een configuratie met één en niet-zwevend IP (non - Direct Server Return) HA-poorten op een interne standaard Load Balancer

Deze configuratie is een basisconfiguratie van de HA-poorten. U kunt een HA poorten load balancing-regel op één front-end-IP-adres door het volgende te doen:
1. Tijdens het configureren van standaard Load Balancer, selecteer de **HA poorten** selectievakje in de configuratie van de Load Balancer-regel.
2. Voor **zwevend IP**, selecteer **uitgeschakelde**.

Deze configuratie is niet toegestaan voor een andere load balancing regelconfiguratie op de huidige load balancer-resource. Kunt u ook geen andere interne load balancer resourceconfiguratie voor de gegeven reeks back-end-exemplaren.

U kunt echter een openbare standaard Load Balancer voor de back-end-exemplaren naast deze regel HA-poorten configureren.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Een configuratie met één en zwevend IP (Direct Server Return) HA-poorten op een interne standaard Load Balancer

U kunt ook de load balancer voor het gebruik van een regel voor load balancing met configureren **HA poort** met een één front-end door in te stellen de **zwevend IP** naar **ingeschakeld**. 

U kunt zwevend IP-load balancing regels en/of een openbare load balancer toevoegen met behulp van deze configuratie. U kunt een niet-zwevend IP-adres, HA-poorten echter niet gebruiken taakverdeling configuratie boven deze configuratie op.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>Configuraties met meerdere HA-poorten op een interne standaard Load Balancer

Als uw scenario vereist dat u meer dan één HA poort front-end voor de dezelfde back-end-pool configureert, kunt u het volgende doen: 
- Meer dan één front-persoonlijke IP-adres configureren voor een enkele interne standaard Load Balancer-resource.
- Configureer meerdere load balancing regels, waarbij elke regel een één unieke front-end-IP-adres geselecteerd heeft.
- Selecteer de **HA poorten** optie en stel vervolgens **zwevend IP** naar **ingeschakeld** voor alle regels load balancing.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>Een interne load balancer met HA poorten en een openbare load balancer op hetzelfde exemplaar van de back-end

U kunt configureren *één* openbare standaard Load Balancer-resource voor de back-end-resources, samen met een enkele interne standaard Load Balancer met HA-poorten.

>[!NOTE]
>Deze mogelijkheid is momenteel beschikbaar is via Azure Resource Manager-sjablonen, maar het is niet beschikbaar via de Azure-portal.

## <a name="limitations"></a>Beperkingen

- HA is poortconfiguratie alleen beschikbaar voor interne load balancers. Het is niet beschikbaar voor openbare load balancers.

- De combinatie van een HA poorten regel voor load balancing en een niet-HA poorten regel taakverdeling wordt niet ondersteund.

- De functie HA-poorten is niet beschikbaar voor IPv6.

- Stroom symmetrie voor NVA scenario's wordt met één NIC ondersteund. Zie de beschrijving en diagram voor [virtuele apparaten](#nva). Echter als doel de NAT voor uw scenario werkt kan, kunt u deze om ervoor te zorgen dat de interne load balancer terugkerend verkeer naar de dezelfde NVA verzendt.


## <a name="next-steps"></a>Volgende stappen

- [HA-poorten configureren op een interne standaard Load Balancer](load-balancer-configure-ha-ports.md)
- [Meer informatie over Standard Load Balancer](load-balancer-standard-overview.md)
