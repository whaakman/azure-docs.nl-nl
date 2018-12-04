---
title: IP-adressen behouden tijdens de failover van de virtuele machine van Azure met Azure Site Recovery | Microsoft Docs
description: Hierin wordt beschreven hoe u IP-adressen behouden wanneer failover van virtuele machines van Azure voor herstel na noodgevallen naar een secundaire regio met Azure Site Recovery
ms.service: site-recovery
ms.date: 11/27/2018
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: f7b546e8a0ca52fd2037e471f01787bb64db032d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842744"
---
# <a name="retain-ip-addresses-during-failover"></a>IP-adressen behouden tijdens failover

[Azure Site Recovery](site-recovery-overview.md) herstel na noodgevallen voor Azure VM's kunnen door virtuele machines te repliceren naar een andere Azure-regio en terug naar de primaire regio mislukken wanneer dingen weer in de normale zijn Failover-overschakeling uitvoeren als er een storing optreedt.

Tijdens de failover, is het raadzaam om te houden van de IP-adressen in de doelregio die identiek is aan de regio van de gegevensbron:

- Wanneer u herstel na noodgevallen voor virtuele machines van Azure inschakelt maakt Site Recovery standaard doelresources op basis van resource-instellingen voor gegevensbron. Voor Azure VM's geconfigureerd met statische IP-adressen, Site Recovery wordt geprobeerd om in te richten van hetzelfde IP-adres voor de doel-VM, als deze zich niet in gebruik. Voor een volledige uitleg over hoe Site Recovery omgaat met adressering, [raadpleegt u dit artikel](azure-to-azure-network-mapping.md#set-up-ip-addressing-for-target-vms).
- Voor eenvoudige toepassingen is de standaardconfiguratie voldoende. Voor meer complexe apps moet u mogelijk aanvullende middelen om ervoor te zorgen dat de verbinding werkt zoals verwacht na een failover inrichten.


Dit artikel vindt enkele voorbeelden voor het behouden van IP-adressen in complexere scenario's met voorbeelden. De voorbeelden zijn onder meer:

- Failover voor een bedrijf met alle resources die worden uitgevoerd in Azure
- Failover voor een bedrijf met een hybride implementatie en resources die worden uitgevoerd zowel on-premises en in Azure

## <a name="resources-in-azure-full-failover"></a>Resources in Azure: volledige failover

Bedrijf A heeft alle bijbehorende apps uitvoeren in Azure.

### <a name="before-failover"></a>Voordat de failover

Hier volgt de architectuur voordat de failover.

- Een bedrijf heeft dezelfde netwerken en subnetten in de bron en doel-Azure-regio's.
- Beoogde hersteltijd (RTO), verminderen bedrijf replica knooppunten gebruikt voor SQL Server Always On, domeincontrollers, enzovoort. Deze replica-knooppunten zijn in een ander VNet in de doelregio zodat ze kunnen site-naar-site-VPN-verbinding tussen de bron- en -regio's instellen. Dit is niet mogelijk als de dezelfde IP-adresruimte wordt gebruikt in de bron en doel.  
- Voordat de failover, wordt is de netwerkarchitectuur als volgt uit:
    - Primaire regio is Azure Oost-Azië
        - Oost-Azië heeft een VNet (**bron VNet**) met adresruimte 10.1.0.0/16.
        - Oost-Azië heeft de werklasten verdeeld over drie subnetten in het VNet:
            - **Subnet 1**: 10.1.1.0/24
            - **Subnet 2**: 10.1.2.0/24,
            - **Subnet 3**: 10.1.3.0/24
    - Secundaire (doel) regio is Azure Zuidoost-Azië
        - Zuidoost-Azië heeft een herstel VNet (**Recovery VNet**) identiek is aan **bron VNet**.
        - Zuidoost-Azië heeft een extra VNet (**Azure VNet**) met adresruimte 10.2.0.0/16.
        - **Azure VNet** bevat een subnet (**Subnet 4**) met adresruimte 10.2.4.0/24.
        - De knooppunten van de replica voor SQL Server Always On, domeincontroller enzovoort bevinden zich in **Subnet 4**.
    - **Bron VNet** en **Azure VNet** zijn verbonden met een site-naar-site VPN-verbinding.
    - **Herstel VNet** niet is verbonden met een ander virtueel netwerk.
    - **Een bedrijf** wijst/controleert of doel-IP-adressen voor gerepliceerde items. Het doel-IP is hetzelfde als de bron-IP voor elke virtuele machine.

![Resources in Azure voordat de volledige failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-before-failover2.png)

### <a name="after-failover"></a>Na een failover

Als een bron regionale uitval optreedt, kan een bedrijf alle bijbehorende resources naar de doelregio failover.

