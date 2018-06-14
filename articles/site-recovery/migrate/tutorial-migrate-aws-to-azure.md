---
title: VM’s vanuit AWS migreren naar Azure met Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u virtuele machines die worden uitgevoerd op Amazon Web Services (AWS) naar Azure migreert met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
editor: ''
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/27/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 9b1aabcd88ce93ff9316d3ee99619eccaeb6a38e
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
ms.locfileid: "29677446"
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>AWS-VM’s (Amazon Web Services) migreren naar Azure

In deze zelfstudie leert u hoe u virtuele AWS-machines (Amazon Web Services) naar Azure migreert met behulp van Site Recovery. Wanneer EC2 exemplaren migreren naar Azure, de VMsare behandeld alsof het fysieke, lokale computers. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure-resources voorbereiden
> * De AWS EC2-instanties voorbereiden voor migratie
> * Een configuratieserver implementeren
> * Replicatie inschakelen voor VM's
> * De failover testen om te controleren of alles werkt
> * Een eenmalige failover uitvoeren naar Azure

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prepare-azure-resources"></a>Azure-resources voorbereiden 

U moet een aantal resources gereed hebben in Azure voor gebruik door de gemigreerde EC2-instanties. Het gaat hier om een opslagaccount, een kluis en een virtueel netwerk.

### <a name="create-a-storage-account"></a>Een opslagaccount maken

Installatiekopieën van gerepliceerde machines worden bewaard in Azure Storage. Azure VM’s worden gemaakt vanuit de opslag wanneer u een failover van on-premises naar Azure uitvoert.

