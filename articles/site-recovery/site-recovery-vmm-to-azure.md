---
title: Virtuele Hyper-V-machines in VMM-clouds repliceren naar Azure | Microsoft Docs
description: Replicatie, failovers en herstel van virtuele Hyper-V-machines in System Center VMM-clouds naar Azure beheren
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 958b61f5de732a882e0a2682b8dd4e18504a6ae7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-site-recovery-in-the-azure-portal"></a>Virtuele Hyper-V-machines in VMM-clouds repliceren naar Azure met Site Recovery in Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Klassieke Azure Portal](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell klassiek](site-recovery-deploy-with-powershell.md)


In dit artikel wordt beschreven hoe u met behulp van [Azure Site Recovery](site-recovery-overview.md) in Azure Portal on-premises virtuele Hyper-V-machines die in System Center Virtual Machine Manager-clouds (VMM) worden beheerd, naar Azure kunt repliceren.

Na het lezen van dit artikel kunt u onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.

In [dit artikel](site-recovery-migrate-to-azure.md) vindt u meer informatie over het migreren van machines naar Azure (zonder failback).


## <a name="deployment-steps"></a>Implementatiestappen

Volg de stappen in het artikel om deze implementatiestappen te voltooien:


1. [Meer informatie](site-recovery-components.md) over de architectuur voor deze implementatie. Lees ook [meer informatie](site-recovery-hyper-v-azure-architecture.md) over de werking van Hyper-V-replicatie in Site Recovery.
2. Controleer de vereisten en beperkingen.
3. Stel Azure-netwerk- en opslagaccounts in.
4. Bereid de on-premises VMM-server en Hyper-V-hosts voor.
5. Maak een Recovery Services-kluis. De kluis bevat configuratie-instellingen en met de kluis wordt de replicatie ingedeeld.
6. Geef de broninstellingen op. Registreer de VMM-server in de kluis. Installeer de Azure Site Recovery Provider op de VMM-server. Installeer de Microsoft Recovery Services-agent op Hyper-V-hosts.
7. Stel de doel- en replicatie-instellingen in.
8. Schakel replicatie voor de VM's in.
9. Voer een testfailover uit om te controleren of alles goed werkt.



## <a name="prerequisites"></a>Vereisten


