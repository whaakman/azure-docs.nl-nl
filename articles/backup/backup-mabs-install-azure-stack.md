---
title: Azure Backup-Server installeren op Azure Stack | Microsoft Docs
description: Azure Backup Server gebruiken om te beveiligen of back-up van workloads in Azure Stack.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: Azure Backup-server; bescherming van workloads; back-up van workloads
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 6/5/2018
ms.author: markgal
ms.openlocfilehash: 14379d82b4b60c0ea555388ac61c9c7b8fbe9e4f
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721427"
---
# <a name="install-azure-backup-server-on-azure-stack"></a>Azure Backup Server installeren op Azure Stack

In dit artikel wordt uitgelegd hoe u Azure Backup Server installeren op Azure Stack. Met Azure Backup Server, kunt u de infrastructuur beveiligen als een Service (IaaS)-werkbelastingen, zoals virtuele machines die worden uitgevoerd in Azure Stack. Een voordeel van het gebruik van Azure Backup Server ter bescherming van uw workloads is dat kunt u alle werkbelastingsbeveiliging beheren vanaf één console.

> [!NOTE]
> Raadpleeg voor meer informatie over mogelijkheden voor beveiliging, [documentatie voor Azure Backup-beveiligingsfuncties](backup-azure-security-feature.md).
>

## <a name="azure-backup-server-protection-matrix"></a>Beveiligingsmatrix voor Azure Backup Server
Azure Backup Server beveiligt de volgende werkbelastingen van de Azure Stack-virtuele machine.

| Beveiligde gegevensbron | Beveiliging en herstel |
| --------------------- | ----------------------- |
| Windows Server Semi-Annual-kanaal - Enterprise-Datacenter/Standard | Volumes, bestanden, mappen |
| WindowsServer 2016 - Enterprise-Datacenter/Standard | Volumes, bestanden, mappen |
| Windows Server 2012 R2 - Enterprise-Datacenter/Standard | Volumes, bestanden, mappen |
| WindowsServer 2012: Datacenter/Entprise/Standard | Volumes, bestanden, mappen |
| Windows Server 2008 R2 - Enterprise-Datacenter/Standard | Volumes, bestanden, mappen |
| SQL Server 2016 | Database |
| SQL Server 2014 | Database |
| SQL Server 2012 SP1 | Database |
| SharePoint 2016 | Farm, database, front-end, webserver |
| SharePoint 2013 | Farm, database, front-end, webserver |
| SharePoint 2010 | Farm, database, front-end, webserver |

## <a name="prerequisites-for-the-azure-backup-server-environment"></a>Vereisten voor de Azure Backup Server-omgeving

Houd rekening met de aanbevelingen in deze sectie bij het installeren van Azure Backup Server in uw Azure Stack-omgeving. De Azure Backup Server wordt gecontroleerd dat uw omgeving de benodigde vereisten heeft, maar u tijd besparen kunt door voorbereiden voordat u installeert.

