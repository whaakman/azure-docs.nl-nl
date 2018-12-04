---
title: Azure Stack-VM's repliceren naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgevallen naar Azure voor Azure Stack-VM's met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.topic: conceptual
ms.service: site-recovery
ms.date: 11/27/2018
ms.author: raynew
ms.openlocfilehash: 8285632d8dea76763c65dd06e8be2d7494a47188
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838987"
---
# <a name="replicate-azure-stack-vms-to-azure"></a>Azure Stack-VM's repliceren naar Azure

Dit artikel leest u over het instellen van herstel na noodgevallen Azure Stack-VM's naar Azure, met behulp van de [Azure Site Recovery-service](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).

Site Recovery draagt bij aan uw strategie voor zakelijke continuïteit en noodherstel herstel (BCDR). De service zorgt ervoor dat uw VM-workloads beschikbaar wanneer verwacht blijven en onverwachte storingen optreden.

- Site Recovery coördineert en beheert de replicatie van virtuele machines naar Azure storage.
- Wanneer er een storing optreedt op de primaire site, kunt u Site Recovery gebruiken om te fungeren als failover voor Azure.
- Azure-VM's zijn gemaakt op basis van de opgeslagen gegevens van de virtuele machine bij failover en gebruikers kunnen doorgaan met het openen van workloads die worden uitgevoerd op deze Azure-VM's.
- Als alles weer actief en werkend is, kunt u Azure-VM naar uw primaire site mislukt dit en beginnen met het repliceren naar Azure-opslag opnieuw.


In dit artikel leert u het volgende:

> [!div class="checklist"]
> * **Stap 1: Azure stack-virtuele machines voorbereiden voor replicatie**. Controleer of de VM's aan de Site Recovery voldoen en voor een installatie van Site Recovery Mobility service voorbereidt. Deze service is geïnstalleerd op elke virtuele machine die u wilt repliceren.
> * **Stap 2: Een Recovery Services-kluis instellen**. Een kluis voor Site Recovery instellen en opgeven wat u wilt repliceren. Site Recovery-onderdelen en acties worden geconfigureerd en beheerd in de kluis.
> * **Stap 3: De bronreplicatieomgeving instellen**. Instellen van een Site Recovery-configuratieserver. De configuratieserver is één Azure Stack-VM die wordt uitgevoerd van alle onderdelen die nodig zijn door Site Recovery. Nadat u de configuratieserver hebt ingesteld, registreert u deze in de kluis.
> * **Stap 4: De replicatiedoelomgeving instellen**. Selecteer uw Azure-account en de Azure storage-account en het netwerk dat u wilt gebruiken. Tijdens de replicatie, wordt de VM-gegevens naar Azure-opslag gekopieerd. Na een failover worden virtuele Azure-machines zijn gekoppeld aan het opgegeven netwerk.
> * **Stap 5: Replicatie inschakelen**. Replicatie-instellingen configureren en inschakelen van replicatie voor virtuele machines. De Mobility-service wordt geïnstalleerd op een virtuele machine wanneer replicatie is ingeschakeld. Site Recovery voert een initiële replicatie van de virtuele machine uit en klikt u vervolgens de voortdurende replicatie begint.
> * **Stap 6: Een Dr-herstelanalyse uitvoeren**: nadat replicatie actief en werkend is, u controleren dat failover werkt zoals verwacht door het uitvoeren van een detailanalyse. Voor het starten van de analyse, kunt u een testfailover uitvoeren in Site Recovery. De testfailover niet van invloed zijn op uw productieomgeving.

Met deze stappen is voltooid, kunt u vervolgens een volledige failover uitvoeren naar Azure als en als u wilt.

## <a name="architecture"></a>Architectuur

![Architectuur](./media/azure-stack-site-recovery/architecture.png)

