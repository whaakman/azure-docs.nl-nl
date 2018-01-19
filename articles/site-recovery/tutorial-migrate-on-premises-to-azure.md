---
title: Lokale machines migreren naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe lokale machines migreren naar Azure met behulp van Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ee9397406cbca21d8bd53019d9daac5a037f508c
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="migrate-on-premises-machines-to-azure"></a>On-premises machines migreren naar Azure

Naast het gebruik van de [Azure Site Recovery](site-recovery-overview.md) service te beheren en te organiseren herstel na noodgevallen van on-premises machines en virtuele Azure-machines voor de doeleinden van zakelijke continuïteit en herstel na noodgevallen (BCDR) kunt u de Site ook gebruiken Herstel voor het beheren van de migratie van lokale machines in Azure.


Deze zelfstudie laat zien hoe u lokale virtuele machines en fysieke servers naar Azure migreren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Selecteer een doel replicatie
> * De bron en doel-omgeving instellen
> * Instellen van een beleid voor wachtwoordreplicatie
> * Replicatie inschakelen
> * Voer een testmigratie om te controleren of dat alles werkt zoals verwacht
> * Een eenmalige failover uitvoeren naar Azure

Dit is de derde zelfstudie in een reeks. Deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. Voorbereiden van de lokale [VMware](tutorial-prepare-on-premises-vmware.md) of Hyper-V-servers.

Voordat u begint, is het handig om te controleren de [VMware](concepts-vmware-to-azure-architecture.md) of [Hyper-V](concepts-hyper-v-to-azure-architecture.md) architecturen voor herstel na noodgevallen.


## <a name="prerequisites"></a>Vereisten

Apparaten die zijn geëxporteerd door paravirtualized stuurprogramma's worden niet ondersteund.


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Nieuw** > **Bewaking en beheer** > **Backup en Site Recovery**.
3. In **naam**, geef de beschrijvende naam **ContosoVMVault**. Als u meer dan één abonnement hebt, selecteert u het juiste bestand.
4. Een resourcegroep maken **ContosoRG**.
5. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
6. Voor snelle toegang tot de kluis vanuit het dashboard, klikt u op **vastmaken aan dashboard** en klik vervolgens op **maken**.

   ![Nieuwe kluis](./media/tutorial-migrate-on-premises-to-azure/onprem-to-azure-vault.png)

De nieuwe kluis wordt toegevoegd aan de **Dashboard** onder **alle resources**, en klik op de hoofdblade **Recovery Services-kluizen** pagina.



## <a name="select-a-replication-goal"></a>Selecteer een doel replicatie

Selecteer wat u wilt repliceren en waarnaar u wilt repliceren.
1. Klik op **Recovery Services-kluizen** > kluis.
2. Klik in het Menu Resource **siteherstel** > **infrastructuur voorbereiden** > **beveiligingsdoel**.
3. In **beveiligingsdoel**, selecteert u wilt migreren.
    - **VMware**: Selecteer **naar Azure** > **Ja, met VMWare vSphere Hypervisor**.
    - **Fysieke machine**: Selecteer **naar Azure** > **niet gevirtualiseerde/andere**.
    - **Hyper-V**: Selecteer **naar Azure** > **Ja, met Hyper-V**. Als Hyper-V-machines worden beheerd door VMM, selecteert u **Ja**.


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

- [Instellen van](tutorial-vmware-to-azure.md#set-up-the-source-environment) de bronomgeving voor virtuele VMware-machines.
- [Instellen van](tutorial-physical-to-azure.md#set-up-the-source-environment) de bronomgeving voor fysieke servers.
- [Instellen van](tutorial-hyper-v-to-azure.md#set-up-the-source-environment) de bronomgeving voor Hyper-V-machines.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Geef het implementatiemodel van Resource Manager.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

## <a name="set-up-a-replication-policy"></a>Instellen van een beleid voor wachtwoordreplicatie

- [Instellen van een beleid voor wachtwoordreplicatie](tutorial-vmware-to-azure.md#create-a-replication-policy) voor virtuele VMware-machines.
- [Instellen van een beleid voor wachtwoordreplicatie](tutorial-physical-to-azure.md#create-a-replication-policy) voor fysieke servers.
- [Instellen van een beleid voor wachtwoordreplicatie](tutorial-hyper-v-to-azure.md#set-up-a-replication-policy) voor Hyper-V-machines.


## <a name="enable-replication"></a>Replicatie inschakelen

- [Replicatie inschakelen](tutorial-vmware-to-azure.md#enable-replication) voor virtuele VMware-machines.
- [Replicatie inschakelen](tutorial-physical-to-azure.md#enable-replication) voor fysieke servers.
- [Replicatie inschakelen](tutorial-hyper-v-to-azure.md#enable-replication) voor Hyper-V-machines.


## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren

Voer een [testfailover](tutorial-dr-drill-azure.md) naar Azure, om te controleren of alles werkt zoals verwacht.


## <a name="migrate-to-azure"></a>Migreren naar Azure

Voer een failover voor de machines die u wilt migreren.

1. In **instellingen** > **gerepliceerde items** Klik op de machine > **Failover**.
2. In **Failover** Selecteer een **herstelpunt** failover naar. Selecteer de meest recente herstelpunt.
3. De instelling voor de versleuteling van sleutel is niet relevant zijn voor dit scenario.
4. Selecteer **machine afsluiten voordat u begint met failover**. Site Recovery probeert te doen virtuele bronmachines afgesloten voordat de failover. Failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de **taken** pagina.
5. Controleer de Azure VM in Azure wordt weergegeven zoals verwacht.
6. In **gerepliceerde items**, met de rechtermuisknop op de virtuele machine > **volledige migratie**. Het migratieproces is voltooid, de replicatie voor de virtuele machine stopt en Hiermee stopt u Site Recovery facturering voor de virtuele machine.

    ![Volledige migratie](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Een failover uitgevoerd niet annuleren**: VM replicatie wordt gestopt voordat failover wordt gestart. Als u een failover uitgevoerd, stopt de failover, annuleren, maar de virtuele machine opnieuw won't repliceren.

In sommige scenario's moet failover extra verwerking die het duurt ongeveer 8 tot tien minuten is voltooid. Ziet u mogelijk meer failovertijden voor de fysieke servers, VMware Linux-machines, virtuele VMware-machines waarvoor geen DHCP-service waarmee en VMware-machines die niet de volgende opstartstuurprogramma's testen: storvsc, vmbus, storflt, intelide, atapi.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u lokale virtuele machines naar Azure Virtual machines gemigreerd. Nu kunt u herstel na noodgevallen voor de Azure VM's configureren.

> [!div class="nextstepaction"]
> [Instellen van herstel na noodgevallen](site-recovery-azure-to-azure-after-migration.md) voor Azure VM's na de migratie van een on-premises site.
