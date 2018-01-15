---
title: Netwerkinterfaces in Azure Site Recovery voor on-premises naar Azure scenario's beheren | Microsoft Docs
description: Hierin wordt beschreven hoe u netwerkinterfaces voor on-premises naar Azure scenario's met Azure Site Recovery beheren
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
ms.date: 12/22/2017
ms.author: manayar
ms.openlocfilehash: 036d5c2945bd6730d65f88f72c9377047fefcde6
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-scenarios"></a>Netwerkinterfaces van virtuele machines voor on-premises naar Azure scenario's beheren

Een virtuele machine (VM) in Azure moet ten minste één netwerkinterface is gekoppeld. Deze kan hebben als veel netwerkinterfaces gekoppeld als de VM-grootte ondersteunt. 

Standaard worden de eerste netwerkinterface is gekoppeld aan een virtuele machine van Azure wordt gedefinieerd als de primaire netwerkinterface. Alle andere netwerkinterfaces in de virtuele machine zijn secundaire netwerkinterfaces. Ook wordt standaard al het uitgaande verkeer van de virtuele machine uit het IP-adres dat toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface verzonden.

In een on-premises-omgeving, kunnen virtuele machines of servers meerdere netwerkinterfaces voor verschillende netwerken binnen de omgeving hebben. Verschillende netwerken worden meestal gebruikt voor het uitvoeren van specifieke bewerkingen zoals upgrades, onderhoud en toegang tot internet. Wanneer u migratie of failover wordt uitgevoerd naar Azure uit een on-premises-omgeving, houd er rekening mee dat netwerkinterfaces in dezelfde virtuele machine allemaal met hetzelfde virtuele netwerk verbonden moeten.

Standaard maakt Azure Site Recovery als veel netwerkinterfaces op Azure een virtuele machine zijn verbonden met de on-premises server. U kunt voorkomen dat redundante netwerkinterfaces tijdens de migratie of failover maken door de netwerkinterface-instellingen onder de instellingen voor de gerepliceerde virtuele machine te bewerken.

## <a name="select-the-target-network"></a>Selecteer het doelnetwerk

Voor VMware en fysieke machines, en voor virtuele machines van Hyper-V (zonder System Center Virtual Machine Manager), kunt u het virtuele doelnetwerk voor afzonderlijke virtuele machines. Gebruik voor Hyper-V virtuele machines die worden beheerd met Virtual Machine Manager, [netwerktoewijzing](site-recovery-network-mapping.md) toewijzen VM-netwerken op een Virtual Machine Manager-beheerserver bron en doel van de Azure-netwerken.

1. Onder **gerepliceerde items** gerepliceerde item toegang krijgen tot de instellingen voor het gerepliceerde object selecteren in een Recovery Services-kluis.

2. Selecteer de **berekening en netwerk** tabblad voor toegang tot de netwerkinstellingen voor het gerepliceerde item.

3. Onder **netwerkeigenschappen**, een virtueel netwerk kiezen uit de lijst met beschikbare netwerkinterfaces.

    ![Netwerkinstellingen](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Wijzigen van het doelnetwerk is van invloed op alle netwerkinterfaces voor die specifieke virtuele machine.

Voor Virtual Machine Manager-clouds, netwerktoewijzing wijzigen geldt voor alle virtuele machines en hun netwerkinterfaces.

## <a name="select-the-target-interface-type"></a>Selecteer het doel-interfacetype

Onder de **netwerkinterfaces** sectie van de **berekening en netwerk** deelvenster kunt u bekijken en bewerken van netwerkinterface-instellingen. Ook kunt u het doeltype van de netwerk-interface.

- Een **primaire** netwerkinterface is vereist voor failover.
- Alle andere geselecteerde netwerkinterfaces, als een, **secundaire** netwerkinterfaces.
- Selecteer **gebruik geen** uitsluiten van een netwerkinterface van het maken van failover wordt uitgevoerd.

Wanneer u bij het inschakelen van replicatie, selecteert Site Recovery standaard alle gedetecteerde netwerkinterfaces op de on-premises server. Gemarkeerd als één **primaire** en alle andere als **secundaire**. Alle volgende interfaces toevoegen op de on-premises server zijn gemarkeerd **gebruik geen** standaard. Wanneer u meer netwerkinterfaces toevoegt, zorg ervoor dat de juiste virtuele machine van Azure doelgrootte is ingeschakeld voor alle vereiste netwerkinterfaces.

## <a name="modify-network-interface-settings"></a>Netwerkinterface-instellingen wijzigen

U kunt het subnet en IP-adres voor de netwerkinterfaces van een gerepliceerde item wijzigen. Als een IP-adres niet is opgegeven, wordt het volgende beschikbare IP-adres van het subnet door Site Recovery aan de netwerkinterface op failover toewijzen.

1. Selecteer een beschikbare netwerkinterface openen van de netwerkinterface-instellingen.

2. Kies de gewenste subnet uit de lijst met beschikbare subnetten.

3. Geef de gewenste IP-adres (indien vereist).

    ![Netwerkinterface-instellingen](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Selecteer **OK** hebt bewerkt en terugkeren naar de **berekening en netwerk** deelvenster.

5. Herhaal stap 1-4 voor andere netwerkinterfaces.

6. Selecteer **opslaan** alle wijzigingen op te slaan.

## <a name="next-steps"></a>Volgende stappen
  [Meer informatie](../virtual-network/virtual-network-network-interface-vm.md) over netwerkinterfaces van virtuele machines in Azure.
