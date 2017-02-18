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
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/19/2017
ms.author: sutalasi
translationtype: Human Translation
ms.sourcegitcommit: 74d51269cdd55e39eaa2963fec2b409decb47d0a
ms.openlocfilehash: a279fdc35c62c76e6f0858d52c64240032669f15


---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>Replicatiebeleid voor VMware naar Azure beheren


## <a name="create-a-new-replication-policy"></a>Een nieuw replicatiebeleid maken

1. Klik op Beheren -> Site Recovery-infrastructuur in het menu aan de linkerzijde. 
2. Selecteer Replicatiebeleid in het gedeelte Voor VMware en fysieke machines.
3. Klik in de rechterbovenhoek op +Replicatiebeleid.

    ![Replicatiebeleid maken](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Voer de naam van het beleid in.

5. Geef de limiet voor de RPO op bij RPO-drempelwaarde. Wanneer de continue replicatie deze limiet overschrijdt, worden er waarschuwingen gegenereerd.
6. Geef in Bewaarperiode van het herstelpunt op hoeveel uur elk herstelpunt moet worden bewaard. Beveiligde machines kunnen te allen tijde worden hersteld naar een willekeurig punt (binnen een bepaald tijdsvenster). 

    > [!NOTE] 
    > Machines die worden gerepliceerd naar Premium-opslag, kunnen maximaal 24 uur worden bewaard. Machines die worden gerepliceerd naar Standard-opslag, kunnen maximaal 72 uur worden bewaard.
    
    > [!NOTE] 
    > Er wordt automatisch een replicatiebeleid voor failback gemaakt.

7. Geef in Frequentie van de app-consistente momentopname op hoe vaak (in minuten) er herstelpunten moeten worden gemaakt met toepassingsconsistente momentopnamen.

8. Klik op OK. Het beleid wordt binnen 30 seconden tot 1 minuut gemaakt.

![Replicatiebeleid maken](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-configuration-server-with-replication-policy"></a>Een configuratieserver koppelen aan het replicatiebeleid
1. Klik op het replicatiebeleid waaraan u de configuratieserver wilt koppelen.
2. Klik bovenaan op Koppelen.
![Replicatiebeleid maken](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Selecteer Configuratieserver in de lijst met servers.
4. Klik op OK. De configuratieserver wordt in ongeveer 1 tot 2 minuten gekoppeld.

![Replicatiebeleid maken](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-replication-policy"></a>Replicatiebeleid bewerken
1. Klik op het replicatiebeleid waarvan u de replicatie-instellingen wilt bewerken.
![Replicatiebeleid maken](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Klik bovenaan op Instellingen bewerken.
![Replicatiebeleid maken](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Wijzig de instellingen op basis van uw behoeften.
4. Klik bovenaan op Opslaan. Het beleid wordt binnen ongeveer 2 tot 5 minuten opgeslagen. Dit is afhankelijk van hoeveel virtuele machines dit replicatiebeleid gebruiken.

![Replicatiebeleid maken](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-configuration-server-from-replication-policy"></a>Een configuratieserver loskoppelen van het replicatiebeleid
1. Klik op het replicatiebeleid waaraan u de configuratieserver wilt koppelen.
2. Klik bovenaan op Loskoppelen.
3. Selecteer Configuratieserver in de lijst met servers.
4. Klik op OK. De configuratieserver wordt in ongeveer 1 tot 2 minuten losgekoppeld.
    
    > [!NOTE] 
    > U kunt een configuratieserver niet loskoppelen als het beleid door een of meer gerepliceerde items wordt gebruikt. Zorg ervoor dat geen gerepliceerde items het beleid gebruiken voordat u de configuratieserver loskoppelt.

## <a name="delete-replication-policy"></a>Replicatiebeleid verwijderen 

1. Klik op het replicatiebeleid dat u wilt verwijderen.
2. Klik op verwijderen. Het beleid wordt binnen 30 seconden tot 1 minuut verwijderd.

    > [!NOTE] 
    > U kunt een replicatiebeleid niet verwijderen als er een of meer configuratieservers aan zijn gekoppeld. Zorg ervoor dat geen gerepliceerde items het beleid gebruiken en verwijder alle gekoppelde configuratieservers voordat u het beleid verwijdert.


<!--HONumber=Jan17_HO4-->


