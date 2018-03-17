---
title: Azure Backup - Offline back-ups of het eerste seeding via de Azure Import/Export-service | Microsoft Docs
description: Meer informatie over hoe Azure back-up kunt u gegevens uit het netwerk via de Azure Import/Export-service worden verzonden. Dit artikel wordt uitgelegd in de offline seeding van de eerste back-upgegevens met de service Azure Import exporteren.
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 2/6/2018
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: 7af2623a25f73f6d9062d476309ecd53da542f70
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Werkstroom voor offline back-ups maken in Azure Backup
Azure Backup heeft diverse ingebouwde efficiency die netwerk- en kosten tijdens de eerste volledige back-ups van gegevens naar Azure besparen. Eerste volledige back-ups brengen grote hoeveelheden gegevens doorgaans en meer netwerkbandbreedte in vergelijking met de volgende back-ups waarbij alleen de delta's / incrementele worden overgedragen. Azure back-up comprimeren van de eerste back-ups. Door het proces van het offline seeding kunt Azure Backup schijven gebruiken voor het uploaden van de gecomprimeerde eerste back-upgegevens offline naar Azure.  

Het proces offline seeding van Azure Backup is nauw geïntegreerd met de [Azure Import/Export-service](../storage/common/storage-import-export-service.md) waarmee u gegevens overdraagt naar Azure met behulp van schijven. Als u de eerste back-upgegevens die worden overgedragen via een netwerk met hoge latentie en een lage bandbreedte terabyte (TBs) hebt, kunt u de werkstroom offline seeding kunt gebruiken voor het verzenden van de eerste back-up op een of meer harde schijven voor een Azure-datacenter. In dit artikel biedt een overzicht en verdere details stappen die deze werkstroom worden voltooid.


## <a name="overview"></a>Overzicht
Met de functie offline seeding van de Azure Backup en Azure Import/Export is het eenvoudig de gegevens offline uploaden naar Azure met behulp van schijven. Het proces Offline back-up omvat de volgende stappen:

> [!div class="checklist"]
> * De back-upgegevens, in plaats van via het netwerk worden verzonden, wordt geschreven naar een *tijdelijke locatie*
> * De gegevens op de *faseringslocatie* vervolgens wordt geschreven naar een of meer SATA-schijven met behulp van de *AzureOfflineBackupDiskPrep* hulpprogramma
> * Een Azure Import-taak wordt automatisch gemaakt door het hulpprogramma
> * De SATA-stations worden vervolgens naar de dichtstbijzijnde Azure-datacenter verzonden
> * Nadat het uploaden van de back-upgegevens naar Azure is voltooid, Azure Backup back-up worden de gegevens gekopieerd naar de back-upkluis en de incrementele back-ups zijn gepland.

## <a name="supported-configurations"></a>Ondersteunde configuraties 
Offline back-up wordt ondersteund voor alle implementatiemodellen van Azure Backup die externe back-upgegevens van on-premises naar de Microsoft Cloud. Dit omvat

