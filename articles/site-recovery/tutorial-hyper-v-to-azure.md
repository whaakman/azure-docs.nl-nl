---
title: Herstel na noodgevallen voor lokale Hyper-V-machines naar Azure met Azure Site Recovery instellen | Microsoft Docs
description: Informatie over het instellen van herstel na noodgevallen van lokale Hyper-V-machines naar Azure met de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 45e9b4dc-20ca-4c14-bee3-cadb8d9b8b24
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 4d43fb03ce1c54a47315b8c3a5c83ec2082bcab9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Herstel na noodgevallen van lokale Hyper-V-machines naar Azure instellen

De [Azure Site Recovery](site-recovery-overview.md) service draagt bij aan uw strategie voor noodherstel door te beheren en te organiseren replicatie, failover en failback van on-premises machines en virtuele Azure-machines (VM's).

Deze zelfstudie laat zien hoe u voor het instellen van herstel na noodgevallen van lokale Hyper-V-machines naar Azure. De zelfstudie is relevant voor Hyper-V-machines die worden beheerd in System Center Virtual Machine Manager (VMM) clouds en die niet zijn. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure en on-premises vereisten instellen
> * Een Recovery Services-kluis maken voor Site Recovery 
> * Instellen van de bron en doel van de replicatie-omgevingen 
> * Stel netwerktoewijzing (Als Hyper-V wordt beheerd door System Center VMM)
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een virtuele machine


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Zorg ervoor dat u begrijpt de [scenario-architectuur en onderdelen](concepts-hyper-v-to-azure-architecture.md).
- Controleer de [ondersteuningsvereisten](site-recovery-support-matrix-to-azure.md) voor alle onderdelen.
- Zorg ervoor dat virtuele machines die u wilt repliceren voldoen [vereisten van de virtuele machine van Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
- Om te achterhalen capaciteitsplanning:
    - Gebruik de [Capaciteitsplanner voor Hyper-V](http://aka.ms/asr-capacity-planner-excel)om uw verloop te achterhalen.
    - Gebruik de [Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) schatten bandbreedte voor het analyseren van uw bronomgeving en vereisten zijn gericht.
- Voorbereiden op Azure. U moet een Azure-abonnement, een Azure-netwerk en een opslagaccount.
- Lokale Hyper-V-hosts en VMM-servers als relevante voorbereiden.





### <a name="set-up-an-azure-account"></a>Een Azure-account instellen

Ophalen van een Microsoft [Azure-account](http://azure.microsoft.com/).

- U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- Meer informatie over [prijzen voor Site Recovery](site-recovery-faq.md#pricing), en [prijsinformatie](https://azure.microsoft.com/pricing/details/site-recovery/).
- Weten wie [regio's worden ondersteund](https://azure.microsoft.com/pricing/details/site-recovery/) voor siteherstel.

### <a name="verify-azure-account-permissions"></a>Controleer de machtigingen van de Azure-account

Zorg ervoor dat uw Azure-account de machtigingen die moeten worden gerepliceerd van virtuele machines.

- Controleer de [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) moet u virtuele machines repliceren naar Azure
- Controleer en wijzig [toegangsgroepen op basis van](../active-directory/role-based-access-control-configure.md) machtigingen. 


### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Instellen van een [Azure-netwerk](../virtual-network/virtual-network-get-started-vnet-subnet.md).

- Virtuele machines in Azure worden geplaatst in dit netwerk wanneer ze zijn gemaakt na een failover.
- Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.


### <a name="set-up-an-azure-storage-account"></a>Een Azure-opslagaccount instellen

Instellen van een [Azure storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Lokale-machines van de site Recovery worden gerepliceerd naar Azure-opslag. Virtuele machines in Azure worden gemaakt van de opslag nadat de failover plaatsvindt.
- Het opslagaccount moet in dezelfde regio bevinden als de Recovery Services-kluis.
- Het opslagaccount kan worden standaard of [premium](../virtual-machines/windows/premium-storage.md).
- Als u een premium-account hebt ingesteld, moet u een extra standaardaccount voor logboekgegevens.

### <a name="prepare-hyper-v-hosts"></a>Voorbereiden van de Hyper-V-hosts

1. Controleer of die Hyper-V-hosts voldoen aan [ondersteuningsvereisten](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
2. Zorg ervoor dat de hosts toegang hebben tot deze URL's:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Alle IP-adressen gebaseerde firewallregels moeten communicatie met Azure toestaan.
    - Sta de [IP-adresbereiken voor Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de HTTPS-poort (443) toe.
    - IP-adresbereiken voor de Azure-regio van uw abonnement en voor VS-West (gebruikt voor beheer en de identiteit van toegangsbeheer) toestaan.

### <a name="prepare-vmm-servers"></a>VMM-servers voorbereiden

Als Hyper-V-hosts worden beheerd door VMM, moet u de lokale VMM-server voorbereiden. 

- Controleren of de VMM-server voldoet aan [ondersteuningsvereisten](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- Zorg ervoor dat de VMM-server een minimaal één cloud, met een of meer hostgroepen. De virtuele machines waarop Hyper-V-host moet zich in de cloud.
- De VMM-beheerserver moet toegang tot internet, met toegang tot de volgende URL's:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Alle IP-adressen gebaseerde firewallregels moeten communicatie met Azure toestaan.
    - Sta de [IP-adresbereiken voor Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de HTTPS-poort (443) toe.
    - Sta de IP-adresbereiken voor de Azure-regio van uw abonnement en voor de regio VS - west toe (deze worden gebruikt voor toegangs- en identiteitsbeheer).
- De VMM-server voorbereiden op netwerktoewijzing.


#### <a name="prepare-vmm-for-network-mapping"></a>VMM voorbereiden op netwerktoewijzing

Als u VMM, [netwerktoewijzing](site-recovery-network-mapping.md) toewijzingen tussen on-premises VMM VM-netwerken en virtuele netwerken in Azure. Toewijzing zorgt ervoor dat de virtuele Azure-machines zijn verbonden met het juiste netwerk wanneer ze zijn gemaakt na een failover.

VMM voorbereiden voor netwerktoewijzing als volgt:

1. Zorg ervoor dat u hebt een [logisch netwerk van VMM](https://docs.microsoft.com/system-center/vmm/network-logical) die is gekoppeld aan de cloud waarin de Hyper-V-hosts zich bevinden.
2. Zorg ervoor dat u hebt een [VM-netwerk](https://docs.microsoft.com/system-center/vmm/network-virtual) gekoppeld aan het logische netwerk.
3. De virtuele machines verbinding met het VM-netwerk.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-a-protection-goal"></a>Selecteer een beveiligingsdoel

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Klik in de kluis op **siteherstel** > **infrastructuur voorbereiden** > **beveiligingsdoel**.
2. In **beveiligingsdoel**, selecteer **naar Azure**> **Ja, met Hyper-V**. 
    - Als Hyper-V-hosts worden niet door VMM wordt beheerd, selecteert u **Nee** om te bevestigen dat u gebruikt geen VMM.
    - Als de hosts in VMM-clouds worden beheerd, selecteert u **Ja**.

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Bij het instellen van de bronomgeving, moet u de Azure Site Recovery Provider en de Azure Recovery Services-agent installeren en on-premises-servers in de kluis registreren. 

1. In **infrastructuur voorbereiden**, klikt u op **bron**. 
    - Als u geen van VMM gebruikmaakt, klikt u op **+ Hyper-V-Site**, en geef een sitenaam op. Klik vervolgens op **+ Hyper-V Server**, en voeg een host of cluster toe aan de site.
    - Als u VMM, **bron voorbereiden**, klikt u op **+ VMM** een VMM-server toevoegen. In **Server toevoegen**, controleert u of **System Center VMM-server** wordt weergegeven in **servertype**.
2. Download de Provider en agent-onderdelen, afhankelijk van uw omgeving.
    - Download het installatiebestand van de Provider voor Hyper-V alleen. U hebt uitgevoerd van het bestand op elke Hyper-V-host om zowel de Provider en de agent te installeren.
        
        ![Provider zonder VMM](./media/tutorial-hyper-v-to-azure/download-no-vmm.png)
    
    - Download afzonderlijk de Provider en de agent voor Hyper-V met VMM. Voer de installatie van de Provider op de VMM-server. Voer de installatie van de agent op elke Hyper-V-host.
    
        ![Provider en de agent met VMM](./media/tutorial-hyper-v-to-azure/download-vmm.png)
    
3. Download de kluisregistratiesleutel. U moet dit als u Provider setup uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.

### <a name="install-components"></a>-Onderdelen installeren

Installeer de onderdelen als summmarized in de tabel. 

**Onderdeel** | **Details** | **Hyper-V alleen** | **Hyper-V met VMM**
--- | --- | --- | ---
**Azure Site Recovery Provider** | Stuurt replicatie naar Azure | Installeren op elke Hyper-V-host | Op de VMM-server installeren
**Recovery Services-agent** | Agent verwerkt de gegevensreplicatie | Installeren op elke Hyper-V-host | Installeer op Hyper-V-host


#### <a name="install-the-provider-on-hyper-v-without-vmm"></a>De Provider installeren op Hyper-V zonder VMM

De Provider installeren op elke Hyper-V-host die u hebt toegevoegd aan de Hyper-V-site. Als u op een Hyper-V-cluster installeert, moet u setup uitvoeren op elk clusterknooppunt. Installeren en registreren van elk clusterknooppunt Hyper-V zorgt ervoor dat virtuele machines worden beschermd, zelfs als ze via knooppunten migreren.

1. In **Microsoft Update** kunt u updates inschakelen, zodat de providerupdates volgens uw Microsoft Update-beleid worden geïnstalleerd.
2. Accepteer of wijzig in **Installatie** de standaardinstallatielocatie van de provider en klik op **Installeren**.
4. In **Kluisinstellingen**, klikt u op **Bladeren** selecteren van het kluissleutelbestand dat u hebt gedownload. Geef het Azure Site Recovery-abonnement, de kluisnaam van de en de Hyper-V-site waarop de Hyper-V-server behoort.
5. In **Proxy-instellingen**, opgeven hoe de Provider die wordt uitgevoerd op de VMM-server of Hyper-V-host verbinding maakt met Site Recovery via internet.
    * Selecteer voor een directe verbinding **rechtstreeks verbinding maken met Azure Site Recovery zonder een proxy**.
    * Selecteer voor een proxy **verbinding maken met Azure Site Recovery via een proxyserver**. Geef de proxy-adres, poort en referenties, indien nodig.
6. Nadat de server is geregistreerd in de kluis, klikt u op **voltooien**.

Metagegevens van de Hyper-V-server worden opgehaald door Azure Site Recovery en de server wordt weergegeven in **Site Recovery-infrastructuur** > **Hyper-V-Hosts**.


#### <a name="install-the-recovery-services-agent-on-hyper-v-host-without-vmm"></a>De Recovery Services-agent op Hyper-V-host zonder VMM installeren

Als u VMM in uw implementatie gebruikt, kunt u de Recovery Services agent setup uitvoeren op elke Hyper-V-host.

1. In de installatiewizard > **controle van vereisten**, klikt u op **volgende**. Ontbrekende vereiste onderdelen worden automatisch geïnstalleerd.

    ![Vereisten voor de Recovery Services-agent](./media/tutorial-hyper-v-to-azure/hyperv-agent-prerequisites.png)
3. Accepteer of wijzig onder **Installatie-instellingen** de installatielocatie en de cachelocatie. De cachestation moet ten minste 5 GB aan opslagruimte. U wordt aangeraden een station met 600 GB of meer aan vrije ruimte. Klik vervolgens op **Installeren**.
4. In **installatie**, wanneer de installatie is voltooid, klikt u op **sluiten** om de wizard te voltooien.

##### <a name="set-up-internet-access-via-a-proxy"></a>Toegang tot internet via een proxyserver instellen

De Recovery Services-agent op de Hyper-V-host moet een internettoegang tot Azure voor het repliceren van virtuele machines. Als u internet gebruikt via een proxy, doet u het volgende:

1. Open de Microsoft Azure Backup MMC-module op de Hyper-V-host. Een snelkoppeling naar Microsoft Azure Backup is standaard beschikbaar zijn op het bureaublad of in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik in de module op **Eigenschappen wijzigen**.
3. Op de **proxyconfiguratie** tabblad, proxy-informatie opgeven.

    ![MARS-agent registreren](./media/tutorial-hyper-v-to-azure/mars-proxy.png)
4. Controleer of de agent kan bereiken de [URL's vereist](#prepare-hyper-v-hosts).

#### <a name="install-the-provider-on-the-vmm-server-hyper-v-with-vmm"></a>De Provider installeren op de VMM-server (Hyper-V met VMM)

1. In **Microsoft Update** kunt u updates inschakelen, zodat de providerupdates volgens uw Microsoft Update-beleid worden geïnstalleerd.
2. Accepteer of wijzig in **Installatie** de standaardinstallatielocatie van de provider en klik op **Installeren**. 
3. Nu gaan we de VMM-server registreren in de kluis. In **Kluisinstellingen**, klikt u op **Bladeren** selecteren van het kluissleutelbestand dat u hebt gedownload. Geef het Azure Site Recovery-abonnement en de kluisnaam van de.

    ![Serverregistratie](./media/tutorial-hyper-v-to-azure/provider-vault-settings.png)

4. In **Proxy-instellingen**, opgeven hoe de Provider die wordt uitgevoerd op de VMM-server of Hyper-V-host verbinding maakt met Site Recovery via internet.

    * Selecteer voor een directe verbinding **rechtstreeks verbinding maken met Azure Site Recovery zonder een proxy**.
    * Selecteer voor een proxy **verbinding maken met Azure Site Recovery via een proxyserver**. Geef de proxy-adres, poort en referenties, indien nodig.
    - Een VMM RunAs-account (DRAProxyAccount) wordt automatisch gemaakt met de opgegeven proxyreferenties. Configureer de proxyserver zodanig dat dit account kan worden geverifieerd. De RunAs-Accountinstellingen kunnen worden gewijzigd in de VMM-console > **instellingen** > **beveiliging** > **Run As-Accounts**. Start de VMM-service voor het bijwerken van wijzigingen.
5. In **gegevensversleuteling**, Geef op of alle gegevens die worden verzonden naar Azure versleuteld. Als u deze optie selecteert problemen bij het herstellen van de Site een certificaat. U moet dit certificaat om gegevens te ontsleutelen later op.
6. In **VMM-Server**, Geef een beschrijvende naam voor het identificeren van de VMM-server in de kluis. Geef in een clusterconfiguratie de VMM-clusterrolnaam op. In **cloudmetagegevens synchroniseren met de kluis**Selecteer of u metagegevens wilt synchroniseren voor alle clouds op de VMM-server met de kluis. Deze actie hoeft op elke server slechts één keer te worden uitgevoerd. Als u niet dat alle clouds synchroniseren wilt, kunt u laat u deze instelling uitgeschakeld en synchroniseert u elke cloud afzonderlijk in de cloudeigenschappen in de VMM-console.

Nadat de registratie is voltooid, metagegevens van de server worden opgehaald door Azure Site Recovery en de VMM-server wordt weergegeven in **Site Recovery-infrastructuur**.






## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen. 

1. Klik op **infrastructuur voorbereiden** > **doel**.
2. Selecteer het abonnement en de resourcegroep waarin de Azure VM's na een failover wordt gemaakt. Kies het implementatiemodel dat u wilt gebruiken in Azure voor de virtuele machines.

3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

## <a name="configure-network-mapping-with-vmm"></a>Netwerktoewijzing (met VMM) configureren

Als u VMM gebruikt, kunt u netwerktoewijzing instellen.

1. Klik in **Site Recovery-infrastructuur** > **Netwerktoewijzingen** > **Netwerktoewijzing** op het pictogram **+Netwerktoewijzing**.
2. In **netwerktoewijzing toevoegen**, selecteert u de bron-VMM-server. Selecteer **Azure** als doel.
3. Controleer na het uitvoeren van een failover het abonnement en het implementatiemodel.
4. In **Bronnetwerk**, selecteert u de bron on-premises VM-netwerk.
5. In **doelnetwerk**, selecteer het Azure-netwerk in welke replica Azure VM's geplaatst worden wanneer ze zijn gemaakt na een failover. Klik vervolgens op **OK**.

    ![Netwerktoewijzing](./media/tutorial-hyper-v-to-azure/network-mapping-vmm.png)

## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Klik op **infrastructuur voorbereiden** > **replicatie-instellingen** > **+ maken en koppelen**.
2. Geef in **Beleid maken en koppelen** een beleidsnaam op.
3. Geef in **Kopieerfrequentie** op hoe vaak u na de eerste replicatie de verschillen wilt repliceren (elke 30 seconden of elke 5 of 15 minuten).

    > [!NOTE]
    >  Een frequentie van 30 seconden wordt niet ondersteund bij het repliceren naar Premium Storage. De beperking wordt bepaald door het aantal momentopnamen per blob (100) dat wordt ondersteund door Premium Storage. [Meer informatie](../virtual-machines/windows/premium-storage.md#snapshots-and-copy-blob).

4. In **herstel bewaarperiode**, opgeven hoe lang het moet worden bewaard (in uren) voor elk herstelpunt. Beveiligde machines kunnen te allen tijde worden hersteld naar een willekeurig punt (binnen een bepaald tijdsvenster).
5. Geef in **Frequentie van de app-consistente momentopname** op hoe vaak (elke 1-12 uur) er herstelpunten moeten worden gemaakt met toepassingsconsistente momentopnamen. Hyper-V gebruikt twee soorten momentopnamen:
    - **Standaardmomentopname**: biedt een incrementele momentopname van de volledige virtuele machine.
    - **App-consistente momentopname**: een momentopname van de punt in tijd van de toepassingsgegevens in de virtuele machine. Volume Shadow Copy Service (VSS) zorgt ervoor dat apps in een consistente status wanneer de momentopname wordt gemaakt. Toepassingsconsistente momentopnamen inschakelen beïnvloedt de appprestaties op de bron-VM's. Stel een waarde die kleiner is dan het aantal aanvullende herstelpunten dat u configureert.
6. Geef in **Begintijd initiële replicatie** aan wanneer de initiële replicatie moet worden gestart. Replicatie vindt plaats via uw internetbandbreedte wilt u deze buiten de piekuren te plannen.
7. Geef in **Gegevens versleutelen die zijn opgeslagen in Azure** op of u at-rest-gegevens in de Azure-opslag wilt versleutelen. Klik vervolgens op **OK**.

    ![Beleid voor replicatie](./media/tutorial-hyper-v-to-azure/replication-policy.png)


Wanneer u een nieuw beleid maakt is deze automatisch gekoppeld aan de VMM-cloud of Hyper-V-site.

## <a name="enable-replication"></a>Replicatie inschakelen


1. Klik op **toepassing repliceren** > **bron**. 
2. In **bron**, selecteer de Hyper-V-site of VMM-server/cloud. Klik vervolgens op **OK**.
3. In **doel**, controleren of Azure als het doel, het abonnement van de kluis en het model dat u wilt gebruiken in Azure na een failover.
4. Selecteer het opslagaccount voor gerepliceerde gegevens en het Azure-netwerk waarin Azure VM's worden geplaatst na een failover.
5. In **virtuele machines** > **Selecteer**, selecteert u de virtuele machines die u wilt repliceren. Klik vervolgens op **OK**.

 U kunt de voortgang van volgen de **beveiliging inschakelen** actie in **taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de initiële replicatie is voltooid en de virtuele machine is gereed voor failover.

## <a name="next-steps"></a>Volgende stappen
[Noodherstelanalyse uitvoeren](tutorial-dr-drill-azure.md)
