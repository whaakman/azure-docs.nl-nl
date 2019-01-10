---
title: Azure Backup - Offline back-up of de eerste seeding via de Azure Import/Export-service
description: Meer informatie over hoe Azure Backup kunt u voor het verzenden van gegevens vanuit het netwerk met behulp van de Azure Import/Export-service. In dit artikel wordt uitgelegd in de offline seeding van de eerste back-upgegevens met behulp van de Azure Import Export-service.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: saurse
ms.openlocfilehash: 9d91ccd04ed06fb6c256a2d9911202d7df6d08a5
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188297"
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Werkstroom voor offline back-ups maken in Azure Backup
Azure Backup heeft diverse ingebouwde efficiëntie die netwerk- en kosten tijdens de eerste volledige back-ups van gegevens naar Azure besparen. Initiële volledige back-ups wordt gewoonlijk grote hoeveelheden gegevens overdragen en meer netwerkbandbreedte in vergelijking met de volgende back-ups waarbij alleen de delta's / plaatsvindt worden overgedragen. Door het proces van het offline-seeding, kunt Azure Backup gebruiken schijven voor de offline back-upgegevens uploaden naar Azure.

De Azure Backup-proces offline-seeding is nauw geïntegreerd met de [Azure Import/Export-service](../storage/common/storage-import-export-service.md) waarmee u kunt de eerste back-upgegevens overbrengen naar Azure met behulp van schijven. Als u terabytes (TB) aan de eerste back-upgegevens die moeten worden overgebracht via een netwerk met hoge latentie en lage bandbreedte hebt, kunt u de werkstroom voor offline-seeding voor het verzenden van de eerste back-up op een of meer harde schijven, naar een Azure-datacenter. De volgende afbeelding bevat een overzicht van de stappen in de werkstroom.

  ![overzicht van de werkstroom voor offline importproces](./media/backup-azure-backup-import-export/offlinebackupworkflowoverview.png)

Het offline back-upproces omvat de volgende stappen uit:

1. De back-upgegevens verzenden via het netwerk, in plaats van de back-upgegevens naar een tijdelijke locatie te schrijven.
2. Gebruik het hulpprogramma AzureOfflineBackupDiskPrep schrijft de gegevens in de faseringslocatie aan een of meer SATA-schijven.
3. Als onderdeel van de werkzaamheden maakt het hulpprogramma AzureOfflineBackupDiskPrep een Azure Import-taak. De SATA-schijven verzenden naar de dichtstbijzijnde Azure-datacenter en verwijzen naar de importtaak voor het verbinding maken met de activiteiten.
4. In de Azure-datacenter, worden de gegevens op de schijven worden gekopieerd naar een Azure storage-account.
5. Azure Backup de back-upgegevens gekopieerd van het opslagaccount naar de Recovery Services-kluis en incrementele back-ups zijn gepland.

## <a name="supported-configurations"></a>Ondersteunde configuraties 
Gebruik van Offline back-up wordt ondersteund door de volgende functies van Azure Backup of werkbelastingen.

