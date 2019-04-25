---
title: On-premises machines migreren naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u on-premises VM en fysieke machines naar Azure migreert met behulp van Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: fc15db91b8f4cc6dbdecd0e7321abdbf81744f08
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60193892"
---
# <a name="migrate-on-premises-machines-to-azure"></a>On-premises machines migreren naar Azure


In dit artikel wordt beschreven hoe u on-premises machines migreren naar Azure, met behulp van de [Azure Site Recovery](site-recovery-overview.md). Site Recovery is over het algemeen gebruikt om te beheren en te organiseren van herstel na noodgevallen van on-premises machines en virtuele Azure-machines. Echter kan deze ook worden gebruikt voor de migratie. Migratie maakt gebruik van dezelfde stappen als herstel na noodgevallen met één uitzondering. Bij de migratie is machines Failover-overschakeling uitvoeren van uw on-premises site de laatste stap. In tegenstelling tot herstel na noodgevallen, niet kan u een failback naar on-premises beschikken in een migratiescenario.


Deze zelfstudie laat u zien hoe u on-premises VM's en fysieke servers naar Azure migreert met Site Recovery. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Instellen van de bron en doel-omgeving voor migratie
> * Een replicatiebeleid instellen
> * Replicatie inschakelen
> * Een testfailover uitvoeren om te controleren of alles goed werkt
> * Een eenmalige failover uitvoeren naar Azure


> [!TIP]
> De service Azure Migrate biedt nu een Preview-versie voor een nieuwe, zonder agent-ervaring voor VMware-VM's migreren naar Azure. [Kom meer te](https://aka.ms/migrateVMs-signup).


## <a name="before-you-start"></a>Voordat u begint

Houd er rekening mee dat de apparaten die zijn geëxporteerd door geparavirtualiseerde stuurprogramma's worden niet ondersteund.


## <a name="prepare-azure-and-on-premises"></a>Azure en on-premises voorbereiden

1. Azure voorbereiden, zoals beschreven in [in dit artikel](tutorial-prepare-azure.md). Hoewel dit artikel wordt beschreven voorbereidende stappen voor herstel na noodgevallen, zijn de stappen ook geldig voor migratie.
2. On-premises [VMware](vmware-azure-tutorial-prepare-on-premises.md)- of [Hyper-V](hyper-v-prepare-on-premises-tutorial.md)-servers voorbereiden. Als u fysieke machines migreert bent, moet u geen voor te bereiden. Alleen controleren of de [ondersteuningsmatrix](vmware-physical-azure-support-matrix.md).


## <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren

Selecteer wat u wilt repliceren en waarnaar u wilt repliceren.
1. Klik op **Recovery Services-kluizen** > kluis.
2. Klik in het resourcemenu op **Site Recovery** > **Infra structuur voorbereiden** > **Beveiligingsdoel**.
3. Selecteer bij **Beveiligingsdoel** wat u wilt migreren.
    - **VMware**: selecteer **Naar Azure** > **Ja, met VMWare vSphere Hypervisor**.
    - **Fysieke machine**: selecteer **Naar Azure** > **Niet gevirtualiseerde/overige**.
    - **Hyper-V**: selecteer **Naar Azure** > **Ja, met Hyper-V**. Als Hyper-V-machines worden beheerd door VMM, selecteert u **Ja**.


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

