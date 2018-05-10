---
title: Azure Backup - Offline back-ups of het eerste seeding via de Azure Import/Export-service | Microsoft Docs
description: Meer informatie over hoe Azure back-up kunt u gegevens uit het netwerk via de Azure Import/Export-service worden verzonden. Dit artikel wordt uitgelegd in de offline seeding van de eerste back-upgegevens met de service Azure Import exporteren.
services: backup
documentationcenter: ''
author: saurabhsensharma
manager: shivamg
editor: ''
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/8/2018
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: 801de343ebb88394f04a65236997f9ec80a2f535
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Werkstroom voor offline back-ups maken in Azure Backup
Azure Backup heeft diverse ingebouwde efficiency die netwerk- en kosten tijdens de eerste volledige back-ups van gegevens naar Azure besparen. Eerste volledige back-ups brengen grote hoeveelheden gegevens doorgaans en meer netwerkbandbreedte in vergelijking met de volgende back-ups waarbij alleen de delta's / incrementele worden overgedragen. Door het proces van het offline seeding kunt Azure Backup schijven gebruiken voor het uploaden van de offline back-upgegevens naar Azure.

De Azure-back-proces offline seeding is nauw geïntegreerd met de [Azure Import/Export-service](../storage/common/storage-import-export-service.md) waarmee u kunt de eerste back-upgegevens overdragen naar Azure met behulp van schijven. Als u de eerste back-upgegevens die worden overgedragen via een netwerk met hoge latentie en een lage bandbreedte terabyte (TBs) hebt, kunt u de werkstroom offline seeding kunt gebruiken voor het verzenden van de eerste back-up op een of meer harde schijven, naar een Azure-datacenter. De volgende afbeelding bevat een overzicht van de stappen in de werkstroom.

  ![overzicht van de offline import-werkstroomproces](./media/backup-azure-backup-import-export/offlinebackupworkflowoverview.png)

Het offline back-upproces omvat de volgende stappen:

1. De back-upgegevens verzenden via het netwerk, in plaats van de back-upgegevens naar een tijdelijke locatie te schrijven.
2. Gebruik het hulpprogramma AzureOfflineBackupDiskPrep schrijft de gegevens in de faseringslocatie naar een of meer SATA-schijven.
3. Als onderdeel van de werkzaamheden maakt het hulpprogramma AzureOfflineBackupDiskPrep Azure Import-taak. De SATA-stations verzenden naar de dichtstbijzijnde Azure-datacenter en verwijzen naar de import-taak om de activiteiten verbinding te maken.
4. Op het datacenter Azure, wordt de gegevens op de schijven worden gekopieerd naar een Azure storage-account.
5. Azure Backup de back-upgegevens gekopieerd van het opslagaccount naar de Recovery Services-kluis en incrementele back-ups zijn gepland.

## <a name="supported-configurations"></a>Ondersteunde configuraties 
De volgende functies van Azure Backup of werkbelastingen ondersteuning voor het gebruik van Offline back-up.

