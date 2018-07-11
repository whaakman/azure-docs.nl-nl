---
title: Azure Backup - Offline back-ups voor DPM en Azure Backup Server
description: Meer informatie over hoe Azure Backup kunt u voor het verzenden van gegevens vanuit het netwerk met behulp van de Azure Import/Export-service. In dit artikel wordt uitgelegd in de offline seeding van de eerste back-upgegevens met behulp van de Azure Import Export-service.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 5/8/2018
ms.author: saurse
ms.openlocfilehash: 1a0e196f4d96494aca1c19a7527ac7d81837fb5c
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "34606474"
---
# <a name="offline-backup-workflow-for-dpm-and-azure-backup-server"></a>Werkstroom voor offline back-ups voor DPM en Azure Backup-Server
Azure Backup heeft diverse ingebouwde efficiëntie die netwerk- en kosten tijdens de eerste volledige back-ups van gegevens naar Azure besparen. Initiële volledige back-ups wordt gewoonlijk grote hoeveelheden gegevens overdragen en meer netwerkbandbreedte in vergelijking met de volgende back-ups waarbij alleen de delta's / plaatsvindt worden overgedragen. Azure Backup wordt de eerste back-ups gecomprimeerd. Door het proces van het offline-seeding, kunt Azure Backup gebruiken schijven voor de gecomprimeerde eerste back-upgegevens offline uploaden naar Azure.

Het proces offline-seeding van Azure Backup is nauw geïntegreerd met de [Azure Import/Export-service](../storage/common/storage-import-export-service.md) waarmee u gegevens overdragen naar Azure met behulp van schijven. Als u terabytes (TB) aan de eerste back-upgegevens die moeten worden overgebracht via een netwerk met hoge latentie en lage bandbreedte hebt, kunt u de werkstroom voor offline-seeding voor het verzenden van de eerste back-up op een of meer harde schijven voor een Azure-datacenter. Dit artikel bevat een overzicht en verdere details-stappen die door deze werkstroom voltooid voor System Center DPM en Azure Backup Server.

> [!NOTE]
> Het proces van het Offline back-up voor de Microsoft Azure Recovery Services agent (MARS) verschilt van System Center DPM en Azure Backup-Server. Zie voor meer informatie over het gebruik van Offline back-ups met de MARS-agent [in dit artikel](backup-azure-backup-import-export.md). Offline back-up wordt niet ondersteund voor System State back-ups wordt gedaan via de Azure backup-agent. 
>

## <a name="overview"></a>Overzicht
Met de offline-seeding mogelijkheid van Azure Backup en Azure Import/Export is het heel eenvoudig de gegevens offline uploaden naar Azure met behulp van schijven. Het proces Offline back-up omvat de volgende stappen uit:

> [!div class="checklist"]
> * De back-upgegevens, in plaats van via het netwerk wordt verzonden is geschreven naar een *tijdelijke locatie*
> * De gegevens op de *faseringslocatie* vervolgens worden geschreven naar een of meer SATA-schijven met behulp van de *AzureOfflineBackupDiskPrep* hulpprogramma
> * Een Azure Import-taak wordt automatisch gemaakt door het hulpprogramma
> * De SATA-schijven worden vervolgens verzonden naar de dichtstbijzijnde Azure-datacenter
> * Nadat het uploaden van de back-upgegevens naar Azure is voltooid, Azure Backup de back-upgegevens gekopieerd naar de back-upkluis en de incrementele back-ups zijn gepland.

## <a name="supported-configurations"></a>Ondersteunde configuraties 
Offline back-up wordt ondersteund voor alle implementatiemodellen van Azure Backup die off-site back-upgegevens van on-premises naar de Microsoft-Cloud. Dit omvat