1. Klik in het menu van [Azure Portal](https://portal.azure.com) op **Een resource maken** -> **Opslag** -> **Opslagaccount**.
2. Voer een naam in voor het opslagaccount. Voor deze zelfstudie gebruiken we de naam **awsmigrated2017**. De naam moet uniek zijn in Azure, tussen de 3 en 24 tekens lang zijn en mag alleen cijfers en kleine letters bevatten.
3. Behoud de standaardinstellingen voor **Implementatiemodel**, **Soort account**, **Prestaties** en **Veilige overdracht vereist**.
5. Selecteer de standaard **RA-GRS** voor **Replicatie**.
6. Selecteer het abonnement dat u voor deze handleiding wilt gebruiken.
7. Selecteer voor **Resourcegroep** de optie **Nieuwe maken**. In dit voorbeeld gebruiken we **migrationRG** als de naam.
8. Selecteer **West-Europa** als de locatie. 
9. Klik op **Maken** om het opslagaccount te maken.

### <a name="create-a-vault"></a>Een kluis maken

1. Klik in [Azure Portal](https://portal.azure.com) in het linkernavigatievenster op **Meer services** en zoek en selecteer **Recovery Services-kluizen**.
2. Klik op de pagina Recovery Services-kluizen op **Toevoegen** in de linkerbovenhoek van de pagina.
3. Typ voor **Naam** de tekst *myVault*. 
4. Selecteer bij **Abonnement** het juiste Azure-abonnement.
4. Selecteer voor **Resourcegroep** de optie **Bestaande gebruiken** en selecteer *migrationRG*. 
5. Selecteer bij **Locatie** de optie *West-Europa*. 
5. Voor snelle toegang tot de nieuwe kluis vanuit het dashboard, selecteert u **Aan dashboard vastmaken**.
7. Klik op **Maken** wanneer u klaar bent.

De nieuwe kluis wordt weergegeven op het **Dashboard** > **Alle resources** en op de hoofdpagina **Recovery Services-kluizen**.

### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Wanneer de Azure VM's zijn gemaakt na de migratie (failover), worden ze gekoppeld aan dit netwerk.

1. In de [Azure-portal](https://portal.azure.com), klikt u op **maken van een resource** > **Networking** >
    **virtueel netwerk**
3. Typ voor **Naam** de tekst *myMigrationNetwork*.
4. Laat de standaardwaarde die is ingesteld voor **Adresruimte**.
5. Selecteer bij **Abonnement** het juiste Azure-abonnement.
6. Selecteer voor de **resourcegroep** de optie **Bestaande gebruiken** en kies *migrationRG* in de vervolgkeuzelijst.
7. Selecteer bij **Locatie** de optie **West-Europa**. 
8. Laat de standaardwaarden voor **Subnet** ongewijzigd, zowel de **Naam** als het **IP-bereik**.
9. Laat **Service-eindpunten** uitgeschakeld.
10. Klik op **Maken** wanneer u klaar bent.


## <a name="prepare-the-ec2-instances"></a>De EC2-instanties voorbereiden

U moet een of meer te migreren virtuele machines hebben. Deze EC2-instantie moet worden uitgevoerd als de 64-bits versie van Windows Server 2008 R2 SP1 of hoger, Windows Server 2012, Windows Server 2012 R2 of Red Hat Enterprise Linux 6.7 (alleen op HVM gebaseerde gevirtualiseerde instanties). De server mag maar één Citrix HW- of AWS HW-stuurprogramma hebben. Instanties waarop een RedHat HW-stuurprogramma wordt uitgevoerd, worden niet ondersteund.

De Mobility-service moet worden geïnstalleerd op elke VM die u wilt repliceren. Site Recovery installeert deze service automatisch wanneer u replicatie voor de VM inschakelt. Voor automatische installatie moet u op de EC2-instanties een account voorbereiden waarmee Site Recovery toegang kan krijgen tot de VM.

U kunt een domein of lokale account gebruiken. Voor Linux-VM’s moet het account een hoofdaccount zijn op de Linux-bronserver. Voor Windows-accounts schakelt u als u geen domeinaccount gebruikt toegangsbeheer voor externe gebruikers uit op de lokale computer:

  - Voeg in het register onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** de DWORD-vermelding **LocalAccountTokenFilterPolicy** toe en stel de waarde in op 1.
    
U hebt ook een afzonderlijke EC2-instantie nodig die u als de Site Recovery-server kunt gebruiken. Op deze instantie moet Windows Server 2012 R2 worden uitgevoerd. 
    

## <a name="prepare-the-infrastructure"></a>De infrastructuur voorbereiden 

Selecteer op de portalpagina van uw kluis de optie **Site Recovery** in de sectie **Aan de slag** en klik vervolgens op **Infrastructuur voorbereiden**.

### <a name="1-protection-goal"></a>1 Beveiligingsdoel

Selecteer de volgende waarden op de pagina **Beveiligingsdoel**:
   
|    |  | 
|---------|-----------|
| Waar bevinden de machines zich? | **On-premises**|
| Waarnaartoe wilt u de machines repliceren? |**Naar Azure**|
| Zijn de machines gevirtualiseerd? | **Niet gevirtualiseerd/overig**|

Klik wanneer u klaar bent op **OK** om door te gaan naar de volgende sectie.

### <a name="2-source-prepare"></a>2 Bron voorbereiden 

Klik op de pagina **Bron voorbereiden** op **Configuratieserver**. 

1. Gebruik een EC2-instantie waarop Windows Server 2012 R2 wordt uitgevoerd om een configuratieserver te maken en deze te registreren bij uw recovery-kluis.

2. Configureer de proxy op de EC2-instantie-VM die u als de configuratieserver gebruikt zodat deze toegang heeft tot de [Service-URL's](../site-recovery-support-matrix-to-azure.md).

3. Download de [Geïntegreerde Setup van Microsoft Azure Site Recovery](http://aka.ms/unifiedinstaller_wus). U kunt dit downloaden naar uw lokale computer en vervolgens kopiëren naar de virtuele machine die u als de configuratieserver gebruikt.

4. Klik op de knop **Downloaden** om de kluisregistratiesleutel te downloaden. Kopieer het gedownloade bestand naar de virtuele machine die u als de configuratieserver gebruikt.

5. Klik op de virtuele machine met de rechtermuisknop op het installatieprogramma dat u hebt gedownload voor de **Geïntegreerde Setup van Microsoft Azure Site Recovery** en selecteer **Als administrator uitvoeren**. 

    1. Selecteer bij **Voordat u begint** de optie **De configuratieserver en processerver installeren** en klik op **Volgende**.
    2. Selecteer bij **Softwarelicentie van derden** de optie **Ik ga akkoord met de licentieovereenkomst van de derde partij.** en klik vervolgens op **Volgende**.
    3. Klik in **Registratie** op Bladeren en navigeer naar de locatie waar u het registratiesleutelbestand voor de kluis hebt geplaatst en klik op **Volgende**.
    4. Selecteer in **Internetinstellingen** de optie **Rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver.** en klik vervolgens op **Volgende**.
    5. Op de pagina **Controle op vereiste onderdelen** worden controles voor verschillende items uitgevoerd. Klik op **Volgende** als dit klaar is. 
    6. Geef in **MySQL-configuratie** de vereiste wachtwoorden op en klik op **Volgende**.
    7. Selecteer in **Details van de omgeving** de optie **Nee, VMware-machines hoeven niet te worden beveiligd** en klik op **Volgende**.
    8. Klik in **Installatielocatie** op **Volgende** om de standaardinstelling te accepteren.
    9. Klik in **Netwerkselectie** op **Volgende** om de standaardinstelling te accepteren.
    10. Klik in **Samenvatting** op **Installeren**.
    11. Bij **Voortgang van de installatie** ziet u informatie over hoe de installatie vordert. Klik op **Voltooien** als dit klaar is. Er wordt een pop-upvenster weergegeven met de melding dat er mogelijk opnieuw moet worden opgestart. Klik hierin op **OK**. Er wordt ook een pop-upvenster weergegeven met een melding over de wachtwoordzin voor verbinding met de configuratieserver. Kopieer de wachtwoordzin naar het Klembord en sla deze op een veilige locatie op.
    
6. Voer op de virtuele machine **cspsconfigtool.exe** uit om een of meer beheeraccounts op de configuratieserver te maken. Zorg ervoor dat de beheeraccounts beheerdersmachtigingen hebben voor de EC2-instanties die u wilt migreren. 

Wanneer u klaar bent met het instellen van de configuratieserver, gaat u terug naar de portal en selecteert u de server die u zojuist hebt gemaakt voor **Configuratieserver** en klikt u op *OK** om door te gaan naar stap 3 Doel voorbereiden.

### <a name="3-target-prepare"></a>3 Doel voorbereiden 

In deze sectie voert u gegevens in over de resources die u hebt gemaakt tijdens het doorlopen van de sectie [Azure-resources voorbereiden](#prepare-azure-resources) eerder in deze zelfstudie.

1. Selecteer bij **Abonnement** het Azure-abonnement dat u hebt gebruikt voor de zelfstudie [Azure voorbereiden](../tutorial-prepare-azure.md).
2. Selecteer **Resource Manager** als het implementatiemodel.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt. Dit moeten de resources zijn die u hebt gemaakt tijdens het doorlopen van de sectie [Azure-resources voorbereiden](#prepare-azure-resources) eerder in deze zelfstudie
4. Klik als u klaar bent op **OK**.


### <a name="4-replication-settings-prepare"></a>4 Replicatie-instellingen voorbereiden 

U moet een beleid voor wachtwoordreplicatie maken voordat u replicatie kunt inschakelen

1. Klik op **Repliceren en Koppelen**.
2. Typ bij **Naam** de tekst **myReplicationPolicy**.
3. Laat de rest van de standaardinstellingen ongewijzigd en klik op **OK** om het beleid te maken. Het nieuwe beleid wordt automatisch gekoppeld aan de configuratieserver. 

### <a name="5-deployment-planning-select"></a>5 Implementatieplanning selecteren 

Selecteer in **Hebt u de implementatieplanning uitgevoerd?** de optie **Dat doe ik later** in de vervolgkeuzelijst en klik vervolgens op **OK**.

Klik als u klaar bent met alle 5 secties van **Infrastructuur voorbereiden** op **OK**.


## <a name="enable-replication"></a>Replicatie inschakelen

Schakel replicatie in voor de machines die u wilt migreren. Wanneer replicatie is ingeschakeld, wordt de Mobility-service automatisch door Site Recovery geïnstalleerd. 

1. Open de [Azure Portal](htts://portal.azure.com).
1. Klik op de pagina voor uw kluis onder **Aan de slag** op **Site Recovery**.
2. Klik onder **Voor on-premises machines en Azure-VM's** op **Stap 1:toepassing repliceren**. Voltooi de wizardpagina's met de volgende informatie en klik op elke pagina op **OK** als u klaar bent:
    - 1 Bron configureren:
      
    |  |  |
    |-----|-----|
    | Bron: | **On-premises**|
    | Bronlocatie:| De naam van de EC2-instantie van uw configuratieserver.|
    |Machinetype: | **Fysieke machines**|
    | Processerver: | Selecteer de configuratieserver in de vervolgkeuzelijst.|
    
    - 2 Doel configureren
        
    |  |  |
    |-----|-----|
    | Doel: | Laat de standaardinstelling staan.|
    | Abonnement: | Selecteer het Azure-abonnement dat u momenteel gebruikt.|
    | Resourcegroep na failover:| Gebruik de resourcegroep die u hebt gemaakt in de sectie [Azure-resources voorbereiden](#prepare-azure-resources).|
    | Implementatiemodel na failover: | Kies **Resource Manager**|
    | Opslagaccount: | Kies de opslagaccount u hebt gemaakt in de sectie [Azure-resources voorbereiden](#prepare-azure-resources).|
    | Azure-netwerk: | Kies **Configureer deze nu voor geselecteerde machines**|
    | Azure-netwerk na failover: | Kies het netwerk dat u hebt gemaakt in de sectie [Azure-resources voorbereiden](#prepare-azure-resources).|
    | Subnet: | Selecteer de **standaardinstelling** in de vervolgkeuzelijst.|
    
    - 3 Fysieke machines selecteren
        
        Klik op **Fysieke machine** en voer vervolgens de **Naam**, het **IP-adres** en het **Type besturingssysteem** in van de EC2-instantie die u wilt migreren en klik op **OK**.
        
    - 4 Eigenschappen: eigenschappen configureren
        
        Selecteer de account die u hebt gemaakt op de configuratieserver in de vervolgkeuzelijst en klik op **OK**.
        
    - 5 Replicatie-instellingen: replicatie-instellingen configureren
    
        Zorg ervoor dat het replicatiebeleid is geselecteerd in de vervolgkeuzelijst **myReplicationPolicy** en klik vervolgens op **OK**.
        
3. Wanneer de wizard is voltooid, klikt u op **Replicatie inschakelen**.
        

U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Bewaking en rapporten** > **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.        
        
Wanneer u replicatie voor een virtuele machine inschakelt, kan het 15 minuten of langer duren voordat wijzigingen zijn doorgevoerd en in de portal worden weergegeven.

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Wanneer u een testfailover uitvoert, gebeurt het volgende:

1. Er wordt een controle uitgevoerd om na te gaan of aan alle vereiste voorwaarden voor failover wordt voldaan.
2. De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
3. Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

Voer de testfailover als volgt uit in de portal:

1. Ga op de pagina voor uw kluis naar **Beveiligde items** > **Gerepliceerde Items**> klik op de virtuele machine > **Failover testen**.

2. Selecteer een herstelpunt om voor de failover te gebruiken:
    - **Laatst verwerkte**: voert een failover van de VM uit naar het laatste herstelpunt dat is verwerkt door Site Recovery. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage RTO (Recovery Time Objective) is.
    - **Laatste toepassingsconsistente punt**: met deze optie wordt er een failover uitgevoerd van alle VM’s naar het laatste toepassingsconsistente herstelpunt. Het tijdstempel wordt weergegeven.
    - **Aangepast**: selecteer een herstelpunt.
3. Selecteer in **Failover testen** het Azure-doelnetwerk waarmee de virtuele Azure-machines moeten worden verbonden nadat de failover heeft plaatsgevonden. Dit moet het netwerk zijn dat u hebt gemaakt in de sectie [Azure-resources voorbereiden](#prepare-azure-resources).
4. Klik op **OK** om te beginnen met de failover. Als u de voortgang wilt volgen, klikt u op de virtuele machine en opent u de eigenschappen. U kunt ook op de taak **Failover testen** op de pagina voor uw kluis klikken in **Bewaking en rapporten** > **Taken** >
   **Site Recovery-taken**.
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in de Azure Portal > **Virtuele machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en of deze actief is.
6. Nu moet u verbinding maken met de gerepliceerde virtuele machine in Azure.
7. Verwijder Azure VM's die tijdens de testfailover zijn gemaakt door in het herstelplan op **Failovertest opschonen** te klikken. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op.

In sommige scenario's vereist de failover extra verwerking die circa acht tot tien minuten duurt. 


## <a name="migrate-to-azure"></a>Migreren naar Azure

Voer een werkelijke failover voor de EC2-instanties uit om deze naar virtuele Azure-machines te migreren. 

1. Klik in **Beveiligde items** > **Gerepliceerde items** op de AWS-instanties > **Failover**.
2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. Selecteer het meest recente herstelpunt.
3. Selecteer **Sluit de computer af voordat de failover wordt gestart** als u wilt dat Site Recovery probeert virtuele bronmachines af te sluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
4. Controleer of de virtuele machine wordt weergegeven in **Gerepliceerde items**. 
5. Klik met de rechtermuisknop op elke virtuele machine > **Migratie voltooien**. Hiermee wordt het migratieproces voltooid, de replicatie voor de AWS-VM gestopt en Site Recovery-facturering voor de virtuele machine gestopt.

    ![Migratie voltooien](./media/tutorial-migrate-aws-to-azure/complete-migration.png)

> [!WARNING]
> **Annuleer een failover die in voortgang is niet**: voordat de failover wordt gestart, wordt de VM-replicatie gestopt. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.  


    

## <a name="next-steps"></a>Volgende stappen

In dit onderwerp hebt u geleerd hoe u AWS EC2-instanties kunt migreren naar Azure VM's. Voor meer informatie over virtuele machines in Azure, gaat u verder met de zelfstudie voor virtuele Windows-machines.

> [!div class="nextstepaction"]
> [Zelfstudies over virtuele Windows-machines](../../virtual-machines/windows/tutorial-manage-vm.md)
