---
title: VPN-Gateway migratie van klassiek naar Resource Manager | Microsoft Docs
description: Deze pagina bevat een overzicht van de VPN-Gateway-klassieke naar Resource Manager-migratie.
documentationcenter: na
services: vpn-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: caa8eb19-825a-4031-8b49-18fbf3ebc04e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: amsriva
ms.openlocfilehash: b65b47389611bcc0e5acb3c7ebff672f72a87581
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60761565"
---
# <a name="vpn-gateway-classic-to-resource-manager-migration"></a>VPN-Gateway migratie van klassiek naar Resource Manager
VPN-Gateways kunnen nu worden gemigreerd van klassiek naar Resource Manager-implementatiemodel. Meer informatie over Azure Resource Manager [functies en voordelen](../azure-resource-manager/resource-group-overview.md). In dit artikel wordt gedetailleerd we beschreven hoe u het migreren van klassieke implementaties naar nieuwere op basis van Resource Manager-model. 

VPN-Gateways worden gemigreerd als onderdeel van de VNet-migratie van klassiek naar Resource Manager. Deze migratie wordt een VNet uitgevoerd op een tijdstip. Er is geen aanvullende vereiste hulpprogramma's of vereisten voor migratie. Migratiestappen zijn identiek aan de migratie van bestaande VNet en worden beschreven op [pagina voor migratie van IaaS-resources](../virtual-machines/windows/migration-classic-resource-manager-ps.md). Er is geen gegevens pad-downtime tijdens de migratie en bestaande workloads wilt blijven werken zonder verlies van gegevens van on-premises connectiviteit tijdens de migratie. Het openbare IP-adres dat is gekoppeld aan de VPN-gateway wordt niet gewijzigd tijdens het migratieproces. Dit betekent dat niet u uw on-premises router opnieuw configureren moet nadat de migratie is voltooid.  

Het model in Resource Manager wijkt af van het klassieke model en bestaat uit virtuele netwerkgateways, lokale netwerkgateways en verbindingsresources. Deze staan voor de VPN-gateway zelf, de lokale-site respectievelijk on premises adresruimte en connectiviteit tussen de twee. Nadat de migratie is voltooid uw gateways niet beschikbaar zijn in het klassieke model en alle bewerkingen op virtuele netwerkgateways, lokale netwerkgateways en connection-objecten moeten worden uitgevoerd met behulp van Resource Manager-model.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
Meest voorkomende scenario's voor VPN-verbindingen zijn inbegrepen bij het klassieke model naar Resource Manager-migratie. De ondersteunde scenario's zijn:

* Verwijzen naar de site-connectiviteit
* Connectiviteit tussen sites met VPN-Gateway verbonden met on premises locatie
* VNet-naar-VNet-connectiviteit tussen twee VNets met VPN-gateways
* Meerdere VNets die is verbonden met hetzelfde on premises locatie
* Meerdere site-connectiviteit
* VNets geforceerde tunnels zijn ingeschakeld

Scenario's die niet worden ondersteund zijn:  

* VNet met zowel ExpressRoute-Gateway en VPN-Gateway wordt momenteel niet ondersteund.
* Doorvoer scenario's waarin de VM-extensies zijn verbonden met on-premises servers. Doorvoer VPN-connectiviteit beperkingen worden hieronder beschreven.

> [!NOTE]
> CIDR-validatie in Resource Manager-model is meer strikt dan in het klassieke model. Zorg ervoor dat klassieke adresbereiken opgegeven aan de geldige CIDR-indeling hebben voldoen voordat u begint met de migratie voordat u migreert. CIDR kan worden gevalideerd met behulp van een algemene CIDR-systeemstatuscontrolepunten. VNet- of lokale sites met een ongeldige CIDR-bereiken wanneer gemigreerd zou leiden tot mislukte status.
> 
> 

