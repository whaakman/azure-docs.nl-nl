---
title: Toepassingen (Azure-Azure) repliceren | Microsoft Docs
description: In dit artikel wordt beschreven hoe de replicatie van virtuele machines in een Azure-regio naar een andere regio in Azure instellen.
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
ms.date: 12/08/2017
ms.author: asgang
ms.openlocfilehash: 209ec47388ee7291f8107df022e0c2bb202ba6b5
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Azure virtuele machines repliceren naar een andere Azure-regio



>[!NOTE]
>
> Site Recovery replicatie voor virtuele machines in Azure is momenteel in preview.

In dit artikel wordt beschreven hoe de replicatie van virtuele machines in een Azure-regio naar een andere Azure-regio instellen.

## <a name="prerequisites"></a>Vereisten

* Het artikel wordt ervan uitgegaan dat u al over de Site Recovery en de Recovery Services-kluis weet. U moet een 'Recovery services-kluis' vooraf gemaakte hebben.

    >[!NOTE]
    >
    > Het verdient aanbeveling dat u de 'Recovery services-kluis' op de locatie waar u uw virtuele machines maken worden gerepliceerd. Als uw doellocatie 'VS-midden', bijvoorbeeld kluis in 'VS-midden' maken.

* Als u regels van de Netwerkbeveiligingsgroep groepen (NSG) of firewall-proxy gebruikt toegang tot uitgaande internetverbinding op de Azure VM's, zorg ervoor dat u goedgekeurde IP-adressen de vereiste URL's of IP-adressen. Raadpleeg [leidraad voor netwerken](./site-recovery-azure-to-azure-networking-guidance.md) voor meer informatie.

* Als u een ExpressRoute- of een VPN-verbinding tussen on-premises en de bronlocatie in Azure hebt, voert u de [Site Recovery-overwegingen voor Azure voor de lokale ExpressRoute / VPN-configuratie](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) document.

* Uw Azure gebruikersaccount moet zijn bepaalde [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) replicatie van Azure een virtuele machine in te schakelen.

* Uw Azure-abonnement moet worden ingeschakeld voor het maken van virtuele machines in de doellocatie die u wilt gebruiken als DR regio. Neem contact op met ondersteuning om in te schakelen van de quota voor vereist.

## <a name="enable-replication-from-azure-site-recovery-vault"></a>Schakel replicatie van Azure Site Recovery-kluis
Virtuele we voor deze afbeelding wordt uitgevoerd in de Oost-Azië-machines repliceren naar de locatie, Zuidoost-Azië ' Azure-locatie. De stappen zijn als volgt:

 Klik op **+ repliceren** in de kluis replicatie voor de virtuele machines in te schakelen.

1. **Bron:** deze verwijst naar het punt van oorsprong van de machines die in dit geval **Azure**.

2. **Bronlocatie:** is de Azure-regio waar u wilt beveiligen van uw virtuele machines. In dit voorbeeld is de locatie van de Oost-Azië

3. **Implementatiemodel:** het verwijst naar het model van de Azure-implementatie van de bronmachine. U kunt ofwel Classic selecteren of Resource Manager en de machines die horen bij het specifieke model wordt weergegeven voor beveiliging in de volgende stap.

      >[!NOTE]
      >
      > U kunt alleen een klassieke virtuele machine repliceren en deze herstellen als een klassieke virtuele machine. U kunt deze niet herstellen als de virtuele machine van een Resource Manager.

4. **Resourcegroep:** is de resourcegroep waartoe uw virtuele bronmachines behoren. Alle VM's onder de geselecteerde resourcegroep wordt vermeld voor beveiliging in de volgende stap.

    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

In **virtuele Machines > Selecteer de virtuele machines**en op elke machine die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op OK.
    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)


Onder de sectie instellingen kunt u doel voor site-eigenschappen configureren

