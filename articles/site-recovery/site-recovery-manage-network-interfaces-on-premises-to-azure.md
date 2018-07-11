---
title: Netwerkinterfaces in Azure Site Recovery voor on-premises naar Azure-replicatie beheren | Microsoft Docs
description: Beschrijft hoe u netwerkinterfaces beheren voor on-premises naar Azure met Azure Site Recovery-replicatie
services: site-recovery
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: 75220d964f3e1208c85d34c1da97fab32044e62b
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917105"
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-replication"></a>Netwerkinterfaces van virtuele machines voor on-premises naar Azure-replicatie beheren

Een virtuele machine (VM) in Azure moet ten minste één netwerkinterface die is gekoppeld aan deze hebben. Er kan als veel netwerkinterfaces die zijn gekoppeld aan deze als de VM-grootte ondersteunt.

Standaard worden de eerste netwerkinterface die is gekoppeld aan een virtuele machine van Azure wordt gedefinieerd als de primaire netwerkinterface. Alle andere netwerkinterfaces in de virtuele machine zijn secundaire netwerkinterfaces. Standaard wordt al het uitgaande verkeer van de virtuele machine ook verzonden van het IP-adres dat toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface.

In een on-premises-omgeving hebben virtuele machines of servers meerdere netwerkinterfaces voor verschillende netwerken binnen de omgeving. Verschillende netwerken worden meestal gebruikt voor het uitvoeren van bepaalde bewerkingen, zoals upgrades, onderhoud en toegang tot internet. Wanneer u migratie of Failover-overschakeling uitvoeren naar Azure vanaf een on-premises-omgeving, houd er rekening mee dat netwerkinterfaces in dezelfde virtuele machine moeten allemaal zijn verbonden met hetzelfde virtuele netwerk.

Azure Site Recovery maakt standaard, zoals veel netwerkinterfaces op een Azure-machine zijn verbonden met de on-premises server. Het maken van redundante netwerkinterfaces tijdens de migratie of failover door te bewerken van de netwerkinterface-instellingen onder de instellingen voor de gerepliceerde virtuele machine, kunt u voorkomen.

## <a name="select-the-target-network"></a>Selecteer het doelnetwerk

Voor VMware en fysieke machines, en voor virtuele machines van Hyper-V (zonder System Center Virtual Machine Manager), kunt u het virtuele doelnetwerk voor afzonderlijke virtuele machines. Gebruik voor Hyper-V virtuele machines die worden beheerd met Virtual Machine Manager, [netwerktoewijzing](site-recovery-network-mapping.md) VM-netwerken die op een bronserver met Virtual Machine Manager toewijzen en doelnetwerken in Azure.

1. Onder **gerepliceerde items** gerepliceerde item voor toegang tot de instellingen voor het gerepliceerde item selecteren in een Recovery Services-kluis.

2. Selecteer de **berekening en netwerk** tabblad voor toegang tot de instellingen voor het gerepliceerde item.

3. Onder **netwerkeigenschappen**, een virtueel netwerk kiezen uit de lijst met beschikbare netwerkinterfaces.

    ![Netwerkinstellingen](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Wijzigen van het doelnetwerk is van invloed op alle netwerkinterfaces voor die specifieke virtuele machine.

Voor Virtual Machine Manager-clouds, Netwerktoewijzingen is van invloed op alle virtuele machines en hun netwerkinterfaces.

## <a name="select-the-target-interface-type"></a>Selecteer het doeltype-interface

Onder de **netwerkinterfaces** sectie van de **berekening en netwerk** deelvenster kunt u bekijken en bewerken van netwerkinterface-instellingen. Ook kunt u het doeltype van het netwerk-interface.

- Een **primaire** netwerkinterface is vereist voor de failover.
- Alle andere geselecteerde netwerkinterfaces, als een, **secundaire** netwerkinterfaces.
- Selecteer **gebruik geen** uitsluiten van een netwerkinterface van maken na een failover.

Wanneer u bij het inschakelen van replicatie, selecteert Site Recovery standaard alle gedetecteerde netwerkinterfaces op de on-premises server. Gemarkeerd als één **primaire** en alle andere als **secundaire**. Alle volgende interfaces toegevoegd aan de on-premises server zijn gemarkeerd **gebruik geen** standaard. Wanneer u meer netwerkinterfaces toevoegt, zorg ervoor dat de doelgrootte van de juiste virtuele machine van Azure voor alle vereiste netwerkinterfaces is geselecteerd.

## <a name="modify-network-interface-settings"></a>Netwerkinterface-instellingen wijzigen

U kunt het subnet en IP-adres voor de netwerkinterfaces van een gerepliceerde item wijzigen. Als een IP-adres niet opgegeven is, wordt Site Recovery de eerstvolgende beschikbare IP-adres van het subnet toewijzen aan de netwerkinterface na een failover.

1. Selecteer een beschikbare netwerkinterface openen van de netwerkinterface-instellingen.

2. Kies de gewenste subnet uit de lijst met beschikbare subnetten.

3. Voer het gewenste IP-adres (indien vereist).

    ![Netwerkinterface-instellingen](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Selecteer **OK** voltooit u de bewerking en terugkeren naar de **berekening en netwerk** deelvenster.

5. Herhaal stappen 1-4 voor andere netwerkinterfaces.

6. Selecteer **opslaan** alle wijzigingen op te slaan.

## <a name="next-steps"></a>Volgende stappen
  [Meer informatie](../virtual-network/virtual-network-network-interface-vm.md) over netwerkinterfaces voor virtuele machines van Azure.
