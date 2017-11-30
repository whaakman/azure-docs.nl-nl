---
title: Virtuele Azure-machines repliceren naar een secundaire regio met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe Azure VM's worden uitgevoerd in een Azure-regio naar een andere regio via de Azure Site Recovery-service worden gerepliceerd.
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/29/2017
ms.author: asgang
ms.openlocfilehash: 114390712f5e5bf62ec515db62469e09361b8f85
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Azure virtuele machines repliceren naar een andere Azure-regio


In dit artikel wordt beschreven hoe virtuele Azure-machines (VM's) in een Azure-regio worden gerepliceerd naar een secundair Azure-regio, via de Azure Site Recovery-service.

>[!NOTE]
>
> Azure VM-replicatie met Site Recovery is momenteel in preview.

## <a name="prerequisites"></a>Vereisten

* U moet beschikken over een Recovery Services-kluis. U wordt aangeraden dat u de kluis in de doelregio waarnaar u wilt dat uw virtuele machines maken worden gerepliceerd.
* Als u regels Netwerkbeveiligingsgroep groepen (NSG) of een firewallproxy gebruikt voor toegang tot uitgaande internetverbinding op de Azure VM's beheren, ervoor zorgen dat u de vereiste URL's of IP-adressen toestaan. [Meer informatie](./site-recovery-azure-to-azure-networking-guidance.md).
* Als u een ExpressRoute- of een VPN-verbinding tussen on-premises en de bronlocatie in Azure, [informatie over het instellen van deze rollen](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration).
* Uw Azure gebruikersaccount moet [specifieke machtigingen](../site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines), replicatie van een virtuele machine van Azure in te schakelen.
Uw Azure-abonnement moet worden ingeschakeld voor het maken van virtuele machines in de doellocatie die u wilt gebruiken als een herstel na noodgevallen regio. Neem contact op met ondersteuning voor het inschakelen van de quota voor vereist.

## <a name="enable-replication"></a>Replicatie inschakelen

In deze procedure Oost-Azië gebruikt als de bronlocatie en Zuidoost-Azië als doel.

1. Klik op **+ repliceren** in de kluis replicatie voor de virtuele machines in te schakelen.
2. Controleer **bron:** is ingesteld op **Azure**.
3. Stel **bronlocatie** naar Oost-Azië.
4. In **implementatiemodel**, selecteer **klassieke** of **Resource Manager**.
5. In **resourcegroep**, selecteer de groep waartoe uw bron-VM's behoren. Alle VM's onder de geselecteerde resourcegroep worden weergegeven.

    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

6. In **virtuele Machines > Selecteer de virtuele machines**op en selecteert u elke virtuele machine die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)
    
7. Onder **instellingen** > **doellocatie**, geef aan waar de bron-VM-gegevens worden gerepliceerd. Site Recovery biedt een lijst met geschikte doelregio, afhankelijk van de regio van de geselecteerde virtuele machines.
8. Site Recovery stelt standaard doelinstellingen. Deze kunnen worden gewijzigd zoals vereist is.

    - **Doelresourcegroep**. Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met het achtervoegsel 'asr'. Als de groep gemaakte resource al bestaat, wordt dit opnieuw gebruikt.
    - **Virtuele doelnetwerk**. Site Recovery maakt standaard een nieuw virtueel netwerk in de doelregio, met het achtervoegsel 'asr'. Dit netwerk wordt toegewezen aan uw Bronnetwerk. [Meer informatie](site-recovery-network-mapping-azure-to-azure.md) over netwerktoewijzing.
    - **Storage-accounts zijn gericht**. Site Recovery maakt standaard een nieuw opslagaccount voor doel die overeenkomt met de opslagconfiguratie van de bron-VM. Als het gemaakte account al bestaat, wordt dit opnieuw gebruikt.
    - **Storage-accounts in de cache**. Azure Site Recovery maakt een extra cache storage-account, de bron-regio. Alle wijzigingen op de bron-VM's worden bijgehouden en verzonden naar de cache opslagaccount voordat de replicatie naar de doellocatie.
    - **Beschikbaarheidsset**. Site Recovery maakt standaard een nieuwe beschikbaarheidsset voor de doelregio met een achtervoegsel 'asr'. Als de gemaakte set al bestaat, wordt dit opnieuw gebruikt.
    - **Beleid voor wachtwoordreplicatie**. Site Recovery definieert de instellingen voor herstelgeschiedenis punt bewaren en de frequentie van toepassingsconsistente momentopnamen. Site Recovery maakt standaard een nieuw replicatiebeleid voor met de standaardinstellingen van 24 uur voor herstel bewaarperiode en 60 minuten voor de frequentie van toepassingsconsistente momentopnamen.

    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)
9. Klik op **replicatie inschakelen** om te beginnen met het beveiligen van virtuele machines.

## <a name="customize-target-resources"></a>Doelresources aanpassen

1. Wijzig een van deze standaardinstellingen doel:

    - **Doelresourcegroep**. Selecteer een resourcegroep in de lijst met alle resourcegroepen in de doellocatie in het abonnement.
    - **Virtuele doelnetwerk**. Selecteer in de lijst van de virtuele netwerken in de doellocatie.
    - **Beschikbaarheidsset** u kunt alleen instellingen voor beschikbaarheid sets toevoegen aan virtuele machines zich in een set in de bron-regio.
    - **Storage-accounts zijn gericht**: toevoegen van een account dat beschikbaar is.

        ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Klik op **maken doelbron** > **replicatie inschakelen**. Tijdens de eerste replicatie kan VM-status even duren om te vernieuwen. Klik op **vernieuwen** ophalen van de meest recente status.

    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)
    
3. Nadat de virtuele machines worden beschermd, Controleer de status van de VM in **gerepliceerde items**.



## <a name="next-steps"></a>Volgende stappen
[Meer informatie over](../azure-to-azure-tutorial-dr-drill.md) hoe u een testfailover uitvoeren.