1. **Doellocatie:** dit is de locatie waar de brongegevens van de virtuele machine wordt gerepliceerd. Afhankelijk van uw locatie voor geselecteerde machines, biedt Site Recovery u de lijst met doelregio geschikt is.

    > [!TIP]
    > Het verdient aanbeveling dat doellocatie dezelfde vanaf uw recovery services-kluis.

2. **Doelresourcegroep:** is de resourcegroep waarin alle uw gerepliceerde virtuele machines behoren. Azure Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met naam 'asr' achtervoegsel heeft. Als resourcegroep gemaakt door Azure Site Recovery al bestaat, wordt opnieuw gebruikt. U kunt ook aanpassen zoals weergegeven in de onderstaande sectie.    
3. **Virtuele doelnetwerk:** Azure Site Recovery maakt standaard een nieuw virtueel netwerk in de doelregio met naam 'asr' achtervoegsel heeft. Dit worden toegewezen aan de bron-netwerk en wordt gebruikt voor alle toekomstige beveiliging.

    > [!NOTE]
    > [Controleer de gegevens van de netwerken](site-recovery-network-mapping-azure-to-azure.md) voor meer informatie over de netwerktoewijzing.

4. **Storage-accounts als doel:** Azure Site Recovery maakt standaard een nieuw doelopslagaccount mimicking de opslagconfiguratie van de bron-VM. Als storage-account gemaakt door Azure Site Recovery al bestaat, wordt opnieuw gebruikt.

5. **Storage-accounts in de cache:** Azure Site Recovery moet extra opslagruimte account met de naam van cache-opslag in de regio van de bron. Alle wijzigingen die plaatsvinden op de bron-VM's worden bijgehouden en verzonden naar de cache storage-account voordat deze naar de doellocatie te repliceren.

6. **Beschikbaarheidsset:** Azure Site Recovery maakt standaard een nieuwe beschikbaarheidsset voor de doelregio met naam 'asr' achtervoegsel heeft. Als beschikbaarheidsset gemaakt door Azure Site Recovery al bestaat, wordt dit opnieuw gebruikt.

7.  **Beleid voor wachtwoordreplicatie:** definieert de instellingen voor herstelpunt bewaren geschiedenis en app consistent de frequentie van momentopnamen. Azure Site Recovery maakt standaard een nieuw replicatiebeleid voor met de standaardinstellingen van 24 uur voor herstel bewaarperiode en "60 minuten voor de frequentie van app-consistente momentopname te maken.

    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Doelresources aanpassen

Als u wijzigen van de standaardwaarden worden gebruikt door Azure Site Recovery wilt, kunt u de instellingen op basis van uw behoeften kunt wijzigen.

1. **Aanpassen:** Klik hierop om te wijzigen van de standaardwaarden worden gebruikt door Azure Site Recovery.

2. **Doelresourcegroep:** kunt u de resourcegroep uit de lijst met alle resourcegroepen die aanwezig is in de doellocatie in het abonnement.

3. **Virtuele doelnetwerk:** vindt u de lijst van het virtuele netwerk in de doellocatie.

4. **Beschikbaarheidsset:** u kunt alleen instellingen voor beschikbaarheid sets toevoegen aan de virtuele machines die deel van de beschikbaarheid in regio van de bron uitmaken.

5. **Doel Storage-accounts:**

![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klikt u op **doelbron maken** en replicatie inschakelen


Wanneer virtuele machines worden beschermd, kunt u de status van de status van de virtuele machines onder controleren **gerepliceerde items**

>[!NOTE]
>Tijdens de periode van de initiële replicatie kan er een kans dat status kost tijd om te vernieuwen en er geen uitgevoerd gedurende een bepaalde periode. U kunt klikken op de knop Vernieuwen boven aan de blade om op te halen van de meest recente status.
>

![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.
- [Meer informatie](site-recovery-failover.md) over verschillende soorten failovers en hoe ze uit te voeren.
- Meer informatie over [met herstelplannen](site-recovery-create-recovery-plans.md) te verminderen RTO.
- Meer informatie over [andere virtuele Azure-machines](site-recovery-how-to-reprotect.md) na een failover.