**Scenario** | **Details**
--- | --- 
VMware | Instellen van de [bronomgeving](vmware-azure-set-up-source.md), en stel de [configuratieserver](vmware-azure-deploy-configuration-server.md).
Fysieke machine | [Instellen van](physical-azure-set-up-source.md) de omgeving en de configuratie van een bronserver.
Hyper-V | Instellen van de [bronomgeving](hyper-v-azure-tutorial.md#set-up-the-source-environment)<br/><br/> Instellen van de [bronomgeving](hyper-v-vmm-azure-tutorial.md#set-up-the-source-environment) voor Hyper-V implementeren met System Center VMM.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Geef het Resource Manager-implementatiemodel op.
3. Site Recovery controleert of de Azure-resources.
    - Als u VMware-machines of fysieke servers migreren wilt, Site Recovery controleert of dat u hebt een Azure-netwerk waarin de Azure VM's geplaatst worden moeten nadat ze na een failover worden gemaakt.
    - Als u Hyper-V-machines migreert bent, Site Recovery controleert of dat u hebt een compatibel Azure storage-account en het netwerk.
4. Als Hyper-V-machines worden beheerd door System Center VMM u migreert waarnaar, stelt u [netwerktoewijzing](hyper-v-vmm-azure-tutorial.md#configure-network-mapping).

## <a name="set-up-a-replication-policy"></a>Een replicatiebeleid instellen

**Scenario** | **Details**
--- | --- 
VMware | Instellen van een [replicatiebeleid](vmware-azure-set-up-replication.md) voor VMware-VM's.
Fysieke machine | Instellen van een [replicatiebeleid](physical-azure-disaster-recovery.md#create-a-replication-policy) voor fysieke computers.
Hyper-V | Instellen van een [replicatiebeleid](hyper-v-azure-tutorial.md#set-up-a-replication-policy)<br/><br/> Instellen van een [replicatiebeleid](hyper-v-vmm-azure-tutorial.md#set-up-a-replication-policy) voor Hyper-V implementeren met System Center VMM.

## <a name="enable-replication"></a>Replicatie inschakelen

**Scenario** | **Details**
--- | --- 
VMware | [Replicatie inschakelen](vmware-azure-enable-replication.md) voor VMware VM's.
Fysieke machine | [Replicatie inschakelen](physical-azure-disaster-recovery.md#enable-replication) voor fysieke computers.
Hyper-V | [Replicatie inschakelen](hyper-v-azure-tutorial.md#enable-replication)<br/><br/> [Replicatie inschakelen](hyper-v-vmm-azure-tutorial.md#enable-replication) voor Hyper-V implementeren met System Center VMM.


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

   - Wordt het migratieproces voltooid, replicatie voor de on-premises VM gestopt en Site Recovery-facturering voor de virtuele machine stopt.
   - Met deze stap worden de replicatiegegevens opgeschoond. De gemigreerde VM's worden niet verwijderd.

     ![Migratie voltooien](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **Annuleer nooit een failover die in uitvoering is**: De VM-replicatie wordt gestopt voordat de failover start. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. U zou langere failover-tijden kunnen waarnemen voor fysieke servers, VMware Linux-computers, VMware VM's waarop de DHCP-service niet is ingeschakeld, en VMware VM's die niet de volgende opstartstuurprogramma’s hebben: storvsc, vmbus, storflt, intelide, atapi.

## <a name="after-migration"></a>Na de migratie

Wanneer machines zijn gemigreerd naar Azure, zijn er nog een aantal stappen die u moet voltooien.

Enkele stappen kunnen worden geautomatiseerd als onderdeel van het migratieproces met behulp van de ingebouwde automation-scripts voor [herstelplannen](site-recovery-runbook-automation.md)   


### <a name="post-migration-steps-in-azure"></a>Stappen na de migratie in Azure

- Voer correcties van de app uit na de migratie, zoals updates van de databaseverbindingsreeksen en webserverconfiguraties. 
- Voer acceptatietesten van de toepassing en de migratie uit op de gemigreerde toepassing die nu wordt uitgevoerd in Azure.
- De [Azure VM-agent](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) beheert de interactie van de VM met de Azure-Infrastructuurcontroller. Dit is vereist voor sommige Azure-services, zoals Azure Backup, Azure Site Recovery en Azure-beveiliging.
    - Als u VMware-machines en fysieke servers wilt migreren, installeert het installatieprogramma van de Mobility-service de beschikbaar Azure VM-agent op Windows-machines. Voor Linux VM‘s raden we u aan om de agent te installeren na failover.
    - Als u Azure VM's migreert naar een secundaire regio, moet de Azure VM-agent vóór de migratie worden ingericht op de virtuele machine.
    - Als u Hyper-V VM‘s naar Azure migreert, installeert u de Azure VM-agent na de migratie op de Azure VM.
- Verwijder eventuele Site Recovery-providers/agenten handmatig van de VM. Als u virtuele VMware-machines of fysieke servers migreren, verwijdert u de Mobility-service van de virtuele machine.
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

In deze zelfstudie hebt u on-premises virtuele machines naar virtuele Azure-machines gemigreerd. Nu

> [!div class="nextstepaction"]
> [Herstel na noodgevallen instellen](azure-to-azure-replicate-after-migration.md) naar een secundaire Azure-regio voor de Azure VM's.

  
