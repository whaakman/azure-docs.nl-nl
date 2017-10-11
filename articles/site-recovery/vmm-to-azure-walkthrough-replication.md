---
title: Instellen van een beleid voor wachtwoordreplicatie voor replicatie naar Azure met Azure Site Recovery Hyper-V-machine (met VMM) | Microsoft Docs
description: Hierin wordt beschreven hoe u een beleid voor wachtwoordreplicatie voor Hyper-V-machine (met VMM)-replicatie naar Azure met Azure Site Recovery instellen
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 592e1c3f647e5b1f1d9aa776657e8f89b60349e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-to-azure"></a>Stap 10: Een replicatiebeleid voor virtuele machine van Hyper-V-replicatie (met VMM) naar Azure instellen


Na het instellen van [netwerktoewijzing](vmm-to-azure-walkthrough-network-mapping.md), gebruik dit artikel voor het configureren van een beleid voor wachtwoordreplicatie T\to repliceren lokale Hyper-V virtuele machines die worden beheerd in System Center Virtual Machine Manager (VMM)-clouds naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.

Na het lezen van dit artikel kunt u onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.



## <a name="create-a-policy"></a>Een beleid maken

1. Als u nieuw replicatiebeleid wilt maken, klikt u op **Infrastructuur voorbereiden** > **Replicatie-instellingen** > **+Maken en koppelen**.

    ![Netwerk](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. Geef in **Beleid maken en koppelen** een beleidsnaam op.
3. Geef in **Kopieerfrequentie** op hoe vaak u na de eerste replicatie de verschillen wilt repliceren (elke 30 seconden of elke 5 of 15 minuten).

    > [!NOTE]
    >  Een frequentie van 30 seconden wordt niet ondersteund bij het repliceren naar Premium Storage. De beperking wordt bepaald door het aantal momentopnamen per blob (100) dat wordt ondersteund door Premium Storage. [Meer informatie](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. Geef in **Bewaarperiode van het herstelpunt** op hoeveel uur elk herstelpunt moet worden bewaard. Beveiligde machines kunnen te allen tijde worden hersteld naar een willekeurig punt (binnen een bepaald tijdsvenster).
5. Geef in **Frequentie van de app-consistente momentopname** op hoe vaak (elke 1-12 uur) er herstelpunten moeten worden gemaakt met toepassingsconsistente momentopnamen. Hyper-V maakt gebruik van twee soorten momentopnamen: standaardmomentopnamen waarmee steeds incrementele momentopnamen van de volledige virtuele machine worden gemaakt, en toepassingsconsistente momentopnamen waarmee een tijdsgebonden momentopname wordt gemaakt van de toepassingsgegevens in de virtuele machine. Toepassingsconsistente momentopnamen gebruiken Volume Shadow Copy Service (VSS) om ervoor te zorgen dat toepassingen een consistente status hebben wanneer er een momentopname wordt gemaakt. Als u toepassingsconsistente momentopnamen inschakelt, is dit van invloed op de prestaties van de toepassingen die via de virtuele bronmachines worden uitgevoerd. Zorg ervoor dat de waarde die u instelt, kleiner is dan het aantal aanvullende herstelpunten dat u configureert.
6. Geef in **Begintijd initiële replicatie** aan wanneer de initiële replicatie moet worden gestart. De replicatie maakt gebruik van uw internetbandbreedte. Daarom is het een goed idee om de replicatie buiten de drukste tijden in te plannen.
7. Geef in **Gegevens versleutelen die zijn opgeslagen in Azure** op of u at-rest-gegevens in de Azure-opslag wilt versleutelen. Klik vervolgens op **OK**.

    ![Beleid voor replicatie](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. Wanneer u nieuw beleid maakt, wordt dit automatisch gekoppeld aan de VMM-cloud. Klik op **OK**. U kunt aanvullende VMM-clouds (en de virtuele machines hierin) koppelen aan dit replicatiebeleid. Dit doet u via **Replicatie** > beleidsnaam > **VMM-cloud koppelen**.

    ![Beleid voor replicatie](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 11: replicatie inschakelen](vmm-to-azure-walkthrough-enable-replication.md)
