---
title: Configureren en beheren van beleidsregels voor replicatie voor VMware-replicatie met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u configureert replicatie-instellingen voor VMware-replicatie naar Azure met Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: sutalasi
ms.openlocfilehash: 6a8e6e7c6fbdbcbf58557a0896e976a608164041
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="configure-and-manage-replication-policies-for-vmware-replication"></a>Configureren en beheren van beleidsregels voor replicatie voor VMware-replicatie
In dit artikel wordt beschreven hoe u een beleid voor wachtwoordreplicatie wanneer u bent virtuele VMware-machines repliceren naar Azure met behulp van [Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>Een beleid maken

1. Selecteer **Beheren** > **Infrastructuur voor Site Recovery**.
2. In **voor VMware en fysieke machines**, selecteer **replicatiebeleid**. 
3. Klik op **+ replicatiebeleid**, en geef de naam van het beleid.
5. Geef de limiet voor de RPO op bij **RPO-drempelwaarde**. Waarschuwingen worden gegenereerd wanneer continue replicatie deze limiet overschrijdt.
6. Geef in **Bewaarperiode van het herstelpunt** de duur (in uren) op dat elk herstelpunt moet worden bewaard. Beveiligde machines kunnen binnen een bepaald tijdsvenster te allen tijde worden hersteld naar een willekeurig punt. Een bewaarperiode van maximaal 24 uur wordt ondersteund voor computers die worden gerepliceerd naar Premium Storage. Tot 72 uur wordt ondersteund voor standard-opslag.
7. Geef in **Frequentie van de app-consistente momentopname** op hoe vaak (in minuten) er herstelpunten moeten worden gemaakt met toepassingsconsistente momentopnamen.
8. Klik op **OK**. Het beleid wordt binnen 30 seconden tot 60 minuut gemaakt.

Wanneer u een beleid voor wachtwoordreplicatie maakt, wordt een overeenkomende failback replicatiebeleid automatisch gemaakt, met het achtervoegsel 'failback'. Nadat het beleid is gemaakt, kunt u deze bewerken door deze te selecteren > **instellingen bewerken**.

## <a name="associate-a-configuration-server"></a>Een configuratieserver koppelen 

Het replicatiebeleid koppelen aan uw configuratie van lokale server.

1. Klik op **koppelen**, en selecteer de configuratieserver.

    ![Configuratieserver koppelen](./media/vmware-azure-set-up-replication/associate1.png)

2. Klik op **OK**. De configuratieserver wordt binnen één tot twee minuten gekoppeld.

    ![Configuratieserver koppelen](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>Ontkoppelen of verwijderen van een beleid voor wachtwoordreplicatie
1. Kies het replicatiebeleid.
    a. Als u wilt ontkoppelen van het beleid van de configuratieserver, zorg ervoor dat er geen gerepliceerde machines worden het beleid. Klik vervolgens op **Dissociate**.
    b. Zorg dat deze niet is gekoppeld aan een configuratieserver voor het verwijderen van het beleid. Klik vervolgens op **verwijderen**. Het duurt 30 tot 60 seconden om te verwijderen.
2. Klik op **OK**.
