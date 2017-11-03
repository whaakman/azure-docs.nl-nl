---
title: VPN-Gateway van klassiek naar Resource Manager-migratie | Microsoft Docs
description: Deze pagina bevat een overzicht van de VPN-Gateway-klassiek naar Resource Manager-migratie.
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
ms.openlocfilehash: 1164fc24355657af22b6befaad74685ebbc2b5cb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="vpn-gateway-classic-to-resource-manager-migration"></a>VPN-Gateway van klassiek naar Resource Manager-migratie
VPN-Gateways kunnen nu worden gemigreerd van klassiek naar Resource Manager-implementatiemodel. U kunt meer lezen over Azure Resource Manager [functies en voordelen](../azure-resource-manager/resource-group-overview.md). In dit artikel wordt beschreven we het migreren van klassieke implementaties naar nieuwere resourcebeheer op basis van model. 

VPN-Gateways worden gemigreerd als onderdeel van de VNet-migratie van klassiek naar Resource Manager. Deze migratie wordt een VNet per keer gedaan. Er is geen aanvullende vereiste in termen van hulpprogramma's of vereisten voor migratie. Migratiestappen identiek zijn aan bestaande VNet migratie en worden beschreven op [IaaS resources migratie pagina](../virtual-machines/windows/migration-classic-resource-manager-ps.md). Er is geen pad gegevens uitvaltijd tijdens de migratie en bestaande werkbelastingen wilt blijven werken zonder verlies van verbinding met lokale tijdens de migratie. Het openbare IP-adres die zijn gekoppeld aan de VPN-gateway wordt niet gewijzigd tijdens het migratieproces. Dit betekent dat niet u de lokale router opnieuw configureren moet nadat de migratie is voltooid.  

Het model in de Resource Manager verschilt van het klassieke model en bestaat uit de virtuele netwerkgateways, lokale netwerkgateways en verbindingsbronnen. Dit zijn de VPN-gateway zelf, de lokale-site op premises-adresruimte en de verbinding tussen de twee respectievelijk. Nadat de migratie is voltooid uw gateways niet beschikbaar in het klassieke model en alle beheerbewerkingen op de virtuele netwerkgateways en lokale netwerkgateways verbindingsobjecten moeten worden uitgevoerd met behulp van Resource Manager-model.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
Meest voorkomende scenario's voor VPN-verbinding, vallen van klassiek naar Resource Manager-migratie. De ondersteunde scenario's omvatten-

* Wijs site-connectiviteit
* Verbindingsmogelijkheden tussen sites met VPN-Gateway is verbonden met lokale locatie
* VNet-naar-VNet-connectiviteit tussen de twee VNets met behulp van VPN-gateways
* Meerdere VNets die is verbonden met dezelfde op de lokale locatie
* Meerdere site-connectiviteit
* Geforceerde tunneling VNets ingeschakeld

Scenario's die niet worden ondersteund:-  

* VNet met zowel de ExpressRoute-Gateway en de VPN-Gateway is momenteel niet ondersteund.
* Doorvoer scenario's waar de VM-extensies zijn verbonden met on-premises servers. Doorvoer VPN-verbinding beperkingen worden hieronder beschreven.

> [!NOTE]
> Validatie van de CIDR in Resource Manager-model is meer dan één in het klassieke model strict. Voordat u migreert overeenstemming klassieke-adresbereiken die zijn opgegeven met geldige CIDR-notatie voordat u begint met de migratie. CIDR kan worden gevalideerd met behulp van een algemene CIDR validatiefuncties. VNet of lokale sites met ongeldige CIDR-bereiken wanneer gemigreerd zou leiden tot mislukte status.
> 
> 

