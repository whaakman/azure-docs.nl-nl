---
title: Lokale machines migreren naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe lokale machines migreren naar Azure met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/27/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: e268a6d53a9f0b001ad0da8c9ce7ea1a9046960c
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-on-premises-machines-to-azure"></a>On-premises machines migreren naar Azure

De [Azure Site Recovery](../site-recovery-overview.md) service beheert en regelt de replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie laat zien hoe u voor het migreren van lokale virtuele machines en fysieke servers naar Azure met Site Recovery. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten voor de implementatie instellen
> * Een Recovery Services-kluis maken voor Site Recovery
> * Lokale-beheerservers implementeren
> * Stel een beleid voor wachtwoordreplicatie en replicatie inschakelen
> * Voer een detailanalyse van het herstel na noodgevallen om te controleren of alles werkt
> * Een eenmalige failover uitvoeren naar Azure

## <a name="overview"></a>Overzicht

U kunt een machine migreren door het inschakelen van replicatie en failover naar Azure.


## <a name="prerequisites"></a>Vereisten

Dit is wat u moet doen voor deze zelfstudie.

- [Voorbereiden](../tutorial-prepare-azure.md) Azure-resources, zoals een Azure-abonnement, een Azure-netwerk en een opslagaccount.
- [Voorbereiden](../tutorial-prepare-on-premises-vmware.md) VMware on-premises VMware-servers en virtuele machines.
- Houd er rekening mee dat de apparaten die zijn geëxporteerd door paravirtualized stuurprogramma's worden niet ondersteund.


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

[!INCLUDE [site-recovery-create-vault](../../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Selecteer een beveiligingsdoel

Selecteer wat u wilt repliceren en waarnaar u wilt repliceren.
1. Klik op **Recovery Services-kluizen** > kluis.
2. Klik in het Menu Resource **siteherstel** > **infrastructuur voorbereiden** > **beveiligingsdoel**.
3. In **beveiligingsdoel**selecteert:
    - **VMware**: Selecteer **naar Azure** > **Ja, met VMWare vSphere Hypervisor**.
    - **Fysieke machine**: Selecteer **naar Azure** > **niet gevirtualiseerde/andere**.
    - **Hyper-V**: Selecteer **naar Azure** > **Ja, met Hyper-V**.


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

- [Instellen van](../tutorial-vmware-to-azure.md#set-up-the-source-environment) de bronomgeving voor virtuele VMware-machines.
- [Instellen van](../tutorial-physical-to-azure.md#set-up-the-source-environment) de bronomgeving voor fysieke servers.
- [Instellen van](../tutorial-hyper-v-to-azure.md#set-up-the-source-environment) de bronomgeving voor Hyper-V-machines.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Geef het doel-implementatiemodel.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

- [Instellen van een beleid voor wachtwoordreplicatie](../tutorial-vmware-to-azure.md#create-a-replication-policy) voor virtuele VMware-machines.


## <a name="enable-replication"></a>Replicatie inschakelen

- [Replicatie inschakelen](../tutorial-vmware-to-azure.md#enable-replication) voor virtuele VMware-machines.


## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren

Voer een [testfailover](../tutorial-dr-drill-azure.md) naar Azure, om te controleren of alles werkt zoals verwacht.


## <a name="migrate-to-azure"></a>Migreren naar Azure

Voer een failover voor de machines die u wilt migreren.

1. In **instellingen** > **gerepliceerde items** Klik op de machine > **Failover**.
2. In **Failover** Selecteer een **herstelpunt** failover naar. Selecteer de meest recente herstelpunt.
3. De instelling voor de versleuteling van sleutel is niet relevant zijn voor dit scenario.
4. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor probeert virtuele bronmachines afgesloten voordat de failover. Failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de **taken** pagina.
5. Controleer de Azure VM in Azure wordt weergegeven zoals verwacht.
6. In **gerepliceerde items**, met de rechtermuisknop op de virtuele machine > **volledige migratie**. Het migratieproces is voltooid, de replicatie voor de virtuele machine stopt en Hiermee stopt u Site Recovery facturering voor de virtuele machine.

    ![Volledige migratie](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Een failover uitgevoerd niet annuleren**: VM replicatie wordt gestopt voordat failover wordt gestart. Als u een failover uitgevoerd, stopt de failover, annuleren, maar de virtuele machine opnieuw won't repliceren.

In sommige scenario's moet failover extra verwerking die het duurt ongeveer 8 tot tien minuten is voltooid. Ziet u mogelijk meer failovertijden voor de fysieke servers, VMware Linux-machines, virtuele VMware-machines waarvoor geen DHCP-service waarmee en VMware-machines die niet de volgende opstartstuurprogramma's testen: storvsc, vmbus, storflt, intelide, atapi.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Virtuele Azure-machines repliceren naar een andere regio na de migratie naar Azure](site-recovery-azure-to-azure-after-migration.md)