> [!div class="checklist"]
> * Back-up van bestanden en mappen met de agent van Microsoft Azure Recovery Services (MARS), ook wel de Azure Backup agent genoemd. 
> * Back-up van alle werkbelastingen en bestanden met System Center Data Protection Manager (DPM SC) 
> * Back-up van alle werkbelastingen en bestanden met Microsoft Azure Backup Server <br/>

   > [!NOTE]
   > Offline back-up wordt niet ondersteund voor System State back-ups wordt gedaan via de Azure backup-agent. 

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="prerequisites"></a>Vereisten

  > [!NOTE]
  > De volgende vereisten en werkstroom gelden alleen voor Offline back-up van bestanden en mappen met behulp van de [nieuwste MARS-agent](https://aka.ms/azurebackup_agent). Als u Offline back-ups voor werkbelastingen met behulp van System Center DPM of Azure Backup Server, raadpleegt u [in dit artikel](backup-azure-backup-server-import-export-.md). 

Voordat u de werkstroom voor Offline back-up begint, moet u de volgende vereisten voldoen: 
* Maak een [Recovery Services-kluis](backup-azure-recovery-services-vault-overview.md). Voor het maken van een kluis, raadpleegt u de stappen in [in dit artikel](tutorial-backup-windows-server-to-azure.md#create-a-recovery-services-vault)
* Zorg ervoor dat alleen de [meest recente versie van de Azure Backup agent](https://aka.ms/azurebackup_agent) op de Windows Server/Windows-client, zoals van toepassing is geïnstalleerd en de computer is geregistreerd bij de Recovery Services-kluis.
* Azure PowerShell 3.7.0 is vereist op de computer met Azure backup-agent. Het is raadzaam dat u downloadt en [versie van Azure PowerShell installeren de 3.7.0](https://github.com/Azure/azure-powershell/releases/tag/v3.7.0-March2017).
* Controleer of Microsoft Edge of Internet Explorer 11 is geïnstalleerd, en JavaScript is ingeschakeld op de computer waarop Azure backup-agent wordt uitgevoerd. 
* Een Azure Storage-account maken in hetzelfde abonnement als de Recovery Services-kluis. 
* Zorg ervoor dat u hebt de [benodigde machtigingen](../active-directory/develop/howto-create-service-principal-portal.md) om de Azure Active Directory-toepassing te maken. De werkstroom voor Offline back-up wordt gemaakt van een Azure Active Directory-toepassing in het abonnement dat is gekoppeld aan het Azure Storage-account. Het doel van de toepassing is om te voorzien van Azure Backup toegang tot beveiligde en binnen het bereik met de Azure Import-Service vereist voor de werkstroom voor Offline back-up. 
* Registreer de resourceprovider Microsoft.ImportExport met het abonnement met de Azure Storage-account. De resourceprovider registreren:
    1. Klik in het hoofdmenu van **abonnementen**.
    2. Als u bent geabonneerd op meerdere abonnementen, selecteert u het abonnement dat u voor de offline back-up. Als u slechts één abonnement hebt gebruikt, is uw abonnement wordt weergegeven.
    3. Klik in het menu abonnement op **Resourceproviders** om de lijst met providers weer te geven.
    4. In de lijst met providers Blader naar beneden naar Microsoft.ImportExport. Als de Status NotRegistered is, klikt u op **registreren**.
    ![de resourceprovider registreren](./media/backup-azure-backup-import-export/registerimportexport.png)
* Een tijdelijke locatie, dit kan een netwerkshare of een extra station op de computer, intern of extern, met voldoende schijfruimte voor het opslaan van uw eerste kopie wordt gemaakt. Bijvoorbeeld, als u back-up van een bestandsserver met 500 GB wilt, controleert u of het faseringsgebied ten minste 500 GB. (Een lager bedrag vanwege compressie wordt gebruikt.)
* Bij het verzenden van schijven naar Azure, alleen 2,5 inch SSD, of 2,5-inch of 3,5-inch SATA II/III interne harde schijven gebruiken. Harde schijven kunt u maximaal 10 TB. Controleer de [documentatie voor Azure Import/Export-service](../storage/common/storage-import-export-requirements.md#supported-hardware) voor de meest recente set van schijven die ondersteuning biedt voor de service.
* De SATA-schijven moeten worden aangesloten op een computer (aangeduid als een *kopie*) waar de kopie van de back-upgegevens van de *faseringslocatie* naar de SATA stations wordt uitgevoerd. Zorg ervoor dat Bitlocker is ingeschakeld op de *kopie*.

## <a name="workflow"></a>Werkstroom
Deze sectie beschrijft de werkstroom voor offline back-ups, zodat uw gegevens kunnen worden geleverd aan een Azure-datacenter en geüpload naar Azure Storage. Als u vragen over de Import-service of een aspect van het proces hebt, raadpleegt u de [importeren van documentatie voor service-overzicht](../storage/common/storage-import-export-service.md).

## <a name="initiate-offline-backup"></a>Offline back-up starten
1. Wanneer u een back-up van de MARS-Agent plant, ziet u het volgende scherm.

    ![Voor importeren](./media/backup-azure-backup-import-export/offlinebackup_inputs.png)

  De beschrijving van de invoer is als volgt:

    * **Faseringslocatie**: De tijdelijke opslaglocatie waarop de eerste back-up is geschreven. Faseringslocatie mogelijk op een netwerkshare of een lokale computer. Als de computer kopiëren en de broncomputer verschillend zijn, wordt u aangeraden dat u het volledige netwerk-pad van de faseringslocatie opgeven.
    * **Azure Resource Manager-Opslagaccount**: De naam van het Resource Manager type opslagaccount dat in elk Azure-abonnement.
    * **Azure-Opslagcontainer**: De naam van de bestemmings-blob-opslag in de Azure Storage-account waarin de back-upgegevens wordt geïmporteerd voordat ze worden gekopieerd naar de Recovery Services-kluis.
    * **Azure-abonnement-ID**: De ID voor de Azure-abonnement waarin de Azure Storage-account is gemaakt.
    * **Azure Import-taaknaam**: De unieke naam die Azure Import-service en Azure Backup de overdracht van gegevens die worden verzonden op schijven naar Azure bijhouden. 
  
  Geef de invoer op het scherm en klik op **volgende**. De opgegeven opslaan *faseringslocatie* en de *Azure Import-taaknaam*, zoals deze informatie vereist voor het voorbereiden van de schijven is.

2. Wanneer u hierom wordt gevraagd, meldt u zich bij uw Azure-abonnement. U moet zich aanmelden zodat Azure Backup kan de Azure Active Directory-toepassing maken en geef de vereiste machtigingen voor toegang tot de Azure Import-Service.

    ![Back-up-nu](./media/backup-azure-backup-import-export/azurelogin.png)

3. Voltooien van de werkstroom en klik in de console van de Azure Backup-agent op **nu een Back-Up maken**.

    ![Back-up-nu](./media/backup-azure-backup-import-export/backupnow.png)

4. Klik in de pagina Bevestiging van de wizard op **Back-Up**. De eerste back-up is geschreven naar het faseringsgebied als onderdeel van de installatie.

   ![Bevestig dat u back-up nu kunt](./media/backup-azure-backup-import-export/backupnow-confirmation.png)

    Nadat de bewerking is voltooid, wordt de faseringslocatie is gereed om te worden gebruikt voor de schijfvoorbereiding van de.

   ![Back-up-nu](./media/backup-azure-backup-import-export/opbackupnow.png)

## <a name="prepare-sata-drives-and-ship-to-azure"></a>SATA-schijven voorbereiden en verzenden naar Azure
De *AzureOfflineBackupDiskPrep* hulpprogramma bereidt de SATA-schijven die worden verzonden naar de dichtstbijzijnde Azure-datacenter. Dit hulpprogramma is beschikbaar in de installatiemap van de Azure Backup-agent (in het volgende pad):

   *\Microsoft azure Recovery Services-Agent\Utils\\*

1. Ga naar de map en kopieer de **AzureOfflineBackupDiskPrep** map naar een andere computer waar de SATA-schijven zijn verbonden. Op de computer met de verbonden zijn met SATA-schijven, zorg ervoor dat:

    * De computer kopiëren hebt toegang tot de faseringslocatie voor de werkstroom voor offline-seeding via hetzelfde netwerkpad dat is opgegeven in de **start offline back-up** werkstroom.
    * BitLocker is ingeschakeld op de computer kopiëren.
    * Azure PowerShell 3.7.0 is geïnstalleerd.
    * De meest recente compatibel browsers (Microsoft Edge of Internet Explorer 11) zijn geïnstalleerd en JavaScript is ingeschakeld. 
    * De computer kopiëren, hebben toegang tot de Azure-portal. Indien nodig, is de computer kopiëren hetzelfde als de broncomputer kan verzenden.
    
    > [!IMPORTANT] 
    > Als de broncomputer een virtuele machine is, klikt u vervolgens moet de kopie-computer een andere fysieke server of clientcomputer van de broncomputer.

2. Open een opdrachtprompt met verhoogde bevoegdheid op de computer kopiëren met de *AzureOfflineBackupDiskPrep* hulpprogramma directory als de huidige map en voer de volgende opdracht uit:

    ```.\AzureOfflineBackupDiskPrep.exe s:<Staging Location Path>```

    | Parameter | Description |
    | --- | --- |
    | s:&lt;*pad naar de locatie van de fasering*&gt; |Verplichte invoer gebruikt voor het pad naar de faseringslocatie die u hebt ingevoerd in de **start offline back-up** werkstroom. |
    | p:&lt;*pad naar PublishSettingsFile*&gt; |Optionele invoer die wordt gebruikt voor het pad naar de **Azure Publish Settings** -bestand dat u hebt ingevoerd in de **start offline back-up** werkstroom. |

    Als u de opdracht uitvoert, vraagt het hulpprogramma voor de selectie van de Azure Import-taak die overeenkomt met de schijven die moeten worden voorbereid. Als er slechts een enkele import-taak gekoppeld aan de opgegeven faseringslocatie is, ziet u een scherm zoals de link die erna.

    ![Azure voorbereiden van de schijf hulpprogramma invoer](./media/backup-azure-backup-import-export/diskprepconsole0_1.png) <br/>

3. Voer de stationsletter zonder de afsluitende dubbele punt voor de gekoppelde schijf die u wilt voorbereiden voor overdracht naar Azure. 
4. Geeft u de bevestiging voor de opmaak van het station wanneer hierom wordt gevraagd.
5. U wordt gevraagd of u zich aanmelden bij uw Azure-abonnement. Geef uw referenties.

    ![Azure voorbereiden van de schijf hulpprogramma invoer](./media/backup-azure-backup-import-export/signindiskprep.png) <br/>

    Het hulpprogramma is vervolgens begint met het voorbereiden van de schijf en worden de back-upgegevens gekopieerd. U moet mogelijk extra schijven wanneer u hierom wordt gevraagd door het hulpprogramma voor het geval de opgegeven schijf beschikt niet over voldoende ruimte voor de back-upgegevens te koppelen. <br/>

    Aan het einde van het hulpprogramma is uitgevoerd biedt de opdrachtprompt drie soorten informatie:
    1. Een of meer schijven die u hebt opgegeven, worden voorbereid voor verzending naar Azure. 
    2. U ontvangt een bevestiging dat uw import-taak is gemaakt. De import-taak maakt gebruik van de naam die u hebt opgegeven.
    3. Het hulpprogramma wordt weergegeven het verzendadres voor de Azure-datacenter.

    ![De voorbereiding van de Azure-schijf is voltooid](./media/backup-azure-backup-import-export/console2.png)<br/>

6. Aan het einde van de uitvoering van de opdracht, kunt u de verzendgegevens bijwerken.

7. Stuur de schijven naar het adres dat de hulpprogramma's beschikbaar gesteld en houd het traceringsnummer voor toekomstig gebruik.

   > [!IMPORTANT] 
   > Geen twee Azure Import-taken kan het dezelfde traceringsnummer hebben. Zorg ervoor dat stations die zijn opgesteld door het hulpprogramma onder een enkele Azure Import-taak samen in één pakket worden verzonden en er is een één unieke nummer voor het pakket. Schijven voorbereid als onderdeel van de afzonderlijke Azure Import-taken in één pakket niet combineren.



## <a name="update-shipping-details-on-the-azure-import-job"></a>Verzendgegevens op de Azure Import-taak bijwerken

De volgende procedure werkt de verzendgegevens van het Azure Import-taak. Deze informatie omvat informatie over:
* de naam van de provider die voorziet in de schijven naar Azure
* verzendgegevens voor de schijven terug

1. Aanmelden bij uw Azure-abonnement.
2. Klik in het hoofdmenu van **alle services** en typ in het dialoogvenster alle importeren. Wanneer de melding **Import/Export-taken**, klikt u erop.
    ![Verzendgegevens invoeren](./media/backup-azure-backup-import-export/search-import-job.png)<br/>

    De lijst met **Import/export-taken** menu wordt geopend en de lijst met alle Import/export-taken in het geselecteerde abonnement wordt weergegeven. 

3. Als u meerdere abonnementen hebt, zorg er dan voor dat het abonnement dat wordt gebruikt voor het importeren van de back-upgegevens selecteren. Selecteer vervolgens de zojuist gemaakte Import-taak de details ervan te openen.

    ![Lees de verzendgegevens](./media/backup-azure-backup-import-export/import-job-found.png)<br/>

4. Klik op het menu instellingen voor de Import-taak **verzendgegevens beheren** en voer de verzendgegevens voor retourneren.

    ![Verzendgegevens opslaan](./media/backup-azure-backup-import-export/shipping-info.png)<br/>

5. Wanneer u het volgnummer van de vervoerder hebt, klikt u op de banner op de overzichtspagina van Azure Import-taak en voer de volgende details:

   > [!IMPORTANT] 
   > Zorg ervoor dat de provider-informatie en Traceringsnummer binnen twee weken na het maken van de Azure import-taak worden bijgewerkt. Fout bij om te controleren of deze informatie binnen twee weken kan resulteren in de taak wordt verwijderd en stations die niet worden verwerkt.

   ![Verzendgegevens opslaan](./media/backup-azure-backup-import-export/joboverview.png)<br/>

   ![Verzendgegevens opslaan](./media/backup-azure-backup-import-export/tracking-info.png)

### <a name="time-to-process-the-drives"></a>Tijd voor het verwerken van de stations 
De hoeveelheid tijd die nodig is om te verwerken die een Azure import-taak, afhankelijk van verschillende factoren, zoals verzendtijd varieert, taak-type, type en grootte van de gegevens worden gekopieerd en de grootte van de schijven die zijn opgegeven. De Azure Import/Export-service beschikt niet over een SLA, maar nadat de schijven zijn ontvangen. de service voor het voltooien van de back-upgegevens kopiëren naar uw Azure storage-account in 7 tot en met 10 dagen streeft ernaar. 

### <a name="monitoring-azure-import-job-status"></a>Status van de Azure Import-taak controleren
U kunt de status van de Import-taak vanuit Azure portal controleren door te navigeren naar de **Import/Export-taken** pagina en uw taak selecteren. Zie voor meer informatie over de status van de taken van gegevensimport, de [service Storage importeren exporteren](../storage/common/storage-import-export-service.md) artikel.

### <a name="complete-the-workflow"></a>Voltooien van de werkstroom
Nadat de import-taak is voltooid, is eerste back-upgegevens beschikbaar in uw storage-account. Op het moment van de volgende geplande back-up kopieert Azure Backup de inhoud van de gegevens van het opslagaccount naar de Recovery Services-kluis, zoals hieronder weergegeven: 

   ![Kopiëren van gegevens naar de Recovery Services-kluis](./media/backup-azure-backup-import-export/copyingfromstorageaccounttoazurebackup.png)<br/>

Op het moment van de volgende geplande back-up voert Azure Backup een incrementele back-up.

### <a name="cleaning-up-resources"></a>Opschonen van resources
Nadat de eerste back-up voltooid is, kunt u de gegevens geïmporteerd in de Azure-Opslagcontainer en de back-upgegevens in de Faseringslocatie veilig verwijderen.

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor vragen over de Azure Import/Export-werkstroom, [gebruik van de Microsoft Azure Import/Export-service gegevens overdraagt naar Blob-opslag](../storage/common/storage-import-export-service.md).
* Raadpleeg het gedeelte offline back-ups van de Azure backup- [Veelgestelde vragen over](backup-azure-backup-faq.md) voor vragen over de werkstroom.