> [!div class="checklist"]
> * Back-up van bestanden en mappen met de agent van Microsoft Azure Recovery Services (MARS), ook wel aangeduid als de Azure Backup agent. 
> * Back-up van alle werkbelastingen en bestanden met System Center Data Protection Manager (DPM SC) 
> * Back-up van alle werkbelastingen en bestanden met Microsoft Azure Backup-Server <br/>

   > [!NOTE]
   > Offline back-up wordt niet ondersteund voor back-ups systeemstatus wordt gedaan via de Azure Backup agent. 

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="prerequisites"></a>Vereisten

  > [!NOTE]
  > De volgende vereisten en werkstroom alleen van toepassing op Offline back-ups van bestanden en mappen met behulp van de [nieuwste MARS-agent](https://aka.ms/azurebackup_agent). Raadpleeg voor het Offline back-ups uitvoeren voor werkbelastingen met behulp van System Center DPM of Azure Backup-Server, [in dit artikel](backup-azure-backup-server-import-export-.md). 

Voordat u de werkstroom voor Offline back-up begint, moet u de volgende vereisten voldoen: 
* Maak een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md). Voor het maken van een kluis, raadpleegt u de stappen in [in dit artikel](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Zorg ervoor dat alleen de [meest recente versie van de Azure Backup agent](https://aka.ms/azurebackup_agent) op de Windows Server/Windows-client, indien van toepassing is geïnstalleerd en de computer is geregistreerd bij de Recovery Services-kluis.
* Azure PowerShell 3.7.0 of hoger is vereist op de computer met Azure backup-agent. Het is raadzaam dat u [Installeer de nieuwste versie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).
* Controleer of Microsoft Edge of Internet Explorer 11 is geïnstalleerd en JavaScript is ingeschakeld op de computer waarop Azure Backup agent wordt uitgevoerd. 
* Een Azure Storage-account maken in hetzelfde abonnement als de Recovery Services-kluis. 
* Zorg ervoor dat u hebt de [noodzakelijke machtigingen](../azure-resource-manager/resource-group-create-service-principal-portal.md) om de Azure Active Directory-toepassing te maken. De werkstroom voor Offline back-up wordt gemaakt van een Azure Active Directory-toepassing in het abonnement is gekoppeld aan de Azure Storage-account. Het doel van de toepassing is Azure Backup veilige en bereik om toegang te bieden met de Azure Import-Service vereist voor de werkstroom voor Offline back-up. 
* Registreer de provider van de resource Microsoft.ImportExport aan het abonnement met de Azure Storage-account. De resourceprovider registreren:
    1. Klik in het hoofdmenu op **abonnementen**.
    2. Als u bent geabonneerd op meerdere abonnementen, selecteert u het abonnement dat u voor de offline back-up gebruikt. Als u slechts één abonnement hebt gebruikt, is uw abonnement wordt weergegeven.
    3. Klik in het menu abonnement **Resourceproviders** om weer te geven van de lijst met providers.
    4. In de lijst met providers Blader naar beneden naar Microsoft.ImportExport. Als de Status NotRegistered is, klikt u op **registreren**.
    ![de resourceprovider wordt geregistreerd](./media/backup-azure-backup-import-export/registerimportexport.png)
* Een tijdelijke locatie, dit kan een netwerkshare of een extra station op de computer, intern of extern, met voldoende schijfruimte voor het opslaan van uw eerste kopie is gemaakt. Bijvoorbeeld, als u back-up van een bestandsserver van 500 GB wilt, zorg ervoor dat het faseringsgebied ten minste 500 GB. (Een lager bedrag wordt gebruikt als gevolg van comprimering.)
* Wanneer u schijven verzendt naar Azure, alleen 2,5 inch SSD of 2,5-inch of 3.5-inch SATA III-II interne harde schijven gebruiken. U kunt harde schijven tot 10 TB. Controleer de [documentatie van de service Azure Import/Export](../storage/common/storage-import-export-service.md#hard-disk-drives) voor de meest recente set met schijven die ondersteuning biedt voor de service.
* De SATA-stations moeten zijn verbonden met een computer (aangeduid als een *kopie*) waar de kopie van de back-upgegevens van de *faseringslocatie* naar de SATA stations wordt uitgevoerd. Zorg ervoor dat Bitlocker is ingeschakeld op de *kopie*.

## <a name="workflow"></a>Werkstroom
Deze sectie beschrijft de werkstroom voor offline back-up, zodat uw gegevens kunnen worden geleverd aan een Azure-datacenter en geüpload naar Azure Storage. Als u vragen over de service importeren of een aspect van het proces hebt, raadpleegt u de [importeren van de documentatie van de service-overzicht](../storage/common/storage-import-export-service.md).

## <a name="initiate-offline-backup"></a>Offline back-up starten
1. Als u een back-up op de Agent MARS plant, ziet u het volgende scherm.

    ![Scherm importeren](./media/backup-azure-backup-import-export/offlinebackup_inputs.png)

  De beschrijving van de invoerwaarden is als volgt:

    * **Tijdelijke locatie**: de tijdelijke opslaglocatie waarnaar de eerste back-up is geschreven. Tijdelijke locatie bevindt zich op een netwerkshare of een lokale computer. Als de computer van de kopie en de broncomputer verschillen, raden wij aan dat u het pad van het volledige netwerk van de faseringslocatie opgeeft.
    * **Azure Resource Manager Storage account**: de naam van het Resource Manager type opslagaccount in Azure-abonnement.
    * **Azure Storage-Container**: de naam van de bestemmings-blob-opslag in de Azure Storage-account waarin de back-upgegevens wordt geïmporteerd voordat ze worden gekopieerd naar de Recovery Services-kluis.
    * **Azure-abonnements-ID**: de ID voor de Azure-abonnement waarop de Azure Storage-account is gemaakt.
    * **Azure Import-taaknaam**: de unieke naam welke Azure Import-service en Azure Backup de overdracht van gegevens die worden verzonden op schijven naar Azure bijhouden. 
  
  Geef de invoer op het scherm en klik op **volgende**. Opslaan van de opgegeven *tijdelijke locatie* en de *Azure Import-taaknaam*, zoals deze informatie vereist is voor het voorbereiden van de schijven.

2. Wanneer u wordt gevraagd, meld u aan bij uw Azure-abonnement. U moet zich aanmelden zodat Azure Backup kan de Azure Active Directory-toepassing maken en geef de vereiste machtigingen voor toegang tot de Azure Import-Service.

    ![Nu reservekopie maken](./media/backup-azure-backup-import-export/azurelogin.png)

3. Voltooien van de werkstroom en klik in de console van Azure Backup agent op **Back-Up uit**.

    ![Nu reservekopie maken](./media/backup-azure-backup-import-export/backupnow.png)

4. Klik in de pagina Bevestiging van de wizard op **Back-Up**. De eerste back-up is geschreven naar het faseringsgebied als onderdeel van de installatie.

   ![Bevestig dat u back-up nu kunt](./media/backup-azure-backup-import-export/backupnow-confirmation.png)

    Nadat de bewerking is voltooid, wordt de faseringslocatie is gereed om te worden gebruikt voor de schijfvoorbereiding van de.

   ![Nu reservekopie maken](./media/backup-azure-backup-import-export/opbackupnow.png)

## <a name="prepare-sata-drives-and-ship-to-azure"></a>Voorbereiden van SATA-stations en verzenden naar Azure
De *AzureOfflineBackupDiskPrep* hulpprogramma bereidt de SATA-schijven die worden verzonden naar de dichtstbijzijnde Azure-datacenter. Dit hulpprogramma is beschikbaar in de installatiemap van de Azure Backup-agent (in het volgende pad):

   *\Microsoft azure Recovery Services Agent\Utils\\*

1. Ga naar de map en kopieer de **AzureOfflineBackupDiskPrep** map naar een andere computer waar de SATA-schijven zijn aangesloten. Op de computer met de verbonden SATA-stations, zorg ervoor dat:

    * De computer kopiëren de faseringslocatie voor de werkstroom offline seeding toegang met behulp van hetzelfde netwerkpad dat is opgegeven in de **offline back-up starten** werkstroom.
    * BitLocker is ingeschakeld op de computer kopiëren.
    * Azure PowerShell 3.7.0, of hoger is geïnstalleerd.
    * De meest recente compatibel browsers (Edge of Internet Explorer 11) zijn geïnstalleerd en JavaScript is ingeschakeld. 
    * De computer kopiëren, hebben toegang tot de Azure-portal. Indien nodig, kan de computer kopiëren niet hetzelfde als de broncomputer.
    
    > [!IMPORTANT] 
    > Als de broncomputer een virtuele machine is, klikt u vervolgens moet de computer van de kopie een andere fysieke server of clientcomputer van de broncomputer.

2. Open een opdrachtprompt met verhoogde bevoegdheid op de computer kopiëren met de *AzureOfflineBackupDiskPrep* hulpprogramma directory als de huidige map en voer de volgende opdracht:

    ```.\AzureOfflineBackupDiskPrep.exe s:<Staging Location Path>```

    | Parameter | Beschrijving |
    | --- | --- |
    | s:&lt;*locatiepad fasering*&gt; |Verplichte invoer gebruikt voor het leveren van het pad naar de faseringslocatie die u hebt ingevoerd in de **offline back-up starten** werkstroom. |
    | p:&lt;*pad naar PublishSettingsFile*&gt; |Optionele invoer die wordt gebruikt voor het pad naar de **Azure Publish Settings** -bestand dat u hebt ingevoerd in de **offline back-up starten** werkstroom. |

    Als u de opdracht uitvoert, vraagt het hulpprogramma de selectie van de Azure Import-taak die overeenkomt met de stations die moeten worden voorbereid. Als er slechts een enkele import-taak gekoppeld aan de opgegeven faseringslocatie is, ziet u een scherm, zoals de link die erna.

    ![Azure schijfvoorbereiding hulpprogramma invoer](./media/backup-azure-backup-import-export/diskprepconsole0_1.png) <br/>

3. Voer de stationsletter zonder de afsluitende dubbele punt voor de gekoppelde schijf die u wilt voorbereiden voor overdracht naar Azure. 
4. Geeft u de bevestiging voor de opmaak van het station als u wordt gevraagd.
5. U wordt gevraagd of u zich aanmeldt bij uw Azure-abonnement. Geef uw referenties.

    ![Azure schijfvoorbereiding hulpprogramma invoer](./media/backup-azure-backup-import-export/signindiskprep.png) <br/>

    Het hulpprogramma begint vervolgens met het voorbereiden van de schijf en de back-upgegevens zijn gekopieerd. U moet mogelijk extra schijven als u wordt gevraagd door het hulpprogramma voor het geval de opgegeven schijf niet voldoende ruimte voor de back-upgegevens heeft koppelen. <br/>

    Aan het einde van het hulpprogramma is uitgevoerd biedt de opdrachtprompt drie soorten informatie:
    1. Een of meer schijven die u hebt opgegeven zijn voorbereid voor back-ups naar Azure. 
    2. U ontvangt een bevestiging dat uw import-taak is gemaakt. De import-taak wordt de naam die u hebt opgegeven.
    3. Het hulpprogramma voor weergegeven het adres van de back-upfunctie voor de Azure-datacenter.

    ![De voorbereiding van de Azure-schijf is voltooid](./media/backup-azure-backup-import-export/console2.png)<br/>

6. Aan het einde van de uitvoering van de opdracht, kunt u de back-ups van gegevens bijwerken.

7. Verzenden van de schijven naar het adres dat u de hulpprogramma's beschikbaar gesteld en houd het volgnummer voor toekomstig gebruik.

   > [!IMPORTANT] 
   > Er zijn geen twee importtaken van Azure kan het hetzelfde volgnummer hebben. Zorg ervoor dat stations die zijn voorbereid door het hulpprogramma onder één Azure Import-taak samen in één pakket worden verzonden en er is een enkel uniek volgnummer voor het pakket. Moet de stations die zijn bedoeld als onderdeel van de afzonderlijke Azure Import-taken in een enkel pakket niet combineren.



## <a name="update-shipping-details-on-the-azure-import-job"></a>Back-upfunctie details op in de Azure Import-taak bijwerken

De volgende procedure de back-upfunctie taakdetails van Azure Import-updates. Deze informatie vindt u meer informatie over:
* de naam van de drager die Azure de schijven biedt
* back-upfunctie details van de schijven retourneren

1. Aanmelden bij uw Azure-abonnement.
2. Klik in het hoofdmenu op **alle services** en typ in het dialoogvenster alle services importeren. Wanneer er **voor importeren/exporteren taken**, klik erop.
    ![Back-upfunctie informatie invoeren](./media/backup-azure-backup-import-export/search-import-job.png)<br/>

    De lijst met **voor importeren/exporteren taken** menu wordt geopend en de lijst van alle taken voor importeren/exporteren in het geselecteerde abonnement wordt weergegeven. 

3. Als u meerdere abonnementen hebt, moet u het abonnement dat is gebruikt voor het importeren van de back-upgegevens selecteren. Selecteer vervolgens de zojuist gemaakte Import-taak de details ervan te openen.

    ![Bekijk informatie verzenden](./media/backup-azure-backup-import-export/import-job-found.png)<br/>

4. Klik op het menu instellingen voor de Import-taak **back-upfunctie Info beheren** en voer de details van de back-ups van return.

    ![Opslaan van back-upfunctie informatie](./media/backup-azure-backup-import-export/shipping-info.png)<br/>

5. Wanneer u het volgnummer van het back-upfunctie telecombedrijf hebt, klikt u op de banner op de overzichtspagina van Azure Import-taak en voer de volgende gegevens:

   > [!IMPORTANT] 
   > Zorg ervoor dat de provider-informatie en het volgnummer binnen twee weken na het maken van de Azure import-taak worden bijgewerkt. Als deze informatie binnen twee weken kan leiden tot de taak wordt verwijderd en de stations die niet worden verwerkt.

   ![Opslaan van back-upfunctie informatie](./media/backup-azure-backup-import-export/joboverview.png)<br/>

   ![Opslaan van back-upfunctie informatie](./media/backup-azure-backup-import-export/tracking-info.png)

### <a name="time-to-process-the-drives"></a>Tijd voor het verwerken van de stations 
De hoeveelheid tijd die nodig is voor een Azure import-taak, afhankelijk van verschillende factoren, zoals back-ups van tijd varieert, proces taak type, type en grootte van de gegevens worden gekopieerd en de grootte van de schijven die zijn opgegeven. De Azure Import/Export-service beschikt niet over een SLA maar nadat de schijven worden ontvangen. de service wil de back-upgegevens kopiëren naar uw Azure storage-account in 7 tot 10 dagen voltooien. 

### <a name="monitoring-azure-import-job-status"></a>Controle van status van de Azure Import-taak
U kunt de status van uw project importeren vanuit Azure portal controleren door te navigeren naar de **voor importeren/exporteren taken** pagina en het selecteren van de taak. Zie voor meer informatie over de status van de taken van gegevensimport de [opslag importeren exporteren service](../storage/common/storage-import-export-service.md) artikel.

### <a name="complete-the-workflow"></a>Voltooien van de werkstroom
Nadat de import-taak is voltooid, is eerste back-upgegevens beschikbaar in uw opslagaccount. Op het moment van de volgende geplande back-up kopieert Azure Backup de inhoud van de gegevens van het opslagaccount naar de Recovery Services-kluis, zoals hieronder wordt weergegeven: 

   ![Gegevens kopiëren naar de Recovery Services-kluis](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Op het moment van de volgende geplande back-up voert Azure Backup een incrementele back-up.

### <a name="cleaning-up-resources"></a>Opschonen van resources
Nadat de eerste back-up voltooid is, kunt u de gegevens die zijn geïmporteerd naar de Azure Storage-Container en de back-upgegevens op de locatie van de tijdelijke veilig verwijderen.

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor vragen over de Azure Import/Export-werkstroom, [de Microsoft Azure Import/Export-service gebruiken om gegevens te brengen naar Blob storage](../storage/common/storage-import-export-service.md).
* Raadpleeg het gedeelte offline back-up van de Azure Backup [Veelgestelde vragen over](backup-azure-backup-faq.md) voor vragen over de werkstroom.