- Met de doel-IP-adressen al aanwezig zijn vóór de failover, bedrijf A failover te organiseren en automatisch verbindingen tot stand brengen na een failover tussen **Recovery VNet** en **Azure VNet**. Dit wordt geïllustreerd in het volgende diagram...
- Afhankelijk van de vereisten voor apps, verbindingen tussen de twee vnet's (**Recovery VNet** en **Azure VNet**) in de doel-regio kan worden vastgesteld voor, tijdens (als een tussenstap) of na de failover.
    - Het bedrijf kan gebruiken [herstelplannen](site-recovery-create-recovery-plans.md) om op te geven wanneer verbindingen wordt tot stand worden gebracht.
    - Ze verbinding kunnen maken tussen de VNets met behulp van VNet-peering of site-naar-site VPN.
        - VNet-peering, geen gebruik maakt van een VPN-gateway en gelden diverse beperkingen.
        - VNet-peering [prijzen](https://azure.microsoft.com/pricing/details/virtual-network) wordt anders berekend dan VNet-naar-VNet-VPN-Gateway [prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway). Voor failovers adviseren we in het algemeen dezelfde verbindingsmethode gebruiken als bronnetwerken, inclusief het verbindingstype onvoorspelbare netwerk incidenten te minimaliseren.

    ![Resources in Azure volledige failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-full-region-failover2.png)



## <a name="resources-in-azure-isolated-app-failover"></a>Resources in Azure: failover van de app (geïsoleerd)

U moet mogelijk een failover op het niveau van de app. Bijvoorbeeld: failover uitvoeren voor een specifieke app of een app-laag zich in een speciaal subnet.

- In dit scenario, maar u IP-adressering behouden kunt, is het niet in het algemeen wordt aangeraden omdat dit de kans op inconsistenties connectiviteit verhoogt. U verliest ook subnetverbinding tot andere subnetten in hetzelfde Azure-VNet.
- Een betere manier om te doen op subnetniveau app failover is een ander doel-IP-adressen gebruiken voor failover (als u verbinding met andere subnetten op de bron VNet moet) of voor het isoleren van elke app in een eigen toegewezen VNet in de bronregio. U kunt met de laatste methode verbinding tussen netwerken in de regio van de gegevensbron en hetzelfde gedrag wanneer u een failover naar de doelregio worden geëmuleerd.  

In dit voorbeeld wordt een plaatsen bedrijfsapps in de bronregio in vnet's toegewezen en verbinding tussen de VNets tot stand gebracht. Met dit ontwerp kunnen ze geïsoleerde app failover uitvoeren en behouden van de bron privé IP-adressen in het doelnetwerk.

### <a name="before-failover"></a>Voordat de failover

Voordat de failover, wordt is de architectuur als volgt uit:

- Toepassings-VM's worden gehost in de primaire regio van Azië Azure:
    - **App1** virtuele machines bevinden zich in VNet **bron VNet 1**: 10.1.0.0/16.
    - **App2** virtuele machines bevinden zich in VNet **bron VNet 2**: 10.2.0.0/16.
    - **Bron VNet 1** heeft twee subnetten.
    - **Bron VNet 2** heeft twee subnetten.
- Secundaire (doel) regio is Azure Zuidoost-Azië - Zuidoost-Azië heeft een herstel VNets (**Recovery VNet 1** en **Recovery VNet 2**) die zijn identiek aan **bron VNet 1** en **Bron VNet 2**.
        - **Herstel VNet 1** en **Recovery VNet 2** hebben twee subnetten die overeenkomen met de subnetten van **bron VNet 1** en **bron VNet 2** -Zuidoost-Azië heeft een aanvullende VNet (**Azure VNet**) met adresruimte 10.3.0.0/16.
        - **Azure VNet** bevat een subnet (**Subnet 4**) met adresruimte 10.3.4.0/24.
        -Knooppunten voor SQL Server Always On replica domeincontroller enzovoort bevinden zich in **Subnet 4**.
- Er zijn een aantal van de site-naar-site VPN-verbindingen: 
    - **Bron VNet 1** en **Azure VNet**
    - **Bron VNet 2** en **Azure VNet**
    - **Bron VNet 1** en **bron VNet 2** zijn verbonden met VPN-site-naar-site
- **Herstel VNet 1** en **Recovery VNet 2** niet zijn verbonden met een andere VNets.
- **Een bedrijf** configureert de VPN-gateways voor **Recovery VNet 1** en **Recovery VNet 2**, te verminderen RTO.  
- **Herstel VNet1** en **Recovery VNet2** niet zijn verbonden met een ander virtueel netwerk.
- Beoogde hersteltijd (RTO), verminderen de VPN-gateways die zijn geconfigureerd op **Recovery VNet1** en **Recovery VNet2** voordat u een failover.

    ![Resources in Azure voordat de failover van de app](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-before-failover2.png)

### <a name="after-failover"></a>Na een failover

In het geval van een storing of een probleem dat betrekking heeft op een één-app (in ** bron VNet 2 in ons voorbeeld), een bedrijf kan de betreffende app als volgt herstellen:


- VPN-verbindingen tussen verbreken **bron VNet1** en **bron VNet2**, en tussen **bron VNet2** en **Azure VNet** .
- VPN-verbindingen maken tussen **bron VNet1** en **Recovery VNet2**, en tussen **Recovery VNet2** en **Azure VNet**.
- Virtuele machines in een failover **bron VNet2** naar **Recovery VNet2**.

![Resources in Azure app-failover](./media/site-recovery-retain-ip-azure-vm-failover/azure-to-azure-connectivity-isolated-application-after-failover2.png)


- In dit voorbeeld kan worden uitgebreid naar meer toepassingen en netwerkverbindingen. De aanbeveling is voor een model like-achtige verbinding, zoveel mogelijk failover-overschakeling uitvoeren van bron naar doel.
- VPN-Gateways maken gebruik van openbare IP-adressen en gateway hops verbindingen tot stand gebracht. Als u niet wilt gebruiken van openbare IP-adressen, of u wilt voorkomen dat extra hops, kunt u [Azure VNet-peering](../virtual-network/virtual-network-peering-overview.md) virtuele netwerken via [Azure-regio's ondersteund](../virtual-network/virtual-network-manage-peering.md#cross-region).

## <a name="hybrid-resources-full-failover"></a>Resources voor de Hybrid: volledige failover

In dit scenario **bedrijf B** wordt uitgevoerd van een bedrijf hybride met onderdeel van de infrastructuur van de toepassing die wordt uitgevoerd op Azure en de resterende on-premises uitgevoerd. 

### <a name="before-failover"></a>Voordat de failover

Hier ziet u hoe de netwerkarchitectuur eruitziet voordat de failover.

- Toepassings-VM's worden gehost in Azure Oost-Azië.
-  Oost-Azië heeft een VNet (**bron VNet**) met adresruimte 10.1.0.0/16.
    - Oost-Azië heeft werklasten verdeeld over drie subnetten in **bron VNet**:
        - **Subnet 1**: 10.1.1.0/24
        - **Subnet 2**: 10.1.2.0/24,
        - **Subnet 3**: een Azure-netwerk met adresruimte 10.1.0.0/16 10.1.3.0/24utilizing. Dit virtuele netwerk is met de naam **bron-VNet**
 - De regio secundaire (doel) is Azure Zuidoost-Azië:
    - Zuidoost-Azië heeft een herstel VNet (**Recovery VNet**) identiek is aan **bron VNet**.
- Virtuele machines in Oost-Azië zijn verbonden met een on-premises datacenter met Azure ExpressRoute of site-naar-site-VPN.
- Als u wilt verkleinen RTO, bepalingen bedrijf B gateways op herstel VNet in Azure Zuidoost-Azië voordat u een failover.
- Bedrijf B wijst/controleert of doel-IP-adressen voor gerepliceerde virtuele machines. Het doel-IP-adres is hetzelfde als de bron-IP-adres voor elke virtuele machine.


![On-premises-to-Azure-connectiviteit voordat de failover](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-before-failover2.png)

### <a name="after-failover"></a>Na een failover


Als een bron regionale uitval optreedt, kan bedrijf B alle bijbehorende resources naar de doelregio failover.

- Met de doel-IP-adressen al aanwezig zijn vóór de failover, bedrijf B failover te organiseren en automatisch verbindingen tot stand brengen na een failover tussen **Recovery VNet** en **Azure VNet**.
- Afhankelijk van de vereisten voor apps, verbindingen tussen de twee vnet's (**Recovery VNet** en **Azure VNet**) in de doel-regio kan worden vastgesteld voor, tijdens (als een tussenstap) of na de failover. Het bedrijf kan gebruiken [herstelplannen](site-recovery-create-recovery-plans.md) om op te geven wanneer verbindingen wordt tot stand worden gebracht.
- De oorspronkelijke verbinding tussen Azure-Oost-Azië en het on-premises datacenter moet worden verbroken voordat het tot stand brengen van de verbinding tussen Azure Zuidoost-Azië en on-premises datacentrum.
- De on-premises routering is geconfigureerd om te verwijzen naar de doelregio en gateways na een failover.

![On-premises-to-Azure-connectiviteit na een failover](./media/site-recovery-retain-ip-azure-vm-failover/on-premises-to-azure-connectivity-after-failover2.png)

## <a name="hybrid-resources-isolated-app-failover"></a>Resources voor de Hybrid: failover van de app (geïsoleerd)

Bedrijf B failover geen geïsoleerde apps op het subnetniveau. Dit is omdat de adresruimte op bron- en herstel VNets hetzelfde is, en de oorspronkelijke bron naar het on-premises verbinding actief is.

 - Voor tolerantie van de app moet Company B elke app plaatsen in een eigen toegewezen Azure-VNet.
 - Met elke app in een apart VNet, bedrijf B failover geïsoleerde apps, en verbindingen routeren naar de doelregio.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md).
