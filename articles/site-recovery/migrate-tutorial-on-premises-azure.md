---
title: On-premises machines migreren naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u on-premises VM en fysieke machines naar Azure migreert met behulp van Azure Site Recovery.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 5023171c4f943b7e698a0b6bbcadef209965e2df
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53789243"
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
2. On-premises [VMware](vmware-azure-tutorial-prepare-on-premises.md)- of [Hyper-V](hyper-v-prepare-on-premises-tutorial.md)-servers voorbereiden.

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
    - **VMware**: selecteer **Naar Azure** > **Ja, met VMWare vSphere Hypervisor**.
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
4. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery zal proberen om virtuele machines af te sluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
5. Controleer of de virtuele Azure-machine in Azure wordt weergegeven zoals verwacht.
6. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Migratie voltooien**. Er gebeurt nu het volgende:

    - Het migratieproces wordt voltooid, de replicatie voor de AWS-VM wordt gestopt en Site Recovery-facturering voor de virtuele machine wordt gestopt.
    - Met deze stap worden de replicatiegegevens opgeschoond. De gemigreerde VM's worden niet verwijderd.

    ![Migratie voltooien](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **Annuleer nooit een failover die in uitvoering is**: De VM-replicatie wordt gestopt voordat de failover start. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. U zou langere failover-tijden kunnen waarnemen voor fysieke servers, VMware Linux-computers, VMware VM's waarop de DHCP-service niet is ingeschakeld, en VMware VM's die niet de volgende opstartstuurprogramma’s hebben: storvsc, vmbus, storflt, intelide, atapi.

## <a name="after-migration"></a>Na de migratie

Wanneer machines zijn gemigreerd naar Azure, zijn er nog een aantal stappen die u moet voltooien.

Enkele stappen kunnen worden geautomatiseerd als onderdeel van het migratieproces met behulp van de ingebouwde automation-scripts voor [herstelplannen]( https://docs.microsoft.com/azure/site-recovery/site-recovery-runbook-automation)   


### <a name="post-migration-steps-in-azure"></a>Stappen na de migratie in Azure

- Voer correcties van de app uit na de migratie, zoals updates van de databaseverbindingsreeksen en webserverconfiguraties. 
- Voer acceptatietesten van de toepassing en de migratie uit op de gemigreerde toepassing die nu wordt uitgevoerd in Azure.
- De [Azure VM-agent](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) beheert de interactie van de VM met de Azure-Infrastructuurcontroller. Dit is vereist voor sommige Azure-services, zoals Azure Backup, Azure Site Recovery en Azure-beveiliging.
    - Als u VMware-machines en fysieke servers wilt migreren, installeert het installatieprogramma van de Mobility-service de beschikbaar Azure VM-agent op Windows-machines. Voor Linux VM‘s raden we u aan om de agent te installeren na failover. a
    - Als u Azure VM's migreert naar een secundaire regio, moet de Azure VM-agent vóór de migratie worden ingericht op de virtuele machine.
    - Als u Hyper-V VM‘s naar Azure migreert, installeert u de Azure VM-agent na de migratie op de Azure VM.
- Verwijder eventuele Site Recovery-providers/agenten handmatig van de VM. Als u VMware VM‘s of fysieke servers migreert, [moet u de Mobility-service][vmware-azure-install-mobility-service.md#uninstall-mobility-service-on-a-windows-server-computer] van de Azure VM verwijderen.
- Voor grotere flexibiliteit:
    - Houd uw gegevens veilig door back-ups van virtuele Azure VM‘s te maken met behulp van de Azure Backup-service. [Meer informatie]( https://docs.microsoft.com/azure/backup/quick-backup-vm-portal).
    - Houd workloads continu beschikbaar door Azure VM‘s naar een secundaire regio te repliceren met Site Recovery. [Meer informatie](azure-to-azure-quickstart.md).
- Voor betere beveiliging:
    - De toegang van binnenkomend verkeer vergrendelen en beperken met [Just-in-time-beheer]( https://docs.microsoft.com/azure/security-center/security-center-just-in-time) van Azure Security Center
    - Beperk het netwerkverkeer naar beheereindpunten met [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Implementeer [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) om schijven te beveiligen en gegevens te beschermen tegen diefstal en onbevoegde toegang.
    - Lees meer informatie over [IaaS-resources beveiligen]( https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/ ) en bezoek het [Azure Security Center](https://azure.microsoft.com/services/security-center/ ).
- Voor controle en beheer:
    - Overweeg de implementatie van [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) om uw resourcegebruik en uitgaven te bewaken.

### <a name="post-migration-steps-on-premises"></a>Stappen om na de migratie on-premises uit te voeren

- Leid appverkeer via de app die wordt uitgevoerd op het gemigreerde Azure VM-exemplaar.
- Verwijder de on-premises VM's uit uw lokale VM-inventaris.
- Verwijder de on-premises VM's uit de lokale back-ups.
- Werk eventuele interne documentatie bij met de nieuwe locatie en het nieuwe IP-adres van de Azure VM's.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u on-premises virtuele machines naar virtuele Azure-machines gemigreerd. U kunt nu [noodherstel instellen](azure-to-azure-replicate-after-migration.md) voor een secundaire Azure-regio voor de Azure VM's.

  