**Vereiste voor ondersteuning** | **Details**
--- | ---
**Azure** | Meer informatie over [Azure-vereisten](site-recovery-prereq.md#azure-requirements).
**On-premises servers** | [Meer informatie](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure) over de vereisten voor de on-premises VMM-server en Hyper-V-hosts.
**On-premises Hyper-V-VM's** | VM's die u wilt repliceren, moeten worden uitgevoerd op een [ondersteund besturingssysteem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) en voldoen aan de [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Azure-URL's** | De VMM-server moet toegang hebben tot deze URL's:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Als u op IP-adressen gebaseerde firewallregels gebruikt, controleert u of de regels communicatie met Azure toestaan.<br/></br> Sta de [IP-adresbereiken voor Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de HTTPS-poort (443) toe.<br/></br> Sta de IP-adresbereiken voor de Azure-regio van uw abonnement en voor de regio VS - west toe (deze worden gebruikt voor toegangs- en identiteitsbeheer).


## <a name="prepare-for-deployment"></a>Implementatie voorbereiden
U moet het volgende doen om de implementatie voor te bereiden:

1. [Een Azure-netwerk instellen](#set-up-an-azure-network) waarin de virtuele Azure-machines worden opgeslagen na de failover.
2. [Een Azure-opslagaccount instellen](#set-up-an-azure-storage-account) voor gerepliceerde gegevens.
3. [De VMM-server voorbereiden](#prepare-the-vmm-server) op de implementatie van Site Recovery.
4. Voorbereiden op netwerktoewijzing. Stel netwerken zodanig in dat u tijdens de implementatie van Site Recovery netwerktoewijzing kunt configureren.

### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen
U hebt een Azure-netwerk nodig waarmee de virtuele Azure-machines die na een failover worden gemaakt, kunnen worden verbonden.

* Het netwerk moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
* Afhankelijk van het resourcemodel dat u wilt gebruiken voor virtuele Azure-machines waarvoor een failover is uitgevoerd, moet u het Azure-netwerk instellen in de [Resource Manager-modus](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of de [klassieke modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* U doet er verstandig aan een netwerk in te stellen voordat u begint. Anders moet u dit doen tijdens de implementatie van Site Recovery.
Azure-netwerken die worden gebruikt door Site Recovery, kunnen niet worden [verplaatst](../azure-resource-manager/resource-group-move-resources.md) binnen hetzelfde abonnement of naar andere abonnementen.

### <a name="set-up-an-azure-storage-account"></a>Een Azure-opslagaccount instellen
* U hebt een Standard-/Premium-account voor Azure Storage nodig voor het opslaan van gegevens die zijn gerepliceerd naar Azure. [Premium Storage](../storage/common/storage-premium-storage.md) wordt gebruikt voor virtuele machines die consistent hoge I/O-prestaties en lage latentie nodig hebben om intensieve I/O-werkbelastingen te hosten. Als u een Premium-account wilt gebruiken om gerepliceerde gegevens op te slaan, hebt u ook een Standard-opslagaccount nodig om replicatielogboeken op te slaan waarin de doorlopende wijzigingen in uw on-premises gegevens worden vastgelegd. Het account moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
* Afhankelijk van het resourcemodel dat u wilt gebruiken voor virtuele Azure-machines waarvoor een failover is uitgevoerd, moet u een account instellen in de [Resource Manager-modus](../storage/common/storage-create-storage-account.md) of de [klassieke modus](../storage/common/storage-create-storage-account.md).
* U doet er verstandig aan een account in te stellen voordat u begint. Anders moet u dit doen tijdens de implementatie van Site Recovery.
- Let op: opslagaccounts die worden gebruikt met Site Recovery, kunnen niet worden [verplaatst](../azure-resource-manager/resource-group-move-resources.md) binnen hetzelfde abonnement of naar andere abonnementen.

### <a name="prepare-the-vmm-server"></a>De VMM-server voorbereiden
* Zorg ervoor dat de VMM-server voldoet aan de [vereisten](#prerequisites).
* Tijdens de implementatie van Site Recovery kunt u opgeven dat alle clouds op een VMM-server beschikbaar moeten zijn in Azure Portal. Als u wilt dat alleen bepaalde clouds in de portal worden weergegeven, kunt u die instelling in de cloud inschakelen via de VMM-beheerconsole.

### <a name="prepare-for-network-mapping"></a>Voorbereiden op netwerktoewijzing
Tijdens de implementatie van Site Recovery moet u netwerktoewijzing instellen. Bij netwerktoewijzing worden VMM-bronnetwerken met virtuele machines toegewezen aan doelnetwerken in Azure om het volgende mogelijk te maken:

* Machines waarvoor een failover naar hetzelfde netwerk wordt uitgevoerd, kunnen met elkaar worden verbonden, zelfs als de failover niet op dezelfde manier of volgens hetzelfde herstelplan wordt uitgevoerd.
* Als er in het doelnetwerk in Azure een netwerkgateway is ingesteld, kunnen virtuele Azure-machines worden verbonden met on-premises virtuele machines.
* Dit hebt u nodig voor het instellen van netwerktoewijzing:

  * Zorg ervoor dat de virtuele machines op de Hyper-V-bronhostserver zijn verbonden met een VMM-netwerk met virtuele machines. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is verbonden met de cloud.
  * Een Azure-netwerk zoals [hierboven](#set-up-an-azure-network) wordt beschreven

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw** > **Bewaking en beheer** > **Backup en Site Recovery (OMS)**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. Geef in **Naam** een beschrijvende naam op om de kluis mee aan te duiden. Als u meer dan één abonnement hebt, selecteert u een van uw abonnementen.
4. [Maak een resourcegroep](../azure-resource-manager/resource-group-template-deploy-portal.md) of selecteer een bestaande resourcegroep. Geef een Azure-regio op. Machines worden naar deze regio gerepliceerd. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund
5. Als u de kluis snel wilt openen via het dashboard, klikt u op **Vastmaken aan dashboard** > **Kluis maken**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-azure/new-vault.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **Alle resources** en op de hoofdblade **Recovery Services-kluizen**.


## <a name="select-the-protection-goal"></a>Het beveiligingsdoel selecteren

Selecteer wat u wilt repliceren en waarnaar u wilt repliceren.

1. Selecteer de kluis in **Recovery Services-kluizen**.
2. Klik in **Aan de slag** op **Site Recovery** > **Infrastructuur voorbereiden** > **Beveiligingsdoel**.

    ![Doelstellingen kiezen](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. Selecteer in **Beveiligingsdoel** de optie **Naar Azure**. Selecteer vervolgens **Ja, met Hyper-V**. Selecteer **Ja** om te bevestigen dat u VMM gebruikt voor het beheren van Hyper-V-hosts en de herstelsite. Klik vervolgens op **OK**.

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Installeer de Azure Site Recovery-provider op de VMM-server en registreer de server in de kluis. Installeer de Azure Recovery Services-agent op de Hyper-V-hosts.

1. Klik op **Infrastructuur voorbereiden** > **Bron**.

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source1.png)

2. Klik in **Bron voorbereiden** op **+ VMM** om een VMM-server toe te voegen.

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source2.png)

3. Controleer in **Server toevoegen** of **System Center VMM-server** wordt weergegeven in **Servertype** en of de VMM-server voldoet aan de [vereisten en URL-vereisten](#prerequisites).
4. Download het installatieprogramma voor de Azure Site Recovery-provider.
5. Download de registratiesleutel. U hebt deze nodig wanneer u de installatie uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source3.png)


## <a name="install-the-provider-on-the-vmm-server"></a>De provider installeren op de VMM-server

1. Voer het providerinstallatiebestand uit op de VMM-server.
2. In **Microsoft Update** kunt u updates inschakelen, zodat de providerupdates volgens uw Microsoft Update-beleid worden geïnstalleerd.
3. Accepteer of wijzig in **Installatie** de standaardinstallatielocatie van de provider en klik op **Installeren**.

    ![Installatielocatie](./media/site-recovery-vmm-to-azure/provider2.png)
4. Wanneer de installatie is voltooid, klikt u op **Registreren** om de VMM-server in de kluis te registreren.
5. Klik op de pagina **Kluisinstellingen** op **Bladeren** om het kluissleutelbestand te selecteren. Geef het Azure Site Recovery-abonnement en de kluisnaam op.

    ![Serverregistratie](./media/site-recovery-vmm-to-azure/provider10.PNG)
6. Geef in **Internetverbinding** op hoe de provider die op de VMM-server wordt uitgevoerd, via internet verbinding moet maken met Site Recovery.

   * Als u wilt dat de provider rechtstreeks verbinding maakt, selecteert u **Rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
   * Als voor uw bestaande proxy verificatie is vereist of als u een aangepaste proxy wilt gebruiken, selecteert u **Verbinding maken met Azure Site Recovery met een proxyserver**.
   * Als u een aangepaste proxy gebruikt, moet u het adres, de poort en referenties opgeven.
   * Als u een proxy gebruikt, hebt u, als het goed is, de URL’s die worden beschreven in [Vereisten](#on-premises-prerequisites), al toegestaan.
   * Als u een aangepaste proxy gebruikt, wordt er automatisch een VMM RunAs-account (DRAProxyAccount) gemaakt met de opgegeven proxyreferenties. Configureer de proxyserver zodanig dat dit account kan worden geverifieerd. De VMM RunAs-accountinstellingen kunnen worden gewijzigd in de VMM-console. Open **Instellingen**, vouw **Beveiliging** > **Run As-accounts** uit en wijzig vervolgens het wachtwoord voor DRAProxyAccount. U moet de VMM-service opnieuw starten om de instelling door te voeren.

     ![internet](./media/site-recovery-vmm-to-azure/provider13.PNG)
7. Accepteer of wijzig de locatie van het SSL-certificaat dat automatisch is gegenereerd voor gegevensversleuteling. Dit certificaat wordt gebruikt als u gegevensversleuteling inschakelt voor een cloud die door Azure wordt beveiligd in de Azure Site Recovery-portal. Bewaar dit certificaat op een veilige plaats. Als gegevensversleuteling is ingeschakeld, moet u de gegevens eerst ontsleutelen wanneer u een failover naar Azure uitvoert.

    > [!NOTE]
    > Het is raadzaam de versleutelingsfunctie die wordt geleverd door Azure te gebruiken voor het versleutelen van data-at-rest in plaats van de gegevensversleutelingsoptie die door Azure Site Recovery wordt geboden. De versleutelingsfunctie die wordt verstrekt door Azure kan worden ingeschakeld voor een opslagaccount en zorgt voor betere prestaties omdat de versleuteling/ontsleuteling wordt verwerkt door Azure Storage.
    > [Meer informatie over de Storage-versleutelingsservice van Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
8. Geef in **Servernaam** een beschrijvende naam op om de VMM-server in de kluis te identificeren. Geef in een clusterconfiguratie de VMM-clusterrolnaam op.
9. Geef in **Cloudmetagegevens synchroniseren** aan of u metagegevens wilt synchroniseren voor alle clouds op de VMM-server met de kluis. Deze actie hoeft op elke server slechts één keer te worden uitgevoerd. Als u niet alle clouds wilt synchroniseren, laat u deze instelling uitgeschakeld en synchroniseert u elke cloud afzonderlijk in de cloudeigenschappen in de VMM-console. Klik op **Registreren** om het proces te voltooien.

    ![Serverregistratie](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. De registratie begint. Na voltooiing van de registratie wordt de server weergegeven in **Site Recovery-infrastructuur** > **VMM-servers**.


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>De Azure Recovery Services-agent installeren op Hyper-V-hosts

1. Nadat u de provider hebt ingesteld, moet u het installatiebestand voor de Azure Recovery Services-agent downloaden. Voer de installatie uit op elke Hyper-V-server in de VMM-cloud.

    ![Hyper-V-sites](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. Klik in **Controle van vereisten** op **Volgende**. Ontbrekende vereiste onderdelen worden automatisch geïnstalleerd.

    ![Vereisten voor de Recovery Services-agent](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. Accepteer of wijzig onder **Installatie-instellingen** de installatielocatie en de cachelocatie. U kunt de cache configureren op een station met ten minste 5 GB aan opslagruimte, maar wij raden u aan een cachestation te gebruiken met 600 GB of meer aan vrije ruimte. Klik vervolgens op **Installeren**.
4. Nadat de installatie is voltooid, klikt u op **Sluiten**.

    ![MARS-agent registreren](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

### <a name="command-line-installation"></a>Installatie vanaf de opdrachtregel
U kunt de Microsoft Azure Recovery Services-agent installeren vanaf de opdrachtregel met de volgende opdracht:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Internetproxytoegang tot Site Recovery vanaf Hyper-V-hosts instellen

Als de Recovery Services-agent wordt uitgevoerd op Hyper-V-hosts, moet Azure via internet toegankelijk zijn voor het repliceren van virtuele machines. Als u internet gebruikt via een proxy, doet u het volgende:

1. Open de Microsoft Azure Backup MMC-module op de Hyper-V-host. Standaard is er een snelkoppeling naar Microsoft Azure Backup beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik in de module op **Eigenschappen wijzigen**.
3. Geef op het tabblad **Proxyconfiguratie** de proxyservergegevens op.

    ![MARS-agent registreren](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Zorg ervoor dat de agent de URL's kan bereiken die worden beschreven in [vereisten](#on-premises-prerequisites).

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen
Geef op welk Azure-opslagaccount moet worden gebruikt voor replicatie en met welk Azure-netwerk de virtuele Azure-machines verbinding moeten maken na het uitvoeren van een failover.

1. Klik op **Infrastructuur voorbereiden** > **Doel**, en selecteer het abonnement en de resourcegroep waarin u de failover-VM's wilt maken. Kies het implementatiemodel dat u wilt gebruiken in Azure (klassiek of resourcebeheer) voor de failover-VM's.

    ![Opslag](./media/site-recovery-vmm-to-azure/enablerep3.png)

2. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

    ![Storage](./media/site-recovery-vmm-to-azure/compatible-storage.png)

3. Als u nog geen opslagaccount hebt gemaakt en u er een wilt maken met Resource Manager, klikt u op **+Opslagaccount** om dat inline te doen.  Geef op de blade **Opslagaccount maken** een accountnaam, het type, het abonnement en de locatie op. Het account moet zich op dezelfde locatie bevinden als de Recovery Services-kluis.

   ![Storage](./media/site-recovery-vmm-to-azure/gs-createstorage.png)


   * Als u een opslagaccount wilt maken op basis van het klassieke model, doet u dat in Azure Portal. [Meer informatie](../storage/common/storage-create-storage-account.md)
   * Als u een Premium-opslagaccount gebruikt voor gerepliceerde gegevens, moet u een extra Standard-opslagaccount instellen om de replicatielogboeken op te slaan waarin de doorlopende wijzigingen aan uw on-premises gegevens worden vastgelegd.
5. Als u nog geen Azure-netwerk hebt gemaakt en u er een wilt maken met Resource Manager, klikt u op **+Netwerk** om dat inline te doen. Geef op de blade **Virtueel netwerk maken** een netwerknaam, het adresbereik, de subnetgegevens, het abonnement en de locatie op. Het netwerk moet zich op dezelfde locatie bevinden als de Recovery Services-kluis.

   ![Netwerk](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

   Als u een netwerk wilt maken op basis van het klassieke model, doet u dat in Azure Portal. [Meer informatie](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Netwerktoewijzing configureren

* [Lees](#prepare-for-network-mapping) een kort overzicht van wat netwerktoewijzing inhoudt.
* Controleer of de virtuele machines op de VMM-server verbonden zijn met een netwerk met virtuele machines en of u ten minste één virtueel Azure-netwerk hebt gemaakt. Aan één Azure-netwerk kunnen meerdere netwerken met virtuele machines worden toegewezen.

Configureer het toewijzen als volgt:

1. Klik in **Site Recovery-infrastructuur** > **Netwerktoewijzingen** > **Netwerktoewijzing** op het pictogram **+Netwerktoewijzing**.

    ![Netwerktoewijzing](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. Selecteer in **Netwerktoewijzing toevoegen** de bron-VMM-server en selecteer **Azure** als doel.
3. Controleer na het uitvoeren van een failover het abonnement en het implementatiemodel.
4. Selecteer in **Bronnetwerk** in de lijst die is gekoppeld aan de VMM-server, het on-premises bronnetwerk met virtuele machines dat u wilt toewijzen.
5. Selecteer in **Doelnetwerk** het Azure-netwerk waarin de replica’s (virtuele Azure-machines) moeten worden geplaatst nadat ze zijn gemaakt. Klik vervolgens op **OK**.

    ![Netwerktoewijzing](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Dit is wat er gebeurt wanneer er netwerktoewijzing wordt uitgevoerd:

* Bestaande virtuele machines in het VM-bronnetwerk zijn verbonden met het netwerk wanneer de toewijzing begint. Nieuwe virtuele machines die zijn verbonden met het bron-VM-netwerk, worden verbonden met het toegewezen Azure-netwerk wanneer replicatie plaatsvindt.
* Als u een bestaande netwerktoewijzing wijzigt, worden gerepliceerde virtuele machines verbonden op basis van de nieuwe instellingen.
* Als het doelnetwerk meerdere subnetten bevat en een van deze subnetten dezelfde naam heeft als het subnet waarin de virtuele bronmachine zich bevindt, wordt de gerepliceerde virtuele machine na een failover verbonden met dat doelsubnet.
* Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.

## <a name="configure-replication-settings"></a>Replicatie-instellingen configureren
1. Als u nieuw replicatiebeleid wilt maken, klikt u op **Infrastructuur voorbereiden** > **Replicatie-instellingen** > **+Maken en koppelen**.

    ![Netwerk](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. Geef in **Beleid maken en koppelen** een beleidsnaam op.
3. Geef in **Kopieerfrequentie** op hoe vaak u na de eerste replicatie de verschillen wilt repliceren (elke 30 seconden of elke 5 of 15 minuten).

    > [!NOTE]
    >  Een frequentie van 30 seconden wordt niet ondersteund bij het repliceren naar Premium Storage. De beperking wordt bepaald door het aantal momentopnamen per blob (100) dat wordt ondersteund door Premium Storage. [Meer informatie](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. Geef in **Bewaarperiode van het herstelpunt** op hoeveel uur elk herstelpunt moet worden bewaard. Beveiligde machines kunnen te allen tijde worden hersteld naar een willekeurig punt (binnen een bepaald tijdsvenster).
5. Geef in **Frequentie van de app-consistente momentopname** op hoe vaak (elke 1-12 uur) er herstelpunten moeten worden gemaakt met toepassingsconsistente momentopnamen. Hyper-V maakt gebruik van twee soorten momentopnamen: standaardmomentopnamen waarmee steeds incrementele momentopnamen van de volledige virtuele machine worden gemaakt, en toepassingsconsistente momentopnamen waarmee een tijdsgebonden momentopname wordt gemaakt van de toepassingsgegevens in de virtuele machine. Toepassingsconsistente momentopnamen gebruiken Volume Shadow Copy Service (VSS) om ervoor te zorgen dat toepassingen een consistente status hebben wanneer er een momentopname wordt gemaakt. Als u toepassingsconsistente momentopnamen inschakelt, is dit van invloed op de prestaties van de toepassingen die via de virtuele bronmachines worden uitgevoerd. Zorg ervoor dat de waarde die u instelt, kleiner is dan het aantal aanvullende herstelpunten dat u configureert.
6. Geef in **Begintijd initiële replicatie** aan wanneer de initiële replicatie moet worden gestart. De replicatie maakt gebruik van uw internetbandbreedte. Daarom is het een goed idee om de replicatie buiten de drukste tijden in te plannen.
7. Geef in **Gegevens versleutelen die zijn opgeslagen in Azure** op of u at-rest-gegevens in de Azure-opslag wilt versleutelen. Klik vervolgens op **OK**.

    ![Beleid voor replicatie](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. Wanneer u nieuw beleid maakt, wordt dit automatisch gekoppeld aan de VMM-cloud. Klik op **OK**. U kunt aanvullende VMM-clouds (en de virtuele machines hierin) koppelen aan dit replicatiebeleid. Dit doet u via **Replicatie** > beleidsnaam > **VMM-cloud koppelen**.

    ![Beleid voor replicatie](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="capacity-planning"></a>Capaciteitsplanning

Nu u de basisinfrastructuur hebt ingesteld, kunt u gaan nadenken over de capaciteitsplanning en nagaan of u aanvullende resources nodig hebt.

Site Recovery bevat een Capacity Planner waarmee u de juiste resources kunt toewijzen aan uw bronomgeving, de Site Recovery-onderdelen, het netwerk en de opslag. U kunt de planner in de snelle modus uitvoeren voor schattingen op basis van het gemiddelde aantal virtuele machines, schijven en opslag. U kunt ook de gedetailleerde modus gebruiken. Hierbij voert u gegevens op workloadniveau in. Voordat u begint:

* Informatie verzamelen over uw replicatieomgeving, waaronder informatie over de virtuele machines, het aantal schijven per virtuele machine en de opslag per schijf.
* Een schatting maken van de dagelijkse wijzigingssnelheid (verloop) voor gerepliceerde gegevens. U kunt hiervoor de [Capacity Planner voor Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057) gebruiken.

1. Klik op **Downloaden** om dit hulpprogramma te downloaden. Voer het vervolgens uit. [Lees het artikel](site-recovery-capacity-planner.md) dat samen met het hulpprogramma wordt gedownload.
2. Wanneer u klaar bent, selecteert u **Ja** in **Hebt u de Capacity Planner uitgevoerd**?

   ![Capaciteitsplanning](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

   Meer informatie over [netwerkbandbreedte beheren](#network-bandwidth-considerations)




## <a name="enable-replication"></a>Replicatie inschakelen

Voordat u begint, controleert u of uw Azure-gebruikersaccount de vereiste [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) heeft om replicatie van een nieuwe virtuele machine naar Azure in te schakelen.

Schakel nu als volgt replicatie in:

1. Klik op **Stap 2: toepassing repliceren** > **Bron**. Wanneer u replicatie voor het eerst inschakelt, klikt u in de kluis op **+Repliceren** om replicatie in te schakelen voor aanvullende machines.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. Selecteer op de blade **Bron** de VMM-server en de cloud waarin de Hyper-V-hosts zich bevinden. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. Selecteer in **Doel** het abonnement, het implementatiemodel voor gebruik na een failover, en het opslagaccount dat u voor gerepliceerde gegevens gebruikt.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Selecteer het opslagaccount dat u wilt gebruiken. Als u een ander opslagaccount wilt gebruiken dan de accounts die u al hebt, kunt u er [een maken](#set-up-an-azure-storage-account). Als u een Premium-opslagaccount gebruikt voor gerepliceerde gegevens, moet u een extra Standard-opslagaccount selecteren om de replicatielogboeken op te slaan waarin de doorlopende wijzigingen in uw on-premises gegevens worden vastgelegd. Als u een opslagaccount wilt maken op basis van het Resource Manager-model, klikt u op **Nieuw**. Als u een opslagaccount wilt maken op basis van het klassieke model, doet u dat [in Azure Portal](../storage/common/storage-create-storage-account.md). Klik vervolgens op **OK**.
5. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren. Als u een ander netwerk wilt gebruiken dan de netwerken die u al hebt, kunt u er [een maken](#set-up-an-azure-network). Als u een opslagaccount wilt maken op basis van het Resource Manager-model, klikt u op **Nieuw**. Als u een netwerk wilt maken op basis van het klassieke model, doet u dat [in Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Selecteer indien van toepassing een subnet. Klik vervolgens op **OK**.
6. Selecteer in **Virtuele machines** > **Virtuele machines selecteren** alle machines die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication5.png)

7. Selecteer in **Eigenschappen** > **Eigenschappen configureren** het besturingssysteem voor de geselecteerde virtuele machines, evenals de schijf met het besturingssysteem.

    - Controleer of de naam van de Azure-VM (doelnaam) voldoet aan de [vereisten voor virtuele Azure-machines](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Standaard worden alle schijven van de virtuele machine geselecteerd voor replicatie, maar u kunt schijven wissen als u deze wilt uitsluiten.

        - Mogelijk wilt u schijven uitsluiten om de bandbreedte voor de replicatie te beperken. Misschien wilt u bijvoorbeeld schijven met tijdelijke gegevens of gegevens die telkens worden vernieuwd wanneer een machine of app opnieuw wordt gestart (zoals pagefile.sys of Microsoft SQL Server tempdb), van de replicatie uitsluiten. U kunt de schijf van replicatie uitsluiten door de selectie van de schijf op te heffen.
        - Alleen standaardschijven kunnen worden uitgesloten. U kunt geen besturingssysteemschijven uitsluiten.
        - We raden u aan geen dynamische schijven uit te sluiten. Met Site Recovery kan niet worden vastgesteld of een virtuele harde schijf in een gast-VM een basisschijf of een dynamische schijf is. Als niet alle afhankelijke, dynamische volumeschijven worden uitgesloten, wordt de beveiligde dynamische schijf weergegeven als een niet-werkende schijf wanneer een VM-failover optreedt. De gegevens op de schijf zijn dan niet toegankelijk.
        - Nadat de replicatie is ingeschakeld, kunt u geen schijven meer toevoegen of verwijderen voor replicatie. Als u een schijf wilt toevoegen of uitsluiten, moet u de beveiliging voor de virtuele machine uitschakelen en vervolgens weer inschakelen.
        - Voor schijven die u handmatig hebt gemaakt in Azure, wordt geen failback uitgevoerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en u er twee rechtstreeks op de virtuele Azure-machine maakt, wordt slechts voor de drie schijven waarvoor een failover is uitgevoerd, een failback uitgevoerd van Azure naar Hyper-V. U kunt schijven die handmatig zijn gemaakt niet opnemen in de failback of in de omgekeerde replicatie van Hyper-V naar Azure.
        - Als u een schijf uitsluit die nodig is voor de werking van een toepassing, moet u deze na een failover naar Azure handmatig maken in Azure, zodat de gerepliceerde toepassing kan worden uitgevoerd. U kunt Azure Automation ook integreren in een herstelplan om de schijf tijdens failover van de machine te maken.

    Klik op **OK** om de wijzigingen op te slaan. Later kunt u eventueel extra eigenschappen instellen.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

8. Selecteer in **Replicatie-instellingen** > **Replicatie-instellingen configureren** het replicatiebeleid dat u op de beveiligde virtuele machines wilt toepassen. Klik vervolgens op **OK**. U kunt het replicatiebeleid wijzigen via **Replicatiebeleid** > beleidsnaam > **Instellingen bewerken**. De wijzigingen die u toepast, worden zowel gebruikt voor machines die al worden gerepliceerd, als voor nieuwe machines.

   ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication7.png)

U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.

### <a name="view-and-manage-vm-properties"></a>Eigenschappen van virtuele machines weergeven en beheren

Het is raadzaam om de eigenschappen van de bronmachine te controleren. Houd er rekening mee dat de naam van de virtuele Azure-machine moet voldoen aan de [vereisten voor virtuele machines van Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. Klik in **Beveiligde items** op **Gerepliceerde items** en selecteer een machine om de details ervan te bekijken.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. In **Eigenschappen** kunt u de replicatie- en failoverinformatie van de virtuele machine weergeven.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. In **Berekening en netwerk** > **Eigenschappen berekenen** kunt u de naam van de virtuele Azure-machine opgeven, evenals de doelgrootte. Wijzig indien nodig de naam om te voldoen aan de [Azure-vereisten](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). U kunt ook informatie bekijken en bewerken over het doelnetwerk, het subnet en het IP-adres dat aan de virtuele Azure-machine wordt toegewezen.
Opmerking:

   * U kunt het doel-IP-adres instellen. Als u geen adres opgeeft, maakt de machine waarvoor een failover is uitgevoerd, gebruik van DHCP. Als u een adres instelt dat tijdens het uitvoeren van de failover niet beschikbaar is, mislukt de failover. Hetzelfde doel-IP-adres kan worden gebruikt voor een testfailover als het adres beschikbaar is in het testfailovernetwerk.
   * Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele doelmachine opgeeft. Dat werkt als volgt:

     * Als het aantal netwerkadapters op de bronmachine kleiner is dan of gelijk is aan het aantal adapters dat is toegestaan voor de grootte van de doelmachine, heeft het doel hetzelfde aantal adapters als de bron.
     * Als het aantal adapters op de virtuele bronmachine groter is dan voor de doelgrootte is toegestaan, wordt het maximum voor de doelgrootte gebruikt.
     * Als de bronmachine bijvoorbeeld twee netwerkadapters heeft en de grootte van de doelmachine vier netwerkadapters ondersteunt, heeft de doelmachine twee adapters. Als de bronmachine twee adapters heeft, maar de grootte van de doelmachine slechts één adapter ondersteunt, heeft de doelmachine één adapter.     
     * Als de virtuele machine meerdere netwerkadapters heeft, worden deze allemaal verbonden met hetzelfde netwerk.

     ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/test-failover4.png)

4. In **Schijven** ziet u het besturingssysteem en de gegevensschijven van de virtuele machine die wordt gerepliceerd.

#### <a name="managed-disks"></a>Managed Disks

In **Berekening en netwerk** > **Eigenschappen berekenen** kunt u de instelling Beheerde schijven gebruiken instellen op Ja voor de virtuele machine als u beheerde schijven aan uw machine wilt koppelen voor migratie naar Azure. Met beheerde schijven vereenvoudigt u schijfbeheer voor Azure IaaS-VM's door de opslagaccounts te beheren die aan de VM-schijven zijn gekoppeld. [Meer informatie over beheerde schijven](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Beheerde schijven worden alleen gemaakt en aan de virtuele machine gekoppeld bij een failover naar Azure. Wanneer de beveiliging wordt ingeschakeld, worden gegevens van de on-premises machines continu gerepliceerd naar de opslagaccounts.
   Beheerde schijven kunnen alleen worden gemaakt voor virtuele machines die zijn geïmplementeerd met het implementatiemodel van Resource Manager.  

  > [!NOTE]
  > Failback vanuit Azure naar een on-premises Hyper-V-omgeving wordt momenteel niet ondersteund voor machines met beheerde-schijven. Stel Beheerde schijven gebruiken alleen in op Ja als u deze machine wilt migreren naar Azure.

   - Als u Beheerde schijven gebruiken instelt op Ja, zijn alleen beschikbaarheidssets in de resourcegroep waarbij Beheerde schijven gebruiken is ingesteld op Ja, beschikbaar voor selectie. Virtuele machines met beheerde schijven kunnen namelijk alleen deel uitmaken van beschikbaarheidssets waarvoor de eigenschap Beheerde schijven gebruiken is ingesteld op Ja. Zorg ervoor dat u beschikbaarheidssets maakt waarbij de eigenschap Beheerde schijven gebruiken is ingesteld op basis van uw gebruik van beheerde schijven voor failover.  Als u Beheerde schijven gebruiken instelt op Nee, zijn alleen beschikbaarheidssets in de resourcegroep waarbij de eigenschap Beheerde schijven gebruiken is ingesteld op Nee, beschikbaar voor selectie. [Meer informatie over beheerde schijven en beschikbaarheidssets](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Als het opslagaccount dat is gebruikt voor replicatie, ooit is versleuteld met Versleuteling voor opslagservice, mislukt het maken van beheerde schijven tijdens failover. U kunt Beheerde schijven gebruiken instellen op Nee en de failover opnieuw uitvoeren of de beveiliging voor de virtuele machine uitschakelen en deze beveiligen via een opslagaccount waarvoor Versleuteling voor opslagservice niet eerder is ingeschakeld.
  > [Meer informatie over Versleuteling voor opslagservice en beheerde schijven](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-the-deployment"></a>De implementatie testen

Als u de implementatie wilt testen, kunt u een testfailover uitvoeren voor één virtuele machine, maar ook een herstelplan uitvoeren dat een of meer virtuele machines bevat.

### <a name="before-you-start"></a>Voordat u begint

 - Zie [verbinding voorbereiden](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) als u na een failover met RDP verbinding wilt maken met Azure VM's.
 - Als u een volledige test wilt uitvoeren, moet u Active Directory en DNS naar uw testomgeving kopiëren. [Meer informatie](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1. Als u een failover wilt uitvoeren voor één virtuele machine, gaat u naar **Gerepliceerde items**. Klik vervolgens op de virtuele machine > **+Testfailover**.
2. Als u een failover wilt uitvoeren op basis van een herstelplan, klikt u in **Herstelplannen** met de rechtermuisknop op het plan > **Testfailover**. Volg [deze instructies](site-recovery-create-recovery-plans.md) om een herstelplan te maken.
3. Selecteer in **Failover testen** het Azure-netwerk waarmee de virtuele Azure-machines moeten worden verbonden nadat er een failover is uitgevoerd.
4. Klik op **OK** om te beginnen met de failover. U kunt de voortgang volgen door op de virtuele machine te klikken en de eigenschappen hiervan te openen, of door in **Site Recovery-taken** op de taak **Testfailover** te klikken.
5. Wanneer de failover is voltooid, moet u de gerepliceerde Azure-machine ook kunnen zien in Azure Portal > **Virtuele machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en of deze actief is.
6. Als u verbindingen hebt voorbereid na een failover, kunt u verbinding maken met de Azure-VM.
7. Als u klaar bent, klikt u op **Testfailover opschonen** op het herstelplan. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op. Hiermee verwijdert u de virtuele machines die tijdens de testfailover zijn gemaakt.

Lees het artikel [Testfailover naar Azure](site-recovery-test-failover-to-azure.md) voor meer informatie.

## <a name="monitor-the-deployment"></a>De implementatie bewaken

Ga als volgt te werk om de configuratie-instellingen en de status van de Site Recovery-implementatie te bewaken:

1. Klik op de kluisnaam om het dashboard **Essentials** te openen. Op dit dashboard ziet u een overzicht van de Site Recovery-taken, de replicatiestatus, de herstelplannen, de serverstatus en de gebeurtenissen.  U kunt **Essentials** zo aanpassen dat de tegels en indelingen worden weergegeven die voor u het belangrijkst zijn. Dit heeft ook betrekking op de status van andere Site Recovery- en Backup-kluizen.

    ![Essentials](./media/site-recovery-vmm-to-azure/essentials.png)
2. In **Status** kunt u problemen op on-premises servers (VMM- of configuratieservers) bewaken. Bovendien kunt u hier zien welke gebeurtenissen er de afgelopen 24 uur door Site Recovery zijn gemeld.
3. U kunt replicatie beheren en bewaken via de tegels **Gerepliceerde items**, **Herstelplannen** en **Site Recovery-taken**. U kunt inzoomen op taken via **Taken** > **Site Recovery-taken**.

## <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Installatie vanaf de opdrachtregel voor de Azure Site Recovery-provider

De Azure Site Recovery-provider kan worden geïnstalleerd vanaf de opdrachtregel. Deze methode kan worden gebruikt om de provider in Server Core voor Windows Server 2012 R2 te installeren.

1. Download het providerinstallatiebestand en de registratiesleutel naar een map. Bijvoorbeeld: C:\ASR.
2. Pak het providerinstallatieprogramma vanaf een opdrachtprompt met verhoogde bevoegdheden uit met de volgende opdrachten:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Voer deze opdracht uit om de onderdelen te installeren:

            C:\ASR> setupdr.exe /i
4. Voer vervolgens deze opdrachten uit om de server in de kluis te registreren:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Waar:

* **/Credentials**: een verplichte parameter waarmee de locatie wordt opgegeven waarop het registratiesleutelbestand zich bevindt.  
* **/FriendlyName**: een verplichte parameter voor de naam van de Hyper-V-hostserver die wordt weergegeven in de Azure Site Recovery-portal.
* * **/EncryptionEnabled**: een optionele parameter voor wanneer u virtuele Hyper-V-machines in VMM-clouds repliceert naar Azure. Geef op of u virtuele machines in Azure wilt versleutelen (versleuteling voor data-at-rest). Controleer of de naam van het bestand de extensie **.pfx** heeft. Versleuteling is standaard uitgeschakeld.

    > [!NOTE]
    > Het is raadzaam de versleutelingsfunctie die wordt geleverd door Azure te gebruiken voor het versleutelen van data-at-rest in plaats van de versleutelingsoptie (de optie EncryptionEnabled) die door Azure Site Recovery wordt geboden. De versleutelingsfunctie die wordt verstrekt door Azure kan worden ingeschakeld voor een opslagaccount en zorgt voor betere prestaties omdat de versleuteling/ontsleuteling wordt uitgevoerd door Azure  
    > Storage.
    > [Meer informatie over de Storage-versleutelingsservice in Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
* **/proxyAddress**: een optionele parameter waarmee het adres van de proxyserver wordt opgegeven.
* **/proxyport**: een optionele parameter waarmee de poort van de proxyserver wordt opgegeven.
* **/proxyUsername**: een optionele parameter waarmee de proxygebruikersnaam wordt opgegeven (als er voor de proxyserver verificatie is vereist).
* **/proxyPassword**: een optionele parameter waarmee het wachtwoord wordt opgegeven voor verificatie bij de proxyserver (als er voor de proxyserver verificatie is vereist).


### <a name="network-bandwidth-considerations"></a>Overwegingen voor netwerkbandbreedte
U kunt het hulpprogramma Capacity Planner gebruiken om de bandbreedte te berekenen die u nodig hebt voor replicatie (initiële replicatie en deltareplicaties). U hebt een aantal opties voor het beheren van de hoeveelheid bandbreedte die voor replicatie wordt gebruikt:

* **Bandbreedte beperken**: Hyper-V-verkeer dat wordt gerepliceerd naar een secundaire site, verloopt via een specifieke Hyper-V-host. U kunt de bandbreedte op de hostserver beperken.
* **Bandbreedte aanpassen**: u kunt zelf bepalen hoeveel bandbreedte er voor replicatie moet worden gebruikt; dit doet u via diverse registersleutels.

#### <a name="throttle-bandwidth"></a>Bandbreedte beperken
1. Open de Microsoft Azure Backup MMC-module op de Hyper-V-hostserver. Standaard is er een snelkoppeling naar Microsoft Azure Backup beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Klik in de module op **Eigenschappen wijzigen**.
3. Selecteer op het tabblad **Beperking** de optie **Internetbandbreedtebeperking inschakelen voor back-upbewerkingen** en stel de limieten in voor werktijden en voor tijden waarop niet wordt gewerkt. Het geldige bereik ligt tussen 512 Kbps en 102 Mbps.

    ![Bandbreedte beperken](./media/site-recovery-vmm-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) gebruiken om een beperking in te stellen. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** geeft aan dat er geen beperking is vereist.

#### <a name="influence-network-bandwidth"></a>Netwerkbandbreedte beïnvloeden
Met de registerwaarde **UploadThreadsPerVM** wordt aangegeven hoeveel threads er moeten worden gebruikt voor gegevensoverdracht (initiële replicatie of deltareplicatie) van een schijf. Hoe hoger de waarde, hoe meer netwerkbandbreedte er voor replicatie wordt gebruikt. Met de registerwaarde **DownloadThreadsPerVM** geeft u aan hoeveel threads er worden gebruikt voor gegevensoverdracht tijdens het uitvoeren van een failback.

1. Navigeer in het register naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

   * Wijzig de waarde **UploadThreadsPerVM** (of maak de sleutel aan als deze niet bestaat) om aan te geven hoeveel threads er voor schijfreplicatie moeten worden gebruikt.
   * Wijzig de waarde **DownloadThreadsPerVM** (of maak de sleutel aan als deze niet bestaat) om aan te geven hoeveel threads er voor failbackverkeer vanuit Azure moeten worden gebruikt.
2. De standaardwaarde is 4. In netwerken met overprovisioning moeten deze registersleutels zodanig worden gewijzigd dat niet de standaardwaarden worden gebruikt. Het maximum is 32. Houd het verkeer in de gaten om de waarde te optimaliseren.

## <a name="next-steps"></a>Volgende stappen

Wanneer de initiële replicatie is voltooid en u de implementatie hebt getest, kunt u failovers aanroepen, als dit nodig is. [Meer informatie](site-recovery-failover.md) over verschillende typen failovers en het uitvoeren hiervan.
