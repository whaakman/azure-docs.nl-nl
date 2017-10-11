---
title: Hyper-V-machines repliceren naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u Hyper-V-machines in VMM-clouds repliceren naar een secundaire VMM-site met de Azure portal.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: b33a1922-aed6-4916-9209-0e257620fded
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 777bddea6b1cb325a6f8ede00196b18e1746d80c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Hyper-V virtuele machines in VMM-clouds repliceren naar een secundaire VMM-site met de Azure portal
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klassieke portal](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell - Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Dit artikel wordt beschreven hoe repliceren lokale Hyper-V virtuele machines worden beheerd in System Center Virtual Machine Manager (VMM)-clouds, naar een secundaire site met [Azure Site Recovery](site-recovery-overview.md) in de Azure portal. Meer informatie over deze [scenarioarchitectuur](site-recovery-components.md).

Na het lezen van dit artikel kunt u onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.


## <a name="prerequisites"></a>Vereisten

**Vereiste** | **Details**
--- | ---
**Azure** | U hebt een [Microsoft Azure](http://azure.microsoft.com/)-account nodig. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over prijzen voor Site Recovery.
**On-premises VMM** | U hebt twee VMM-servers, één in de primaire site en een van de secundaire wordt aangeraden.<br/><br/> U kunt repliceren tussen clouds op één VMM-server.<br/><br/> VMM-servers waarop ten minste System Center 2012 SP1 met de nieuwste updates.<br/><br/> VMM-servers moeten toegang tot internet.
**VMM-clouds** | Elke VMM-server moet op een of meer clouds, en alle clouds moeten het Hyper-V-Capaciteitsprofiel ingesteld hebben. <br/><br/>Clouds moeten een of meer VMM-hostgroepen bevatten.<br/><br/> Als u slechts één VMM-server hebt, moet deze ten minste twee clouds, om te fungeren als primair en secundair.
**Hyper-V** | Hyper-V-servers moeten ten minste draaien op Windows Server 2012 met de Hyper-V-functie en de meest recente updates hebt geïnstalleerd.<br/><br/> Een Hyper-V-server moet een of meer virtuele machines bevatten.<br/><br/>  Hyper-V-hostservers moeten zich in hostgroepen in de primaire en secundaire VMM-clouds.<br/><br/> Als u op Windows Server 2012 R2 Hyper-V in een cluster uitvoert, installeert u [2961977 bijwerken](https://support.microsoft.com/kb/2961977)<br/><br/> Als u op Windows Server 2012 Hyper-V in een cluster uitvoert, wordt niet clusterbroker automatisch als u een statisch IP-adressen gebaseerde cluster hebt gemaakt. De clusterbroker handmatig configureren. [Lees meer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).<br/><br/> Hyper-V-servers moeten toegang tot internet.
**URL 's** | VMM-servers en Hyper-V-hosts moeten kunnen bereiken deze URL's zijn:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

## <a name="deployment-steps"></a>Implementatiestappen

Dit is wat u doen:

1. Controleer of vereisten.
2. Bereid de VMM-server en Hyper-V-hosts.
3. Maak een Recovery Services-kluis. De kluis bevat configuratie-instellingen en met de kluis wordt de replicatie ingedeeld.
4. Bron, doel en replicatie-instellingen opgeven.
5. Implementatie van de Mobility-service op virtuele machines die u wilt repliceren.
6. Voorbereiden voor replicatie en replicatie inschakelen voor Hyper-V-machines.
7. Voer een testfailover uit om te controleren of alles goed werkt.

## <a name="prepare-vmm-servers-and-hyper-v-hosts"></a>VMM-servers en Hyper-V-hosts voorbereiden

Voorbereiden voor implementatie:

1. Zorg ervoor dat de VMM-server en Hyper-V-hosts voldoen aan de vereisten die hierboven worden beschreven, en de vereiste URL's kunnen bereiken.
2. Instellen van VMM-netwerken, zodat u kunt configureren [netwerktoewijzing](#network-mapping-overview).

    - Zorg ervoor dat de virtuele machines op de Hyper-V-bronhostserver zijn verbonden met een VMM-netwerk met virtuele machines. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is verbonden met de cloud.
    Controleer of de secundaire cloud die u voor herstel gebruikt heeft een bijbehorende VM-netwerk is geconfigureerd. Deze VM-netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de secundaire cloud.

3. Voorbereiden voor een [single server-implementatie](#single-vmm-server-deployment), als u wilt repliceren van virtuele machines tussen clouds op dezelfde VMM-server.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw** > **Beheer** > **Recovery Services**.
3. Geef in **Naam** een beschrijvende naam op om de kluis mee aan te duiden. Als u meer dan één abonnement hebt, selecteert u een van uw abonnementen.
4. [Maak een resourcegroep](../azure-resource-manager/resource-group-template-deploy-portal.md) of selecteer een bestaande resourcegroep. Geef een Azure-regio op. Machines worden gerepliceerd naar deze regio. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund
5. Als u de kluis snel wilt openen via het dashboard, klikt u op **Vastmaken aan dashboard** > **Kluis maken**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op de **Dashboard**in **alle resources**, en klik op de hoofdblade **Recovery Services-kluizen** blade.


## <a name="choose-a-protection-goal"></a>Kies een beveiligingsdoel

Selecteer wat u wilt repliceren en waar u naar wilt repliceren.

2. Klik op **siteherstel** > **stap 1: infrastructuur voorbereiden** > **beveiligingsdoel**.
3. Selecteer **met site recovery**, en selecteer **Ja, met Hyper-V**.
4. Selecteer **Ja** om aan te geven u VMM voor het beheren van Hyper-V-hosts.
5. Selecteer **Ja** hebt u een secundaire VMM-server. Als u replicatie tussen clouds op één VMM-server implementeert, klikt u op **Nee**. Klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/site-recovery-vmm-to-vmm/choose-goals.png)

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

De Azure Site Recovery Provider installeren op VMM-servers en detecteren en servers in de kluis registreren.

1. Klik op **stap 1: infrastructuur voorbereiden** > **bron**.

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/goals-source.png)
2. Klik in **Bron voorbereiden** op **+ VMM** om een VMM-server toe te voegen.

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/set-source1.png)
3. In **Server toevoegen**, controleert u of **System Center VMM-server** wordt weergegeven in **servertype** en dat de VMM-server voldoet aan de [vereisten](#prerequisites).
4. Download het installatieprogramma voor de Azure Site Recovery-provider.
5. Download de registratiesleutel. U hebt deze nodig wanneer u de installatie uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/set-source3.png)
6. Installeer de Azure Site Recovery-provider op de VMM-server. U hoeft niet expliciet niets te installeren op Hyper-V-hostservers.


### <a name="install-the-azure-site-recovery-provider"></a>De Azure Site Recovery Provider installeren

1. Voer de Provider setup-bestand op elke VMM-server. Als VMM is geïmplementeerd in een cluster, als volgt de eerste keer dat u installeert:
    -  De provider installeren op een actief knooppunt en voltooi de installatie om de VMM-server in de kluis registreren.
    - Vervolgens installeert u de Provider op de andere knooppunten. Clusterknooppunten moeten allemaal dezelfde versie van de Provider uitvoeren.
2. Setup enkele vereiste controles uitgevoerd en vraagt toestemming om de VMM-service te stoppen. De VMM-service wordt opnieuw opgestart nadat setup is voltooid. Als u op een VMM-cluster installeert, wordt u gevraagd om te stoppen van de Cluster-rol.
3. In **Microsoft Update**, u kunt ervoor kiezen om op te geven dat de provider-updates zijn geïnstalleerd in overeenstemming met uw Microsoft Update-beleid.
4. In **installatie**, accepteer of wijzig de standaardlocatie voor installatie, en klikt u op **installeren**.

    ![Installatielocatie](./media/site-recovery-vmm-to-vmm/provider-location.png)
5. Nadat de installatie is voltooid, klikt u op **registreren** de server in de kluis registreren.

    ![Installatielocatie](./media/site-recovery-vmm-to-vmm/provider-register.png)
6. Controleer in **Kluisnaam** de naam van de kluis waarin de server wordt geregistreerd. Klik op *Volgende*.

    ![Serverregistratie](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)
7. In **internetverbinding**, opgeven hoe de provider die wordt uitgevoerd op de VMM-server verbinding maakt met Azure.

    ![Instellingen voor internet](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

   - U kunt opgeven dat de provider verbinding met het internet rechtstreeks of via een proxyserver maken moet.
   - Proxy-instellingen opgeven, indien nodig.
   - Als u een proxy gebruikt, wordt een VMM RunAs-account (DRAProxyAccount) automatisch gemaakt met de opgegeven proxyreferenties. Configureer de proxyserver zodanig dat dit account kan worden geverifieerd. De RunAs-Accountinstellingen kunnen worden gewijzigd in de VMM-console > **instellingen** > **beveiliging** > **Run As-Accounts**. Start de VMM-service voor het bijwerken van wijzigingen.
8. Selecteer in **Registratiesleutel** de sleutel die u vanuit Azure Site Recovery hebt gedownload en naar de VMM-server hebt gekopieerd.
9. De versleutelingsinstelling wordt alleen gebruikt wanneer u Hyper-V virtuele machines in VMM-clouds naar Azure repliceert. Als u naar een secundaire site repliceren, wordt de instelling niet gebruikt.
10. Geef in **Servernaam** een beschrijvende naam op om de VMM-server in de kluis te identificeren. Geef in een clusterconfiguratie de VMM-clusterrolnaam op.
11. In **cloudmetagegevens synchroniseren**Selecteer of u metagegevens wilt synchroniseren voor alle clouds op de VMM-server met de kluis. Deze actie hoeft op elke server slechts één keer te worden uitgevoerd. Als u niet dat alle clouds synchroniseren wilt, kunt u laat u deze instelling uitgeschakeld en synchroniseert u elke cloud afzonderlijk in de cloudeigenschappen in de VMM-console.
12. Klik op **Volgende** om het proces te voltooien. Na de registratie worden door Azure Site Recovery metagegevens van de VMM-server opgehaald. De server wordt weergegeven op het tabblad **VMM-servers** op de pagina **Servers** in de kluis.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)
13. Nadat de server beschikbaar in de Site Recovery-console is in **bron** > **bron voorbereiden** selecteert u de VMM-server en selecteer de cloud waarin de Hyper-V-host bevindt. Klik vervolgens op **OK**.

U kunt ook de provider installeren vanaf de opdrachtregel:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer de doel-VMM-server en de cloud.

1. Klik op **infrastructuur voorbereiden** > **doel**, en selecteer de doel-VMM-server die u wilt gebruiken.
2. Clouds die zijn gesynchroniseerd met Site Recovery op de server wordt weergegeven. Selecteer de doelcloud.

   ![doel](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="set-up-replication-settings"></a>Replicatie-instellingen instellen

- Wanneer u een beleid voor wachtwoordreplicatie maakt, moet alle host met behulp van het beleid hetzelfde besturingssysteem hebben. De VMM-cloud Hyper-V-hosts met verschillende versies van Windows Server kan bevatten, maar in dit geval moet u meerdere beleidsregels voor replicatie.
- U kunt de initiële replicatie offline uitvoeren. [Meer informatie](#prepare-for-initial-offline-replication)

1. Als u nieuw replicatiebeleid wilt maken, klikt u op **Infrastructuur voorbereiden** > **Replicatie-instellingen** > **+Maken en koppelen**.

    ![Netwerk](./media/site-recovery-vmm-to-vmm/gs-replication.png)
2. Geef in **Beleid maken en koppelen** een beleidsnaam op. Het type bron en doel moet **Hyper-V**.
3. In **versie van Hyper-V-host**, selecteer welk besturingssysteem wordt uitgevoerd op de host.
4. In **verificatietype** en **verificatiepoort**, opgeven hoe verkeer tussen de primaire en herstelsites Hyper-V-hostservers is geverifieerd. Selecteer **certificaat** tenzij u beschikken over een werkende Kerberos-omgeving. Azure Site Recovery configureert automatisch certificaten voor HTTPS-verificatie. U hoeft niet verder niets te doen handmatig. Standaard wordt poort 8083 en 8084 (voor certificaten) in de Windows Firewall op de Hyper-V-hostservers worden geopend. Als u selecteert **Kerberos**, een Kerberos-ticket wordt gebruikt voor wederzijdse verificatie van de host-servers. Houd er rekening mee dat deze instelling is alleen relevant voor Hyper-V-host-servers waarop Windows Server 2012 R2.
5. Geef in **Kopieerfrequentie** op hoe vaak u na de eerste replicatie de verschillen wilt repliceren (elke 30 seconden of elke 5 of 15 minuten).
6. Geef in **Bewaarperiode van het herstelpunt** op hoeveel uur elk herstelpunt moet worden bewaard. Beveiligde machines kunnen te allen tijde worden hersteld naar een willekeurig punt (binnen een bepaald tijdsvenster).
7. In **App-consistente momentopname frequentie**, opgeven hoe vaak (1-12 uur) herstelpunten met toepassingsconsistente momentopnamen worden gemaakt. Hyper-V maakt gebruik van twee soorten momentopnamen: standaardmomentopnamen waarmee steeds incrementele momentopnamen van de volledige virtuele machine worden gemaakt, en toepassingsconsistente momentopnamen waarmee een tijdsgebonden momentopname wordt gemaakt van de toepassingsgegevens in de virtuele machine. Toepassingsconsistente momentopnamen gebruiken Volume Shadow Copy Service (VSS) om ervoor te zorgen dat toepassingen een consistente status hebben wanneer er een momentopname wordt gemaakt. Als u toepassingsconsistente momentopnamen inschakelt, wordt dit invloed heeft op de prestaties van toepassingen die worden uitgevoerd op virtuele machines die bron. Zorg ervoor dat de waarde die u instelt, kleiner is dan het aantal aanvullende herstelpunten dat u configureert.
8. In **gegevensoverdracht compressie**, opgeven of de gerepliceerde gegevens die worden overgedragen moeten worden gecomprimeerd.
9. Selecteer **replica verwijderen VM**, om op te geven dat de replica virtuele machine moet worden verwijderd als u beveiliging voor de bron-VM uitschakelt. Als u deze instelling inschakelt wanneer u beveiliging voor de bron-VM wordt deze verwijderd uit de Site Recovery-console uitschakelt, Site Recovery-instellingen voor VMM worden verwijderd uit de VMM-console en de replica is verwijderd.
10. In **initiële replicatiemethode**, als u via het netwerk repliceert, geef aan of de initiële replicatie start of plannen. Mogelijk wilt u plannen buiten de drukste tijden voor het opslaan van de netwerkbandbreedte. Klik vervolgens op **OK**.

     ![Beleid voor replicatie](./media/site-recovery-vmm-to-vmm/gs-replication2.png)
11. Wanneer u nieuw beleid maakt, wordt dit automatisch gekoppeld aan de VMM-cloud. In **replicatiebeleid**, klikt u op **OK**. U kunt aanvullende VMM-Clouds (en de virtuele machines erin) koppelen aan dit replicatiebeleid in **replicatie** > beleidsnaam > **VMM-Cloud koppelen**.

     ![Beleid voor replicatie](./media/site-recovery-vmm-to-vmm/policy-associate.png)


### <a name="configure-network-mapping"></a>Netwerktoewijzing configureren

- Meer informatie over [netwerktoewijzing](#prepare-for-network-mapping) voordat u begint.
- Controleren of de virtuele machines op de VMM-servers zijn verbonden met een VM-netwerk.


1. In **Site Recovery-infrastructuur** > **netwerktoewijzing** > **Netwerktoewijzingen**, klikt u op **+ netwerktoewijzing**.

    ![Netwerktoewijzing](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. In **netwerktoewijzing toevoegen** tabblad, selecteert u de bron en doel van de VMM-servers. De VM-netwerken die zijn gekoppeld aan de VMM-servers worden opgehaald.
3. In **Bronnetwerk**, selecteer het netwerk die u wilt gebruiken in de lijst met VM-netwerken die zijn gekoppeld aan de primaire VMM-server.
4. In **doelnetwerk**, selecteer het netwerk die u wilt gebruiken op de secundaire VMM-server. Klik vervolgens op **OK**.

    ![Netwerktoewijzing](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Dit is wat er gebeurt wanneer er netwerktoewijzing wordt uitgevoerd:

* Alle bestaande gerepliceerde virtuele machines die met de bron-VM-netwerk overeenkomen wordt worden verbonden met het VM-netwerk van het doel.
* Nieuwe virtuele machines die zijn verbonden met de bron-VM-netwerk, wordt na de replicatie worden verbonden met het doelnetwerk toegewezen.
* Als u een bestaande toewijzing wijzigt met een nieuw netwerk, worden gerepliceerde virtuele machines verbonden met de nieuwe instellingen.
* Als het doelnetwerk meerdere subnetten bevat en een van deze subnetten dezelfde naam heeft als het subnet waarin de virtuele bronmachine zich bevindt, wordt de gerepliceerde virtuele machine na een failover verbonden met dat doelsubnet. Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.

### <a name="configure-storage-mapping"></a>Toewijzing van opslag configureren.

[Toewijzing van opslag](#prepare-for-storage-mapping) wordt niet ondersteund in de nieuwe Azure portal. Echter, kan worden ingeschakeld met Powershell. [Meer informatie](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-7-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Stap 5: capaciteitsplanning

Nu u de basisinfrastructuur hebt ingesteld, kunt u gaan nadenken over de capaciteitsplanning en nagaan of u aanvullende resources nodig hebt.

- Downloaden en uitvoeren van de [Azure Site Recovery-Capaciteitsplanner](site-recovery-capacity-planner.md), voor het verzamelen van informatie over uw replicatieomgeving, met inbegrip van virtuele machines, schijven per virtuele machine en opslag per schijf.
- Als u realtime replicatie-informatie hebt verzameld, kunt u het beleid NetQos voor het beheren van de bandbreedte van de replicatie voor virtuele machines wijzigen. Lees meer over [beperking Hyper-V Replica verkeer](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/), op de blog van Thomas Maurer. Vindt u meer informatie over de [cmdlet New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx.).

## <a name="enable-replication"></a>Replicatie inschakelen

1. Klik op **Stap 2: toepassing repliceren** > **Bron**. Nadat u replicatie voor het eerst hebt ingeschakeld, u klikt u op **+ repliceren** in de kluis aanvullende machines replicatie in te schakelen.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication1.png)
2. In **bron**, selecteert u de VMM-server en de cloud waarin u wilt repliceren Hyper-V-hosts zich bevinden. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication2.png)
3. In **doel**, controleert u de secundaire VMM-server en de cloud.
4. In **virtuele machines**, selecteert u de virtuele machines die u wilt beveiligen in de lijst.

    ![Beveiliging van de virtuele machine inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

U kunt de voortgang van volgen de **beveiliging inschakelen** actie in **taken** > **Site Recovery-taken**. Na de **beveiliging voltooien** taak is voltooid, de virtuele machine is gereed voor failover.

Opmerking:

- U kunt ook beveiliging voor virtuele machines in de VMM-console inschakelen. Klik op **beveiliging inschakelen** op de werkbalk van de eigenschappen van de virtuele machine > **Azure Site Recovery** tabblad.
- Nadat u replicatie hebt ingeschakeld, kunt u de eigenschappen weergeven voor de virtuele machine in **gerepliceerde Items**. Op de **Essentials** dashboard ziet u informatie over het replicatiebeleid voor de virtuele machine en de status ervan. Klik op **eigenschappen** voor meer informatie.

### <a name="onboard-existing-virtual-machines"></a>Ingebouwde bestaande virtuele machines
Als u een bestaande virtuele machines in VMM die worden gerepliceerd met Hyper-V Replica hebt, kunt u vrijgeven ze voor Azure Site Recovery replicatie als volgt:

1. Zorg ervoor dat de Hyper-V-server die als host fungeert voor de bestaande virtuele machine zich bevinden in de primaire cloud en dat de Hyper-V-server die als host fungeert voor de replica virtuele machine bevindt zich in de secundaire cloud.
2. Zorg ervoor dat een beleid voor wachtwoordreplicatie is geconfigureerd voor de primaire VMM-cloud.
3. Replicatie inschakelen voor de primaire virtuele machine. Azure Site Recovery en VMM Zorg ervoor dat de dezelfde replicatiehost en de virtuele machine wordt gedetecteerd en Azure Site Recovery wordt opnieuw gebruikt en herstellen van replicatie met de opgegeven instellingen.

## <a name="test-your-deployment"></a>Uw implementatie testen

Als u wilt testen van uw implementatie, kunt u uitvoeren een [testfailover](site-recovery-test-failover-vmm-to-vmm.md) voor een enkele virtuele machine of [een herstelplan maken](site-recovery-create-recovery-plans.md) die een of meer virtuele machines bevat.



## <a name="prepare-for-offline-initial-replication"></a>Voorbereiden voor de offline initiële replicatie

U kunt doen offline replicatie voor de initiële gegevens opnieuw te kopiëren. U kunt dit als volgt voorbereiden:

* Op de bronserver geeft u een padlocatie van waaruit het exporteren van gegevens wordt gehouden. Volledig beheer toewijzen om machtigingen voor NTFS en deelmachtigingen voor de VMM-service op het pad voor exporteren. Op de doelserver geeft u een locatie van waaruit het importeren van gegevens wordt uitgevoerd. Dezelfde machtigingen op deze Importpad toewijzen.
* Als het pad importeren of exporteren wordt gedeeld, wijst u beheerder, Hoofdgebruikers, Print Operators of Server Operator lidmaatschap voor de VMM-serviceaccount op de externe computer waarop de gedeelde zich bevindt.
* Als u een Run As-accounts toevoegen hosts op de import- en exportpaden lezen toewijzen en schrijfmachtigingen voor het Run As-accounts in VMM.
* De shares importeren en exporteren zich niet bevinden op elke computer die wordt gebruikt als een Hyper-V-hostserver omdat loopback-configuratie wordt niet ondersteund door Hyper-V.
* In Active Directory op elke Hyper-V-hostserver die virtuele machines die u wilt beveiligen bevat, inschakelen en configureren-beperkte overdracht te vertrouwen van de externe computers waarop de import- en exportpaden, als volgt bevinden:
  1. Open op de domeincontroller **Active Directory: gebruikers en Computers**.
  2. Klik in de consolestructuur op **DomainName** > **Computers**.
  3. Met de rechtermuisknop op de naam van de Hyper-V-hostserver > **eigenschappen**.
  4. Op de **delegering** tabblad **deze computer mag alleen aan opgegeven services delegeren**.
  5. Klik op **elk verificatieprotocol voor gebruiken**.
  6. Klik op **toevoegen** > **gebruikers en Computers**.
  7. Typ de naam van de computer die als host fungeert voor het exportpad > **OK**. In de lijst met beschikbare services de CTRL-toets ingedrukt en klik op **cifs** > **OK**. Herhaal dit voor de naam van de computer die als host fungeert voor de import-pad. Herhaal zo nodig voor extra Hyper-V-hostservers.



## <a name="prepare-for-network-mapping"></a>Voorbereiden op netwerktoewijzing
Koppelingen van Netwerktoewijzingen tussen VMM VM-netwerken op de primaire en secundaire VMM-servers aan:

* Replica-VM optimaal op secundaire Hyper-V-hosts plaatsen na een failover.
* Verbinding met het maken van replica-VM met de juiste VM-netwerken na een failover.

Opmerking:
- Netwerktoewijzing kan worden geconfigureerd tussen VM-netwerken op twee VMM-servers of op één VMM-server als de twee sites op dezelfde server worden beheerd.
- Wanneer toewijzing correct is geconfigureerd replicatie is ingeschakeld, wordt een virtuele machine op de primaire locatie worden verbonden met een netwerk en de replica op de doellocatie wordt verbonden met de toegewezen netwerkpad.
- Als netwerken hebt wanneer u een VM-netwerk van het doel bij netwerktoewijzing selecteert in VMM correct is ingesteld, wordt de VMM-bron-clouds die gebruikmaken van de bron-VM-netwerk weergegeven, samen met de beschikbare doelservers VM-netwerken op de doel-clouds die worden gebruikt voor beveiliging .
- Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten dezelfde naam heeft als het subnet waarop de virtuele bronmachine zich bevindt, klikt u vervolgens de replica virtuele machine verbonden met dat Doelsubnet na een failover. Als er geen doelsubnet met een overeenkomende naam bestaat, wordt de virtuele machine verbonden met het eerste subnet in het netwerk.


Hier volgt een voorbeeld ter illustratie van dit mechanisme. U gaat nu een organisatie met twee locaties in New York en Chicago.

| **Locatie** | **VMM-server** | **VM-netwerken** | **Toegewezen aan** |
| --- | --- | --- | --- |
| New York |VMM-NewYork |VMNetwork1 NewYork |Toegewezen aan VMNetwork1 Chicago |
| VMNetwork2 NewYork |Niet toegewezen | | |
| Chicago |VMM-Chicago |VMNetwork1 Chicago |Toegewezen aan VMNetwork1 NewYork |
| VMNetwork2 Chicago |Niet toegewezen | | |

Met dit voorbeeld:

* Wanneer een replica virtuele machine is gemaakt voor elke virtuele machine die is verbonden met VMNetwork1 NewYork, wordt het verbonden zijn met VMNetwork1 Chicago.
* Wanneer een replica virtuele machine wordt gemaakt voor VMNetwork2 NewYork of VMNetwork2 Chicago, wordt deze niet verbonden met een netwerk.

Hier volgt hoe VMM-clouds worden ingesteld in ons voorbeeldorganisatie en de logische netwerken die zijn gekoppeld aan clouds.

### <a name="cloud-protection-settings"></a>Instellingen voor cloudbeveiliging
| **Beveiligde cloud** | **Beveiligen van de cloud** | **Logisch netwerk (Den Haag)** |
| --- | --- | --- |
| GoldCloud1 |GoldCloud2 | |
| SilverCloud1 |SilverCloud2 | |
| GoldCloud2 |<p>N.v.t.</p><p></p> |<p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p> |
| SilverCloud2 |<p>N.v.t.</p><p></p> |<p>LogicalNetwork1 NewYork</p><p>LogicalNetwork1 Chicago</p> |

### <a name="logical-and-vm-network-settings"></a>Logische schijf als VM-netwerkinstellingen
| **Locatie** | **Logisch netwerk** | **Bijbehorende VM-netwerk** |
| --- | --- | --- |
| New York |LogicalNetwork1 NewYork |VMNetwork1 NewYork |
| Chicago |LogicalNetwork1 Chicago |VMNetwork1 Chicago |
| LogicalNetwork2Chicago |VMNetwork2 Chicago | |

### <a name="target-networks"></a>Doelnetwerken
Op basis van deze instellingen wanneer u het doel VM-netwerk selecteert, ziet de volgende tabel u de opties die beschikbaar zijn.

| **Selecteren** | **Beveiligde cloud** | **Beveiligen van de cloud** | **Doelnetwerk beschikbaar** |
| --- | --- | --- | --- |
| VMNetwork1 Chicago |SilverCloud1 |SilverCloud2 |Beschikbaar |
| GoldCloud1 |GoldCloud2 |Beschikbaar | |
| VMNetwork2 Chicago |SilverCloud1 |SilverCloud2 |Niet beschikbaar |
| GoldCloud1 |GoldCloud2 |Beschikbaar | |


### <a name="failback"></a>Failback
Als u wilt zien wat er gebeurt in het geval van een failback (omgekeerde replicatie), gaan we ervan uit dat VMNetwork1 NewYork is toegewezen aan VMNetwork1-Chicago, met de volgende instellingen.

| **Virtuele machine** | **Verbonden met het VM-netwerk** |
| --- | --- |
| VM1 |VMNetwork1-netwerk |
| VM2 (replica van VM1) |VMNetwork1 Chicago |

Met deze instellingen gaan we bekijken wat er gebeurt in een aantal mogelijke scenario's.

| **Scenario** | **Resultaat** |
| --- | --- |
| Er is geen wijziging in de netwerkeigenschappen van VM-2 na een failover. |VM-1 blijft verbonden met het Bronnetwerk. |
| Netwerkeigenschappen van VM-2 worden gewijzigd na een failover en is verbroken. |VM-1 wordt verbroken. |
| Netwerkeigenschappen van VM-2 zijn gewijzigd na een failover en is verbonden met VMNetwork2 Chicago. |Als VMNetwork2 Chicago niet is toegewezen, kunt u VM-1 wordt verbroken. |
| Netwerktoewijzing van VMNetwork1 Chicago wordt gewijzigd. |VM-1 wordt verbonden met het netwerk is nu toegewezen aan VMNetwork1 Chicago. |


## <a name="prepare-for-single-server-deployment"></a>Voorbereiden voor implementatie met één server


Als u slechts één VMM-server hebt, kunt u virtuele machines in Hyper-V-hosts in de VMM-cloud te repliceren [Azure](site-recovery-vmm-to-azure.md) of naar een secundaire VMM-cloud. De eerste optie wordt aangeraden omdat repliceren tussen clouds niet naadloos. Als u repliceren tussen clouds wilt, kunt u met een enkele zelfstandige VMM-server of met één VMM-server geïmplementeerd in een Windows-cluster gespreide repliceren

### <a name="standalone-vmm-server"></a>Standalone VMM-server

In dit scenario kunt u één VMM-server implementeren als een virtuele machine in de primaire site en deze virtuele machine worden gerepliceerd naar een secundaire site met Site Recovery en Hyper-V Replica.

1. **Instellen van VMM op een Hyper-V-virtuele machine**. Het is raadzaam dat u de SQL Server-exemplaar gebruikt door VMM op dezelfde virtuele machine plaatsen. Dit bespaart u tijd omdat slechts één virtuele machine moet worden gemaakt. Als u wilt gebruiken extern exemplaar van SQL Server en een storing optreedt, moet u dat exemplaar herstellen voordat u VMM kunt herstellen.
2. **Zorg ervoor dat de VMM-server heeft ten minste twee clouds die zijn geconfigureerd**. Bevat één cloud de virtuele machines u wilt repliceren en de andere cloud dient als de secundaire locatie. De cloud waarin de virtuele machines die u wilt beveiligen, moet voldoen aan [vereisten](#prerequisites).
3. Site Recovery ingesteld zoals beschreven in dit artikel. Maken en registreer de VMM-server in een kluis, instellen van een beleid voor wachtwoordreplicatie en replicatie inschakelen. De namen van de bron en doel-VMM wordt hetzelfde zijn. Geef op dat de initiële replicatie vindt plaats via het netwerk.
4. Wanneer u netwerktoewijzing instellen kunt u de VM-netwerk voor de primaire cloud toewijzen aan het VM-netwerk voor de secundaire cloud.
5. Hyper-V Replica hebt ingeschakeld op de Hyper-V-host met de VMM VM in de Hyper-V Manager-console en replicatie inschakelen voor de virtuele machine. Zorg ervoor dat u de virtuele machine van VMM niet toevoegen aan clouds die zijn beveiligd door Site Recovery om ervoor te zorgen dat de Hyper-V Replica-instellingen worden niet door Site Recovery overschreven.
6. Als u voor failover-herstelplannen maken kunt u dezelfde VMM-server gebruiken voor de bron en doel.
7. In een volledige onderbreking failover en als volgt herstellen:

   1. Een niet-geplande failover worden uitgevoerd in de Hyper-V Manager-console in de secundaire site, failover van de primaire VMM VM naar de secundaire site.
   2. Controleer of de VMM VM up en actief en in de kluis, een niet-geplande failover voor de virtuele machines van primaire naar secundaire clouds failover worden uitgevoerd. De failover doorvoeren en selecteert u een alternatief herstelpunt indien nodig.
   3. Nadat de niet-geplande failover voltooid is, zijn alle resources toegankelijk vanaf de primaire site opnieuw.
   4. Wanneer de primaire site opnieuw in de Hyper-V Manager-console in de secundaire site beschikbaar is, schakel u omgekeerde replicatie voor de VMM-VM. Hiermee start u replicatie voor de virtuele machine van de secundaire op primaire.
   5. Een geplande failover uitvoeren in de Hyper-V Manager-console in de secundaire site, failover van de VMM VM naar de primaire site. De failover doorvoeren. Omgekeerde replicatie vervolgens ingeschakeld zodat de VMM VM wordt opnieuw gerepliceerd van primaire server naar de secundaire.
   6. Schakel in de Recovery Services-kluis omgekeerde replicatie voor de werkbelasting van virtuele machines, start ze van secundaire op primaire repliceren.
   7. Voer een geplande failover naar een failback uit op de werkbelasting van virtuele machines naar de primaire site in de Recovery Services-kluis. De failover voor het voltooien van deze doorvoeren. Omgekeerde replicatie starten met het repliceren van de werkbelasting van virtuele machines van primaire naar secundaire activeert.

### <a name="stretched-vmm-cluster"></a>Gespreide VMM-cluster

In plaats van een zelfstandige VMM-server implementeren als een virtuele machine die wordt gerepliceerd naar een secundaire site, kunt u VMM maximaal beschikbaar maken door deze te implementeren als een virtuele machine in een Windows-failovercluster. Dit biedt veerkracht werkbelasting en bescherming tegen hardwarefouten. Als u wilt implementeren met Site Recovery moeten VMM VM worden geïmplementeerd in een cluster stretch geografisch afzonderlijke sites. Om dit te doen:

1. Installeer VMM op een virtuele machine in een Windows-failovercluster en selecteer de optie voor het uitvoeren van de server als maximaal beschikbaar tijdens de installatie.
2. De SQL Server-exemplaar dat wordt gebruikt door VMM moet worden gerepliceerd met SQL Server AlwaysOn-beschikbaarheidsgroepen, zodat er een replica van de database in de secundaire site is.
3. Volg de instructies in dit artikel voor het maken van een kluis, de-server registreren en instellen van beveiliging. U moet elke VMM-server registreren in het cluster in de Recovery Services-kluis. U doet dit door de Provider installeren op een actief knooppunt en registreer de VMM-server. Vervolgens installeert u de Provider op andere knooppunten.
4. Wanneer er een storing optreedt, worden de VMM-server en de bijbehorende SQL Server-database failover, en toegankelijk vanuit de secundaire site.



## <a name="prepare-for-storage-mapping"></a>Voorbereiden op opslagkoppeling


Instellen opslag toewijzing door toewijzing opslagclassificaties in een bron en doel van de VMM-servers om het volgende doen:

  * **Doelopslag voor gerepliceerde virtuele machines identificeren**: een bron VM hardeschijf wordt gerepliceerd naar de opslag die u hebt opgegeven (SMB-share of het cluster shared volumes (CSV's)) in de doellocatie.
  * **Plaatsing van de replica virtuele machine**: toewijzing van opslag wordt gebruikt om optimaal gerepliceerde virtuele machines op Hyper-V-hostservers. Virtuele replicamachines worden geplaatst op hosts die toegang heeft tot de opslagclassificatie toegewezen.
  * **Er is geen toewijzing van opslag**— als u de toewijzing van opslag niet configureert, virtuele machines moet worden gerepliceerd naar de standaardopslaglocatie opgegeven op de Hyper-V-hostserver die zijn gekoppeld aan de replica virtuele machine.

Opmerking:
- U kunt de toewijzing tussen twee VMM-clouds op één server instellen.
- Opslagclassificaties moet beschikbaar zijn voor de hostgroepen die zich in de bron en doel-clouds.
- Classificaties hoeft niet te zijn van hetzelfde type opslag. U kunt bijvoorbeeld een bron-classificatie met SMB-shares voor een doel-classificatie met CSV's toewijzen.

### <a name="example"></a>Voorbeeld
Als classificaties juist zijn geconfigureerd in VMM wanneer u de bron en doel-VMM-server tijdens de toewijzing van opslag selecteert, kunt u de bron en doel-classificaties wordt weergegeven. Hier volgt een voorbeeld van bestandsshares voor opslag en de classificaties voor een organisatie met twee locaties in New York en Chicago.

| **Locatie** | **VMM-server** | **Bestandsshare (bron)** | **Classificatie (bron)** | **Toegewezen aan** | **Bestandsshare (doel)** |
| --- | --- | --- | --- | --- | --- |
| New York |VMM_Source |SourceShare1 |GOUD |GOLD_TARGET |TargetShare1 |
| SourceShare2 |ZILVER |SILVER_TARGET |TargetShare2 | | |
| SourceShare3 |BRONS |BRONZE_TARGET |TargetShare3 | | |
| Chicago |VMM_Target | |GOLD_TARGET |Niet toegewezen | |
|  |SILVER_TARGET |Niet toegewezen | | | |
|  |BRONZE_TARGET |Niet toegewezen | | | |

Met dit voorbeeld:

* Wanneer een replica virtuele machine wordt gemaakt voor elke virtuele machine op GOLD-opslag (SourceShare1), wordt deze worden gerepliceerd naar de opslag van een GOLD_TARGET (TargetShare1).
* Wanneer een replica virtuele machine wordt gemaakt voor elke virtuele machine op zilver opslag (SourceShare2), deze wordt gerepliceerd naar de opslag van een SILVER_TARGET (TargetShare2), enzovoort.

### <a name="multiple-storage-locations"></a>Meerdere opslaglocaties
Als de doel-classificatie is toegewezen aan meerdere SMB-shares of CSV's, wordt de optimale opslaglocatie automatisch geselecteerd wanneer de virtuele machine is beveiligd. Als er geen geschikte doelopslag beschikbaar met de opgegeven indeling is, wordt de standaardopslaglocatie opgegeven op de Hyper-V-host wordt gebruikt voor het plaatsen van de replica virtuele harde schijven.

De volgende tabel wordt weergegeven hoe opslagclassificatie en gedeelde clustervolumes zijn ingesteld in ons voorbeeld.

| **Locatie** | **Classificatie** | **Gekoppelde opslag** |
| --- | --- | --- |
| New York |GOUD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> |
| ZILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p> | |
| Chicago |GOLD_TARGET |<p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p> |
| SILVER_TARGET |<p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p> | |

Deze tabel ziet u het gedrag wanneer u de beveiliging voor virtuele machines (VM1 - VM5) in dit Voorbeeldomgeving inschakelen.

| **Virtuele machine** | **Bron-opslag** | **Classificatie van bron** | **Toegewezen doelopslag** |
| --- | --- | --- | --- |
| VM1 |C:\ClusterStorage\SourceVolume1 |GOUD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Beide GOLD_TARGET</p> |
| VM2 |\\FileServer\SourceShare1 |GOUD |<p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Beide GOLD_TARGET</p> |
| VM3 |C:\ClusterStorage\SourceVolume2 |ZILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p> |
| VM4 |\FileServer\SourceShare2 |ZILVER |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Beide SILVER_TARGET</p> |
| VM5 |C:\ClusterStorage\SourceVolume3 |N.v.t. |Er is geen toewijzing, zodat de standaardopslaglocatie van de Hyper-V-host wordt gebruikt |



### <a name="data-privacy-overview"></a>Overzicht van privacy

Deze tabel ziet u hoe gegevens worden opgeslagen in dit scenario:

- - -
| Actie | **Details** | **Verzamelde gegevens** | **Gebruiken** | **Vereist** |
| --- | --- | --- | --- | --- |
| **Registratie** | U kunt een VMM-server registreren in een Recovery Services-kluis. Als u later Hef de registratie van een server wilt, kunt u dit doen door de server-gegevens verwijderen uit de Azure portal. | Wanneer een VMM-server is geregistreerd Site Recovery worden verzameld, processen, en de metagegevens over de VMM-server en de namen van de VMM-clouds gedetecteerd door Site Recovery. | De gegevens worden gebruikt om te bepalen en communiceren met de juiste VMM-server en het configureren van instellingen voor de juiste VMM-clouds. | Deze functie is vereist. Als u niet wilt dat deze gegevens worden verzonden naar Site Recovery kunt u de Site Recovery-service niet gebruiken. |
| **Replicatie inschakelen** | De Azure Site Recovery Provider is geïnstalleerd op de VMM-server en het kanaal voor communicatie met de Site Recovery-service. De Provider is een dynamic-link library (DLL) gehost in de VMM-proces. Nadat de Provider is geïnstalleerd, wordt de functie 'Datacenterherstel' opgehaald ingeschakeld in de VMM administrator-console. Nieuwe en bestaande virtuele machines kunnen deze functie inschakelen van beveiliging voor een virtuele machine inschakelen. |Met deze eigenschap is ingesteld verzendt de Provider de naam en ID van de virtuele machine naar de Site Recovery.  Replicatie is ingeschakeld door Windows Server 2012 of Windows Server 2012 R2 Hyper-V Replica. Gegevens van de virtuele machine worden gerepliceerd van de ene Hyper-V-host naar een andere (meestal zich in een datacenter van verschillende "herstelpunt'). |Site Recovery maakt gebruik van de metagegevens voor het vullen van de VM-informatie in de Azure-portal. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze gegevens worden verzonden wilt, niet de Site Recovery-beveiliging voor virtuele machines inschakelt. Houd er rekening mee dat alle gegevens die door de Provider wordt verzonden naar Site Recovery wordt verzonden via HTTPS. |
| **Plan voor herstel** | De herstelplannen waarmee u kunt een plan voor het datacenter van het herstel van de orchestration maken. U kunt de volgorde waarin virtuele machines of een groep van virtuele machines moet worden gestart op de site recovery definiëren. U kunt ook de geautomatiseerde scripts worden uitgevoerd, of een handmatige actie moeten worden uitgevoerd op het moment van herstel voor elke virtuele machine opgeven. Failover wordt doorgaans geactiveerd op het niveau van het plan herstel voor gecoördineerde herstel. | Site Recovery worden verzameld, verwerkt en metagegevens voor het herstelplan, met inbegrip van metagegevens van de virtuele machine en metagegevens van eventuele automatiseringsscripts en -opmerkingen bij de handmatige actie verzendt. |De metagegevens wordt gebruikt voor het bouwen van het herstelplan in de Azure portal. |Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze gegevens worden verzonden naar Site Recovery wilt, niet herstelplannen maken. |
| **Netwerktoewijzing** | Gegevens van het netwerk van het primaire datacenter tot herstel Datacenter wordt toegewezen. Wanneer virtuele machines zijn hersteld op de site recovery, helpt het netwerktoewijzing netwerkverbinding tot stand wordt gebracht. |Site Recovery worden verzameld, verwerkt en verzendt de metagegevens van de logische netwerken voor elke site (primair en datacenter). |De metagegevens wordt gebruikt voor het vullen van netwerkinstellingen, zodat u de gegevens van het netwerk kunt toewijzen. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze gegevens worden verzonden naar Site Recovery wilt, hoeft u netwerktoewijzing. |
| **Failover (geplande en ongeplande/testen)** | Failover is failover van virtuele machines van één VMM beheerde Datacenter naar de andere. De failover-actie wordt handmatig geactiveerd in de Azure portal. |De Provider op de VMM-server is op de hoogte van de failover-gebeurtenis met Site Recovery en wordt een actie voor failover uitgevoerd op de Hyper-V-host via de VMM-interfaces. Er is een werkelijke failover van een virtuele machine van een Hyper-V-host naar een andere en verwerkt door Windows Server 2012 of Windows Server 2012 R2 Hyper-V Replica. Of achterwaartse richting Site Recovery gebruikt de informatie verzonden naar het vullen van de status van de failover-actie-informatie in de Azure-portal. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze gegevens worden verzonden naar Site Recovery wilt, hoeft u failover. |

## <a name="next-steps"></a>Volgende stappen

Nadat u de implementatie hebt getest, meer informatie over andere typen [failover](site-recovery-failover.md)
