---
title: Virtuele machines van AWS migreren naar Azure met Azure Site Recovery | Microsoft Docs
description: In dit artikel beschrijft het migreren van virtuele machines in Amazon Web Services (AWS) uitgevoerd in Azure, met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
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
ms.openlocfilehash: 6eb6489deacab71e870585d209e26dad801ddc07
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Amazon Web Services (AWS) virtuele machines migreren naar Azure

Deze zelfstudie leert u hoe Amazon Web Services (AWS) virtuele machines (VM's) te migreren virtuele Azure-machines met Site Recovery. Wanneer EC2 exemplaren migreren naar Azure, de VMsare behandeld alsof het fysieke, lokale computers. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure-resources voorbereiden
> * De exemplaren AWS EC2 voorbereiden voor migratie
> * Een configuratieserver implementeren
> * Replicatie voor virtuele machines inschakelen
> * Test de failover om te controleren of alles werkt
> * Een eenmalige failover uitvoeren naar Azure

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prepare-azure-resources"></a>Azure-resources voorbereiden 

U moet een aantal bronnen in Azure voor de gemigreerde exemplaren van EC2 gebruiken gereed hebt. Het gaat hierbij om een opslagaccount, een kluis en een virtueel netwerk.

### <a name="create-a-storage-account"></a>Een opslagaccount maken

Installatiekopieën van gerepliceerde machines worden bewaard in Azure-opslag. Virtuele machines in Azure worden gemaakt uit de opslag wanneer failover van on-premises naar Azure.

1. In de [Azure-portal](https://portal.azure.com) menu, klikt u op **nieuw** -> **opslag** -> **opslagaccount**.
2. Voer een naam in voor het opslagaccount. Voor deze zelfstudie gebruiken we de naam van de **awsmigrated2017**. De naam moet uniek zijn binnen Azure en tussen 3 en 24 tekens lang, alleen cijfers en kleine letters.
3. Behoud de standaardinstellingen voor **implementatiemodel**, **Account kind**, **prestaties**, en **vereiste gegevensoverdracht Secure**.
5. Selecteer de standaard **RA-GRS** voor **replicatie**.
6. Selecteer het abonnement dat u wilt gebruiken voor deze zelfstudie.
7. Voor **resourcegroep**, selecteer **nieuw**. In dit voorbeeld gebruiken we **migrationRG** als de naam.
8. Selecteer **West-Europa** als de locatie. 
9. Klik op **Maken** om het opslagaccount te maken.

### <a name="create-a-vault"></a>Een kluis maken

1. In de [Azure-portal](https://portal.azure.com), klik in het linkernavigatievenster op **meer services** en zoek en selecteer **Recovery Services-kluizen**.
2. Klik op de pagina Recovery Services-kluizen **+ toevoegen** in de linkerbovenhoek van de pagina.
3. Voor **naam**, type *myVault*. 
4. Voor **abonnement**, selecteert u het juiste abonnement.
4. Voor **resourcegroep**, selecteer **gebruik bestaande** en selecteer *migrationRG*. 
5. In **locatie**, selecteer *West-Europa*. 
5. Voor snelle toegang tot de nieuwe kluis vanuit het dashboard, selecteert u **vastmaken aan dashboard**.
7. Wanneer u klaar bent, klikt u op **maken**.

De nieuwe kluis wordt weergegeven op de **Dashboard** > **alle resources**, en klik op de hoofdblade **Recovery Services-kluizen** pagina.

### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Wanneer de Azure VM's zijn gemaakt na de migratie (failover), zijn ze gekoppeld aan dit netwerk.

1. In de [Azure-portal](https://portal.azure.com), klikt u op **nieuw** > **Networking** >
    **virtueel netwerk**
3. Voor **naam**, type *myMigrationNetwork*.
4. Laat de standaardwaarde voor **adresruimte**.
5. Voor **abonnement**, selecteert u het juiste abonnement.
6. Voor **resourcegroep**, selecteer **gebruik bestaande** en kies *migrationRG* in de vervolgkeuzelijst.
7. Voor **locatie**, selecteer **West-Europa**. 
8. Laat de standaardwaarden voor **Subnet**, zowel de **naam** en **IP-bereik**.
9. Laat **Service-eindpunten** uitgeschakeld.
10. Wanneer u klaar bent, klikt u op **maken**.


## <a name="prepare-the-ec2-instances"></a>De exemplaren EC2 voorbereiden

U moet een of meer virtuele machines die u wilt migreren. Deze EC2-exemplaar moet worden uitgevoerd als de 64-bits versie van Windows Server 2008 R2 SP1 of hoger, Windows Server 2012, Windows Server 2012 R2 of Red Hat Enterprise Linux 6.7 (alleen HVM gevirtualiseerd exemplaren). De server moet alleen Citrix HW of AWS HW stuurprogramma's hebben. Exemplaren met RedHat HW-stuurprogramma's worden niet ondersteund.

De Mobility-service moet worden geïnstalleerd op elke virtuele machine die u wilt repliceren. Site Recovery wordt deze service automatisch geïnstalleerd wanneer u replicatie voor de virtuele machine inschakelt. Automatische installatie moet u een account op de EC2-exemplaren die door Site Recovery wordt gebruikt voor toegang tot de virtuele machine voorbereiden.

U kunt een domein of lokale account gebruiken. Voor virtuele Linux-machines, moet het account hoofdmap op de bronserver Linux zijn. Voor Windows virtuele machines, als u niet een domeinaccount, Schakel externe gebruiker toegangsbeheer op de lokale computer:

  - In het register, onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, de DWORD-vermelding toevoegen **LocalAccountTokenFilterPolicy** en de waarde ingesteld op 1.
    
U moet ook een afzonderlijk exemplaar van EC2 die u als de configuratieserver Site Recovery gebruiken kunt. Dit exemplaar moet Windows Server 2012 R2 worden uitgevoerd. 
    

## <a name="prepare-the-infrastructure"></a>De infrastructuur voorbereiden 

Selecteer op de portal-pagina voor uw kluis **siteherstel** van de **aan de slag** sectie en klik vervolgens op **infrastructuur voorbereiden**.

### <a name="1-protection-goal"></a>1 beveiligingsdoel

Selecteer de volgende waarden op de **Beveiligingsdoel** pagina:
   
|    |  | 
|---------|-----------|
| Waar bevinden de machines zich? | **Lokale**|
| Waar wilt u uw machines repliceren? |**Naar Azure**|
| Worden uw machines gevirtualiseerd? | **Niet gevirtualiseerde / andere**|

Wanneer u klaar bent, klikt u op **OK** verplaatsen naar de volgende sectie.

### <a name="2-source-prepare"></a>2 bron voorbereiden 

Op de **bron voorbereiden** pagina, klikt u op **+ configuratieserver**. 

1. Een EC2-exemplaar met Windows Server 2012 R2 gebruiken voor het maken van een configuratieserver en bij uw recovery-kluis registreren.

2. Configureer de proxy op het exemplaar EC2 VM die u als de configuratieserver gebruikt zodat deze toegang heeft tot de [-URL's](../site-recovery-support-matrix-to-azure.md).

3. Download de [Microsoft Azure Site Recovery Unified Setup](http://aka.ms/unifiedinstaller_wus) programma. U kunt dit downloaden naar uw lokale computer en vervolgens naar de virtuele machine die u als de configuratieserver gebruikt via kopiëren.

4. Klik op de **downloaden** knop voor het downloaden van de sleutel van de registratie van de kluis. Het gedownloade bestand kopiëren naar de virtuele machine die u als de configuratieserver gebruikt.

5. De virtuele machine met de rechtermuisknop op installatieprogramma die u hebt gedownload voor de **Microsoft Azure Site Recovery Unified Setup** en selecteer **als administrator uitvoeren**. 

    1. In **voordat u begint**, selecteer **installeren van de configuratieserver en de processerver** en klik vervolgens op **volgende**.
    2. In **van derden softwarelicentie**, selecteer **ik ga akkoord met de licentieovereenkomst van derden.** en klik vervolgens op **volgende**.
    3. In **registratie**, klik op Bladeren en navigeer naar waar u het registratiesleutelbestand kluis wordt geplaatst en klik vervolgens op **volgende**.
    4. In **internetinstellingen**, selecteer **verbinding maken met Azure Site Recovery zonder een proxy-server.** en klik vervolgens op **volgende**.
    5. In de **controle van vereisten** pagina uitgevoerd controles voor verschillende items. Wanneer deze voltooid is, klikt u op **volgende**. 
    6. In **MySQL configuratie**, geef de vereiste wachtwoorden en klik op **volgende**.
    7. In **omgeving Details**, selecteer **Nee**, u hoeft niet te beveiligen, VMware-machines en klik vervolgens op **volgende**.
    8. In **installatielocatie**, klikt u op **volgende** de standaardoptie.
    9. In **netwerk selecteren**, klikt u op **volgende** de standaardoptie.
    10. In **samenvatting** klikt u op **installeren**.
    11. **Voortgang van installatie** ziet u informatie over waar u zich bij de installatie. Wanneer deze voltooid is, klikt u op **voltooien**. Ophalen van een pop-upvenster over hoeven mogelijk opnieuw worden opgestart, klikt u **OK**. U ook ophalen van een pop-upvenster over de configuratie-Server verbinding wachtwoordzin, de wachtwoordzin naar het Klembord kopiëren en een veilige locatie opslaan.
    
6. Voer op de virtuele machine **cspsconfigtool.exe** voor het maken van een of meer management accounts op de configuratieserver. Zorg ervoor dat de management-accounts beheerdersmachtigingen op de EC2-exemplaren die u wilt migreren. 

Wanneer u klaar bent de configuratieserver instellen, gaat u terug naar de portal en selecteert u de server die u zojuist hebt gemaakt voor **configuratieserver** en klik op *OK** om door te gaan naar stap 3 doel voorbereiden.

### <a name="3-target-prepare"></a>3 doel voorbereiden 

In deze sectie voert u gegevens over de resources die u hebt gemaakt tijdens de [bereiden Azure resources](#prepare-azure-resources) sectie eerder in deze zelfstudie.

1. In **abonnement**, selecteer de Azure-abonnement dat u gebruikt voor de [voorbereiden Azure](../tutorial-prepare-azure.md) zelfstudie.
2. Selecteer **Resource Manager** als het implementatiemodel.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt. Dit moeten de resources die u hebt gemaakt tijdens de [bereiden Azure resources](#prepare-azure-resources) sectie eerder in deze zelfstudie
4. Klik als u klaar bent op **OK**.


### <a name="4-replication-settings-prepare"></a>4 replicatie-instellingen voorbereiden 

U moet een beleid voor wachtwoordreplicatie maken voordat u kunt replicatie inschakelen

1. Klik op **+ repliceren en koppel**.
2. In **naam**, type **myReplicationPolicy**.
3. Laat de rest van de standaardinstellingen en klik op **OK** om het beleid te maken. Het nieuwe beleid is automatisch gekoppeld aan de configuratieserver. 

### <a name="5-deployment-planning-select"></a>5-implementatie plannen selecteren 

In **hebt u voltooid-implementatie plannen?**, selecteer **ik zal dit later doen** in de vervolgkeuzelijst en klik vervolgens op **OK**.

Wanneer u alle klaar bent met alle 5 secties van **infrastructuur voorbereiden**, klikt u op **OK**.


## <a name="enable-replication"></a>Replicatie inschakelen

Replicatie inschakelen voor elke VM die u wilt migreren. Wanneer replicatie is ingeschakeld, wordt de Mobility-service automatisch door Site Recovery geïnstalleerd. 

1. Open de [Azure Portal](htts://portal.azure.com).
1. Op de pagina voor uw kluis onder **aan de slag**, klikt u op **siteherstel**.
2. Onder **voor on-premises machines en virtuele Azure-machines**, klikt u op **1:Replicate toepassing stap**. De wizardpagina's met de volgende informatie en klik op Voltooien **OK** op elke pagina wanneer voltooid:
    - 1 bron configureren:
      
    |  |  |
    |-----|-----|
    | Bron: | **On-Premises**|
    | Bronlocatie:| De naam van uw configuratie-serverexemplaar EC2.|
    |Machinetype: | **Fysieke machines**|
    | Processerver: | Selecteer de configuratieserver in de vervolgkeuzelijst.|
    
    - 2 doel configureren
        
    |  |  |
    |-----|-----|
    | Doel: | Laat de standaardwaarde.|
    | Abonnement: | Selecteer het abonnement dat u hebt gebruikt.|
    | Na een failover-resourcegroep:| De resourcegroep die u hebt gemaakt in de [bereiden Azure resources](#prepare-azure-resources) sectie.|
    | Na een failover-implementatiemodel: | Kies **Resource Manager**|
    | Storage-account: | Kies de storage-account u hebt gemaakt in de [bereiden Azure resources](#prepare-azure-resources) sectie.|
    | Azure-netwerk: | Kies **nu configureren voor geselecteerde machines**|
    | Na een failover Azure-netwerk: | Kies het netwerk die u hebt gemaakt in de [bereiden Azure resources](#prepare-azure-resources) sectie.|
    | Subnet: | Selecteer de **standaard** in de vervolgkeuzelijst.|
    
    - 3 fysieke Machines selecteren
        
        Klik op **+ fysieke machine** en voer vervolgens de **naam**, wordt de **IP-adres** en **Type besturingssysteem** van het EC2-exemplaar dat u wilt migreren en Klik vervolgens op **OK**.
        
    - 4 eigenschappen configureren eigenschappen
        
        Selecteer het account dat u hebt gemaakt op de configuratieserver in de vervolgkeuzelijst en klik op **OK**.
        
    - 5 replicatie-instellingen configureert replicatie-instellingen
    
        Zorg ervoor dat het replicatiebeleid geselecteerd in de vervolgkeuzelijst **myReplicationPolicy** en klik vervolgens op **OK**.
        
3. Wanneer de wizard voltooid is, klikt u op **replicatie inschakelen**.
        

U kunt de voortgang van volgen de **beveiliging inschakelen** taak **bewaking en rapporten** > **taken** > **Site Recovery-taken** . Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.        
        
Wanneer u replicatie voor een virtuele machine inschakelt, kan het 15 minuten duren of langer om wijzigingen te laten voeren en in de portal weergegeven.

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Wanneer u een failovertest uitvoert, gebeurt het volgende:

1. Controle wordt uitgevoerd om te controleren of alle van de voorwaarden voor failover zijn in plaats van een vereisten.
2. Failover verwerkt de gegevens, zodat een virtuele machine van Azure kunnen worden gemaakt. Als deze optie wordt het meest recente herstelpunt, een herstelpunt van de gegevens gemaakt.
3. Een Azure VM is gemaakt met behulp van de gegevens die worden verwerkt in de vorige stap.

Voer de testfailover als volgt in de portal:

1. Op de pagina voor uw kluis, gaat u naar **beveiligde items** > **gerepliceerde Items**> Klik op de virtuele machine > **+ Testfailover**.

2. Selecteer een herstelpunt dat moet worden gebruikt voor de failover:
    - **Meest recente verwerkte**: de virtuele machine wordt overgenomen door de meest recente herstelpunt dat is verwerkt door Site Recovery. Het tijdstempel wordt weergegeven. Met deze optie geen tijd besteed aan het verwerken van gegevens, zodat het biedt een laag RTO (beoogde hersteltijd).
    - **Meest recente app-consistente**: deze optie alle VM's naar de meest recente herstelpunt van app-consistente failover. Het tijdstempel wordt weergegeven.
    - **Aangepaste**: eender welk herstelpunt kiezen.
3. In **Testfailover**, selecteer de doel-Azure netwerk welke virtuele Azure-machines moet worden verbonden nadat er failover plaatsvindt. Dit is het netwerk die u hebt gemaakt in de [bereiden Azure resources](#prepare-azure-resources) sectie.
4. Klik op **OK** om te beginnen met de failover. U kunt de voortgang volgen door te klikken op de virtuele machine om de eigenschappen te openen. U kunt ook op de **Testfailover** -taak op de pagina voor uw kluis in **bewaking en rapporten** > **taken** >
   **Site Hersteltaken**.
5. Nadat de failover is voltooid, de replica virtuele machine in Azure wordt weergegeven in de Azure portal > **virtuele Machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd.
6. Nu moet u verbinding maken met de gerepliceerde virtuele machine in Azure.
7. Als u wilt verwijderen op Azure VM's die zijn gemaakt tijdens de testfailover, klikt u op **testfailover opschonen** op het herstelplan. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover.

In sommige scenario's moet failover extra verwerking die het duurt ongeveer 8 tot tien minuten is voltooid. 


## <a name="migrate-to-azure"></a>Migreren naar Azure

Voer een werkelijke failover voor de EC2 exemplaren voor het migreren van deze virtuele Azure-machines. 

1. In **beveiligde items** > **gerepliceerde items** klikt u op de exemplaren AWS > **Failover**.
2. In **Failover** Selecteer een **herstelpunt** failover. Selecteer de meest recente herstelpunt.
3. Selecteer **machine afsluiten voordat u begint met failover** als u wilt dat Site Recovery voor probeert virtuele bronmachines afgesloten voordat de failover. Failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de **taken** pagina.
4. Controleer of de virtuele machine wordt weergegeven in **gerepliceerde items**. 
5. Met de rechtermuisknop op elke virtuele machine > **migratie voltooien**. Het migratieproces is voltooid, de replicatie voor de VM AWS stopt en Hiermee stopt u Site Recovery facturering voor de virtuele machine.

    ![Volledige migratie](./media/tutorial-migrate-aws-to-azure/complete-migration.png)

> [!WARNING]
> **Een failover uitgevoerd niet annuleren**: voordat failover wordt gestart, VM-replicatie is gestopt. Als u een failover uitgevoerd, stopt de failover, annuleren, maar de virtuele machine opnieuw won't repliceren.  


    

## <a name="next-steps"></a>Volgende stappen

In dit onderwerp hebt u geleerd EC2 AWS-instanties migreren naar Azure VM's. Blijven de zelfstudies voor VM's van Windows voor meer informatie over virtuele Azure-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](../../virtual-machines/windows/tutorial-manage-vm.md)
