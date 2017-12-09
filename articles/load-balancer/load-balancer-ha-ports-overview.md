---
title: Overzicht van hoge beschikbaarheid poorten in Azure | Microsoft Docs
description: Meer informatie over hoge beschikbaarheid poorten taakverdeling op een interne load balancer.
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: 46e284d1636988390f3533d93bfd07399f45dc92
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="high-availability-ports-overview"></a>Overzicht van hoge beschikbaarheid-poorten

Azure Load Balancer standaard kunt u tegelijk, saldo TCP en UDP-overdrachten op alle poorten wordt geladen wanneer u een interne Load Balancer. 

>[!NOTE]
> De functie voor hoge beschikbaarheid (HA)-poorten is beschikbaar met Load Balancer standaard en is momenteel in preview. Tijdens de preview hebben de functie mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid als de functies die zich in de release van de algemene beschikbaarheid. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie. Aanmelden voor de Load Balancer standaard preview HA-poorten te gebruiken met Load Balancer standaard resources. Volg de instructies voor aanmelding met Load Balancer [standaard preview](https://aka.ms/lbpreview#preview-sign-up) ook.

De regel van een HA-poorten is een variant van een regel op een interne Load Balancer standaard geconfigureerd voor taakverdeling. Door een enkele regel om taken te verdelen alle TCP en UDP-stromen die binnenkomen op alle poorten van een interne Load Balancer standaard kunt u uw gebruik van Load Balancer vereenvoudigen. De beslissing voor taakverdeling wordt per overdracht uitgevoerd. Dit is gebaseerd op de volgende 5-tuple verbinding: IP-bronadres, bronpoort, IP-doeladres, doelpoort en -Protocol.

De functie HA-poorten kunt u met essentiële scenario's, zoals hoge beschikbaarheid en schaal voor de virtuele netwerkapparaten (NVA) binnen de virtuele netwerken. Ook kunt u wanneer een groot aantal poorten verdeeld worden moet. 

De functie van de poorten HA is geconfigureerd als u de front-end en back-end-poorten ingesteld op **0**, en het protocol voor **alle**. De interne Load Balancer-resource vervolgens een compromis tussen alle TCP en UDP-stromen, ongeacht het poortnummer.

## <a name="why-use-ha-ports"></a>Waarom HA poorten gebruiken?

### <a name="nva"></a>Virtuele netwerkapparaten

U kunt NVAs gebruiken voor het beveiligen van uw Azure-workload uit meerdere typen beveiligingsrisico's. Wanneer NVAs worden gebruikt in deze scenario's, moeten ze betrouwbaar en maximaal beschikbare, en ze moeten worden uitgebreid voor de vraag.

U kunt deze doelstellingen kunt bereiken door NVA exemplaren toevoegen aan de back-end-pool van de Azure interne Load Balancer en het configureren van een HA-poorten Load Balancer-regel.

HA bieden poorten verschillende voordelen voor NVA HA-scenario's:
- Snelle failover op in orde exemplaren, met statuscontroles per exemplaar
- Betere prestaties, met de scale-out voor  *n* -actieve exemplaren
- *N*-actief en actief / passief-scenario's
- Hoeft u de complexe oplossingen zoals Apache ZooKeeper-knooppunten voor de bewaking van apparaten

Het volgende diagram toont een virtueel netwerk hub en spoke-implementatie. Geforceerde tunneling in de spaken hun verkeer van het virtuele netwerk hub en via de NVA, voordat u de vertrouwde ruimte verlaat. De NVAs zich achter een interne Load Balancer Standard met de configuratie van een HA-poorten. Al het verkeer worden verwerkt en dienovereenkomstig doorgestuurd.

![Diagram van hub en spoke-virtueel netwerk met NVAs geïmplementeerd in de HA-modus](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Als u NVAs gebruikt, controleert u met de respectieve provider voor optimaal gebruik HA poorten en welke scenario's worden ondersteund.

### <a name="load-balancing-large-numbers-of-ports"></a>Taakverdeling van een groot aantal poorten

U kunt ook de HA-poorten gebruiken voor toepassingen waarvoor taakverdeling van grote aantallen poorten. U kunt deze scenario's met behulp van een interne vereenvoudigen [Load Balancer standaard](https://aka.ms/lbpreview) met HA-poorten. Een enkele taakverdelingsregel vervangt meerdere afzonderlijke taakverdelingsregels, één voor elke poort.

## <a name="region-availability"></a>Beschikbaarheid in regio’s

De HA-poorten-functie is beschikbaar in de [dezelfde regio's als Load Balancer standaard](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Preview-registratie

Als u wilt deelnemen aan de evaluatieversie van de functie van de HA-poorten in Load Balancer standaard Registreer uw abonnement voor de Load Balancer [standaard preview](https://aka.ms/lbpreview#preview-sign-up). U kunt registreren met Azure CLI 2.0 of PowerShell.

>[!NOTE]
>Registratie kan een uur duren.

## <a name="limitations"></a>Beperkingen

Hier volgen de ondersteunde configuraties of uitzonderingen voor de functie HA-poorten:

- Een IP-configuratie voor één front-kan een enkele Direct Server Return (DSR - zwevend IP in Azure) load balancer-regel met HA-poorten, of deze een enkele niet DSR load balancer-regel met HA poorten kan hebben. Er kan niet beide.
- Een netwerk met één interface IP-configuratie kan slechts één niet-DSR load balancer-regel met HA poorten hebben. U kunt nog andere regels voor deze ipconfig niet configureren.
- Een netwerk met één interface IP-configuratie kan een of meer DSR load-balancerregels met HA-poorten zijn opgegeven alle hun respectieve front-end-IP-configuraties uniek zijn.
- Als alle van de load-balancingregels HA-poorten (alleen DSR), worden twee (of meer) Load Balancer-regels die verwijst naar de dezelfde back-end-pool kunnen naast elkaar bestaan. Hetzelfde geldt als alle regels niet zijn-HA-poorten (DSR en niet-DSR). Naast elkaar als er een combinatie van de HA-poorten en poorten niet HA regels, echter twee dergelijke regels voor taakverdeling kunnen niet bestaan.
- De functie HA-poorten is niet beschikbaar voor IPv6.
- Stroom symmetrie voor NVA scenario's wordt met één NIC ondersteund. Zie de beschrijving en diagram voor [virtuele apparaten](#nva). 



## <a name="next-steps"></a>Volgende stappen

- [HA-poorten configureren op een interne Load Balancer Standard](load-balancer-configure-ha-ports.md)
- [Meer informatie over de Load Balancer standaard preview](https://aka.ms/lbpreview)

