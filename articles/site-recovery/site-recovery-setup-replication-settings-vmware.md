---
title: Replicatie-instellingen voor Azure Site Recovery instellen | Microsoft Docs
description: Dit artikel beschrijft hoe u Site Recovery implementeert om replicatie, failovers en herstel van virtuele Hyper-V-machines in VMM-clouds naar Azure te beheren.
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 73a1f19177f23441f5f7165cf2bc92ba85e62aa5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>Replicatiebeleid voor VMware naar Azure beheren


## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Selecteer **Beheren** > **Infrastructuur voor Site Recovery**.
2. Selecteer **Replicatiebeleid** onder **Voor VMware en fysieke machines**.
3. Selecteer **+Replicatiebeleid**.

    ![Replicatiebeleid maken](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Voer de naam van het beleid in.

5. Geef de limiet voor de RPO op bij **RPO-drempelwaarde**. Wanneer de continue replicatie deze limiet overschrijdt, worden er waarschuwingen gegenereerd.
6. Geef in **Bewaarperiode van het herstelpunt** de duur (in uren) op dat elk herstelpunt moet worden bewaard. Beveiligde machines kunnen binnen een bepaald tijdsvenster te allen tijde worden hersteld naar een willekeurig punt.

    > [!NOTE]
    > Een bewaarperiode van maximaal 24 uur wordt ondersteund voor computers die worden gerepliceerd naar Premium Storage. Een bewaarperiode van maximaal 72 uur wordt ondersteund voor computers die worden gerepliceerd naar Standard Storage.

    > [!NOTE]
    > Er wordt automatisch een replicatiebeleid voor failback gemaakt.

7. Geef in **Frequentie van de app-consistente momentopname** op hoe vaak (in minuten) er herstelpunten moeten worden gemaakt met toepassingsconsistente momentopnamen.

8. Klik op **OK**. Het beleid wordt binnen 30 seconden tot 60 minuut gemaakt.

![Replicatiebeleid genereren](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a>Een configuratieserver koppelen aan het replicatiebeleid
1. Kies het replicatiebeleid waaraan u de configuratieserver wilt koppelen.
2. Klik op **Koppelen**.
![Configuratieserver koppelen](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Selecteer de configuratieserver in de lijst met servers.
4. Klik op **OK**. De configuratieserver wordt binnen één tot twee minuten gekoppeld.

![Configuratieserver koppelen](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a>Een replicatiebeleid bewerken
1. Kies het replicatiebeleid waarvan u de replicatie-instellingen wilt bewerken.
![Replicatiebeleid bewerken](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Klik op **Instellingen bewerken**.
![Instellingen van replicatiebeleid bewerken](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Wijzig de instellingen op basis van uw behoeften.
4. Klik op **Opslaan**. Het beleid wordt binnen twee tot vijf minuten opgeslagen. Dit is afhankelijk van hoeveel virtuele machines gebruikmaken van dit replicatiebeleid.

![Replicatiebeleid opslaan](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a>Een configuratieserver loskoppelen van een replicatiebeleid
1. Kies het replicatiebeleid waaraan u de configuratieserver wilt koppelen.
2. Klik op **Ontkoppelen**.
3. Selecteer de configuratieserver in de lijst met servers.
4. Klik op **OK**. De configuratieserver wordt binnen één tot twee minuten ontkoppeld.

    > [!NOTE]
    > U kunt een configuratieserver niet ontkoppelen als het beleid door een of meer gerepliceerde items wordt gebruikt. Voordat u de configuratieserver ontkoppelt, moet u ervoor zorgen dat het beleid niet door gerepliceerde items wordt gebruikt.

## <a name="delete-a-replication-policy"></a>Een replicatiebeleid verwijderen

1. Kies het replicatiebeleid dat u wilt verwijderen.
2. Klik op **Verwijderen**. Het beleid wordt binnen 30 seconden tot 60 minuut verwijderd.

    > [!NOTE]
    > U kunt een replicatiebeleid niet verwijderen als er een of meer configuratieservers aan zijn gekoppeld. Zorg ervoor dat het beleid niet door gerepliceerde items wordt gebruikt en verwijder alle gekoppelde configuratieservers voordat u het beleid verwijdert.
