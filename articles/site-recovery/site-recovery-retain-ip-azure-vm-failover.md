---
title: IP-adressen behouden wanneer failover van virtuele machines naar een andere Azure-regio in Azure | Microsoft Docs
description: Hierin wordt beschreven hoe u IP-adressen voor scenario's voor failover van Azure naar Azure met Azure Site Recovery behouden
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 28d772df384e620c7e82812adfa2bfa148401132
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="ip-address-retention-for-azure-virtual-machine-failover"></a>IP-adres bewaren voor failover van de virtuele machine van Azure

Azure Site Recovery inschakelen herstel na noodgevallen voor Azure Virtual machines Wanneer failover uit van een Azure-regio naar een andere, moeten klanten vaak bewaren van hun IP-configuraties. Site Recovery nabootst standaard bron virtueel netwerk en subnetstructuur bij het maken van deze bronnen op de doelregio. Voor de Azure VM's zijn geconfigureerd met statische privé IP-adressen, maakt Site Recovery ook een zo goed mogelijke poging probeert in te richten op de doel-virtuele machine, dezelfde persoonlijke IP als die IP al niet wordt geblokkeerd door een Azure-resource of een gerepliceerde virtuele machine.

Voor eenvoudige toepassingen is de bovenstaande standaardconfiguratie alles wat nodig is. Voor complexere bedrijfstoepassingen wellicht klanten aanvullende netwerkresources om ervoor te zorgen na een failover-connectiviteit met andere onderdelen van hun infrastructuur inrichten. Dit artikel wordt uitgelegd de netwerkvereisten voor mislukte via Azure VM's van de ene regio naar een andere terwijl de VM IP-adressen behouden.

## <a name="azure-to-azure-connectivity"></a>Connectiviteit van Azure naar Azure

Voor het eerste scenario we beschouwen **bedrijf A** die alle van de toepassing-infrastructuur worden uitgevoerd in Azure heeft. Voor zakelijke continuïteit en naleving redenen **bedrijf A** beslist haar toepassingen beveiligen met Azure Site Recovery.

Gezien de vereiste IP-retentieperiode (zoals voor bindingen van toepassing), heeft bedrijf A hetzelfde virtuele netwerk en subnet structuur op de doelregio. Verder verlagen beoogde hersteltijd (RTO) **bedrijf A** maakt gebruik van de knooppunten van de replica voor SQL Always ON, domeincontrollers, enz. en deze replica knooppunten in een ander virtueel netwerk op de doelregio worden geplaatst. Met een andere adresruimte voor de replica-knooppunten kunt **bedrijf A** om vast te stellen VPN site-naar-site-connectiviteit tussen de bron en doel-regio's, die anders niet mogelijk zijn zou als dezelfde adresruimte aan beide uiteinden wordt gebruikt .

Hier ziet u hoe de netwerkarchitectuur uitziet voordat failover wordt uitgevoerd:
- Toepassing virtuele machines worden gehost in Azure Oost-Azië, met behulp van een Azure-netwerk met adresruimte 10.1.0.0/16. Dit virtuele netwerk heet **bron VNet**.
- Toepassingsworkloads zijn gesplitst in drie subnetten – 10.1.0.0/24, 10.1.1.0/24, 10.1.2.0/24, respectievelijk met de naam **Subnet 1**, **Subnet 2**, **Subnet 3**.
- Azure Zuidoost-Azië is de doelregio en een herstel virtueel netwerk dat lijkt op de-ruimte en subnet adresconfiguratie op de bron. Dit virtuele netwerk heet **herstel VNet**.
- Replica knooppunten zoals die nodig zijn voor Always On, domeincontroller, enz., worden in een virtueel netwerk met adresruimte 20.1.0.0/16 binnen het Subnet 4 met adres 20.1.0.0/24 geplaatst. De naam van het virtuele netwerk **Azure VNet** en op Azure Zuidoost-Azië.
- **Bron VNet** en **Azure VNet** zijn verbonden via VPN-site-naar-site-verbinding.
- **Herstel VNet** niet met een virtueel netwerk is verbonden.
- **Een bedrijf** wijst/controleert of IP-adres van doel voor gerepliceerde items. Voor dit voorbeeld is IP-adres doel hetzelfde als bron-IP voor elke virtuele machine.

![Azure naar Azure-connectiviteit voor failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover.png)

### <a name="full-region-failover"></a>Volledige regio failover

In het geval van een storing regionale **bedrijf A** kunt herstellen van de volledige implementatie snel en eenvoudig met behulp van Azure Site Recovery krachtige [herstelplannen](site-recovery-create-recovery-plans.md). Dat het IP-adres van het doel al ingesteld voor elke VM voordat failover, **bedrijf A** kan failover indelen en verbinding tot stand brengen tussen VNet Recovery en Azure Vnet automatiseren, zoals wordt weergegeven in het onderstaande diagram.

