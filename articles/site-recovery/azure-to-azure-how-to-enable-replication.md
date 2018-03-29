---
title: Configureren van replicatie voor virtuele Azure-machines in Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven hoe u replicatie voor virtuele Azure-machines, van een Azure-regio naar de andere met Site Recovery configureren.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/09/2018
ms.author: asgang
ms.openlocfilehash: e5947242295a9c57b1c73e202c061d222cd0842f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Azure virtuele machines repliceren naar een andere Azure-regio


>[!NOTE]
>
> Site Recovery replicatie voor virtuele machines in Azure is momenteel in preview.

In dit artikel wordt beschreven hoe de replicatie van Azure VM's van een Azure-regio naar een andere in te schakelen.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u al hebt ingesteld Site Recovery voor dit scenario, zoals beschreven in de [Azure naar Azure zelfstudie](azure-to-azure-tutorial-enable-replication.md). Zorg ervoor dat u hebt voorbereid, de vereisten en de Recovery Services-kluis gemaakt.



## <a name="enable-replication"></a>Replicatie inschakelen

Replicatie inschakelen. Deze procedure wordt ervan uitgegaan dat de primaire Azure-regio Oost-Azië, en de secundaire regio Zuidoost-Azië.

1. Klik in de kluis op **+ repliceren**.
2. Houd rekening met de volgende velden:
    - **Bron**: het punt van de oorsprong van de virtuele machines, die in dit geval **Azure**.
    - **Bronlocatie**: de Azure-regio waar u wilt beveiligen van uw virtuele machines. In dit voorbeeld is de locatie van de Oost-Azië
    - **Implementatiemodel**: model van de Azure-implementatie van de bronmachine.
    - **Resourcegroep**: de resourcegroep waartoe uw virtuele bronmachines behoren. Alle VM's onder de geselecteerde resourcegroep worden vermeld voor beveiliging in de volgende stap.

    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. In **virtuele Machines > Selecteer de virtuele machines**en op elke virtuele machine die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.
    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. In **instellingen**, kunt u eventueel doel site-instellingen configureren:

    - **Doellocatie**: de locatie waar de brongegevens van de virtuele machine wordt gerepliceerd. Afhankelijk van uw locatie voor geselecteerde machines, biedt Site Recovery u de lijst met doelregio geschikt is. We adviseren dat u de doellocatie hetzelfde als de locatie van de Recovery Services-kluis.
    - **Doelresourcegroep**: de resourcegroep waarin alle uw gerepliceerde virtuele machines behoren. Azure Site Recovery maakt standaard een nieuwe resourcegroep in de doelregio met naam 'asr' achtervoegsel heeft. Als resourcegroep gemaakt door Azure Site Recovery al bestaat, wordt dit opnieuw gebruikt. U kunt ook aanpassen zoals weergegeven in de onderstaande sectie.
    - **Virtueel netwerk als doel**: Site Recovery maakt standaard een nieuw virtueel netwerk in de doelregio met naam 'asr' achtervoegsel heeft. Dit is toegewezen aan de bron-netwerk en voor eventuele toekomstige beveiliging gebruikt. [Meer informatie](site-recovery-network-mapping-azure-to-azure.md) over netwerktoewijzing.
    - **Storage-accounts als doel (als de bron-VM maakt geen gebruik van schijven die worden beheerd)**: Site Recovery maakt standaard een nieuw doelopslagaccount mimicking de opslagconfiguratie van de bron-VM. Als storage-account al bestaat, wordt dit opnieuw gebruikt.
    - **Replica beheerde schijven (als de bron-VM beheerde schijven gebruikt)**: maakt Site Recovery nieuwe replica beheerd schijven in de doelregio voor het spiegelen van de bron-VM beheerde schijven met hetzelfde opslagtype (Standard of premium) als de bron-VM van schijf beheerd.
    - **Storage-accounts in de cache**: Site Recovery moet extra opslagruimte account met de naam van cache-opslag in de regio van de bron. Alle wijzigingen die plaatsvinden op de bron-VM's worden bijgehouden en verzonden naar de cache storage-account voordat deze naar de doellocatie te repliceren.
    - **Beschikbaarheidsset**: Azure Site Recovery maakt standaard een nieuwe beschikbaarheidsset voor de doelregio met naam 'asr' achtervoegsel heeft. Als beschikbaarheidsset gemaakt door Azure Site Recovery al bestaat, wordt dit opnieuw gebruikt.
    - **Beleid voor wachtwoordreplicatie**: definieert de instellingen voor herstelpunt bewaren geschiedenis en app consistent de frequentie van momentopnamen. Azure Site Recovery maakt standaard een nieuw replicatiebeleid voor met de standaardinstellingen van 24 uur voor herstel bewaarperiode en "60 minuten voor de frequentie van app-consistente momentopname te maken.

    ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Doelresources aanpassen

U kunt de standaardinstellingen van de doel-gebruikt door Site Recovery kunt wijzigen.

1. Klik op **aanpassen:** standaardinstellingen wijzigen:
    - In **doelresourcegroep**, selecteer de resourcegroep in de lijst met alle resourcegroepen in de doellocatie van het abonnement.
    - In **virtuele doelnetwerk**, het netwerk selecteren in een lijst van het virtuele netwerk in de doellocatie.
    - In **beschikbaarheidsset**, u instellingen van de beschikbaarheidsset voor de virtuele machine kunt toevoegen als ze deel uitmaken van een beschikbaarheidsset voor de bron-regio.
    - In **doel Storage-accounts**, selecteert u het account dat u wilt gebruiken.

        ![Replicatie inschakelen](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Klik op **maken doelbron** > **replicatie inschakelen**.
3. Nadat de virtuele machines zijn ingeschakeld voor replicatie, kunt u de status van de status van de virtuele machine onder controleren **gerepliceerde items**

>[!NOTE]
>Tijdens de eerste replicatie kan de status even duren wilt vernieuwen, zonder uitgevoerd. Klik op de **vernieuwen** knop, de meest recente status ophalen.
>

# <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.
