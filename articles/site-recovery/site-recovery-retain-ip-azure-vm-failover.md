---
title: IP-adressen voor Azure VM-failover behouden | Microsoft Docs
description: Hierin wordt beschreven hoe u IP-adressen behouden wanneer failover van virtuele machines van Azure voor herstel na noodgevallen naar een secundaire regio met Azure Site Recovery
ms.service: site-recovery
ms.date: 10/16/2018
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 86adaa21a069c168b512231ba231940bfa2ef9e8
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50213029"
---
# <a name="ip-address-retention-for-azure-vm-failover"></a>IP-adres bewaartermijn voor Azure VM-failover

Azure Site Recovery maakt herstel na noodgevallen voor Azure VM's. Wanneer Failover-overschakeling uitvoeren van een Azure-regio naar een andere, vereisen klanten vaak behoud van hun IP-configuraties. Site Recovery, imiteert standaard, bron van virtueel netwerk en subnetstructuur bij het maken van deze resources in de doelregio. Voor Azure VM's geconfigureerd met statische privé IP-adressen, maakt Site Recovery ook een best-effort probeert te gebruiken voor het inrichten van de dezelfde privé IP-adres op de doel-VM, als dat IP-adres niet al wordt geblokkeerd door een Azure-resource of een gerepliceerde virtuele machine.

Voor eenvoudige toepassingen is de bovenstaande standaardconfiguratie alles dat nodig is. Voor complexere bedrijfstoepassingen, klanten mogelijk extra netwerkresources om ervoor te zorgen na een failover-connectiviteit met andere onderdelen van de infrastructuur inrichten. Dit artikel wordt uitgelegd de netwerkvereisten waarbij een failover via Azure-VM's van de ene regio naar een andere VM-IP-adressen behouden.

## <a name="azure-to-azure-connectivity"></a>Azure-naar-Azure-connectiviteit

Voor het eerste scenario wij beschouwen **bedrijf A** waarvoor alle van de toepassingsinfrastructuur worden uitgevoerd in Azure. Voor zakelijke continuïteit en naleving redenen **bedrijf A** wil Azure Site Recovery gebruiken de toepassingen te beschermen.

Opgegeven de vereiste IP-retentie (zoals toepassing bindingen), heeft een bedrijf hetzelfde virtuele netwerk en subnet structuur op de doelregio. Om beoogde hersteltijd (RTO) verder te beperken **bedrijf A** maakt gebruik van replica-knooppunten voor SQL Always ON, domeincontrollers, enz. en deze replica knooppunten worden geplaatst in een ander virtueel netwerk in de doelregio. Met behulp van een andere adresruimte voor de replica-knooppunten kunt **bedrijf A** voor het maken van VPN-site-naar-site-connectiviteit tussen bron en doel-regio's, die anders niet mogelijk zou zijn als dezelfde adresruimte aan beide uiteinden wordt gebruikt .

Hier ziet u hoe de netwerkarchitectuur eruitziet voordat de failover:
- Toepassings-VM's worden gehost in Azure Oost-Azië, met behulp van een Azure-netwerk met adresruimte 10.1.0.0/16. Dit virtuele netwerk is met de naam **bron VNet**.
- Werkbelastingen van toepassingen zijn verdeeld over drie subnetten: 10.1.1.0/24, 10.1.2.0/24, 10.1.3.0/24, respectievelijk met de naam **Subnet 1**, **Subnet 2**, **Subnet 3**.
- Azure Zuidoost-Azië is de doelregio en een virtuele herstelnetwerk dat lijkt op de-ruimte en het subnetmasker adresconfiguratie op de bron. Dit virtuele netwerk is met de naam **Recovery VNet**.
- Replica-knooppunten, zoals die nodig zijn voor Always On, domeincontroller, enzovoort worden geplaatst in een virtueel netwerk met adresruimte 10.2.0.0/16 binnen het Subnet 4 met adres 10.2.4.0/24. Het virtuele netwerk is met de naam **Azure VNet** en op Azure Zuidoost-Azië.
- **Bron VNet** en **Azure VNet** zijn verbonden via site-naar-site-VPN-verbinding.
- **Herstel VNet** niet is verbonden met een ander virtueel netwerk.
- **Een bedrijf** wijst/controleert of doel-IP-adres voor gerepliceerde items. Voor dit voorbeeld is doel-IP hetzelfde als bron-IP voor elke virtuele machine.

![Azure-naar-Azure-connectiviteit voordat de failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover2.png)

### <a name="full-region-failover"></a>Volledige regio failover

In het geval van een regionale onderbreking **bedrijf A** kunt herstellen de volledige implementatie snel en eenvoudig met behulp van Azure Site Recovery krachtige [herstelplannen](site-recovery-create-recovery-plans.md). Dat het doel-IP-adres al ingesteld voor elke virtuele machine voordat u een failover, **bedrijf A** failover te organiseren en automatiseren verbinding tot stand brengen tussen VNet Recovery en Azure Vnet, zoals wordt weergegeven in het onderstaande diagram.

