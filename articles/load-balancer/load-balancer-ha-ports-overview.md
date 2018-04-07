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
ms.openlocfilehash: f6e9dd09558a3485629d5b70dd8b68b292427b18
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="high-availability-ports-overview"></a>Overzicht van hoge beschikbaarheid-poorten

Azure Load Balancer standaard kunt u tegelijk, saldo TCP en UDP-overdrachten op alle poorten wordt geladen wanneer u een interne Load Balancer. 

Een regel voor HA-poorten is een variant van een regel op een interne standaard Load Balancer geconfigureerd voor taakverdeling. U kunt uw gebruik van Load Balancer vereenvoudigen door een enkele regel om taken te verdelen alle TCP en UDP-stromen die binnenkomen op alle poorten van een interne standaard Load Balancer. De beslissing voor taakverdeling wordt per overdracht uitgevoerd. Dit is gebaseerd op de volgende 5-tuple verbinding: bron-IP-adres, bronpoort, IP-doeladres, doelpoort en -protocol.

De functie HA-poorten kunt u met essentiële scenario's, zoals hoge beschikbaarheid en schaal voor de virtuele netwerkapparaten (NVA) binnen de virtuele netwerken. Ook kunt u wanneer een groot aantal poorten verdeeld worden moet. 

De functie van de poorten HA is geconfigureerd als u de front-end en back-end-poorten ingesteld op **0**, en het protocol voor **alle**. De interne Load Balancer-resource vervolgens een compromis tussen alle TCP en UDP-stromen, ongeacht het poortnummer.

## <a name="why-use-ha-ports"></a>Waarom HA poorten gebruiken?

### <a name="nva"></a>Virtuele netwerkapparaten

U kunt NVAs gebruiken voor het beveiligen van uw Azure-workload uit meerdere typen beveiligingsrisico's. Wanneer NVAs worden gebruikt in deze scenario's, moeten ze betrouwbaar en maximaal beschikbare, en ze moeten worden uitgebreid voor de vraag.

U kunt deze doelstellingen kunt bereiken door NVA exemplaren toevoegen aan de back-end-pool van de Azure interne Load Balancer en het configureren van een HA-poorten Load Balancer-regel.

HA bieden poorten verschillende voordelen voor NVA HA-scenario's:
- Snelle failover op in orde exemplaren, met statuscontroles per exemplaar
- Betere prestaties, met de scale-out voor *n*-actieve exemplaren
- *N*-actief en actief / passief-scenario's
- Hoeft u de complexe oplossingen zoals Apache ZooKeeper-knooppunten voor de bewaking van apparaten

Het volgende diagram toont een virtueel netwerk hub en spoke-implementatie. Geforceerde tunneling in de spaken hun verkeer van het virtuele netwerk hub en via de NVA, voordat u de vertrouwde ruimte verlaat. De NVAs zich achter een interne standaard Load Balancer is met de configuratie van een HA-poorten. Al het verkeer worden verwerkt en dienovereenkomstig doorgestuurd.

