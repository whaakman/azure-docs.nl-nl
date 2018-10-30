---
title: Configureren en beheren van replicatiebeleid voor voor VMware naar Azure met Azure Site Recovery een noodgeval | Microsoft Docs
description: Beschrijft hoe u configureert replicatie-instellingen voor noodherstel van VMware naar Azure met Azure Site Recovery.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: sutalasi
ms.openlocfilehash: fd987097c2ca7b1e7509a1a0e63905c36ec8fec8
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212893"
---
# <a name="configure-and-manage-replication-policies-for-vmware-disaster-recovery-to-azure"></a>Configureren en beheren van replicatiebeleid voor noodherstel van VMware naar Azure
In dit artikel wordt beschreven hoe u een replicatiebeleid configureren wanneer u bent virtuele VMware-machines repliceren naar Azure, met behulp van [Azure Site Recovery](site-recovery-overview.md).


## <a name="create-a-policy"></a>Een beleid maken

1. Selecteer **Beheren** > **Infrastructuur voor Site Recovery**.
1. In **voor VMware en fysieke machines**, selecteer **replicatiebeleid**. 
1. Klik op **+ replicatiebeleid**, en geef de naam van het beleid.
1. Geef de limiet voor de RPO op bij **RPO-drempelwaarde**. Waarschuwingen worden gegenereerd wanneer de continue replicatie deze limiet overschrijdt.
1. Geef in **Bewaarperiode van het herstelpunt** de duur (in uren) op dat elk herstelpunt moet worden bewaard. Beveiligde machines kunnen binnen een bepaald tijdsvenster te allen tijde worden hersteld naar een willekeurig punt. Een bewaarperiode van maximaal 24 uur wordt ondersteund voor computers die worden gerepliceerd naar Premium Storage. Maximaal 72 uur wordt ondersteund voor standard-opslag.
1. Geef in **Frequentie van de app-consistente momentopname** op hoe vaak (in minuten) er herstelpunten moeten worden gemaakt met toepassingsconsistente momentopnamen.
1. Klik op **OK**. Het beleid wordt binnen 30 seconden tot 60 minuut gemaakt.

Wanneer u een replicatiebeleid maakt, wordt een overeenkomende failback-replicatiebeleid automatisch gemaakt, met het achtervoegsel 'failback'. Nadat het beleid is gemaakt, kunt u deze bewerken door deze te selecteren > **instellingen bewerken**.

## <a name="associate-a-configuration-server"></a>Een configuratieserver koppelen 

Het replicatiebeleid koppelen aan uw on-premises configuratieserver.

1. Klik op **koppelen**, en selecteer de configuratieserver.

    ![Configuratieserver koppelen](./media/vmware-azure-set-up-replication/associate1.png)

1. Klik op **OK**. De configuratieserver wordt binnen één tot twee minuten gekoppeld.

    ![Configuratieserver koppelen](./media/vmware-azure-set-up-replication/associate2.png)


## <a name="disassociate-or-delete-a-replication-policy"></a>Koppeling verbreken of een replicatiebeleid verwijderen
1. Kies het replicatiebeleid.
    a. Als u wilt ontkoppelen van het beleid op basis van de configuratieserver, zorg ervoor dat geen gerepliceerde machines worden het beleid. Klik vervolgens op **ontkoppelen**.
    b. U wilt verwijderen van het beleid, zorg ervoor dat het is niet gekoppeld aan een configuratieserver. Klik vervolgens op **verwijderen**. Het duurt 30 tot 60 seconden om te verwijderen.
1. Klik op **OK**.