### <a name="determining-size-of-virtual-machine"></a>Grootte van virtuele machine vaststellen
Azure Backup Server op een virtuele machine van Azure Stack uitgevoerd, gebruikt u de grootte A2 of hoger. Download voor hulp bij het kiezen van de grootte van een virtuele machine, de [calculator voor Azure Stack-VM-grootte](https://www.microsoft.com/download/details.aspx?id=56832).

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Virtuele netwerken op virtuele machines van Azure Stack
Alle virtuele machines die worden gebruikt in een Azure Stack-workload moet behoren tot de dezelfde Azure-netwerk en Azure-abonnement.

### <a name="azure-backup-server-vm-performance"></a>Azure Backup Server-VM-prestaties
Als gedeeld met andere virtuele machines, het opslagaccount grootte en IOPS-limieten impact Azure Backup Server VM-prestaties. Daarom moet u een afzonderlijk opslagaccount voor de virtuele machine van Azure Backup Server gebruiken. De Azure Backup-agent die wordt uitgevoerd op de Azure Backup Server, heeft tijdelijke opslag voor:
- eigen gebruik nodig (een cachelocatie)
- gegevens die zijn hersteld vanuit de cloud (lokaal faseringsgebied)

### <a name="configuring-azure-backup-temporary-disk-storage"></a>Azure Backup tijdelijke schijfopslag configureren
Elke virtuele machine van Azure Stack wordt geleverd met tijdelijke disk-opslag, die beschikbaar is voor de gebruiker als volume `D:\`. Het lokale faseringsgebied dat Azure Backup nodig kan worden geconfigureerd dat het zich `D:\`, en de cachelocatie kan worden geplaatst op `C:\`. Op deze manier moet er is geen opslag verzinken weg van de gegevensschijven die zijn gekoppeld aan de virtuele machine van Azure Backup Server.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Back-upgegevens opslaan op de lokale schijf en in Azure
Azure Backup-Server slaat de back-upgegevens op Azure-schijven die zijn gekoppeld aan de virtuele machine, voor operationeel herstel. Nadat de schijven en de opslagruimte zijn gekoppeld aan de virtuele machine, beheert Azure Backup Server opslag voor u. De hoeveelheid back-upgegevens opslag is afhankelijk van het aantal en de grootte van schijven die zijn gekoppeld aan elk [virtuele machine van Azure Stack](../azure-stack/user/azure-stack-storage-overview.md). Elke grootte van virtuele machine in Azure Stack is een maximum aantal schijven dat kan worden gekoppeld aan de virtuele machine. A2 is bijvoorbeeld vier schijven. A3 is acht schijven. A4 is 16 schijven. Bepaalt de back-upopslag van de totale nogmaals, de grootte en het aantal schijven.

> [!IMPORTANT]
> U moet **niet** operationele herstelgegevens (back-up) op Azure Backup-Server-gekoppelde schijven meer dan vijf dagen bewaren.
>

Back-upgegevens opslaan in Azure, vermindert een back-upinfrastructuur in Azure Stack. Als gegevens meer dan vijf dagen oud is, moet deze worden opgeslagen in Azure.

Voor het opslaan van back-upgegevens in Azure, maak of gebruik een Recovery Services-kluis. Bij het maken van back-up van de Azure Backup Server-workload u [configureren van de Recovery Services-kluis](backup-azure-microsoft-azure-backup.md#create-a-recovery-services-vault). Wanneer geconfigureerd, telkens wanneer die een back-uptaak wordt uitgevoerd, wordt er een herstelpunt gemaakt in de kluis. Elke Recovery Services-kluis bevat maximaal 9999 herstelpunten. Afhankelijk van het aantal herstelpunten die zijn gemaakt en hoe lang ze worden bewaard, kunt u back-upgegevens behouden jaren. U kunt bijvoorbeeld maandelijks herstelpunten maken en deze vijf jaar bewaren.
 
### <a name="scaling-deployment"></a>Schalen van implementatie
Als u de schaal van uw implementatie wilt, hebt u de volgende opties:
  - Opschalen: verhoog de grootte van de Azure Backup Server virtuele machine van een reeks D-serie en verhogen van de lokale opslag [per de instructies van de virtuele machine Azure Stack](../azure-stack/user/azure-stack-manage-vm-disks.md).
  - Gegevens overdragen via: verzend oudere gegevens naar Azure en bewaar alleen de nieuwste gegevens op de opslag die is gekoppeld aan de Azure Backup-Server.
  - Uitschalen - toevoegen van meer back-up van Azure-Servers om de werkbelastingen te beveiligen.

### <a name="net-framework"></a>.NET Framework

.NET framework 3.5 SP1 of hoger moet zijn geïnstalleerd op de virtuele machine.

### <a name="joining-a-domain"></a>Deelnemen aan een domein

De virtuele machine van Azure Backup Server moet worden toegevoegd aan een domein. Een gebruiker met beheerdersrechten moet Azure Backup Server installeren op de virtuele machine.

## <a name="using-an-iaas-vm-in-azure-stack"></a>Met behulp van een IaaS-VM in Azure Stack

Bij het kiezen van een server voor Azure Backup Server, kunt u beginnen met een installatiekopie van de galerie met Windows Server 2012 R2 Datacenter of Windows Server 2016 Datacenter. Het artikel [uw eerste Windows-machine maken in Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), bevat een zelfstudie voor aan de slag met de aanbevolen virtuele machine. De aanbevolen minimale vereisten voor de server-machine (VM) moeten zijn: Standard A2 met twee kernen en 3,5 GB RAM-geheugen.

Werklasten beveiligen met Azure Backup Server heeft veel aspecten. Het artikel [DPM installeren als een virtuele machine van Azure](https://technet.microsoft.com/library/jj852163.aspx), helpt deze aspecten uitgelegd. Lees dit artikel volledig voordat het implementeren van de machine.

> [!NOTE]
> Azure Backup Server is ontworpen om uit te voeren op een virtuele machine van toegewezen, één doel. U kunt Azure Backup Server niet installeren op:
> - Een computer die wordt uitgevoerd als een domeincontroller
> - Een computer waarop de toepassingsserverfunctie is geïnstalleerd
> - Een computer waarop Exchange Server wordt uitgevoerd
> - Een computer die een knooppunt van een cluster is

Altijd Azure Backup Server toevoegen aan een domein. Als u Azure Backup Server verplaatst naar een ander domein, eerst Azure Backup Server installeren, en voeg deze toe aan het nieuwe domein. Nadat u Azure Backup Server implementeert, kunt u deze niet verplaatsen naar een nieuw domein.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Opslagreplicatie instellen

De optie Recovery Services-kluis voor opslagreplicatie kunt u kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Recovery Services-kluizen gebruiken standaard geografisch redundante opslag. Als deze kluis uw primaire kluis, laat u de opslagoptie die is ingesteld op geografisch redundante opslag. Kies lokaal redundante opslag als u een goedkopere optie wilt die minder duurzaam is. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy-grs.md) en [lokaal redundante](../storage/common/storage-redundancy-lrs.md) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

1. Selecteer uw kluis om het dashboard van de kluis en het menu instellingen te openen. Als de **instellingen** menu niet wordt geopend, klikt u op **alle instellingen** in het dashboard van de kluis.
2. Op de **instellingen** menu, klikt u op **back-upinfrastructuur** > **back-upconfiguratie** openen de **back-upconfiguratie**menu. Op de **back-upconfiguratie** menu, kiest u de optie voor opslagreplicatie voor uw kluis.

    ![Lijst met back-upkluizen](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

## <a name="download-azure-backup-server-installer"></a>Azure Backup Server-installatieprogramma downloaden

Er zijn twee manieren om te downloaden van het installatieprogramma van Azure Backup Server. U kunt de Azure Backup Server installer uit downloaden de [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55269). Als u een Recovery Services-kluis wilt configureren, kunt u het installatieprogramma van Azure Backup Server downloaden. De volgende stappen helpen u bij het downloaden van het installatieprogramma van de Azure-portal tijdens het configureren van een Recovery Services-kluis.

1. Van uw virtuele machine van Azure Stack, [aanmelden bij uw Azure-abonnement in Azure portal](https://portal.azure.com/).
2. Selecteer in het menu links **alle Services**.

    ![Kies de optie alle Services in in het hoofdmenu](./media/backup-mabs-install-azure-stack/click-all-services.png)

3. In de **alle services** dialoogvenster, type *herstelservices*. Als u te typen begint, filtert de invoer van de lijst met resources. Als u dit ziet, selecteert u **Recovery Services-kluizen**.

    ![Recovery Services typt in het dialoogvenster voor alle services](./media/backup-mabs-install-azure-stack/all-services.png)

    De lijst met Recovery Services-kluizen in het abonnement wordt weergegeven.

4. Selecteer in de lijst met Recovery Services-kluizen, uw kluis om het dashboard te openen.

    ![Recovery Services typt in het dialoogvenster voor alle services](./media/backup-mabs-install-azure-stack/rs-vault-dashboard.png)

5. Klik in het menu aan de slag van de kluis op **back-up** om de wizard aan de slag te openen.

    ![Back-up aan de slag](./media/backup-mabs-install-azure-stack/getting-started-backup.png)

    Het menu back-up wordt geopend.

    ![Back-up-doelen-standaard-geopend](./media/backup-mabs-install-azure-stack/getting-started-menu.png)

6. In het menu back-up van de **waar wordt uw werkbelasting uitgevoerd** in het menu **On-premises**. Uit de **waarvan wilt u back-up maken?** vervolgkeuzelijst, selecteer de werkbelastingen die u wilt beveiligen met behulp van Azure Backup-Server. Als u niet zeker weet welke workloads om te selecteren, kiest u **Hyper-V virtuele Machines** en klik vervolgens op **infrastructuur voorbereiden**.

    ![on-premises en werkbelastingen als doelen](./media/backup-mabs-install-azure-stack/getting-started-menu-onprem-hyperv.png)

    De **infrastructuur voorbereiden** menu wordt geopend.

7. In de **infrastructuur voorbereiden** menu, klikt u op **downloaden** om een webpagina voor het downloaden van bestanden voor installatie van Azure Backup Server te openen.

    ![Wijzigen van de wizard aan de slag](./media/backup-mabs-install-azure-stack/prepare-infrastructure.png)

    De Microsoft-website die als host fungeert voor de downloadbare bestanden voor Azure Backup Server wordt geopend.

8. Selecteer een taal in de downloadpagina van Microsoft Azure Backup Server en klikt u op **downloaden**.

    ![Download center wordt geopend](./media/backup-mabs-install-azure-stack/mabs-download-center-page.png)

9. Het installatieprogramma van Azure Backup Server is samengesteld uit acht bestanden - installatieprogramma en zeven .bin-bestanden. Controleer **bestandsnaam** voor het selecteren van alle vereiste bestanden en klikt u op **volgende**. Alle bestanden downloaden naar dezelfde map.

    ![1 center downloaden](./media/backup-mabs-install-azure-stack/download-center-selected-files.png)

    Het downloaden van alle bestanden voor installatie is groter dan 3 GB. Op een download van 10 Mbps kan de koppeling naar alle installatiebestanden downloaden tot 60 minuten duren. De bestanden downloadt naar uw opgegeven downloadlocatie.

## <a name="extract-azure-backup-server-install-files"></a>Uitpakken van bestanden van Azure Backup Server installeren

Nadat u alle bestanden naar uw virtuele machine van Azure Stack hebt gedownload, gaat u naar de downloadlocatie. De eerste fase van het installeren van Azure Backup Server is het extraheren van de bestanden.

![1 center downloaden](./media/backup-mabs-install-azure-stack/download-mabs-installer.png)

1. U start de installatie, in de lijst van de gedownloade bestanden, klikt u op **MicrosoftAzureBackupserverInstaller.exe**.

    > [!WARNING]
    > Ten minste 4GB aan vrije ruimte is vereist voor het uitpakken van de setup-bestanden.
    >

2. Klik in de wizard Azure Backup Server **volgende** om door te gaan.

    ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wiz-1.png)

3. Kies het pad voor de Azure Backup Server-bestanden, en klik op **volgende**.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-select-destination-1.png)

4. Controleer de locatie van de extractie en klikt u op **extraheren**.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-extract-2.png)

5. De wizard de bestanden worden uitgepakt en bereidt het installatieproces.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-install-3.png)

6. Nadat de extractie-proces is voltooid, klikt u op **voltooien**. Standaard **setup.exe uitvoeren** is geselecteerd. Wanneer u klikt op **voltooien**, Setup.exe installeert Microsoft Azure Backup Server naar de opgegeven locatie.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-finish-4.png)

## <a name="install-the-software-package"></a>De software-updatepakket installeren

In de vorige stap hebt die u hebt geklikt **voltooien** afsluiten van de extractie-fase en start de wizard setup van Azure Backup Server.

![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5.png)

Azure Backup Server deelt code met Data Protection Manager. Hier ziet u verwijzingen naar Data Protection Manager en DPM in het installatieprogramma van Azure Backup Server. Hoewel Azure Backup Server en Data Protection Manager afzonderlijke producten zijn, worden deze producten nauw verwant.

1. Als u wilt de wizard setup starten, klikt u op **Microsoft Azure Backup Server**.

   ![Back-installatiewizard voor Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5b.png)

2. Op de **Welkom** scherm, klikt u op **volgende**.

    ![Azure Backup Server - welkomstpagina en de vereisten controleren](./media/backup-mabs-install-azure-stack/mabs-install-wizard-setup-6.png)

3. Op de **Vereistencontroles** scherm, klikt u op **controleren** om te bepalen als de hardware- en vereisten voor Azure Backup Server wordt voldaan.

    ![Azure Backup Server - welkomstpagina en de vereisten controleren](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-7.png)

    Als uw omgeving de benodigde vereisten heeft, ziet u een bericht weergegeven dat aangeeft dat de machine voldoet aan de vereisten. Klik op **Volgende**.  

    ![Met Azure Backup Server - vereisten controleren is voltooid](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-passed-8.png)

    Als uw omgeving niet voldoet aan de vereisten die nodig zijn, worden de problemen worden opgegeven. De vereisten die niet is voldaan, worden ook weergegeven in de DpmSetup.log. Los de fouten met vereisten en voer **nogmaals controleren**. Installatie kan niet worden voortgezet totdat alle vereisten wordt voldaan.

    ![Azure Backup Server - vereisten voor de installatie niet voldaan aan](./media/backup-mabs-install-azure-stack/installation-errors.png)

4. Microsoft Azure Backup Server vereist SQL Server. Het installatiepakket voor de Azure Backup Server wordt geleverd met de juiste binaire bestanden van SQL Server. Als u gebruiken van uw eigen SQL-installatie wilt, kunt u. De aanbevolen optie is echter kunt het installatieprogramma een nieuw exemplaar van SQL Server toevoegen. Als u wilt controleren of uw keuze met uw omgeving werkt, klikt u op **controleren en installeren**.

   > [!NOTE]
   > Azure Backup Server werkt niet met een extern SQL Server-exemplaar. Het exemplaar dat wordt gebruikt door Azure Backup Server moet zich lokaal worden.
   >

    ![Azure Backup Server - welkomstpagina en de vereisten controleren](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-install-9.png)

    Nadat u hebt gecontroleerd, als de virtuele machine de benodigde vereisten heeft voor het installeren van Azure Backup Server, klikt u op **volgende**.

    ![Azure Backup Server - welkomstpagina en de vereisten controleren](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-ready-10.png)

    Als er een fout optreedt met een aanbeveling de machine opnieuw starten, start u de computer vervolgens opnieuw. Na de machine opnieuw opstarten, opnieuw opstarten van het installatieprogramma en wanneer u de **SQL-instellingen** scherm, klikt u op **nogmaals controleren**.

5. In de **installatie-instellingen**, Geef een locatie voor de installatie van Microsoft Azure Backup server-bestanden en klikt u op **volgende**.

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-settings-11.png)

    De nieuwe locatie is vereist voor back-up naar Azure. Zorg ervoor dat de grootte van de nieuwe locatie is gelijk aan ten minste 5% van de gegevens die zijn gepland om te worden back-ups op Azure. Voor bescherming van de schijf moeten afzonderlijke schijven worden geconfigureerd nadat de installatie is voltooid. Zie voor meer informatie over opslaggroepen [configureert u opslaggroepen en schijfopslag](https://technet.microsoft.com/library/hh758075.aspx).

6. Op de **beveiligingsinstellingen** scherm, een sterk wachtwoord opgeven voor de beperkte lokale gebruikersaccounts en klikt u op **volgende**.

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-security-12.png)

7. Op de **Opt-In voor Microsoft Update** scherm, selecteert u of u wilt gebruiken *Microsoft Update* om te controleren op updates en klikt u op **volgende**.

   > [!NOTE]
   > We raden u aan Windows Update wordt omgeleid naar de Microsoft Update beveiligingsupdates en belangrijke updates voor Windows en andere producten, zoals Microsoft Azure Backup Server biedt.
   >

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-update-13.png)

8. Controleer de *samenvatting van instellingen* en klikt u op **installeren**.

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-summary-14.png)

    Wanneer Azure Backup Server is voltooid installeert, wordt het installatieprogramma van de Microsoft Azure Recovery Services-agent in het installatieprogramma onmiddellijk gestart.

9. De Microsoft Azure Recovery Services-Agent-installatieprogramma wordt geopend en de controles voor verbinding met Internet. Als de verbinding met Internet beschikbaar is, gaat u verder met de installatie. Als er geen verbinding, geeft u de proxygegevens verbinding maken met Internet. Nadat u de proxy-instellingen hebt opgegeven, klikt u op **volgende**.

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-mabs-install-azure-stack/mabs-install-wizard-proxy-15.png)

10. Voor het installeren van de Microsoft Azure Recovery Services-Agent, klikt u op **installeren**.

    ![Azure Backup Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-mars-agent-16.png)

    De Microsoft Azure Recovery Services-agent, ook wel de Azure Backup-agent, configureert u de Azure Backup-Server naar de Recovery Services-kluis. Wanneer geconfigureerd, wordt Azure Backup-Server altijd back-upgegevens naar dezelfde Recovery Services-kluis.

11. Nadat de Microsoft Azure Recovery Services-agent de installatie heeft voltooid, klikt u op **volgende** starten van de volgende fase: Bezig met Azure Backup Server registreren bij de Recovery Services-kluis.

    ![Azure Backup Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-complete-16.png)

    Het installatieprogramma start de **Wizard Server registreren**.

12. Schakel over naar uw Azure-abonnement en uw Recovery Services-kluis. In de **infrastructuur voorbereiden** menu, klikt u op **downloaden** kluisreferenties downloaden. Als de **downloaden** knop in stap 2 is niet actief, selecteer **al gedownload of met behulp van de installatie van de meest recente Azure Backup Server** op de knop activeren. De kluisreferenties downloaden naar de locatie waar u downloads opslaat. Houd rekening met deze locatie omdat u hebt deze nodig voor de volgende stap.

    ![Azure Backup Server PreReq2](./media/backup-mabs-install-azure-stack/download-mars-credentials-17.png)

13. In de **kluis-id** menu, klikt u op **Bladeren** te vinden van de Recovery Services-kluis-referenties.

    ![Azure Backup Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-vault-id-18.png)

    In de **Kluisreferenties selecteren** dialoogvenster, gaat u naar de downloadlocatie, selecteert u de kluisreferenties en klikt u op **Open**.

    Het pad naar de referenties wordt weergegeven in het menu kluis-id. Klik op **volgende** om door te gaan naar de instelling voor wachtwoordversleuteling.

14. In de **Versleutelingsinstelling** dialoogvenster, Geef een wachtwoordzin op voor de back-codering en een locatie voor het opslaan van de wachtwoordzin, en op **volgende**.

    ![Azure Backup Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-encryption-19.png)

    U kunt uw eigen wachtwoordzin opgeven of de wachtwoordzin-generator een voor u te maken. De wachtwoordzin is aan u en Microsoft niet opslaan of beheren van deze wachtwoordzin. Om voor te bereiden voor een noodgeval, sla de wachtwoordzin op een toegankelijke locatie.

    Nadat u op **volgende**, de Azure Backup Server is geregistreerd bij de Recovery Services-kluis. Het installatieprogramma blijft de installatie van SQL Server en de Azure Backup-Server.

    ![Azure Backup Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-still-installing-20.png)

15. Wanneer het installatieprogramma is voltooid, wordt de Status wordt weergegeven dat alle software die is geïnstalleerd.

    ![Azure Backup Server PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-done-22.png)

    Wanneer de installatie is voltooid, worden de Azure Backup Server-console en de Azure Backup Server PowerShell-pictogrammen gemaakt op het bureaublad van de server.

## <a name="add-backup-storage"></a>Backup storage toevoegen

De eerste back-up wordt opgeslagen op opslag die is gekoppeld aan de Azure Backup-Server-machine. Zie voor meer informatie over het toevoegen van schijven [toevoegen Modern Backup storage](https://docs.microsoft.com/system-center/dpm/add-storage?view=sc-dpm-1801).

> [!NOTE]
> U moet back-upopslag toevoegen, zelfs als u van plan bent om gegevens te verzenden naar Azure. In de Azure Backup-Server-architectuur, de Recovery Services-kluis bevat de *tweede* kopie van de gegevens tijdens de lokale opslag bevat de eerste (en verplichte) back-up.
>
>

## <a name="network-connectivity"></a>Verbinding met het netwerk

Azure Backup Server is de verbinding met de Azure Backup-service voor het product om te werken is vereist. Als u wilt valideren of de machine de verbinding met Azure heeft, gebruikt u de ```Get-DPMCloudConnection``` cmdlet in de Azure Backup Server PowerShell-console. Als de uitvoer van de cmdlet waar is, wordt de verbinding, anders bestaat wordt er geen verbinding.

Op hetzelfde moment moet de Azure-abonnement in een foutloze toestand bevindt. Om erachter te komen de status van uw abonnement en te beheren, meld u aan bij de [-abonnementsportal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

Zodra u weet de status van de Azure-connectiviteit en van het Azure-abonnement dat, kunt u de onderstaande tabel om de impact op de functionaliteit van back-up/herstel erachter te komen.

| Status connectiviteit | Azure-abonnement | Back-up naar Azure | Back-up naar schijf | Herstellen van Azure | Herstellen uit een schijf |
| --- | --- | --- | --- | --- | --- |
| Verbonden |Actief |Toegestaan |Toegestaan |Toegestaan |Toegestaan |
| Verbonden |Vervallen |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verbonden |De inrichting ongedaan gemaakt |Gestopt |Gestopt |Gestopt en Azure herstelpunten verwijderd |Gestopt |
| Verbinding is verbroken > 15 dagen |Actief |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verbinding is verbroken > 15 dagen |Vervallen |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verbinding is verbroken > 15 dagen |De inrichting ongedaan gemaakt |Gestopt |Gestopt |Gestopt en Azure herstelpunten verwijderd |Gestopt |

### <a name="recovering-from-loss-of-connectivity"></a>Herstellen uit het verlies van connectiviteit

Als een firewall of een proxy is geen toegang tot Azure, adressen whitelist het volgende domein in de firewall/proxy-profiel:

- `http://www.msftncsi.com/ncsi.txt`
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

Zodra de verbinding met Azure aan de Azure Backup Server is hersteld, bepaalt de status van de Azure-abonnement de bewerkingen die kunnen worden uitgevoerd. Zodra de server is **verbonden**, gebruikt u de tabel in [netwerkverbinding](backup-mabs-install-azure-stack.md#network-connectivity) om te zien van de beschikbare bewerkingen.

### <a name="handling-subscription-states"></a>Afhandeling van abonnementsstatussen

Het is mogelijk om te wijzigen van een Azure-abonnement van *verlopen* of *inrichting ongedaan gemaakt* status naar *Active* staat. Terwijl de abonnementsstatus niet is *Active*:

- Terwijl een abonnement is *inrichting ongedaan gemaakt*, verliest deze functionaliteit. Herstellen van het abonnement op *Active*, revives van de functionaliteit van back-up/herstellen. Als de back-upgegevens op de lokale schijf met een groot genoeg bewaarperiode is bewaard, kan deze back-upgegevens worden opgehaald. Back-upgegevens in Azure is echter onherstelbaar verloren zodra het abonnement voert de *inrichting ongedaan gemaakt* staat.
- Terwijl een abonnement is *verlopen*, verliest deze functionaliteit. Geplande back-ups niet worden uitgevoerd terwijl een abonnement is *verlopen*.

## <a name="troubleshooting"></a>Problemen oplossen

Als Microsoft Azure Backup server is mislukt met fouten tijdens de setup-fase (of back-up of herstellen), raadpleegt u de [fout codes document](https://support.microsoft.com/kb/3041338).
U kunt ook verwijzen naar [Azure back-up die betrekking hebben Veelgestelde vragen](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Volgende stappen

Het artikel [uw omgeving voorbereiden voor DPM](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1801), informatie over ondersteunde configuraties voor Azure Backup Server bevat.

U kunt de volgende artikelen een dieper inzicht te krijgen van werkbelastingsbescherming met Microsoft Azure Backup Server gebruiken.

- [Back-up van SQL Server](https://docs.microsoft.com/azure/backup/backup-mabs-sql-azure-stack)
- [Back-up van SharePoint server](https://docs.microsoft.com/azure/backup/backup-mabs-sharepoint-azure-stack)
- [Alternatieve server back-up](backup-azure-alternate-dpm-server.md)