![Diagram van hub en spoke-virtueel netwerk met NVAs geïmplementeerd in de HA-modus](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Als u NVAs gebruikt, controleert u met de respectieve provider voor optimaal gebruik HA poorten en welke scenario's worden ondersteund.

### <a name="load-balancing-large-numbers-of-ports"></a>Taakverdeling van een groot aantal poorten

U kunt ook de HA-poorten gebruiken voor toepassingen waarvoor taakverdeling van grote aantallen poorten. U kunt deze scenario's met behulp van een interne vereenvoudigen [standaard Load Balancer](load-balancer-standard-overview.md) met HA-poorten. Een enkele taakverdelingsregel vervangt meerdere afzonderlijke taakverdelingsregels, één voor elke poort.

## <a name="region-availability"></a>Beschikbaarheid in regio’s

De HA-poorten-functie is beschikbaar in alle globale Azure-regio's.

## <a name="supported-configurations"></a>Ondersteunde configuraties

### <a name="one-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-the-internal-standard-load-balancer"></a>Één enkele zonder zwevend IP (non - Direct Server Return) HA poortconfiguratie op de interne standaard Load Balancer

Dit is een basisconfiguratie HA poorten. De volgende configuratie kunt u HA poorten voor de load balancer op een enkele frontend-IP-adres - configureren
- Selecteer bij het configureren van uw standaard Load Balancer **HA poorten** selectievakje in de configuratie van de regel voor Load Balancer 
- Om ervoor te zorgen **zwevend IP** is ingesteld op **uitgeschakelde**.

Deze configuratie is niet toegestaan voor een andere load balancing regelconfiguratie op de huidige bron voor de Load Balancer, evenals geen andere interne load balancer resourceconfiguratie voor de gegeven reeks back-end-exemplaren.

U kunt echter een openbare standaard Load Balancer voor de back-end-exemplaren naast deze regel HA poort configureren.

### <a name="one-single-floating-ip-direct-server-return-ha-ports-configuration-on-the-internal-standard-load-balancer"></a>Een zwevend IP (Direct Server Return) HA poorten configuratie voor één op de interne standaard Load Balancer

U kunt ook de load balancer voor het gebruik van een regel met voor taakverdeling configureren **HA poort** met een één front-end en de **zwevend IP** ingesteld op **ingeschakeld**. 

Deze configuratie kunt u meer zwevend IP-load balancing-regels en / of een openbare Load Balancer toevoegen. U kunt een niet - zwevend IP-HA boad taakverdeling poortconfiguratie boven op deze configuratie echter niet gebruiken.

### <a name="multiple-ha-ports-configurations-on-the-internal-standard-load-balancer"></a>Configuraties met meerdere HA poorten op de interne standaard Load Balancer

Als uw scenario vereist dat u meer dan één HA poort frontends voor dezelfde back-endadresgroep configureert, kunt u dit doen door: 
- configureren van meer dan één frontend adressen persoonlijke IP-voor een enkele interne standaard Load Balancer-resource.
- Configureer meerdere regels, waarbij elke regel één heeft taakverdeling unieke frontend-IP-adres is geselecteerd.
- Selecteer **HA poorten** optie en stel **zwevend IP** naar **ingeschakeld** voor alle van de load-balancingregels.

### <a name="internal-load-balancer-with-ha-ports--public-load-balancer-on-the-same-backend-instances"></a>Interne Load Balancer met HA poorten & openbare Load Balancer op dezelfde back-end-exemplaren

U kunt configureren **één** openbare standaard Load Balancer-resource voor de back endresources samen met een enkele interne standaard Load Balancer met HA-poorten.

>[!NOTE]
>Deze mogelijkheid is beschikbaar via vandaag nog Azure Resource Manager-sjablonen, maar niet via de Azure-portal.

## <a name="limitations"></a>Beperkingen

- HA poortconfiguratie is alleen beschikbaar voor interne Load Balancer, het is niet beschikbaar voor een openbare Load Balancer.

- Een combinatie van regel HA poorten Load Balancing en de regel niet - HA poorten taakverdeling wordt niet ondersteund.

- De functie van de HA-poorten is niet beschikbaar voor IPv6.

- Stroom symmetrie voor NVA scenario's wordt met één NIC ondersteund. Zie de beschrijving en diagram voor [virtuele apparaten](#nva). Echter, als een bestemming NAT voor uw scenario werken kunt, u kunt die om ervoor te zorgen dat de interne Load Balancer terugkerend verkeer verzendt naar de dezelfde NVA.


## <a name="next-steps"></a>Volgende stappen

- [HA poorten op een interne standaard Load Balancer configureren](load-balancer-configure-ha-ports.md)
- [Meer informatie over Standard Load Balancer](load-balancer-standard-overview.md)
