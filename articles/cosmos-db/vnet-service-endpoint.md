---
title: Toegang tot een Azure Cosmos DB-account beveiligen met behulp van Azure Virtual Network service-eindpunt
description: Dit document beschrijft over virtueel netwerk en subnet-toegangsbeheer voor een Azure Cosmos-account.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: govindk
ms.openlocfilehash: 5f3ee4e5af32ff0ea90d8807c65d14ddce417e7b
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51626079"
---
# <a name="access-azure-cosmos-db-resources-from-virtual-networks"></a>Toegang tot Azure Cosmos DB-resources van virtuele netwerken

U kunt de Azure Cosmos-account om toe te staan alleen toegankelijk is vanaf een specifiek subnet van het virtuele netwerk (VNET) configureren. Door in te schakelen [Service-eindpunt](../virtual-network/virtual-network-service-endpoints-overview.md) voor toegang tot Azure Cosmos DB op het subnet binnen een virtueel netwerk, het verkeer op dat subnet wordt verzonden met Azure Cosmos DB met de identiteit van het subnet en het Virtueelnetwerk. Zodra het Azure Cosmos DB-service-eindpunt is ingeschakeld, kunt u toegang tot het subnet beperken door toe te voegen aan uw Azure Cosmos-account.

Een Azure Cosmos-account is standaard toegankelijk is vanaf elke willekeurige bron als de aanvraag wordt vergezeld gaan van een geldige Autorisatietoken. Wanneer u een of meer subnetten binnen VNETs toevoegt, krijgt alleen aanvragen die afkomstig zijn van deze subnetten een geldige reactie. Aanvragen die afkomstig zijn uit andere bronnen worden beantwoord een 404 (niet gevonden). 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

Hier volgen enkele veelgestelde vragen over het configureren van toegang van virtuele netwerken:

### <a name="can-i-specify-both-virtual-network-service-endpoint-and-ip-access-control-policy-on-an-azure-cosmos-account"></a>Kan ik zowel service-eindpunt voor virtueel netwerk en IP-beleid voor toegangsbeheer opgeven voor een Azure Cosmos-account? 

U kunt zowel de service-eindpunt van het virtuele netwerk en een IP-beleid voor toegangsbeheer (ook wel firewall) inschakelen voor uw Azure Cosmos-account. Deze twee functies zijn aanvullende en gezamenlijk zorgen isolatie en beveiliging van uw Azure Cosmos-account. Met IP-garandeert firewall dat statische IP-adressen toegang uw account tot krijgen. 

### <a name="how-do-i-limit-access-to-subnet-within-a-virtual-network"></a>Hoe beperk ik de toegang tot subnet binnen een virtueel netwerk? 

Er zijn twee stappen die nodig zijn om toegang te beperken naar Azure Cosmos-account van een subnet. Eerst, kunt u verkeer van het subnet voor het uitvoeren van het subnet en de identiteit van de virtueel netwerk met Azure Cosmos DB. Dit wordt gedaan door het inschakelen van service-eindpunt voor Azure Cosmos DB op het subnet. Vervolgens wordt toegevoegd een regel in het Azure Cosmos-account dat dit subnet op te geven als een bron waaruit account kan worden geopend.

### <a name="will-virtual-network-acls-and-ip-firewall-reject-requests-or-connections"></a>Virtueel netwerk ACL's en IP-Firewall weigert aanvragen of verbindingen? 

Als IP-firewall of een virtueel netwerk toegang hebben tot worden regels toegevoegd, alleen aanvragen van de geldige antwoorden voor toegestane bronnen ophalen. Andere aanvragen zijn afgewezen met een 404 (niet gevonden). Het is belangrijk om te onderscheiden van Azure Cosmos-account firewall van een firewall voor het niveau van verbindingen. De bron, kunt u wel verbinding maken met de service en de verbindingen zelf niet worden geweigerd.

### <a name="my-requests-started-getting-blocked-when-i-enabled-service-endpoint-to-azure-cosmos-db-on-the-subnet-what-happened"></a>Mijn aanvragen aan de slag geblokkeerd wanneer ik de service-eindpunt voor Azure Cosmos DB op het subnet ingeschakeld. Wat is er gebeurd?

Zodra de service-eindpunt voor Azure Cosmos DB is ingeschakeld op een subnet, verandert de bron van het bereiken van het account verkeer vanaf openbare IP-adres op virtueel netwerk en subnet. Als uw Azure Cosmos-account op basis van IP-firewall heeft alleen verkeer van het subnet van de service is ingeschakeld zou niet meer overeen met de IP-firewall-regels en daarom worden geweigerd. Ga via de stappen voor het naadloos migreren van IP-gebaseerde firewall naar virtuele netwerk gebaseerd toegangsbeheer.

### <a name="do-the-peered-virtual-networks-also-have-access-to-azure-cosmos-account"></a>De gekoppelde virtuele netwerken ook toegang hebben tot Azure Cosmos-account? 
Alleen virtuele netwerken en hun subnetten die zijn toegevoegd aan Azure Cosmos-account hebben toegang. De gekoppelde VNETs heeft geen toegang tot het account totdat de subnetten in gekoppelde virtuele netwerken worden toegevoegd aan het account.

### <a name="what-is-the-maximum-number-of-subnets-allowed-to-access-a-single-cosmos-account"></a>Wat is het maximum aantal subnetten toegestaan voor toegang tot een enkel Cosmos-account? 
U kunt op dit moment maximaal 64 subnetten toegestaan voor een Azure Cosmos-account hebben.

### <a name="can-i-enable-access-from-vpn-and-express-route"></a>Kan ik de toegang van VPN en Expressroute inschakelen? 
Voor toegang tot Azure Cosmos-account via de Express-route van on-premises, moet u Microsoft-peering inschakelen. Wanneer u IP-firewall of een virtueel netwerktoegangsregels plaatst, kunt u het openbare IP-adressen die worden gebruikt voor Microsoft-peering in uw Azure Cosmos-account IP-firewall om toe te staan op de lokale services toegang tot Azure Cosmos-account toevoegen. 

### <a name="do-i-need-to-update-the-network-security-groups-nsg-rules"></a>Heb ik nodig om bij te werken van de regels van de Netwerkbeveiligingsgroep groepen (NSG)? 
NSG-regels worden gebruikt om de connectiviteit van en naar een subnet met een virtueel netwerk beperken. Wanneer u service-eindpunt voor Azure Cosmos DB aan het subnet toevoegt, is er niet nodig om te openen van de uitgaande connectiviteit in NSG voor uw Azure Cosmos-account. 

### <a name="are-service-endpoints-available-for-all-vnets"></a>Service-eindpunten zijn beschikbaar voor alle vnet's?
Azure Resource Manager alleen virtuele netwerken kunnen geen service-eindpunt ingeschakeld. Klassieke virtuele netwerken bieden geen ondersteuning voor service-eindpunten.


## <a name="next-steps"></a>Volgende stappen

* [Hoe u Azure Cosmos-accounttoegang tot de subnetten in virtuele netwerken beperken](how-to-configure-vnet-service-endpoint.md)
* [IP-firewall voor uw Azure Cosmos-account configureren](how-to-configure-firewall.md)