## <a name="vnet-to-vnet-connectivity-migration"></a>VNet-naar-VNet-connectiviteit migratie
VNet-naar-VNet-connectiviteit in klassieke werd bereikt door het maken van een lokale site weergave van de verbonden VNet. Klanten zijn vereist voor het maken van twee lokale sites die de twee VNets die nodig om met elkaar worden verbonden weergegeven. Deze zouden zijn vervolgens met de bijbehorende VNets tot stand brengen van verbindingen tussen de twee VNets met behulp van IPsec-tunnel verbonden. Dit model heeft beheerbaarheid uitdagingen omdat wijzigingen adresbereik in een VNet moeten ook worden bijgehouden in de weergave van het bijbehorende lokale site. Deze tijdelijke oplossing is niet meer nodig in het Resource Manager-model. De verbinding tussen de twee VNets kan rechtstreeks worden bereikt met het verbindingstype 'Vnet2Vnet' in verbindingsbron. 

![Schermopname van VNet VNet-migratie.](./media/vpn-gateway-migration/migration1.png)

Tijdens de migratie van de VNet wordt gedetecteerd dat de verbonden entiteit met huidige VNet VPN-gateway is een andere VNet en ervoor te zorgen dat zodra de migratie van beide VNets is voltooid, niet langer ziet u twee lokale sites die het andere VNet vertegenwoordigt. Het klassieke model van twee VPN-gateways, twee lokale sites en twee verbindingen tussen deze twee wordt omgezet naar Resource Manager-model met twee VPN-gateways en twee verbindingen van het type Vnet2Vnet.

## <a name="transit-vpn-connectivity"></a>Doorvoer VPN-verbinding
U kunt VPN-gateways configureren in een topologie zodanig zijn dat lokale connectiviteit voor een VNet wordt bereikt door verbinding te maken met een andere VNet dat rechtstreeks is verbonden met on-premises. Dit is doorvoer VPN-verbinding waar exemplaren in de eerste VNet zijn verbonden met lokale bronnen via verzending naar de VPN-gateway in verbonden VNet dat rechtstreeks is verbonden met on-premises. Voor deze configuratie in het klassieke implementatiemodel, moet u een lokale site dat is samengevoegd voorvoegsels voor de verbonden VNet maken en lokale adresruimte. Deze representational lokale site is vervolgens naar het VNet als u de doorvoer-connectiviteit verbonden. Dit model klassieke heeft ook vergelijkbare beheerbaarheid uitdagingen omdat eventuele wijzigingen in de on-premises adresbereik moet ook worden ingesteld op de lokale site voor de statistische functie van het VNet en on-premises. Introductie van BGP-ondersteuning in Resource Manager ondersteunde gateways vereenvoudigt het beheer omdat verbonden gateways de routes van on-premises zonder handmatige aanpassingen op voorvoegsels leert.

![Schermopname van routeringsscenario doorvoer.](./media/vpn-gateway-migration/migration2.png)

Aangezien we VNet naar VNet-connectiviteit transformeren zonder lokale sites, verliest het scenario onderweg op-premises-connectiviteit voor VNet dat indirect is verbonden met on-premises. Het verlies van verbinding kan worden opgevangen in de volgende twee manieren nadat de migratie is voltooid- 

* Hiermee schakelt u BGP op VPN-gateways die zijn verbonden met on-premises en samen. Connectiviteit zonder andere configuratiewijziging BGP inschakelen worden hersteld omdat routes worden geleerd en aangekondigd tussen VNet-gateways. Houd er rekening mee BGP-optie is alleen beschikbaar in Standard en hoger SKU's.
* Een expliciete verbinding maken van de betrokken VNet met de lokale netwerkgateway die on-premises locatie vertegenwoordigt. Hiervoor moeten ook wijzigen op de lokale router maken en configureren van de IPsec-tunnel-configuratie.

## <a name="next-steps"></a>Volgende stappen
Na leren over de ondersteuning van VPN-gateway migratie, gaat u naar [platform ondersteund migratie van IaaS-middelen van klassiek naar Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-ps.md) aan de slag.