## <a name="vnet-to-vnet-connectivity-migration"></a>VNet-naar-VNet-connectiviteit migratie
VNet-naar-VNet-connectiviteit in de klassieke versie werd bereikt door het maken van een lokale site-weergave van het gekoppelde VNet. Klanten zijn vereist voor het maken van twee lokale sites die de twee vnet's die nodig is om te worden met elkaar verbonden weergegeven. Deze zijn verbonden met het bijbehorende vnet's met behulp van IPsec-tunnel om verbinding tussen de twee vnet's te maken. Dit model heeft beheerbaarheid uitdagingen omdat wijzigingen adresbereik in het ene VNet moeten ook worden beheerd in de weergave van het bijbehorende lokale site. Deze tijdelijke oplossing is niet meer nodig in Resource Manager-model. De verbinding tussen de twee VNets kan rechtstreeks worden bereikt met behulp van 'Vnet2Vnet-verbindingstype in resource verbinding. 

![Schermafbeelding van de VNet-van VNet migreren.](./media/vpn-gateway-migration/migration1.png)

Tijdens de migratie van de VNet detecteren we dat de gekoppelde entiteit met huidige VNet-VPN-gateway is een ander VNet en ervoor te zorgen dat zodra de migratie van beide Vnetten is voltooid, niet meer ziet u twee lokale sites voor het andere VNet. Het klassieke model van twee VPN-gateways, twee lokale sites en twee verbindingen tussen deze wordt omgezet naar Resource Manager-model met twee VPN-gateways en twee verbindingen van het type Vnet2Vnet.

## <a name="transit-vpn-connectivity"></a>Doorvoer VPN-verbinding
U kunt VPN-gateways configureren in een topologie dat on-premises connectiviteit voor een VNet wordt bereikt door verbinding te maken met een ander VNet die rechtstreeks is verbonden met on-premises. Dit is doorvoer VPN-connectiviteit waar-exemplaren in de eerste VNet zijn verbonden met on-premises bronnen via tijdens verzending naar de VPN-gateway in het gekoppelde VNet dat rechtstreeks is verbonden met on-premises. Voor het bereiken van deze configuratie in het klassieke implementatiemodel, moet u een lokale site dat is samengevoegd voorvoegsels voor het gekoppelde VNet te maken en on-premises-adresruimte. Deze representational lokale site wordt vervolgens verbonden met de VNet-naar-connectiviteit van de doorvoer te bereiken. Dit klassieke model heeft ook dezelfde beheerbaarheid uitdagingen omdat eventuele wijzigingen in de on-premises adresbereik moet ook worden ingesteld op de lokale site die de combinatie van VNet- en on-premises. Introductie van BGP-ondersteuning in Resource Manager ondersteund gateways vereenvoudigt beheerbaarheid, omdat de verbonden gateways kunnen informatie over routes van on-premises zonder handmatige aanpassingen op voorvoegsels.

![Schermopname van het scenario met routering doorvoer.](./media/vpn-gateway-migration/migration2.png)

Omdat we VNet naar VNet-connectiviteit transformeren zonder lokale sites, verliest het scenario doorvoer on-premises connectiviteit voor VNet die indirect is verbonden met on-premises. Het verlies van connectiviteit kan grotendeels worden opgevangen in de volgende twee manieren, nadat de migratie is voltooid: 

* BGP inschakelen op VPN-gateways die bij elkaar en met on-premises zijn verbonden. BGP inschakelen, wordt connectiviteit zonder andere configuratiewijziging hersteld omdat routes hebt geleerd en aangekondigd tussen VNet-gateways. Houd er rekening mee dat BGP-optie alleen beschikbaar op Standard en hoger SKU's is.
* Verbinding met een expliciete van betrokken VNet naar de lokale netwerkgateway die on-premises locatie vertegenwoordigt. Hiervoor moeten ook wijzigen van de configuratie op de on-premises router maken en configureren van de IPsec-tunnel.

## <a name="next-steps"></a>Volgende stappen
Nadat u meer over de ondersteuning van VPN-gateway-migratie, gaat u naar [platform ondersteunde migratie van IaaS-resources van klassiek naar Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-ps.md) aan de slag.