![Failover van Azure naar Azure-verbinding volledige regio](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover.png)

Afhankelijk van de vereisten van webtoepassingen verbindingen tussen de twee VNets op het gebied van het doel kunnen worden gebracht voor, tijdens (als een tussenstap) of na een failover. Gebruik [herstelplannen](site-recovery-create-recovery-plans.md) scripts toevoegen en de failovervolgorde definiëren.

Bedrijf A heeft ook de keuze van de VNet-peering of Site-naar-Site VPN-verbinding tussen VNet Recovery en Azure VNet tot stand te gebruiken. Bij VNET-peering wordt geen VPN-gateway gebruikt en er gelden diverse beperkingen voor. Bovendien worden de [prijzen voor VNET-peering](https://azure.microsoft.com/pricing/details/virtual-network) anders berekend dan voor [VNet-naar-VNet-VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway). Voor failovers is het in het algemeen wordt aangeraden om na te bootsen bron connectiviteit, met inbegrip van het verbindingstype om te minimaliseren onvoorspelbare incidenten die voortvloeien uit wijzigingen in het netwerk.

### <a name="isolated-application-failover"></a>Geïsoleerde toepassing failover

Onder bepaalde omstandigheden mogelijk gebruikers failover delen van hun infrastructuur van de toepassing. Een voorbeeld de failover van een specifieke toepassingen of lagen die is ondergebracht in een subnet toegewezen. Tijdens een failover subnet met IP-bewaren mogelijk is, wordt het niet aangeraden voor de meeste situaties als deze connectiviteit inconsistenties aanzienlijk toeneemt. U verliest bestemmingssubnet ook naar andere subnetten in hetzelfde virtuele netwerk van Azure.

Een betere manier ter compensatie van failover op subnetniveau toepassingsvereisten is voor het gebruik van andere doel-IP-adressen voor failover (als de verbinding vereist voor andere subnetten in het virtuele Bronnetwerk is) of elke toepassing in een eigen toegewezen virtuele isoleren netwerk op de bron. U kunt met de laatste methode tot stand brengen tussen netwerkverbinding op de bron- en dezelfde wanneer failover wordt uitgevoerd naar de doelregio worden geëmuleerd.

Als u wilt bouwen afzonderlijke toepassingen voor tolerantie, wordt u aangeraden bevatten van een toepassing in een eigen toegewezen virtueel netwerk en stel de verbinding tussen deze virtuele netwerken, zoals vereist. Hierdoor geïsoleerde toepassing failover terwijl de oorspronkelijke privé IP-adressen behouden.

De configuratie van de pre-failover vervolgens ziet er als volgt uit:
- Toepassing virtuele machines worden gehost in Azure Oost-Azië, met behulp van een Azure-netwerk met adresruimte 10.1.0.0/16 voor de eerste toepassing en 15.1.0.0/16 voor de tweede toepassing. De virtuele netwerken zijn benoemde **bron VNet1** en **bron VNet2** voor de eerste en tweede toepassing, respectievelijk.
- Elke VNet verdere gesplitst in twee subnetten.
- Azure Zuidoost-Azië is de doelregio en virtuele netwerken recovery herstel VNet1 en herstel VNet2.
- Replica knooppunten zoals die nodig zijn voor altijd aan, domeincontroller, enzovoort worden geplaatst in een virtueel netwerk met adresruimte 20.1.0.0/16 binnen **Subnet 4** met adres 20.1.0.0/24. Het virtuele netwerk heet Azure VNet en is op Azure Zuidoost-Azië.
- **Bron VNet1** en **Azure VNet** zijn verbonden via VPN-site-naar-site-verbinding. Op deze manier **bron VNet2** en **Azure VNet** ook zijn verbonden via VPN-site-naar-site-verbinding.
- **Bron VNet1** en **bron VNet2** zijn ook aangesloten via S2S VPN-verbinding in dit voorbeeld. Aangezien de twee VNets in dezelfde regio, kan VNet-peering ook worden gebruikt in plaats van de S2S-VPN.
- **Herstel VNet1** en **herstel VNet2** niet zijn verbonden met een virtueel netwerk.
- Als u beoogde hersteltijd (RTO), VPN-gateways zijn geconfigureerd op **herstel VNet1** en **herstel VNet2** voordat failover.

![De toepassing Azure naar Azure-connectiviteit geïsoleerd voordat failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover.png)

In het geval van een noodherstel situatie die van invloed is op slechts één toepassing (in dit voorbeeld in bron VNet2 was opgenomen), herstellen bedrijf A als volgt de desbetreffende toepassing:
- VPN-verbindingen tussen **bron VNet1** en **bron VNet2**, en tussen **bron VNet2** en **Azure VNet** verbinding wordt verbroken.
- VPN-verbindingen worden tot stand gebracht tussen **bron VNet1** en **herstel VNet2**, en tussen **herstel VNet2** en **Azure VNet**.
- Virtuele machines van **bron VNet2** mislukt via **herstel VNet2**.

![Azure naar Azure-connectiviteit geïsoleerd toepassing na een failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover.png)

De bovenstaande geïsoleerde failover voorbeeld kan worden uitgebreid bevatten meer toepassingen en netwerkverbindingen. Aanbevolen wordt een model like-achtige verbinding zoveel mogelijk uitvoeren als failover wordt uitgevoerd van bron naar doel.

### <a name="further-considerations"></a>Verdere overwegingen

VPN-Gateways gebruikmaken van openbare IP-adressen en gateway hops verbindingen tot stand gebracht. Als u niet wilt gebruiken van openbare IP-adres en/of wilt voorkomen dat extra hops, kunt u nu globale VNet-Peering gebruiken als peer virtuele netwerken in Azure-regio's.

Deze functie is momenteel in de openbare preview en ter ondersteuning van meer regio's is wordt uitgevouwen: directe verbinding voor VM-VM zonder de betrokkenheid van alle openbare internet of eventuele extra hops inschakelen.

Raadpleeg voor meer informatie de [peering documentatie](../virtual-network/virtual-network-create-peering.md#register) en [prijzen](https://azure.microsoft.com/en-us/pricing/details/virtual-network/).

## <a name="on-premises-to-azure-connectivity"></a>Connectiviteit op lokale-die naar Azure

Voor het tweede scenario we beschouwen **bedrijf B** die een deel van de infrastructuur van toepassingen uitgevoerd op Azure en de overige met lokale heeft. Voor zakelijke continuïteit en naleving redenen **bedrijf B** beslist Azure Site Recovery gebruiken om te beveiligen van de toepassingen die worden uitgevoerd in Azure.

Hier ziet u hoe de netwerkarchitectuur uitziet voordat failover wordt uitgevoerd:
- Toepassing virtuele machines worden gehost in Azure Oost-Azië, met behulp van een Azure-netwerk met adresruimte 10.1.0.0/16. Dit virtuele netwerk heet **bron VNet**.
- Toepassingsworkloads zijn gesplitst in drie subnetten – 10.1.0.0/24, 10.1.1.0/24, 10.1.2.0/24, respectievelijk met de naam **Subnet 1**, **Subnet 2**, **Subnet 3**.
- Azure Zuidoost-Azië is de doelregio en een herstel virtueel netwerk dat lijkt op de-ruimte en subnet adresconfiguratie op de bron. Dit virtuele netwerk heet **herstel VNet**.
- Virtuele machines in Azure Oost-Azië zijn verbonden met on-premises datacentrum via ExpressRoute of Site-naar-Site VPN.
- Als u beoogde hersteltijd (RTO), voorziet in bedrijf B gateways op herstel VNet in Azure Zuidoost-Azië voordat failover.
- **Bedrijf B** wijst/controleert of IP-adres van doel voor gerepliceerde items. In dit voorbeeld is IP-adres doel hetzelfde als de bron-IP voor elke virtuele machine

![Connectiviteit vóór de failover op lokale-die naar Azure](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover.png)

### <a name="full-region-failover"></a>Volledige regio failover

In het geval van een storing regionale **bedrijf B** kunt herstellen van de volledige implementatie snel en eenvoudig met behulp van Azure Site Recovery krachtige [herstelplannen](site-recovery-create-recovery-plans.md). Dat het IP-adres van het doel al ingesteld voor elke VM voordat failover, **bedrijf B** kan failover indelen en verbinding tot stand brengen tussen VNet van herstel en on-premises datacentrum automatiseren, zoals wordt weergegeven in het onderstaande diagram.

De oorspronkelijke verbinding tussen Oost-Azië Azure en de on-premises datacentrum moet worden verbroken voordat het maken van de verbinding tussen Zuidoost-Azië Azure en on-premises datacentrum. De on-premises routering ook opnieuw wordt geconfigureerd om te verwijzen naar de doelregio en gateways na de failover.

![Connectiviteit na een failover op lokale-die naar Azure](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover.png)

### <a name="subnet-failover"></a>Subnet failover

In tegenstelling tot het Azure naar Azure scenario beschreven voor **bedrijf A**, een failover op subnetniveau is niet mogelijk in dit geval voor **bedrijf B**. Dit is omdat de adresruimte op bron- en herstel van de virtuele netwerken hetzelfde is en de oorspronkelijke bron naar lokale verbinding actief is.

Als u wilt bereiken tolerantie van toepassing, is het raadzaam dat elke toepassing is ondergebracht in een eigen toegewezen virtuele Azure-netwerk. Toepassingen kunnen vervolgens failover in een geïsoleerde omgeving en de vereiste on-premises verbindingen tussen bron kunnen worden doorgestuurd naar de doelregio, zoals hierboven is beschreven.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md).
