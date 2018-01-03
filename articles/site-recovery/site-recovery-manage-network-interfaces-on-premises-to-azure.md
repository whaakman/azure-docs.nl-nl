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
ms.openlocfilehash: b60c746ae6c243d2b448056f8768df3baaed4cc1
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-scenarios"></a>Netwerkinterfaces van virtuele machines voor on-premises naar Azure scenario's beheren

Een virtuele machine in Azure moet ten minste één netwerkinterface is gekoppeld, en kunnen hebben zoveel netwerkinterfaces gekoppeld als de VM-grootte ondersteunt. Standaard worden de eerste netwerkinterface is gekoppeld aan een virtuele machine van Azure wordt gedefinieerd als de primaire netwerkinterface. Alle andere netwerkinterfaces in de virtuele machine zijn secundaire netwerkinterfaces. Standaard wordt al het uitgaande verkeer van de virtuele machine het IP-adres dat is toegewezen aan de primaire IP-adresconfiguratie van de primaire netwerkinterface verzonden.

Virtuele machines /-servers kunnen meerdere netwerkinterfaces voor verschillende netwerken binnen de omgeving hebben in een on-premises omgeving. Verschillende netwerken worden meestal gebruikt voor het uitvoeren van specifieke bewerkingen zoals upgrades, het onderhoud, toegang tot internet, enzovoort. Wanneer u migratie of failover wordt uitgevoerd naar Azure uit een on-premises-omgeving, houd er rekening mee dat netwerkinterfaces in dezelfde virtuele machine allemaal met hetzelfde virtuele netwerk verbonden moeten.

Standaard maakt Site Recovery als veel netwerkinterfaces op Azure een virtuele machine zijn verbonden met de on-premises server. U kunt voorkomen dat redundante netwerkinterfaces tijdens de migratie of failover maken door de netwerkinterface-instellingen onder instellingen voor de gerepliceerde virtuele machine te bewerken.

## <a name="select-the-target-network"></a>Selecteer het doelnetwerk

Voor VMware en fysieke machines, en voor Hyper-V (zonder VMM) virtuele machines, kunt u het virtuele doelnetwerk voor afzonderlijke virtuele machines. Voor Hyper-V virtuele machines die worden beheerd door VMM, [netwerktoewijzing](site-recovery-network-mapping.md) toewijzen van VM-netwerken op de VMM-server van een bron en doel van de Azure-netwerken wordt gebruikt.

1. Klik op een gerepliceerde item toegang krijgen tot de instellingen voor het gerepliceerde object onder 'Gerepliceerde items' in een Recovery Services-kluis.

2. Klik op het tabblad 'Berekening en netwerk' voor toegang tot de netwerkinstellingen voor het gerepliceerde item.

3. Kies onder 'Eigenschappen van het netwerk', een virtueel netwerk in de lijst met beschikbare netwerkinterfaces.

    ![Berekening en netwerk](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Wijzigen van het doelnetwerk is van invloed op alle netwerkinterfaces voor die specifieke virtuele machine.

Voor VMM-clouds netwerktoewijzing wijzigen geldt voor alle virtuele machines en hun netwerkinterfaces.

## <a name="select-the-target-interface-type"></a>Selecteer het doel-interfacetype

Onder het gedeelte 'Netwerkinterfaces' op de blade 'Berekening en netwerk' kunt u weergeven en bewerken netwerkinterface-instellingen, en geef het doeltype van de netwerk-interface.

- Een **primaire** netwerkinterface is vereist voor failover.
- Alle andere geselecteerde netwerkinterfaces, als een, **secundaire** netwerkinterfaces.
- Selecteer **gebruik geen** uitsluiten van een netwerkinterface van het maken van failover wordt uitgevoerd.

Standaard, wanneer het inschakelen van replicatie, Site Recovery selecteert gedetecteerde alle netwerkinterfaces op de lokale server, een 'Primaire' en alle andere als 'Secundair' netwerkinterfaces is gemarkeerd. Alle volgende interfaces toevoegen op de on-premises server zijn gemarkeerd, gebruik niet' standaard. Zorg ervoor dat de juiste virtuele machine van Azure doelgrootte voor alle vereiste netwerkinterfaces is geselecteerd bij het toevoegen van meer netwerkinterfaces.

## <a name="modifying-network-interface-settings"></a>Netwerkinterface-instellingen wijzigen

U kunt het subnet en IP-voor de netwerkinterfaces van een gerepliceerde item wijzigen. Als een IP-adres niet is opgegeven, wordt het volgende beschikbare IP-adres van het subnet door Site Recovery aan de netwerkinterface op failover-toewijzen.

1. Klik op een beschikbare netwerkinterface openen van de instellingenblade van network interface.

2. Kies de gewenste subnet uit de lijst met beschikbare subnetten.

3. Voer het gewenste IP-adres (indien vereist).

    ![Netwerkinterface-instellingen](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Klik op OK hebt bewerkt en terug te keren naar de blade 'Berekening en netwerk'.

5. Herhaal stap 1-4 voor andere netwerkinterfaces.

6. Klik op 'Opslaan' alle wijzigingen op te slaan.

## <a name="next-steps"></a>Volgende stappen
  [Meer informatie](../virtual-network/virtual-network-network-interface-vm.md) over netwerkinterfaces van virtuele machines in Azure.
