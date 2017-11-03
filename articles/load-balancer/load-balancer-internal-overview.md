---
title: Overzicht van Azure interne Load Balancer | Microsoft Docs
description: Hoe werkt een interne load balancer in Azure en scenario's voor het configureren van interne eindpunten.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 54e390dbdb07cb4c45c801b638099aa0dcc6db1a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="overview-of-azure-internal-load-balancer"></a>Overzicht van Azure interne Load Balancer

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Azure interne Load Balancer (ILB) stuurt alleen verkeer naar bronnen die zich in een cloudservice of die een VPN gebruiken voor toegang tot Azure-infrastructuur. In dit opzicht verschilt ILB van een internet gerichte load balancer. Azure-infrastructuur beperkt de toegang aan de adressen virtuele IP-adres (VIP) taakverdeling van een cloudservice of aan een virtueel netwerk. VIP-adressen en virtuele netwerken worden nooit rechtstreeks blootgesteld aan een internet-eindpunt. Interne line-of-business-toepassingen worden uitgevoerd in Azure en zijn toegankelijk vanuit Azure of via lokale bronnen.

## <a name="why-you-might-need-an-internal-load-balancer"></a>Waarom moet u mogelijk een interne load balancer

Interne Load Balancer zorgt voor taakverdeling tussen virtuele machines (VM's) die zich in een cloudservice of een virtueel netwerk met een regionaal bereik bevinden. Zie voor meer informatie over virtuele netwerken met een regionaal bereik [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in de Azure-blog. Bestaande virtuele netwerken die zijn geconfigureerd voor een affiniteitsgroep kunnen ILB niet gebruiken.

ILB maakt gebruik van de volgende soorten taakverdeling:

* Binnen een cloudservice: taakverdeling van VM's aan een set van virtuele machines die zich in dezelfde cloudservice bevinden. Zie dit <a href="#figure1">voorbeeld</a>.
* Binnen een virtueel netwerk: taakverdeling van VM's in het virtuele netwerk naar een set van virtuele machines die zich in dezelfde cloudservice van het virtuele netwerk bevinden. Zie dit <a href="#figure2">voorbeeld</a>.
* Voor een virtueel netwerk met cross-premises: taakverdeling van lokale computers aan een set van virtuele machines die zich in dezelfde cloudservice van het virtuele netwerk bevinden. Zie dit <a href="#figure3">voorbeeld</a>.
* Voor toepassingen met meerdere lagen: Load balancing voor toepassingen met meerdere lagen internetgerichte waarin de back-end-lagen niet verbonden met internet zijn. De back-end-lagen vereisen verkeer taakverdeling van de tier internetgerichte.
* Voor line-of-business-toepassingen: taakverdeling voor line-of-business-toepassingen die worden gehost in Azure zonder extra load balancer-hardware of software. Dit scenario omvat het on-premises servers die in de set van computers waarvan het verkeer verdeeld wordt.

## <a name="load-balancing-for-internet-facing-multi-tier-applications"></a>Taakverdeling voor toepassingen met meerdere lagen internetgerichte

De weblaag internetgerichte eindpunten voor internet-clients heeft en deel uitmaakt van een set met gelijke taakverdeling. ILB verdeelt inkomende verkeer van de webserver en webclients voor TCP-poort 443 (HTTPS) met de webservers.

De database-servers zich achter een ILB-eindpunt dat de webservers voor de opslag gebruikt. Het ILB-eindpunt is een database Netwerktaakverdeling-eindpunt. Verkeer wordt verdeeld over de databaseservers in de set ILB.

De volgende afbeelding toont de interne load balancer voor de toepassing met meerdere lagen internetgerichte binnen dezelfde cloudservice.

<a name="figure1"></a>
![Internetgerichte toepassing met meerdere lagen](./media/load-balancer-internal-overview/IC736321.png)

Een ander scenario is beschikbaar voor toepassingen met meerdere lagen. De load balancer wordt geïmplementeerd op een andere cloud-service van de versie die de service voor de ILB verbruikt.

Cloudservices die gebruikmaken van hetzelfde virtuele netwerk hebben toegang tot de ILB-eindpunt. De volgende afbeelding toont de front-end-webservers die zich in een andere cloud-service van de database van de back-end. De front-end-servers gebruiken de ILB-eindpunt binnen hetzelfde virtuele netwerk als de back-end.

<a name="figure2"></a>
![Front-end-servers in een andere cloudservice](./media/load-balancer-internal-overview/IC744147.png)

## <a name="load-balancing-for-intranet-line-of-business-applications"></a>Taakverdeling voor intranet line-of-business-toepassingen

Verkeer van clients op de on-premises netwerk is taakverdeling voor de set line-of-business-servers die gebruikmaken van een VPN-verbinding met de Azure-netwerk.

De clientcomputer toegang tot een IP-adres van de Azure VPN-service met behulp van een punt-naar-site-VPN. De line-of-business-toepassing kan worden gehost achter de ILB-eindpunt.

<a name="figure3"></a>
![LOB-toepassing die wordt gehost achter ILB-eindpunt](./media/load-balancer-internal-overview/IC744148.png)

Een ander scenario voor line-of-business-toepassingen is een site-naar-site-VPN naar het virtuele netwerk waarin de ILB-eindpunt is geconfigureerd. Lokale-netwerkverkeer wordt doorgestuurd naar de ILB-eindpunt.

<a name="figure4"></a>
![Lokale netwerkverkeer doorgestuurd naar ILB-eindpunt](./media/load-balancer-internal-overview/IC744150.png)

## <a name="limitations"></a>Beperkingen

Interne Load Balancer-configuraties ondersteunen geen snat omzetten. In dit artikel verwijst snat omzetten naar scenario's voor bron netwerkadresomzetting zich voordoet op poort. Een virtuele machine in een pool van load balancer moet de front-end-IP-adres van de respectieve interne load balancer bereiken. Verbindingsfouten optreden wanneer de stroom is netwerktaakverdeling naar de virtuele machine die afkomstig zijn van de stroom. Deze scenario's worden niet ondersteund voor de ILB. Een proxy-stijl load balancer moet in plaats daarvan worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

* [Ondersteuning voor Azure Load Balancer van Azure Resource Manager](load-balancer-arm.md)
* [Aan de slag met een internetgerichte load balancer configureren](load-balancer-get-started-internet-arm-ps.md)
* [Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-ps.md)
* [Distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)
* [TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)