> [!div class="checklist"]
> * Back-up van bestanden en mappen met de Microsoft Azure Recovery Services (MARS)-Agent of de Azure Backup agent. 
> * Back-up van alle werkbelastingen en bestanden met System Center Data Protection Manager (DPM SC) 
> * Back-up van alle werkbelastingen en bestanden met Microsoft Azure Backup-Server <br/>

   > [!NOTE]
   > Offline back-up wordt niet ondersteund voor back-ups systeemstatus wordt gedaan via de Azure Backup agent. 

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de volgende vereisten wordt voldaan voordat u begint de werkstroom voor Offline back-up
* Een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md) is gemaakt. Raadpleeg de stappen in om een [in dit artikel](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Azure backup-agent of Azure Backup-Server of SC DPM op een Windows Server/Windows-client, indien van toepassing is geïnstalleerd en de computer is geregistreerd bij de Recovery Services-kluis. Zorg ervoor dat alleen de [meest recente versie van Azure Backup](https://go.microsoft.com/fwlink/?linkid=229525) wordt gebruikt. 
* [Download het bestand van de instellingen Azure publiceren](https://portal.azure.com/#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) op de computer van waaruit u wilt back-up van uw gegevens. Het abonnement van waaruit u het bestand publicatie-instellingen downloaden kan afwijken van het abonnement dat de Recovery Services-kluis bevat. Als uw abonnement op Azure-Clouds soevereine, gebruikt u de volgende koppelingen naar gelang van toepassing om de instellingen Azure publiceren bestand te downloaden.

    | Soevereine cloud-regio | Bestandskoppeling Azure publicatie-instellingen |
    | --- | --- |
    | Verenigde Staten | [Link](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |
    | China | [Link](https://portal.azure.us#blade/Microsoft_Azure_ClassicResources/PublishingProfileBlade) |

* Een Azure Storage-account met *klassieke* implementatiemodel is gemaakt in het abonnement van waaruit u het bestand hebt gedownload publiceren instellingen zoals hieronder wordt weergegeven: 

 ![Een klassieke storage-account maken](./media/backup-azure-backup-import-export/storageaccountclassiccreate.png)

* Een tijdelijke locatie, dit kan een netwerkshare of een extra station op de computer, intern of extern, met voldoende schijfruimte voor het opslaan van uw eerste kopie is gemaakt. Bijvoorbeeld, als u back-up van een bestandsserver van 500 GB wilt, zorg ervoor dat het faseringsgebied ten minste 500 GB. (Een lager bedrag wordt gebruikt als gevolg van comprimering.)
* Zorg ervoor dat er alleen 2,5 inch SSD of 2,5-inch of 3.5-inch SATA II/III interne harde schijven worden gebruikt met betrekking tot de schijven die worden verzonden naar Azure. U kunt harde schijven tot 10 TB. Controleer de [documentatie van de service Azure Import/Export](../storage/common/storage-import-export-service.md#hard-disk-drives) voor de meest recente set met schijven die ondersteuning biedt voor de service.
* De SATA-schijven moeten zijn verbonden met een computer (aangeduid als een *kopie*) waar de kopie van de back-upgegevens van de *faseringslocatie* naar de SATA stations wordt uitgevoerd. Zorg ervoor dat Bitlocker is ingeschakeld op de *computer kopiëren* 

## <a name="workflow"></a>Werkstroom
De informatie in deze sectie helpt u de werkstroom voor offline back-up uitvoeren, zodat uw gegevens kunnen worden geleverd aan een Azure-datacenter en geüpload naar Azure Storage. Als u vragen over de service importeren of een aspect van het proces hebt, raadpleegt u de [importeren Serviceoverzicht](../storage/common/storage-import-export-service.md) documentatie waarnaar wordt verwezen eerder.

### <a name="initiate-offline-backup"></a>Offline back-up starten
1. Als u een back-up plannen, ziet u het volgende scherm (in Windows Server, Windows-client of System Center Data Protection Manager).

    ![Scherm importeren](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Hier wordt het bijbehorende scherm in System Center Data Protection Manager: <br/>
    ![SC DPM en de Azure Backup-server importeren scherm](./media/backup-azure-backup-import-export/dpmoffline.png)

    De beschrijving van de invoerwaarden is als volgt:

    * **Tijdelijke locatie**: de tijdelijke opslaglocatie waarnaar de eerste back-up is geschreven. Tijdelijke locatie bevindt zich op een netwerkshare of een lokale computer. Als de computer van de kopie en de broncomputer verschillen, raden wij aan dat u het pad van het volledige netwerk van de faseringslocatie opgeeft.
    * **Azure Import-taaknaam**: de unieke naam welke Azure Import-service en Azure Backup de overdracht van gegevens die worden verzonden op schijven naar Azure bijhouden.
    * **Azure Publish Settings**: Geef het lokale pad naar het bestand publicatie-instellingen.
    * **Azure-abonnements-ID**: de Azure-abonnements-ID voor het abonnement van waar u de instellingen Azure publiceren bestand hebt gedownload. 
    * **Azure Storage-Account**: de naam van het opslagaccount in de Azure-abonnement gekoppeld het instellingenbestand Azure publiceren.
    * **Azure Storage-Container**: de naam van de bestemmings-blob-opslag in de Azure-opslagaccount waarin de back-upgegevens is geïmporteerd.

     Sla de *faseringslocatie* en de *Azure Import-taaknaam* u hebt opgegeven dat dit is vereist voor het voorbereiden van de schijven.  
     
2. Voltooien van de werkstroom en voor het initiëren van het exemplaar offline back-up op **Back-Up uit** in de Azure Backup agent-beheerconsole. De eerste back-up is geschreven naar het faseringsgebied als onderdeel van deze stap.

    ![Nu reservekopie maken](./media/backup-azure-backup-import-export/backupnow.png)

    Voor het voltooien van de bijbehorende werkstroom in System Center Data Protection Manager of Azure Backup-server met de rechtermuisknop op de **beveiligingsgroep**, en kies vervolgens de **herstelpunt maken** optie. Kiest u de **Onlinebeveiliging** optie.

    ![SC DPM en de Azure Backup-server back-up nu](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Nadat de bewerking is voltooid, wordt de faseringslocatie is gereed om te worden gebruikt voor de schijfvoorbereiding van de.

    ![Voortgang van de back-up](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-sata-drives-and-ship-to-azure"></a>Voorbereiden van SATA-stations en verzenden naar Azure
De *AzureOfflineBackupDiskPrep* hulpprogramma wordt gebruikt voor het voorbereiden van de SATA-schijven die worden verzonden naar de dichtstbijzijnde Azure-Datacenter. Dit hulpprogramma is beschikbaar in de map voor installatie van de Recovery Services-agent in het volgende pad:

   *\Microsoft* *Azure* *Recovery* *Services* *Agent\Utils\*

1. Ga naar de map en kopieer de **AzureOfflineBackupDiskPrep** map op een kopie-computer waarop de SATA-stations worden voorbereid zijn verbonden. Zorg ervoor dat de volgende met betrekking tot de computer kopiëren:

    * De computer kopiëren de faseringslocatie voor de werkstroom offline seeding toegang met behulp van hetzelfde netwerkpad dat is opgegeven in de **offline back-up starten** werkstroom.
    * BitLocker is ingeschakeld op de computer kopiëren.
    * De computer kopiëren, hebben toegang tot de Azure-portal.

    Indien nodig, kan de computer kopiëren niet hetzelfde als de broncomputer. 
    
    > [!IMPORTANT] 
    > Als de broncomputer een virtuele machine is, is het gebruik van een andere fysieke server of client-computer als de computer kopiëren.
    
    
2. Open een opdrachtprompt met verhoogde bevoegdheid op de computer kopiëren met de *AzureOfflineBackupDiskPrep* hulpprogramma directory als de huidige map en voer de volgende opdracht:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to AzurePublishSettingsFile*>]`

    | Parameter | Beschrijving |
    | --- | --- |
    | s:&lt;*locatiepad fasering*&gt; |Verplichte invoer die wordt gebruikt voor het pad naar de faseringslocatie die u hebt ingevoerd in de **offline back-up starten** werkstroom. |
    | p:&lt;*pad naar PublishSettingsFile*&gt; |Optionele invoer die wordt gebruikt voor het pad naar de **Azure Publish Settings** -bestand dat u hebt ingevoerd in de **offline back-up starten** werkstroom. |

    > [!NOTE]
    > De &lt;pad naar AzurePublishSettingFile&gt; waarde is verplicht wanneer de computer van de kopie en de broncomputer verschillen.
    >
    >

    Als u de opdracht uitvoert, vraagt het hulpprogramma de selectie van de Azure Import-taak die overeenkomt met de stations die moeten worden voorbereid. Als er slechts een enkele import-taak gekoppeld aan de opgegeven faseringslocatie is, ziet u een scherm, zoals de link die erna.

    ![Azure schijfvoorbereiding hulpprogramma invoer](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
    
3. Voer de stationsletter zonder de afsluitende dubbele punt voor de gekoppelde schijf die u wilt voorbereiden voor overdracht naar Azure. Geeft u de bevestiging voor de opmaak van het station als u wordt gevraagd.

    Het hulpprogramma begint vervolgens met het voorbereiden van de schijf en de back-upgegevens zijn gekopieerd. U moet mogelijk extra schijven als u wordt gevraagd door het hulpprogramma voor het geval de opgegeven schijf niet voldoende ruimte voor de back-upgegevens heeft koppelen. <br/>

    Aan het einde van een geslaagde uitvoering van het hulpprogramma, worden een of meer schijven die u hebt opgegeven voorbereid voor back-ups naar Azure. Bovendien een import-taak met de naam u hebt opgegeven tijdens de **offline back-up starten** werkstroom is gemaakt in Azure. Het hulpprogramma geeft ten slotte het verzendadres weer voor de Azure-datacenter waarin de schijven moeten worden verzonden.

    ![De voorbereiding van de Azure-schijf is voltooid](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>
    
4. Aan het einde van de uitvoering van de opdracht ziet u ook de optie voor het bijwerken van back-ups van gegevens, zoals hieronder wordt weergegeven:

    ![Back-ups van gegevens optie bijwerken](./media/backup-azure-backup-import-export/updateshippingutility.png)<br/>

5. U kunt de details meteen invoeren. Het hulpprogramma leidt u door het proces met betrekking tot een reeks invoer. Als u geen informatie, zoals het nummer of andere gegevens die betrekking hebben op de back-upfunctie bijhouden, kunt u echter de sessie beëindigen. In dit artikel vindt u de stappen voor het back-ups van gegevens later bijwerken. 

6. Verzenden van de schijven naar het adres dat u de hulpprogramma's beschikbaar gesteld en houd het volgnummer voor toekomstig gebruik.

   > [!IMPORTANT] 
   > Er zijn geen twee importtaken van Azure kan het hetzelfde volgnummer hebben. Zorg ervoor dat stations die zijn voorbereid door het hulpprogramma onder één Azure Import-taak samen in één pakket worden verzonden en er is een enkel uniek volgnummer voor het pakket. Bereid als onderdeel van stations niet combineren **verschillende** Azure Import-taken in een enkel pakket. 

5. Wanneer u het bijhouden van gegevens over, gaat u naar de broncomputer, die wacht op voltooiing van de Import-taak en voer de volgende opdracht in een opdrachtprompt met verhoogde bevoegdheid met hebt *AzureOfflineBackupDiskPrep* hulpprogramma directory als de huidige map: 

   `*.\AzureOfflineBackupDiskPrep.exe*  u:`

   U kunt eventueel uitvoeren met de volgende opdracht vanaf een andere computer, zoals de *kopie*, met *AzureOfflineBackupDiskPrep* hulpprogramma directory als de huidige map:
   
   `*.\AzureOfflineBackupDiskPrep.exe*  u:  s:<*Staging Location Path*>   p:<*Path to AzurePublishSettingsFile*>`

    | Parameter | Beschrijving |
    | --- | --- |
    | u: | Verplichte invoer gebruikt voor het bijwerken van de back-upfunctie details voor een Azure Import-taak |
    | s:&lt;*locatiepad fasering*&gt; | Verplichte invoer wanneer de opdracht wordt niet uitgevoerd op de broncomputer. Gebruikt voor het leveren van het pad naar de faseringslocatie die u hebt ingevoerd in de **offline back-up starten** werkstroom. |
    | p:&lt;*pad naar PublishSettingsFile*&gt; | Verplichte invoer wanneer de opdracht wordt niet uitgevoerd op de broncomputer. Gebruikt voor het leveren van het pad naar de **Azure Publish Settings** -bestand dat u hebt ingevoerd in de **offline back-up starten** werkstroom. |
    
    Het hulpprogramma detecteert automatisch de Import-taak die wacht op de broncomputer op of de taken van gegevensimport die zijn gekoppeld aan de faseringslocatie wanneer de opdracht wordt uitgevoerd op een andere computer. Vervolgens wordt de optie voor het bijwerken van back-upfunctie informatie via een reeks invoer zoals hieronder wordt weergegeven: 
    
    ![Back-upfunctie informatie invoeren](./media/backup-azure-backup-import-export/shippinginputs.png)<br/>

6. Zodra alle invoer worden aangeboden, de details zorgvuldig te controleren en doorvoeren van de back-ups van gegevens u geleverd door het typen *Ja*. 

    ![Bekijk informatie verzenden](./media/backup-azure-backup-import-export/reviewshippinginformation.png)<br/>

7. Op de back-ups van gegevens is bijgewerkt, voorziet het hulpprogramma in een lokale locatie waar de back-ups van gegevens die je hebt ingevoerd zijn opgeslagen, zoals hieronder wordt weergegeven 

    ![Opslaan van back-upfunctie informatie](./media/backup-azure-backup-import-export/storingshippinginformation.png)<br/>

   > [!IMPORTANT] 
   > Zorg ervoor dat de stations bereiken van het Azure-Datacenter binnen twee weken van het bieden van de back-ups van gegevens met behulp van de *AzureOfflineBackupDiskPrep* hulpprogramma. Niet doet, kan leiden tot de stations die niet worden verwerkt.  

Nadat u de bovenstaande stappen hebt voltooid, is het Azure-Datacenter gereed om te ontvangen van de stations en ze om de back-upgegevens van de stations zetten aan het klassieke type Azure storage-account dat u hebt gemaakt niet verder verwerken. 

### <a name="time-to-process-the-drives"></a>Tijd voor het verwerken van de stations 
De hoeveelheid tijd die nodig is voor een Azure import-taak, afhankelijk van verschillende factoren, zoals back-ups van tijd varieert, proces taak type, type en grootte van de gegevens worden gekopieerd en de grootte van de schijven die zijn opgegeven. De Azure Import/Export-service beschikt niet over een SLA maar nadat de schijven worden ontvangen. de service wil de back-upgegevens kopiëren naar uw Azure storage-account in 7 tot 10 dagen voltooien. De volgende sectie beschrijft hoe u de status van de Azure import-taak kunt bewaken. 

### <a name="monitoring-azure-import-job-status"></a>Controle van status van de Azure Import-taak
Toont de status van de volgende taak voor de geplande back-ups terwijl uw zijn tijdens de overdracht of in het Azure-datacenter wordt gekopieerd naar het opslagaccount, de Azure Backup agent of SC DPM of de Azure Backup-server-console op de broncomputer. 

  `Waiting for Azure Import Job to complete. Please check on Azure Management portal for more information on job status`

Volg de stappen hieronder om de status van de Import-taak te controleren. 
1. Open een opdrachtprompt met verhoogde bevoegdheid op de broncomputer en voer de volgende opdracht:
    
     `AzureOfflineBackupDiskPrep.exe u:`
    
2.  De uitvoer ziet u de huidige status van de importtaak zoals hieronder wordt weergegeven: 

    ![De Status van de Import-taak controleren](./media/backup-azure-backup-import-export/importjobstatusreporting.png)<br/>

Zie voor meer informatie over de verschillende statussen van de Azure import-taak [in dit artikel](../storage/common/storage-import-export-service.md#how-does-the-azure-importexport-service-work)

### <a name="complete-the-workflow"></a>Voltooien van de werkstroom
Nadat de import-taak is voltooid, is eerste back-upgegevens beschikbaar in uw opslagaccount. Op het moment van de volgende geplande back-up kopieert Azure backup de inhoud van de gegevens van het opslagaccount naar de Recovery Services-kluis zoals hieronder wordt weergegeven: 

   ![Gegevens kopiëren naar de Recovery Services-kluis](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Op het moment van de volgende geplande back-up voert Azure Backup incrementele back-up via de eerste back-up.

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor vragen over de Azure Import/Export-werkstroom, [de Microsoft Azure Import/Export-service gebruiken om gegevens te brengen naar Blob storage](../storage/common/storage-import-export-service.md).
* Raadpleeg het gedeelte offline back-up van de Azure Backup [Veelgestelde vragen over](backup-azure-backup-faq.md) voor vragen over de werkstroom.
