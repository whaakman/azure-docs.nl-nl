---
title: Configureren en beheren van replicatiebeleid voor voor VMware naar Azure met Azure Site Recovery een noodgeval | Microsoft Docs
description: Beschrijft hoe u configureert replicatie-instellingen voor noodherstel van VMware naar Azure met Azure Site Recovery.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: sutalasi
ms.openlocfilehash: 4f558d8d1e5e4e743e6d44cca1a804b3e5898b30
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58443424"
---
# <a name="configure-and-manage-replication-policies-for-vmware-disaster-recovery-to-azure"></a>Configureren en beheren van replicatiebeleid voor noodherstel van VMware naar Azure
In dit artikel wordt beschreven hoe u een replicatiebeleid configureren wanneer u bent virtuele VMware-machines repliceren naar Azure, met behulp van [Azure Site Recovery](site-recovery-overview.md).

## <a name="create-a-policy"></a>Een beleid maken

1. Selecteer **Beheren** > **Infrastructuur voor Site Recovery**.
2. In **voor VMware en fysieke machines**, selecteer **replicatiebeleid**. 
3. Klik op **+ replicatiebeleid**, en geef de naam van het beleid.
4. Geef de limiet voor de RPO op bij **RPO-drempelwaarde**. Waarschuwingen worden gegenereerd wanneer de continue replicatie deze limiet overschrijdt.
5. Geef in **Bewaarperiode van het herstelpunt** de duur (in uren) op dat elk herstelpunt moet worden bewaard. Beveiligde machines kunnen binnen een bepaald tijdsvenster te allen tijde worden hersteld naar een willekeurig punt. Een bewaarperiode van maximaal 24 uur wordt ondersteund voor computers die worden gerepliceerd naar Premium Storage. Maximaal 72 uur wordt ondersteund voor standard-opslag.
6. In **frequentie App-consistente momentopname**, kiest u in de vervolgkeuzelijst hoe vaak (in uren) herstelpunten met toepassingsconsistente momentopnamen moeten worden gemaakt. Als u genereren van de toepassing consistentie punten uitschakelen wilt, kiest u 'Uitschakelen'-waarde in de vervolgkeuzelijst.
7. Klik op **OK**. Het beleid wordt binnen 30 seconden tot 60 minuut gemaakt.

Wanneer u een replicatiebeleid maakt, wordt een overeenkomende failback-replicatiebeleid automatisch gemaakt, met het achtervoegsel 'failback'. Nadat het beleid is gemaakt, kunt u deze bewerken door deze te selecteren > **instellingen bewerken**.

## <a name="associate-a-configuration-server"></a>Een configuratieserver koppelen 

Het replicatiebeleid koppelen aan uw on-premises configuratieserver.

1. Klik op **koppelen**, en selecteer de configuratieserver.

    ![Configuratieserver koppelen](./media/vmware-azure-set-up-replication/associate1.png)
2. Klik op **OK**. De configuratieserver wordt binnen één tot twee minuten gekoppeld.

    ![Configuratieserver koppelen](./media/vmware-azure-set-up-replication/associate2.png)

## <a name="edit-a-policy"></a>Een beleid bewerken

1. Selecteer **beheren** > **infrastructuur voor Site Recovery** > **replicatiebeleid**.
2. Selecteer het replicatiebeleid dat u wilt wijzigen.
3. Klik op **instellingen bewerken**, en de RPO/herstel van drempelwaarde punt retentie uur/app-consistente momentopname frequentie velden bijwerken zoals vereist.
4. Als u genereren van de toepassing consistentie punten uitschakelen wilt, kiest u 'Uit'-waarde in de vervolgkeuzelijst van de gearchiveerde **frequentie App-consistente momentopname**.
5. Klik op **Opslaan**. Het beleid moet worden bijgewerkt in 30 tot 60 seconden.

## <a name="disassociate-or-delete-a-replication-policy"></a>Koppeling verbreken of een replicatiebeleid verwijderen

1. Kies het replicatiebeleid.
    a. Als u wilt ontkoppelen van het beleid op basis van de configuratieserver, zorg ervoor dat geen gerepliceerde machines worden het beleid. Klik vervolgens op **ontkoppelen**.
    b. U wilt verwijderen van het beleid, zorg ervoor dat het is niet gekoppeld aan een configuratieserver. Klik vervolgens op **verwijderen**. Het duurt 30 tot 60 seconden om te verwijderen.
2. Klik op **OK**.