![Azure-naar-Azure-verbinding volledige regio failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover2.png)

Afhankelijk van de vereisten van webtoepassingen-verbindingen tussen de twee VNets in de doelregio kunnen worden vastgesteld voor, tijdens (als een tussenstap) of na een failover. Gebruik [herstelplannen](site-recovery-create-recovery-plans.md) scripts toevoegen en de failovervolgorde definiëren.

Een bedrijf heeft ook de keuze van de verbinding tussen VNet Recovery en Azure VNet met behulp van VNet-peering of Site-naar-Site VPN. Bij VNET-peering wordt geen VPN-gateway gebruikt en er gelden diverse beperkingen voor. Bovendien worden de [prijzen voor VNET-peering](https://azure.microsoft.com/pricing/details/virtual-network) anders berekend dan voor [VNet-naar-VNet-VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway). Voor failover is het in het algemeen wordt aangeraden om na te bootsen bron connectiviteit, met inbegrip van het verbindingstype om te minimaliseren onvoorspelbare incidenten die voortvloeien uit wijzigingen in het netwerk.

### <a name="isolated-application-failover"></a>Geïsoleerde toepassing failover

Onder bepaalde omstandigheden kunnen gebruikers moeten failover-onderdelen van de infrastructuur van hun toepassing. Een voorbeeld is failover van een bepaalde toepassing of de categorie die is ondergebracht in een toegewezen subnet. Failover met een bewaarperiode van IP-subnet is mogelijk, wordt het niet aangeraden voor de meeste situaties als deze connectiviteit inconsistenties aanzienlijk toeneemt. U verliest subnetverbinding ook naar andere subnetten binnen hetzelfde Azure virtual network.

Een betere manier ter compensatie van failover op subnetniveau toepassingsvereisten heeft aan ander doel-IP-adressen gebruiken voor failover (als connectiviteit vereist voor andere subnetten op de bron van virtueel netwerk is) of isolatie van elke toepassing in een eigen toegewezen virtuele netwerk op de bron. U kunt met de laatste benadering tot stand brengen tussen netwerkverbinding beschikbaar is op de bron en hetzelfde als failover-overschakeling uitvoeren naar de doelregio worden geëmuleerd.

Voor het ontwerpen van afzonderlijke toepassingen voor tolerantie, is het raadzaam om te bevatten van een toepassing in een eigen toegewezen virtueel netwerk en stel verbinding in tussen deze virtuele netwerken zoals vereist. Hierdoor geïsoleerde toepassing failover behoud de oorspronkelijke privé IP-adressen.

De configuratie van vóór de failover vervolgens ziet er als volgt uit:
- Toepassings-VM's worden gehost in Azure Oost-Azië, met behulp van een Azure-netwerk met adresruimte 10.1.0.0/16 voor de eerste toepassing en 10.2.0.0/16 voor de tweede toepassing. De virtuele netwerken zijn benoemde **bron VNet1** en **bron VNet2** voor de eerste en tweede toepassing, respectievelijk.
- Elk VNet wordt verder gesplitst in twee subnetten.
- Azure Zuidoost-Azië is de doelregio en herstel in de virtuele netwerken Recovery VNet1 en het herstel VNet2.
- Replica-knooppunten, zoals die nodig zijn voor Always On, domeincontroller, enz. worden geplaatst in een virtueel netwerk met adresruimte 10.3.0.0/16 binnen **Subnet 4** met adres 10.3.4.0/24. Het virtuele netwerk Azure VNet wordt genoemd en is op Azure Zuidoost-Azië.
- **Bron van VNet1** en **Azure VNet** zijn verbonden via site-naar-site-VPN-verbinding. Op deze manier **bron VNet2** en **Azure VNet** ook zijn verbonden via site-naar-site-VPN-verbinding.
- **Bron van VNet1** en **bron VNet2** ook zijn verbonden via S2S VPN-verbinding in dit voorbeeld. Aangezien de twee VNets in dezelfde regio, kan VNet-peering ook worden gebruikt in plaats van S2S-VPN.
- **Herstel VNet1** en **Recovery VNet2** niet zijn verbonden met een ander virtueel netwerk.
- Beoogde hersteltijd (RTO), verminderen de VPN-gateways die zijn geconfigureerd op **Recovery VNet1** en **Recovery VNet2** voordat u een failover.

![Azure-naar-Azure-connectiviteit (geïsoleerd)-toepassing voor failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover2.png)

In het geval van een noodherstel situatie die van invloed is op slechts één toepassing (in dit voorbeeld is ondergebracht in bron VNet2), een bedrijf kan de betreffende toepassing als volgt herstellen:
- VPN-verbindingen tussen **bron VNet1** en **bron VNet2**, en tussen **bron VNet2** en **Azure VNet** niet zijn verbonden.
- VPN-verbindingen tot stand worden gebracht tussen **bron VNet1** en **Recovery VNet2**, en tussen **Recovery VNet2** en **Azure VNet**.
- Virtuele machines van **bron VNet2** failover is uitgevoerd naar **Recovery VNet2**.

![Azure-naar-Azure-connectiviteit (geïsoleerd)-toepassing na een failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover2.png)

De bovenstaande geïsoleerde failover voorbeeld kan worden uitgebreid om meer toepassingen bevatten en netwerkverbindingen. De aanbeveling is voor een model like-achtige verbinding, zoveel mogelijk failover-overschakeling uitvoeren van bron naar doel.

### <a name="further-considerations"></a>Verdere overwegingen

VPN-Gateways gebruikmaken van openbare IP-adressen en gateway hops verbindingen tot stand gebracht. Als u niet wilt gebruiken van openbaar IP-adres en/of om te voorkomen dat extra hops, kunt u Azure [peering op Virtueelnetwerk](../virtual-network/virtual-network-peering-overview.md) virtuele netwerken via [Azure-regio's ondersteund](../virtual-network/virtual-network-manage-peering.md#cross-region).

## <a name="on-premises-to-azure-connectivity"></a>On-premises-to-Azure-connectiviteit

Voor het tweede scenario wij beschouwen **bedrijf B** waarvoor een deel van de infrastructuur van de toepassingen die worden uitgevoerd op Azure en de resterende on-premises uitgevoerd. Voor zakelijke continuïteit en naleving redenen **bedrijf B** wil Azure Site Recovery gebruiken de toepassingen die worden uitgevoerd in Azure te beschermen.

Hier ziet u hoe de netwerkarchitectuur eruitziet voordat de failover:
- Toepassings-VM's worden gehost in Azure Oost-Azië, met behulp van een Azure-netwerk met adresruimte 10.1.0.0/16. Dit virtuele netwerk is met de naam **bron VNet**.
- Werkbelastingen van toepassingen zijn verdeeld over drie subnetten: 10.1.1.0/24, 10.1.2.0/24, 10.1.3.0/24, respectievelijk met de naam **Subnet 1**, **Subnet 2**, **Subnet 3**.
- Azure Zuidoost-Azië is de doelregio en een virtuele herstelnetwerk dat lijkt op de-ruimte en het subnetmasker adresconfiguratie op de bron. Dit virtuele netwerk is met de naam **Recovery VNet**.
- Virtuele machines in Azure Oost-Azië zijn verbonden met on-premises datacenter via ExpressRoute of Site-naar-Site VPN.
- Beoogde hersteltijd (RTO), verminderen bepalingen bedrijf B gateways op herstel VNet in Azure Zuidoost-Azië voordat u een failover.
- **Bedrijf B** wijst/controleert of doel-IP-adres voor gerepliceerde items. In dit voorbeeld is hetzelfde als de bron-IP voor elke virtuele machine met doel-IP

![On-premises-to-Azure-connectiviteit voordat de failover](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover2.png)

### <a name="full-region-failover"></a>Volledige regio failover

In het geval van een regionale onderbreking **bedrijf B** kunt herstellen de volledige implementatie snel en eenvoudig met behulp van Azure Site Recovery krachtige [herstelplannen](site-recovery-create-recovery-plans.md). Dat het doel-IP-adres al ingesteld voor elke virtuele machine voordat u een failover, **bedrijf B** failover te organiseren en automatiseren verbinding tot stand brengen tussen Recovery VNet en on-premises datacentrum, zoals wordt weergegeven in het onderstaande diagram.

De oorspronkelijke verbinding tussen Azure-Oost-Azië en het on-premises datacenter moet worden verbroken voordat het tot stand brengen van de verbinding tussen Azure Zuidoost-Azië en on-premises datacentrum. De on-premises routering ook opnieuw wordt geconfigureerd om te verwijzen naar de doelregio en gateways na een failover.

![On-premises-to-Azure-connectiviteit na een failover](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover2.png)

### <a name="subnet-failover"></a>Subnet-failover

In tegenstelling tot het Azure-naar-Azure-scenario beschreven voor **bedrijf A**, een failover op subnetniveau is niet mogelijk in dit geval voor **bedrijf B**. Dit is omdat de adresruimte op bron- en herstel van virtuele netwerken dezelfde is en de oorspronkelijke bron naar het on-premises verbinding actief is.

Voor het bereiken van tolerantie voor toepassing, is het aanbevolen dat elke toepassing is ondergebracht in een eigen toegewezen virtueel Azure-netwerk. Toepassingen kunnen vervolgens failover in een geïsoleerde omgeving en de vereiste on-premises bron verbindingen kunnen worden doorgestuurd naar de doelregio zoals hierboven is beschreven.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md).