> [!div class="checklist"]
> * Back-up van bestanden en mappen met de Microsoft Azure Recovery Services Agent (MARS) of de Azure backup-agent. 
> * Back-up van alle werkbelastingen en bestanden met System Center Data Protection Manager (DPM SC) 
> * Back-up van alle werkbelastingen en bestanden met Microsoft Azure Backup Server <br/>

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de volgende vereisten wordt voldaan voordat u begint de werkstroom voor Offline back-up
* Een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md) is gemaakt. Als u wilt maken, raadpleegt u de stappen in [in dit artikel](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Azure backup-agent of Azure Backup Server of SC DPM is geïnstalleerd op een Windows Server/Windows-client, zoals van toepassing en de computer is geregistreerd bij de Recovery Services-kluis. Zorg ervoor dat alleen de [meest recente versie van Azure Backup](https://go.microsoft.com/fwlink/?linkid=229525) wordt gebruikt. 
* [Download het Azure Publish settings-bestand](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) op de computer van waaruit u wilt back-up van uw gegevens. Het abonnement waarvan u het publish settings-bestand downloaden kan afwijken van het abonnement met de Recovery Services-kluis. Als uw abonnement in zelfstandige Clouds voor Azure is, gebruikt u de volgende koppelingen als geschikt is voor download het Azure Publish settings-bestand.

    | Onafhankelijke Clouds regio | Koppeling van het bestand Azure publicatie-instellingen |
    | --- | --- |
    | Verenigde Staten | [Link](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | China | [Link](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Een Azure Storage-account met *klassieke* implementatiemodel is gemaakt in het abonnement waarvan u het publish settings-bestand gedownload, zoals hieronder weergegeven: 

 ![Het maken van een klassiek opslagaccount](./media/backup-azure-backup-import-export/storageaccountclassiccreate.png)

* Een tijdelijke locatie, dit kan een netwerkshare of een extra station op de computer, intern of extern, met voldoende schijfruimte voor het opslaan van uw eerste kopie wordt gemaakt. Bijvoorbeeld, als u back-up van een bestandsserver met 500 GB wilt, controleert u of het faseringsgebied ten minste 500 GB. (Een lager bedrag vanwege compressie wordt gebruikt.)
* Zorg ervoor dat alleen 2,5 inch SSD, of 2,5-inch of 3,5-inch SATA/III II interne harde schijven worden gebruikt met betrekking tot de schijven die worden verzonden naar Azure. Harde schijven kunt u maximaal 10 TB. Controleer de [documentatie voor Azure Import/Export-service](../storage/common/storage-import-export-requirements.md#supported-hardware) voor de meest recente set van schijven die ondersteuning biedt voor de service.
* De SATA-schijven moeten worden verbonden met een computer (aangeduid als een *kopie*) waar de kopie van de back-upgegevens van de *faseringslocatie* naar de SATA stations wordt uitgevoerd. Zorg ervoor dat Bitlocker is ingeschakeld op de *computer kopiëren* 

## <a name="workflow"></a>Werkstroom
De informatie in deze sectie helpt u de werkstroom voor offline back-ups uitvoeren, zodat uw gegevens kunnen worden geleverd aan een Azure-datacenter en geüpload naar Azure Storage. Als u vragen over de Import-service of een aspect van het proces hebt, raadpleegt u de [importeren Serviceoverzicht](../storage/common/storage-import-export-service.md) documentatie waarnaar wordt verwezen eerder.

### <a name="initiate-offline-backup"></a>Offline back-up starten
1. Wanneer u een back-up plannen, ziet u het volgende scherm (in Windows Server, Windows-client of System Center Data Protection Manager).

    ![Voor importeren](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Hier wordt het bijbehorende scherm in System Center Data Protection Manager: <br/>
    ![SC DPM en Azure Backup-server importeren scherm](./media/backup-azure-backup-import-export/dpmoffline.png)

    De beschrijving van de invoer is als volgt:

    * **Faseringslocatie**: de tijdelijke opslaglocatie waarop de eerste back-up is geschreven. Faseringslocatie mogelijk op een netwerkshare of een lokale computer. Als de computer kopiëren en de broncomputer verschillend zijn, wordt u aangeraden dat u het volledige netwerk-pad van de faseringslocatie opgeven.
    * **Azure Import-taaknaam**: de unieke naam die Azure Import-service en Azure Backup de overdracht van gegevens die worden verzonden op schijven naar Azure bijhouden.
    * **Azure Publish Settings**: het lokale pad naar het publish settings-bestand opgeven.
    * **Azure-abonnements-ID**: de Azure-abonnements-ID voor het abonnement aan vanaf waar u het bestand Azure Publish-instellingen hebt gedownload. 
    * **Azure Storage-Account**: de naam van het opslagaccount in de Azure-abonnement dat is gekoppeld aan het Azure Publish-instellingenbestand.
    * **Azure Storage-Container**: de naam van de bestemmings-blob-opslag in de Azure-opslagaccount waarin de back-upgegevens wordt geïmporteerd.

     Sla de *faseringslocatie* en de *Azure Import-taaknaam* u hebt opgegeven als het nodig is om voor te bereiden van de schijven.  
     
2. Voltooien van de werkstroom en voor het starten van de kopie offline back-ups op **nu een Back-Up maken** in de console voor beheer van Azure backup-agent. De eerste back-up is geschreven naar het faseringsgebied als onderdeel van deze stap.

    ![Back-up-nu](./media/backup-azure-backup-import-export/backupnow.png)

    Voor het voltooien van de bijbehorende werkstroom in System Center Data Protection Manager of Azure Backup server, met de rechtermuisknop op de **beveiligingsgroep**, en kies vervolgens de **herstelpunt maken** optie. Kiest u de **Onlinebeveiliging** optie.

    ![SC DPM en Azure Backup server back nu-up](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Nadat de bewerking is voltooid, wordt de faseringslocatie is gereed om te worden gebruikt voor de schijfvoorbereiding van de.

    ![Back-up wordt uitgevoerd](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA-schijven voorbereiden en verzenden naar Azure
De *AzureOfflineBackupDiskPrep* hulpprogramma wordt gebruikt voor het voorbereiden de SATA-schijven die worden verzonden naar de dichtstbijzijnde Azure-Datacenter. Dit hulpprogramma is beschikbaar in de installatiemap van de Recovery Services-agent in het volgende pad:

   *\Microsoft* *Azure* *Recovery* *Services* *Agent\Utils\*

1. Ga naar de map en kopieer de **AzureOfflineBackupDiskPrep** map naar een kopie-computer waarop de SATA-schijven worden voorbereid zijn verbonden. Zorg ervoor dat de volgende met betrekking tot de computer kopiëren:

    * De computer kopiëren hebt toegang tot de faseringslocatie voor de werkstroom voor offline-seeding via hetzelfde netwerkpad dat is opgegeven in de **start offline back-up** werkstroom.
    * BitLocker is ingeschakeld op de computer kopiëren.
    * De computer kopiëren, hebben toegang tot de Azure-portal.

    Indien nodig, is de computer kopiëren hetzelfde als de broncomputer kan verzenden. 
    
    > [!IMPORTANT] 
    > Als de broncomputer een virtuele machine is, is het verplichte gebruik van een andere fysieke server of client-computer als de computer kopiëren.
    
    
2. Open een opdrachtprompt met verhoogde bevoegdheid op de computer kopiëren met de *AzureOfflineBackupDiskPrep* hulpprogramma directory als de huidige map en voer de volgende opdracht uit:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parameter | Beschrijving |
    | --- | --- |
    | s:&lt;*pad naar de locatie van de fasering*&gt; |Verplichte invoer die wordt gebruikt voor het pad naar de faseringslocatie die u hebt ingevoerd in de **start offline back-up** werkstroom. |
    | p:&lt;*pad naar PublishSettingsFile*&gt; |Optionele invoer die wordt gebruikt voor het pad naar de **Azure Publish Settings** -bestand dat u hebt ingevoerd in de **start offline back-up** werkstroom. |

    > [!NOTE]
    > De &lt;pad naar AzurePublishSettingFile&gt; waarde is verplicht wanneer de computer kopiëren en de broncomputer verschillend zijn.
    >
    >

    Als u de opdracht uitvoert, vraagt het hulpprogramma voor de selectie van de Azure Import-taak die overeenkomt met de schijven die moeten worden voorbereid. Als er slechts een enkele import-taak gekoppeld aan de opgegeven faseringslocatie is, ziet u een scherm zoals de link die erna.

    ![Azure voorbereiden van de schijf hulpprogramma invoer](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
    
3. Voer de stationsletter zonder de afsluitende dubbele punt voor de gekoppelde schijf die u wilt voorbereiden voor overdracht naar Azure. Geeft u de bevestiging voor de opmaak van het station wanneer hierom wordt gevraagd.

    Het hulpprogramma is vervolgens begint met het voorbereiden van de schijf en worden de back-upgegevens gekopieerd. U moet mogelijk extra schijven wanneer u hierom wordt gevraagd door het hulpprogramma voor het geval de opgegeven schijf beschikt niet over voldoende ruimte voor de back-upgegevens te koppelen. <br/>

    Aan het einde van de uitvoering van het hulpprogramma is geslaagd, worden een of meer schijven die u hebt opgegeven voorbereid voor verzending naar Azure. Bovendien een import-taak met de naam u hebt opgegeven tijdens de **start offline back-up** werkstroom is gemaakt in Azure. Het hulpprogramma wordt ten slotte het verzendadres weergegeven in de Azure-datacenter waar de schijven moeten worden verzonden.

    ![De voorbereiding van de Azure-schijf is voltooid](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>
    
4. Aan het einde van de uitvoering van de opdracht ziet u ook de optie voor het bijwerken van verzendinformatie zoals hieronder wordt weergegeven:

    ![Verzending info optie bijwerken](./media/backup-azure-backup-import-export/updateshippingutility.png)<br/>

5. U kunt meteen de details invoeren. Het hulpprogramma leidt u door het proces met betrekking tot een reeks van invoer. Als u geen informatie, zoals het getal of andere details met betrekking tot verzending bijhouden, kunt u echter de sessie beëindigen. In dit artikel vindt u de stappen voor het bijwerken van verzendgegevens later opnieuw. 

6. Stuur de schijven naar het adres dat de hulpprogramma's beschikbaar gesteld en houd het traceringsnummer voor toekomstig gebruik.

   > [!IMPORTANT] 
   > Geen twee Azure Import-taken kan het dezelfde traceringsnummer hebben. Zorg ervoor dat stations die zijn opgesteld door het hulpprogramma onder een enkele Azure Import-taak samen in één pakket worden verzonden en er is een één unieke nummer voor het pakket. Voorbereid als onderdeel van de schijven niet combineren **verschillende** Azure Import-taken in één pakket. 

5. Wanneer u het bijhouden van gegevens over, gaat u naar de broncomputer kan verzenden, die wacht op voltooiing van de Import-taak en voer de volgende opdracht uit in een opdrachtprompt met verhoogde bevoegdheid met hebt *AzureOfflineBackupDiskPrep* hulpprogramma directory als de huidige map: 

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   U kunt (optioneel) Voer de volgende opdracht vanaf een andere computer, zoals de *kopie*, met *AzureOfflineBackupDiskPrep* hulpprogramma directory als de huidige map:
   
   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parameter | Beschrijving |
    | --- | --- |
    | u: | Verplichte invoer gebruikt voor het bijwerken van de verzendgegevens voor een Azure Import-taak |
    | s:&lt;*pad naar de locatie van de fasering*&gt; | Verplichte invoer wanneer de opdracht wordt niet uitgevoerd op de broncomputer kan verzenden. Gebruikt voor het pad naar de faseringslocatie die u hebt ingevoerd in de **start offline back-up** werkstroom. |
    | p:&lt;*pad naar PublishSettingsFile*&gt; | Verplichte invoer wanneer de opdracht wordt niet uitgevoerd op de broncomputer kan verzenden. Gebruikt voor het pad naar de **Azure Publish Settings** -bestand dat u hebt ingevoerd in de **start offline back-up** werkstroom. |
    
    Het hulpprogramma detecteert automatisch de Import-taak die wacht op de broncomputer of de taken van gegevensimport die zijn gekoppeld aan de faseringslocatie wanneer de opdracht wordt uitgevoerd op een andere computer. Vervolgens biedt ook de mogelijkheid om bij te werken verzendgegevens via een reeks invoer zoals hieronder wordt weergegeven: 
    
    ![Verzendgegevens invoeren](./media/backup-azure-backup-import-export/shippinginputs.png)<br/>

6. Zodra alle invoergegevens worden geleverd, de details zorgvuldig te controleren en het doorvoeren van de gegevens van de verzending die u hebt opgegeven door te typen *Ja*. 

    ![Lees de verzendgegevens](./media/backup-azure-backup-import-export/reviewshippinginformation.png)<br/>

7. Op de gegevens van de verzending is bijgewerkt, vindt het hulpprogramma u een lokale locatie waar de door u opgegeven verzendgegevens worden opgeslagen, zoals hieronder wordt weergegeven 

    ![Verzendgegevens opslaan](./media/backup-azure-backup-import-export/storingshippinginformation.png)<br/>

   > [!IMPORTANT] 
   > Zorg ervoor dat de stations het Azure-Datacenter bereiken binnen twee weken leveren de verzending informatie over het gebruik van de *AzureOfflineBackupDiskPrep* hulpprogramma. Dit niet doet, kan resulteren in de stations die niet worden verwerkt.  

Nadat u de bovenstaande stappen hebt voltooid, is de Azure-Datacenter gereed is voor het ontvangen van de stations en ze om over te dragen van de back-upgegevens van de schijven aan de klassiek type Azure storage-account dat u hebt gemaakt niet verder verwerken. 

### <a name="time-to-process-the-drives"></a>Tijd voor het verwerken van de stations 
De hoeveelheid tijd die nodig is om te verwerken die een Azure import-taak, afhankelijk van verschillende factoren, zoals verzendtijd varieert, taak-type, type en grootte van de gegevens worden gekopieerd en de grootte van de schijven die zijn opgegeven. De Azure Import/Export-service beschikt niet over een SLA, maar nadat de schijven zijn ontvangen. de service voor het voltooien van de back-upgegevens kopiëren naar uw Azure storage-account in 7 tot en met 10 dagen streeft ernaar. De volgende sectie wordt uitgelegd hoe u de status van de Azure import-taak kunt bewaken. 

### <a name="monitoring-azure-import-job-status"></a>Status van de Azure Import-taak controleren
Uw schijven zijn onderweg zijn of in de Azure-datacenter worden gekopieerd naar het opslagaccount, toont de Azure backup-agent of SC DPM of de console van de Azure Backup-server op de broncomputer de status van de volgende taak voor de geplande back-ups. 

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

Volg de stappen hieronder om de status van de Import-taak te controleren. 
1. Open een opdrachtprompt met verhoogde bevoegdheid op de broncomputer kan verzenden en voer de volgende opdracht uit:
    
     `AzureOfflineBackupDiskPrep.exe u:`
    
2.  De uitvoer ziet u de huidige status van de Import-taak, zoals hieronder weergegeven: 

    ![De Status van de Import-taak controleren](./media/backup-azure-backup-import-export/importjobstatusreporting.png)<br/>

Zie voor meer informatie over de verschillende statussen van de Azure import-taak [in dit artikel](../storage/common/storage-import-export-view-drive-status.md)

### <a name="complete-the-workflow"></a>Voltooien van de werkstroom
Nadat de import-taak is voltooid, is eerste back-upgegevens beschikbaar in uw storage-account. Op het moment van de volgende geplande back-up kopieert Azure backup de inhoud van de gegevens van het opslagaccount naar de Recovery Services-kluis, zoals hieronder weergegeven: 

   ![Kopiëren van gegevens naar de Recovery Services-kluis](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Op het moment van de volgende geplande back-up uitvoert Azure back-up incrementele back-up van de eerste back-up.

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor vragen over de Azure Import/Export-werkstroom, [gebruik van de Microsoft Azure Import/Export-service gegevens overdraagt naar Blob-opslag](../storage/common/storage-import-export-service.md).
* Raadpleeg het gedeelte offline back-ups van de Azure backup- [Veelgestelde vragen over](backup-azure-backup-faq.md) voor vragen over de werkstroom.
