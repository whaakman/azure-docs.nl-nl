---
title: Een testfailover voor virtuele machine van Azure replicatie uitvoeren met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de stappen die u nodig hebt voor een testfailover voor het virtuele Azure-machines repliceren naar een andere Azure-regio met de Azure Site Recovery-service wordt uitgevoerd.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: e15c1b0c-5d75-4fdf-acb0-e61def9e9339
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 8babb0d016729f318442af93596d206c38d91206
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-run-a-test-failover-for-azure-vm-replication"></a>Stap 6: Een testfailover voor replicatie van de virtuele machine in Azure uitvoeren

Wanneer u replicatie voor virtuele Azure-machine (VM's) hebt ingeschakeld, volgt u de stappen in dit artikel voor testfailover uitvoeren vanaf een Azure-regio naar de andere met het [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.

- Wanneer u klaar bent met het artikel, moet met een testfailover hebt gecontroleerd dat ten minste één virtuele machine in Azure een failover naar uw secundaire Azure-regio. 
- Eventuele opmerkingen onder aan dit artikel plaatsen of vragen in de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Replicatie van Azure VM is momenteel in preview.


## <a name="before-you-start"></a>Voordat u begint

- Voordat u een failovertest uitvoert, wordt aangeraden dat u controleren of de VM-eigenschappen en breng eventuele wijzigingen die u wilt. u kunt toegang tot de VM-eigenschappen in **gerepliceerde items**. De **Essentials** blade ziet u informatie over machines instellingen en status.
- We raden u een afzonderlijk Azure VM-netwerk gebruiken voor de testfailover en niet op het netwerk (standaard of aangepast) die is geconfigureerd voor failover voor productie.
- De testfailover is mislukt via Azure Virtual machines (en hun opslag) naar de secundaire Azure-regio. Het niet alle afhankelijke apps of resources worden gerepliceerd. Als apps worden uitgevoerd op mislukte gedurende VM's afhankelijk van andere resources, zoals Active Directory en DNS zijn, moet u deze ook gerepliceerd als ze nog niet beschikbaar in uw secundaire regio. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-active-directory-and-dns)
- Als u toegang wilt tot gerepliceerde virtuele machines na failover van een on-premises site, moet u [voorbereiden op het verbinden](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) voor deze virtuele machines.

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1. In **instellingen** > **gerepliceerde Items**, klikt u op de virtuele machine **+ Testfailover** pictogram. 

2. In **Testfailover**, selecteer een herstelpunt voor de failover gebruiken:

    - **Meest recente verwerkte**: de virtuele machine wordt overgenomen door de meest recente herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Met deze optie geen tijd besteed aan het verwerken van gegevens, zodat het biedt een laag RTO (beoogde hersteltijd).
    - **Meest recente app-consistente**: deze optie alle VM's naar de meest recente herstelpunt van app-consistente failover. Het tijdstempel wordt weergegeven. 
    - **Aangepaste**: eender welk herstelpunt kiezen.
 
3. Selecteer de doel-Azure virtueel netwerk voor welke Azure VM's in de secundaire regio moet worden verbonden nadat de failover plaatsvindt.
4. Voor het starten van de failover, klikt u op **OK**. Om de voortgang volgen, klikt u op de virtuele machine om de eigenschappen te openen. U kunt ook op de **Testfailover** taak in de kluisnaam > **instellingen** > **taken** > **Site Recovery-taken**.
5. Nadat de failover is voltooid, de replica virtuele machine in Azure wordt weergegeven in de Azure portal > **virtuele Machines**. Zorg ervoor dat de virtuele machine is de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd.
6. Als u wilt verwijderen van de virtuele machines die zijn gemaakt tijdens de testfailover, klikt u op **opschonen testfailover** voor het gerepliceerde artikel of het herstelplan. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover. 

[Meer informatie](site-recovery-test-failover-to-azure.md) over testfailovers.

## <a name="next-steps"></a>Volgende stappen

Nadat u de failover hebt getest, wordt in dit scenario is voltooid. Nu kunt u informatie over het uitvoeren van failovers in productie:

- [Meer informatie](site-recovery-failover.md) over verschillende soorten failovers en hoe ze uit te voeren.
- Meer informatie over mislukte over meerdere virtuele machines [met behulp van een herstelplan](site-recovery-create-recovery-plans.md).
- Meer informatie over [herstelplannen met](site-recovery-create-recovery-plans.md).
- Meer informatie over [andere virtuele Azure-machines](site-recovery-how-to-reprotect.md) na een failover.