**Locatie** | **Onderdeel** |**Details**
--- | --- | ---
**Configuratieserver** | Wordt uitgevoerd op een enkele virtuele machine een Azure-Stack. | In elk abonnement moet u de virtuele machine van een configuratieserver instellen. Deze virtuele machine wordt uitgevoerd de volgende Site Recovery-onderdelen:<br/><br/> -Server configuratie: coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie. -Server proces: fungeert als replicatiegateway. Deze ontvangt replicatiegegevens, optimaliseert met caching, compressie en codering; en verzendt dit naar Azure storage.<br/><br/> Als VM's die u wilt repliceren van de hieronder vermelde limieten overschrijdt, kunt u een afzonderlijke zelfstandige processerver instellen. [Meer informatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-process-server-scale).
**Mobility-service** | Geïnstalleerd op elke virtuele machine die u wilt repliceren. | In de stappen in dit artikel wordt een account voorbereiden, zodat de Mobility-service wordt automatisch geïnstalleerd op een virtuele machine wanneer replicatie is ingeschakeld. Als u niet wilt dat de service automatisch te installeren, zijn er een aantal andere methoden die u kunt gebruiken. [Meer informatie](https://docs.microsoft.com/azure/site-recovery/vmware-azure-install-mobility-service).
**Azure** | U moet een Recovery Services-kluis, een storage-account en een virtueel netwerk in Azure. |  Gerepliceerde gegevens worden opgeslagen in de storage-account. Azure-VM's worden toegevoegd aan het Azure-netwerk wanneer een failover optreedt. 


Replicatie werkt als volgt:

1. In de kluis geeft u de replicatiebron en het doel, instellen van de configuratieserver maken van beleid voor replicatie en replicatie inschakelen.
2. De Mobility-service is geïnstalleerd op de computer (als u bekend bent met het push-installatie) en machines beginnen replicatie volgens het replicatiebeleid.
3. Een initiële kopie van de server-gegevens wordt gerepliceerd naar Azure storage.
4. Nadat de initiële replicatie is voltooid, begint de replicatie van deltaverschillen naar Azure. Bijgehouden wijzigingen voor een machine worden opgeslagen in een .hrl-bestand.
5. De configuratieserver coördineert het replicatiebeheer met Azure (HTTPS-poort 443 uitgaande).
6. De processerver ontvangt gegevens van de bronmachines, optimaliseert en versleutelt deze en verzendt ze naar Azure storage (poort 443, uitgaand).
7. Gerepliceerde machines communiceren met de configuratieserver (poort 443 voor HTTPS inkomend, voor het replicatiebeheer van. Machines verzenden replicatiegegevens naar de processerver (poort 9443 HTTPS inkomend - kunnen worden gewijzigd).
8. Verkeer wordt via internet gerepliceerd naar de openbare eindpunten van Azure Storage. U kunt ook kunt u openbare Azure ExpressRoute-peering. Het repliceren van verkeer via een site-naar-site-VPN van een on-premises site naar Azure wordt niet ondersteund.

## <a name="prerequisites"></a>Vereisten

Dit is wat u nodig hebt voor het instellen van dit scenario.

**Vereiste** | **Details**
--- | ---
**Account voor Azure-abonnement** | Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.
**Machtigingen voor Azure-account** | De Azure account dat u gebruikt, moeten toegangsmachtigingen voor:<br/><br/> -Een Recovery Services-kluis maken<br/><br/> -Een virtuele machine maken in de resourcegroep en het virtuele netwerk dat u voor het scenario gebruikt<br/><br/> -Schrijven naar het opslagaccount dat u opgeeft<br/><br/> Houd rekening met het volgende:<br/><br/> -Als u een account maakt, kunt u de beheerder van uw abonnement bent en alle acties kan uitvoeren.<br/><br/> -Als u een bestaand abonnement gebruiken en u niet de beheerder bent, moet u contact op met de beheerder om u de eigenaar of bijdrager machtigingen.<br/><br/> -Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](https://docs.microsoft.com/azure/site-recovery/site-recovery-role-based-linked-access-control). 
**Azure Stack VM** | U moet een Azure Stack-VM in de tenantabonnement, die worden geïmplementeerd als de Site Recovery-configuratieserver. 


### <a name="prerequisites-for-the-configuration-server"></a>Vereisten voor de configuratieserver

[!INCLUDE [site-recovery-config-server-reqs-physical](../../includes/site-recovery-config-server-reqs-physical.md)]


 
## <a name="step-1-prepare-azure-stack-vms"></a>Stap 1: Azure Stack-virtuele machines voorbereiden

### <a name="verify-the-operating-system"></a>Controleer of het besturingssysteem

Zorg ervoor dat de virtuele machines worden uitgevoerd een van de besturingssystemen die worden samengevat in de tabel.


**Besturingssysteem** | **Details**
--- | ---
**64-bits Windows** | WindowsServer 2016, Windows Server 2012 R2, WindowsServer 2012, Windows Server 2008 R2 (van SP1)
**CentOS** | 5.2 naar 5,11, 6.1 naar 6,9, 7.0-7.3
**Ubuntu** | 14.04 server LTS, 16.04 LTS-server. Beoordeling [kernels ondersteund](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#ubuntu-kernel-versions)

### <a name="prepare-for-mobility-service-installation"></a>Voorbereiden voor de installatie van de Mobility-service

Elke virtuele machine die u wilt repliceren, moet de Mobility-service geïnstalleerd hebben. Controleer of de instellingen van de virtuele machine in de volgorde voor de processerver om de service wordt automatisch op de virtuele machine installeren wanneer replicatie is ingeschakeld.

#### <a name="windows-machines"></a>Windows-machines

- U moet de netwerkverbinding tussen de virtuele machine waarop de replicatie in te schakelen en de machine met de processerver (standaard is dit de configuratieserver VM).
- U moet een account met beheerdersrechten (domein of lokaal) op de computer waarvoor u replicatie inschakelt.
    - U geeft dit account bij het instellen van Site Recovery. De processerver gebruikt vervolgens dit account voor het installeren van de Mobility-service wanneer replicatie is ingeschakeld.
    - Dit account wordt alleen worden gebruikt door Site Recovery voor de push-installatie en voor het bijwerken van de Mobility-service.
    - Als u niet een domeinaccount gebruikt, moet u toegangsbeheer voor externe gebruikers op de virtuele machine uitschakelen:
        - In het register, maakt u de DWORD-waarde **LocalAccountTokenFilterPolicy** onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System.
        - Stel de waarde in op 1.
        - Om dit te doen bij de opdrachtprompt, typt u het volgende: **REG toevoegen HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1**.
- In de Windows Firewall op de virtuele machine die u wilt repliceren, bestands- en printerdeling- en WMI-toestaan.
    - U doet dit door uitvoeren **wf.msc** om de console Windows Firewall te openen. Klik met de rechtermuisknop **regels voor binnenkomende verbindingen** > **nieuwe regel**. Selecteer **vooraf gedefinieerde**, en kies **van bestanden en printers delen** in de lijst. De wizard hebt voltooid, selecteert u de verbinding toestaan > **voltooien**.
    - Voor computers in het domein, kunt u een groepsbeleidsobject om dit te doen.

    
#### <a name="linux-machines"></a>Linux-machines

- Zorg dat er een netwerkverbinding is tussen de Linux-computer en de processerver.
- Op de computer waarvoor u replicatie inschakelt, moet u een account dat een hoofdgebruiker zijn op de Linux-bronserver:
    - U geeft dit account bij het instellen van Site Recovery. De processerver gebruikt vervolgens dit account voor het installeren van de Mobility-service wanneer replicatie is ingeschakeld.
    - Dit account wordt alleen worden gebruikt door Site Recovery voor de push-installatie en voor het bijwerken van de Mobility-service.
- Controleer of het bestand /etc/hosts op de Linux-bronserver vermeldingen bevat die de lokale hostnaam toewijzen aan IP-adressen die zijn gekoppeld aan alle netwerkadapters.
- Installeer de meest recente openssh, openssh-server en openssl-pakketten op de computer die u wilt repliceren.
- Zorg ervoor dat SSH (Secure Shell) is ingeschakeld en wordt uitgevoerd op poort 22.
- Schakel het SFTP-subsysteem en wachtwoordverificatie in het bestand sshd_config in:
    1. Om dit te doen, moet u zich aanmelden als root.
    2. Zoek de regel die met begint **PasswordAuthentication**, in het bestand /etc/ssh/sshd_config. Verwijder opmerkingen bij de regel en wijzig de waarde in **ja**.
    3. Zoek de regel die begint met **Subsystem** en verwijder opmerkingen bij de regel.

        ![Linux Mobility-service](./media/azure-stack-site-recovery/linux-mobility.png)

    4. Start de service sshd.


### <a name="note-the-vm-private-ip-address"></a>Houd er rekening mee de privé IP-adres van VM

Voor elke machine die u wilt repliceren, moet u de IP-adres vinden:

1. Klik in de Azure Stack-Portal op de virtuele machine.
2. Op de **Resource** menu, klikt u op **netwerkinterfaces**.
3. Noteer het particuliere IP-adres.

    ![Privé IP-adres](./media/azure-stack-site-recovery/private-ip.png)


## <a name="step-2-create-a-vault-and-select-a-replication-goal"></a>Stap 2: Een kluis maken en een replicatiedoel selecteren

1. Selecteer in Azure Portal **Een resource maken** > **Controle en beheer** > **Backup en Site Recovery**.
2. Voer in **Naam** een beschrijvende naam in om de kluis aan te duiden. 
3. In **resourcegroep**, maak of Selecteer een resourcegroep. We maken gebruik van **contosoRG**.
4. In **locatie**, voer de Azure-regio. gebruiken we **Europa - west**.
5. Voor snelle toegang tot de kluis vanuit het dashboard, selecteert u **Aan dashboard vastmaken** > **Maken**.

   ![Een nieuwe kluis maken](./media/azure-stack-site-recovery/new-vault-settings.png)

   De nieuwe kluis wordt weergegeven in **Dashboard** > **Alle resources** en op de hoofdpagina **Recovery Services-kluizen**.

### <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren

1. In **Recovery Services-kluizen** > Geef de kluisnaam van een. We maken gebruik van **ContosoVMVault**.
2. Selecteer in **Aan de slag** Site Recovery. Selecteer vervolgens **Infrastructuur voorbereiden**.
3. In **Beveiligingsdoel** > **Waar bevinden de machines zich**, selecteert u **On-premises**.
4. In **Waarnaartoe wilt u de machines repliceren** selecteert u **Naar Azure**.
5. In **zijn de machines gevirtualiseerd**, selecteer **niet gevirtualiseerd/Overig**. Selecteer vervolgens **OK**.

    ![Beveiligingsdoel](./media/azure-stack-site-recovery/protection-goal.png)

## <a name="step-3-set-up-the-source-environment"></a>Stap 3: De bronomgeving instellen

De configuratie van server-machine instelt, in de kluis te registreren en detecteren van computers die u wilt repliceren.

1. Klik op **Infrastructuur voorbereiden** > **Bron**.
2. Klik in **Bron voorbereiden** op **+Configuratieserver**.

    ![Bron instellen](./media/azure-stack-site-recovery/plus-config-srv.png)

3. In **-Server toevoegen**, controleert u of **configuratieserver** wordt weergegeven in **servertype**.
5. Download het installatiebestand van de geïntegreerde Setup van Site Recovery.
6. Download de registratiesleutel voor de kluis. Wanneer u geïntegreerde Setup uitvoert moet u de registratiesleutel. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Bron instellen](./media/azure-stack-site-recovery/set-source2.png)


### <a name="run-azure-site-recovery-unified-setup"></a>Voer Azure Site Recovery van geïntegreerde Setup

Als u wilt installeren en registreren van de configuratieserver, doen een RDP-verbinding met de virtuele machine die u wilt gebruiken voor de configuratieserver en geïntegreerde Setup uitgevoerd.

Voordat u begint, zorg ervoor dat de klok [gesynchroniseerd met een tijdserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) op de virtuele machine voordat u begint. Installatie mislukt als de tijd meer dan vijf minuten afwijkt van lokale tijd is.

Nu de configuratieserver installeren:

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> De configuratieserver kan ook worden geïnstalleerd vanaf de opdrachtregel. [Meer informatie](physical-manage-configuration-server.md#install-from-the-command-line).

> Het duurt 15 minuten of langer voordat de accountnaam wordt weergegeven in de portal. Selecteer **Configuratieservers** > ***servernaam*** > **Server vernieuwen** om direct bij te werken.

## <a name="step-4-set-up-the-target-environment"></a>Stap 4: De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. In **infrastructuur voorbereiden** > **doel**, selecteer het Azure-abonnement u wilt gebruiken.
2. Geef het doel-implementatiemodel.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt. Als deze niet vinden, moet u ten minste één opslagaccount en een virtueel netwerk maken om de wizard hebt voltooid.


## <a name="step-5-enable-replication"></a>Stap 5: Replicatie inschakelen

### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Klik op **infrastructuur voorbereiden** > **replicatie-instellingen**.
2. Geef in **Replicatiebeleid maken** een beleidsnaam op.
3. Geef in **RPO-drempelwaarde** de limiet van de Recovery Point Objective (RPO) op.
    - Herstelpunten voor gerepliceerde gegevens worden gemaakt in overeenstemming met de tijd instellen.
    - Deze instelling heeft geen invloed op replicatie, continue is. Het gewoon problemen met een waarschuwing als de drempelwaarde is bereikt zonder dat een herstelpunt wordt gemaakt.
4. In **bewaarperiode voor herstelpunten**, opgeven hoe lang elk herstelpunt wordt bewaard. Gerepliceerde VM's kunnen worden hersteld naar een willekeurig punt in de opgegeven periode.
5. In **frequentie App-consistente momentopname**, Geef op hoe vaak toepassingsconsistente momentopnamen worden gemaakt.

    - Een app-consistente momentopname is een point-in-time-momentopname van de app-gegevens binnen de virtuele machine.
    - Volume Shadow Copy Service (VSS) zorgt ervoor dat apps op de virtuele machine in een consistente status hebben wanneer de momentopname wordt gemaakt.
6. Selecteer **OK** om het beleid te maken.


### <a name="confirm-deployment-planning"></a>Implementatieplanning bevestigen

U kunt deze stap nu overslaan. In **implementatieplanning** vervolgkeuzelijst, klikt u op **Ja, heb ik gedaan**.



### <a name="enable-replication"></a>Replicatie inschakelen

Zorg ervoor dat u hebt voltooid, de taken in [stap 1: voorbereiden machine](#step-1-prepare-azure-stack-vms). Schakel replicatie vervolgens als volgt in:

1. Selecteer **Toepassing repliceren** > **Bron**.
2. Selecteer in **Bron** de configuratieserver.
3. In **type Machine**, selecteer **fysieke machines**.
4. Selecteer de processerver (configuratieserver). Klik vervolgens op **OK**.
5. In **doel**, selecteer het abonnement en de resourcegroep waarin u wilt maken van de virtuele machines na een failover. Kies het implementatiemodel dat u wilt gebruiken voor de failover-VM's.
6. Selecteer het Azure storage-account waarin u wilt gerepliceerde gegevens opgeslagen.
7. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt.
8. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **later configureren** als u wilt het Azure-netwerk voor elke computer afzonderlijk selecteren.
9. In **fysieke Machines**, klikt u op **fysieke machine**. Geef de naam van IP-adres van elke computer en het besturingssysteem die u wilt repliceren.

    - Gebruik het interne IP-adres van de machine.
    - Als u opgeeft in dat de openbare IP-adres replicatie werkt mogelijk niet zoals verwacht.

10. In **eigenschappen** > **eigenschappen configureren**, selecteert u het account dat de processerver wordt gebruikt voor het installeren van Mobility Service automatisch op de machine.
11. In **replicatie-instellingen** > **replicatie-instellingen configureren**, Controleer of het juiste replicatiebeleid is geselecteerd.
12. Klik op **Replicatie inschakelen**.
13. Voortgang bijhouden van de **beveiliging inschakelen** taak **instellingen** > **taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.

> [!NOTE]
> Site Recovery installeert Mobility Service wanneer replicatie wordt ingeschakeld voor een VM.

> Het kan 15 minuten of langer duren voordat wijzigingen zijn doorgevoerd en in de portal worden weergegeven.

> Voor het bewaken van virtuele machines die u toevoegt, controleert u de laatste detectietijd voor VM's in **Configuratieservers** > **Laatst contact om**. Als u VM's wilt toevoegen zonder te wachten op de geplande detectie, markeert u de configuratieserver (zonder deze te selecteren), en selecteert u **Vernieuwen**.


## <a name="step-6-run-a-disaster-recovery-drill"></a>Stap 6: Een Dr-herstelanalyse uitvoeren

U voert een testfailover uit naar Azure om ervoor te zorgen dat alles werkt zoals verwacht. Deze failover heeft geen invloed op uw productieomgeving.

### <a name="verify-machine-properties"></a>Controleer de eigenschappen van de machine

Voordat u een failovertest uitvoert, Controleer de eigenschappen van de machine en zorg ervoor dat ze voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements). U kunt weergeven en eigenschappen als volgt wijzigen:

1. Klik in **Beveiligde items** op **Gerepliceerde items** > VM.
2. In het deelvenster **Gerepliceerd item** bevindt zich een overzicht van VM-informatie, status, en de laatste beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.
3. In **berekening en netwerk**, instellingen zo nodig wijzigen.

    - U kunt de Azure VM-naam, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md), en beheerde-Schijfinstellingen.
    - U kunt ook bekijken en wijzigen van netwerkinstellingen. Het gaat hierbij om het netwerk/subnet waaraan de virtuele Azure-machine na een failover en het IP-adres dat wordt toegewezen aan de virtuele machine is gekoppeld.
1. In **schijven**, informatie over het besturingssysteem en gegevensschijven weergeven op de virtuele machine.
   

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Wanneer u een testfailover uitvoert, gebeurt het volgende:

1. Er wordt een controle uitgevoerd om na te gaan of aan alle vereiste voorwaarden voor failover wordt voldaan.
2. De failover verwerkt de gegevens met behulp van het opgegeven herstelpunt:
    - **Laatst verwerkte**: de machine een failover naar de meest recente herstelpunt wordt verwerkt door Site Recovery. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage RTO (Recovery Time Objective) is.
    - **Laatste toepassingsconsistente**. De machine failover-schakeling naar de meest recente toepassingsconsistente herstelpunt.
    - **Aangepaste**. Selecteer het herstelpunt dat is gebruikt voor de failover.

3. Een Azure-VM wordt gemaakt met behulp van de verwerkte gegevens.
4. Test-failover kan automatisch opschonen van virtuele Azure-machines die zijn gemaakt tijdens de analyse.

Voer een failovertest uitvoeren voor een virtuele machine als volgt uit:

1. Klik in **Instellingen** > **Gerepliceerde items** op de VM **+Failover testen**.
2. Voor dit scenario, selecteren we gebruiken de **laatst verwerkte** herstelpunt. 
3. In **Testfailover**, selecteert u de doel-Azure-netwerk.
4. Klik op **OK** om te beginnen met de failover.
5. De voortgang bijhouden door te klikken op de virtuele machine om de eigenschappen te openen. Of klik op de **Testfailover** taak *kluisnaam* > **instellingen** > **taken**  > **Site Recovery-taken**.
6. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in Azure Portal > **Virtuele machines**. Controleer of de virtuele machine de juiste grootte heeft, verbonden met het juiste netwerk en wordt uitgevoerd.
7. Nu moet u verbinding maken met de gerepliceerde virtuele machine in Azure. [Meer informatie](https://docs.microsoft.com/azure/site-recovery/site-recovery-test-failover-to-azure#prepare-to-connect-to-azure-vms-after-failover).
8. Als u VM's van Azure wilt verwijderen die tijdens de failovertest zijn gemaakt, klikt u in de VM op **Failovertest opschonen**. In **opmerkingen bij de**, eventuele opmerkingen die zijn gekoppeld aan de testfailover opslaan.

## <a name="fail-over-and-fail-back"></a>Failover en failback

Nadat u replicatie hebt ingesteld en uitvoeren van een detailanalyse om te controleren of alles goed werkt, kunt u machines voor failover naar Azure als vereist.

Voordat u een failover uitvoeren als u wilt verbinding maken met de machine in Azure na de failover vervolgens [voorbereiden om verbinding te](https://docs.microsoft.com/azure/site-recovery/site-recovery-test-failover-to-azure#prepare-to-connect-to-azure-vms-after-failover) voordat u begint.

Voer een failover als volgt:


1. In **instellingen** > **gerepliceerde Items**, klikt u op de machine > **Failover**.
2. Selecteer het herstelpunt dat u wilt gebruiken.
3. In **Testfailover**, selecteert u de doel-Azure-netwerk.
4. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Met deze instelling kan Site Recovery wordt geprobeerd om de bron-VM voordat u begint met de failover af te sluiten. Maar de failover wordt voortgezet zelfs als het afsluiten is mislukt. 
5. Klik op **OK** om te beginnen met de failover. U kunt de voortgang van de failover volgen op de pagina **Taken**.
6. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in Azure Portal > **Virtuele machines**. Als u voorbereid om na een failover verbinding te maken, Controleer of de virtuele machine de juiste grootte heeft, verbonden met het juiste netwerk en het uitvoeren van.
7. Klik na het verifiëren van de virtuele machine op **doorvoeren** de failover te voltooien. Hiermee verwijdert u alle beschikbare herstelpunten.

> [!WARNING]
> Annuleer een failover wordt uitgevoerd niet: voordat de failover wordt gestart, wordt de VM-replicatie gestopt. Als u een failover die in voortgang is annuleert, wordt de failover gestopt, maar de VM wordt niet meer gerepliceerd.


### <a name="fail-back-to-azure-stack"></a>Een failback uitvoeren naar Azure Stack

Als u primaire site opnieuw actief is, kunt u failback van Azure naar Azure Stack. Om dit te doen, moet u de Azure VM VHD downloaden en te uploaden naar Azure Stack.

1. Sluit de Azure-VM, zodat de VHD kan worden gedownload. 
2. Als u wilt beginnen met het downloaden van de VHD, installeer [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).
3. Navigeer naar de virtuele machine in de Azure-Portal (met de naam van de virtuele machine).
4. In **schijven**, klikt u op de naam van de schijf en instellingen verzamelen.

    - Een voorbeeld: de VHD-URI in onze test gebruikt: https://502055westcentralus.blob.core.windows.net/wahv9b8d2ceb284fb59287/copied-3676553984.vhd kan worden opgesplitst om op te halen van de volgende invoerparameters die worden gebruikt voor het downloaden van de VHD.
        - Storage-Account: 502055westcentralus
        - Container: wahv9b8d2ceb284fb59287
        - Naam van de VHD: gekopieerd-3676553984.vhd

5. Azure Storage Explorer nu gebruiken voor het downloaden van de VHD.
6. De VHD uploaden naar Azure Stack met [stappen](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-manage-vm-disks#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm).
7. In de bestaande virtuele machine of een nieuwe virtuele machine, koppelt u de geüploade VHD's.
8. Controleer of de Besturingssysteemschijf juist is en start de virtuele machine.


In dit stadium is failback voltooid.


## <a name="conclusion"></a>Conclusie

In dit artikel wordt Azure Stack-VM's worden gerepliceerd naar Azure. Met replicatie plaats, maar er is een Dr-herstelanalyse om te controleren of failover naar Azure gewerkt zoals verwacht. Het artikel bevat ook stappen voor het uitvoeren van een volledige failover naar Azure, en Failover-overschakeling terug naar Azure Stack.

## <a name="next-steps"></a>Volgende stappen

Na de failback, kunt u de virtuele machine opnieuw beveiligen en start deze repliceert naar Azure opnieuw om dit te doen, herhaalt u de stappen in dit artikel.

