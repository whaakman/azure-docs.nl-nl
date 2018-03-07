---
title: On-premises machines migreren naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u on-premises VM en fysieke machines naar Azure migreert met behulp van Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 02/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 656ba02401d9ba610d0ebe33a683164af0b871f0
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="migrate-on-premises-machines-to-azure"></a>On-premises machines migreren naar Azure

Behalve dat u de service [Azure Site Recovery](site-recovery-overview.md) kunt gebruiken om herstel na noodgevallen van on-premises machines en virtuele Azure-machines te beheren en te organiseren met als doel de bedrijfscontinuïteit te waarborgen, en deze service kunt gebruiken voor herstel na een noodgeval (BCDR), kunt u Site Recovery ook gebruiken om de migratie van on-premises machines naar Azure te beheren.


Deze zelfstudie laat u zien hoe u on-premises VM's en fysieke servers naar Azure migreert met Site Recovery. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een replicatiedoel selecteren
> * De bron- en doelomgeving instellen
> * Een replicatiebeleid instellen
> * Replicatie inschakelen
> * Een testfailover uitvoeren om te controleren of alles goed werkt
> * Een eenmalige failover uitvoeren naar Azure

Dit is de derde zelfstudie in een reeks. In deze zelfstudie wordt ervan uitgegaan dat u de taken in de vorige zelfstudies al hebt voltooid:

1. [Azure voorbereiden](tutorial-prepare-azure.md)
2. On-premises [VMware](vmware-azure-tutorial-prepare-on-premises.md) of Hyper-V-servers voorbereiden.

Voordat u begint, is het handig om de architectuur voor noodherstel van [VMware](vmware-azure-architecture.md) of [Hyper-V](hyper-v-azure-architecture.md) te bekijken.


## <a name="prerequisites"></a>Vereisten

Apparaten die zijn geëxporteerd door geparavirtualiseerde stuurprogramma's worden niet ondersteund.


## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Een resource maken** > **Controle en beheer** > **Backup en Site Recovery**.
3. Bij **Naam** geeft u de beschrijvende naam **ContosoVMVault** op. Als u meer dan één abonnement hebt, selecteert u het gewenste abonnement.
4. Maak een resourcegroep met de naam **ContosoRG**.
5. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
6. Als u de kluis snel wilt kunnen openen via het dashboard, klikt u op **Vastmaken aan dashboard** en vervolgens op **Maken**.

   ![Nieuwe kluis](./media/migrate-tutorial-on-premises-azure/onprem-to-azure-vault.png)

De nieuwe kluis wordt toegevoegd op het **Dashboard** onder **Alle resources** en op de hoofdpagina van **Recovery Services-kluizen**.



## <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren

Selecteer wat u wilt repliceren en waarnaar u wilt repliceren.
1. Klik op **Recovery Services-kluizen** > kluis.
2. Klik in het resourcemenu op **Site Recovery** > **Infra structuur voorbereiden** > **Beveiligingsdoel**.
3. Selecteer bij **Beveiligingsdoel** wat u wilt migreren.
    - **VMware**: selecteer **naar Azure** > **Ja, met VMWare vSphere Hypervisor**.
    - **Fysieke machine**: selecteer **Naar Azure** > **Niet gevirtualiseerde/overige**.
    - **Hyper-V**: selecteer **Naar Azure** > **Ja, met Hyper-V**. Als Hyper-V-machines worden beheerd door VMM, selecteert u **Ja**.


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

- De bronomgeving [instellen](vmware-azure-tutorial.md#set-up-the-source-environment) voor VMware-VM's.
- De bronomgeving [instellen](physical-azure-disaster-recovery.md#set-up-the-source-environment) voor fysieke servers.
- De bronomgeving [instellen](hyper-v-azure-tutorial.md#set-up-the-source-environment) voor Hyper-V-VM's.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Geef het Resource Manager-implementatiemodel op.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

- [Beleid voor replicatie](vmware-azure-tutorial.md#create-a-replication-policy) voor VMware-VM's instellen.
- [Beleid voor replicatie](physical-azure-disaster-recovery.md#create-a-replication-policy) voor fysieke servers instellen.
- [Beleid voor replicatie](hyper-v-azure-tutorial.md#set-up-a-replication-policy) voor Hyper-V-VM's instellen.


## <a name="enable-replication"></a>Replicatie inschakelen

- [Replicatie inschakelen](vmware-azure-tutorial.md#enable-replication) voor VMware VM's.
- [Replicatie inschakelen](physical-azure-disaster-recovery.md#enable-replication) voor fysieke servers.
- Replicatie inschakelen voor Hyper-V-VM's [met](hyper-v-vmm-azure-tutorial.md#enable-replication) of [zonder VMM](hyper-v-azure-tutorial.md#enable-replication).


## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren

Voer een [testfailover](tutorial-dr-drill-azure.md) naar Azure uit om te controleren of alles goed werkt.


## <a name="migrate-to-azure"></a>Migreren naar Azure

Een failover uitvoeren voor de machines die u wilt migreren.

1. Klik in **Instellingen** > **Gerepliceerde items** op de machine > **Failover**.
2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. Selecteer het meest recente herstelpunt.
3. De instelling voor de coderingssleutel is niet relevant in dit scenario.
4. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery zal proberen om virtuele bronmachines af te sluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
5. Controleer of de virtuele Azure-machine in Azure wordt weergegeven zoals verwacht.
6. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Migratie voltooien**. Hiermee wordt het migratieproces voltooid, de replicatie voor de VM gestopt en Site Recovery-facturering voor de virtuele machine gestopt.

    ![Migratie voltooien](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **Annuleer een failover die in voortgang is niet**: voordat de failover wordt gestart, wordt de VM-replicatie gestopt. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. U zou langere failover-tijden kunnen waarnemen voor fysieke servers, VMware Linux-computers, VMware-VM's waarop de DHCP-service niet is ingeschakeld, en VMware-VM's die niet de volgende opstartstuurprogramma’s hebben: storvsc, vmbus, storflt, intelide, atapi.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u on-premises virtuele machines naar virtuele Azure-machines gemigreerd. U kunt nu herstel na noodgevallen configureren voor de virtuele Azure-machines.

> [!div class="nextstepaction"]
> [Stel herstel na noodgeval in](azure-to-azure-replicate-after-migration.md) voor Azure-VM's na de migratie van een on-premises locatie.
