---
title: Configureren en beheren van replicatiebeleid voor voor VMware-replicatie met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u configureert replicatie-instellingen voor VMware-replicatie naar Azure met Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: sutalasi
ms.openlocfilehash: 115f4e76630a0ecd659fdd3aec0c6aa67d503817
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920484"
---
# <a name="configure-and-manage-replication-policies-for-vmware-replication"></a>Configureren en beheren van replicatiebeleid voor voor VMware-replicatie
In dit artikel wordt beschreven hoe u een replicatiebeleid configureren wanneer u bent virtuele VMware-machines repliceren naar Azure, met behulp van [Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>Een beleid maken

1. Selecteer **Beheren** > **Infrastructuur voor Site Recovery**.
2. In **voor VMware en fysieke machines**, selecteer **replicatiebeleid**. 
3. Klik op **+ replicatiebeleid**, en geef de naam van het beleid.
5. Geef de limiet voor de RPO op bij **RPO-drempelwaarde**. Waarschuwingen worden gegenereerd wanneer de continue replicatie deze limiet overschrijdt.
6. Geef in **Bewaarperiode van het herstelpunt** de duur (in uren) op dat elk herstelpunt moet worden bewaard. Beveiligde machines kunnen binnen een bepaald tijdsvenster te allen tijde worden hersteld naar een willekeurig punt. Een bewaarperiode van maximaal 24 uur wordt ondersteund voor computers die worden gerepliceerd naar Premium Storage. Maximaal 72 uur wordt ondersteund voor standard-opslag.
7. Geef in **Frequentie van de app-consistente momentopname** op hoe vaak (in minuten) er herstelpunten moeten worden gemaakt met toepassingsconsistente momentopnamen.
8. Klik op **OK**. Het beleid wordt binnen 30 seconden tot 60 minuut gemaakt.

Wanneer u een replicatiebeleid maakt, wordt een overeenkomende failback-replicatiebeleid automatisch gemaakt, met het achtervoegsel 'failback'. Nadat het beleid is gemaakt, kunt u deze bewerken door deze te selecteren > **instellingen bewerken**.

## <a name="associate-a-configuration-server"></a>Een configuratieserver koppelen 

Het replicatiebeleid koppelen aan uw on-premises configuratieserver.

1. Klik op **koppelen**, en selecteer de configuratieserver.

    ![Configuratieserver koppelen](./media/vmware-azure-set-up-replication/associate1.png)

2. Klik op **OK**. De configuratieserver wordt binnen één tot twee minuten gekoppeld.

    ![Configuratieserver koppelen](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>Koppeling verbreken of een replicatiebeleid verwijderen
1. Kies het replicatiebeleid.
    a. Als u wilt ontkoppelen van het beleid op basis van de configuratieserver, zorg ervoor dat geen gerepliceerde machines worden het beleid. Klik vervolgens op **ontkoppelen**.
    b. U wilt verwijderen van het beleid, zorg ervoor dat het is niet gekoppeld aan een configuratieserver. Klik vervolgens op **verwijderen**. Het duurt 30 tot 60 seconden om te verwijderen.
2. Klik op **OK**.
