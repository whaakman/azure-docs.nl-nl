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
ms.date: 4/20/2017
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: 074d21269206b243f8b0e8747811544132805229
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Werkstroom voor offline back-ups maken in Azure Backup
Azure Backup heeft diverse ingebouwde efficiency die netwerk- en kosten tijdens de eerste volledige back-ups van gegevens naar Azure besparen. Eerste volledige back-ups brengen grote hoeveelheden gegevens doorgaans en meer netwerkbandbreedte in vergelijking met de volgende back-ups waarbij alleen de delta's / incrementele worden overgedragen. Azure back-up comprimeren van de eerste back-ups. Door het proces van het offline seeding kunt Azure Backup schijven gebruiken voor het uploaden van de gecomprimeerde eerste back-upgegevens offline naar Azure.  

Het proces offline seeding van Azure Backup is nauw geïntegreerd met de [Azure Import/Export-service](../storage/common/storage-import-export-service.md) waarmee u gegevens overdraagt naar Azure met behulp van schijven. Als u de eerste back-upgegevens die worden overgedragen via een netwerk met hoge latentie en een lage bandbreedte terabyte (TBs) hebt, kunt u de werkstroom offline seeding kunt gebruiken voor het verzenden van de eerste back-up op een of meer harde schijven voor een Azure-datacenter. Dit artikel bevat een overzicht van de stappen die deze werkstroom voltooien.

## <a name="overview"></a>Overzicht
Met de functie offline seeding van de Azure Backup en Azure Import/Export is het eenvoudig de gegevens offline uploaden naar Azure met behulp van schijven. In plaats van de eerste volledige kopie via het netwerk wordt overgedragen, wordt de back-upgegevens geschreven naar een *faseringslocatie*. Nadat het kopiëren naar de faseringslocatie met het hulpprogramma Azure Import/Export is voltooid, wordt deze gegevens naar een of meer SATA-stations, afhankelijk van de hoeveelheid gegevens geschreven. Deze stations worden uiteindelijk naar de dichtstbijzijnde Azure-datacenter verzonden.

De [augustus 2016 bijwerken van Azure Backup (en hoger)](http://go.microsoft.com/fwlink/?LinkID=229525) bevat de *hulpprogramma voor systeemvoorbereiding van Azure-schijf*, AzureOfflineBackupDiskPrep, met de naam die:

* Helpt u bij de stations met het hulpprogramma Azure Import/Export voorbereiden Azure Import.
* Maakt automatisch een Azure Import-taak voor de Azure Import/Export-service op de [klassieke Azure-portal](https://manage.windowsazure.com) in plaats van dezelfde handmatig te maken met oudere versies van Azure Backup.

Nadat het uploaden van de back-upgegevens naar Azure is voltooid, Azure Backup back-up worden de gegevens gekopieerd naar de back-upkluis en de incrementele back-ups zijn gepland.

> [!NOTE]
> Zorg ervoor dat de update augustus 2016 van Azure Backup (of hoger) hebt geïnstalleerd en voer de stappen van de werkstroom met het voor het gebruik van het hulpprogramma voor systeemvoorbereiding van Azure-schijf. Als u een oudere versie van Azure Backup gebruikt, kunt u de SATA harde schijf voorbereiden met het Azure Import/Export-hulpprogramma zoals beschreven in latere secties van dit artikel.
>
>

## <a name="prerequisites"></a>Vereisten
* [Maak uzelf bekend met de Azure Import/Export-werkstroom](../storage/common/storage-import-export-service.md).
* Voordat u begint de werkstroom, Controleer het volgende:
  * Een Azure Backup-kluis is gemaakt.
  * Referenties voor de kluis zijn gedownload.
  * De Azure Backup agent is geïnstalleerd op Windows Server/Windows client- of System Center Data Protection Manager en de computer is geregistreerd bij de Azure Backup-kluis.
* [Downloaden van de instellingen voor het publiceren van Azure](https://manage.windowsazure.com/publishsettings) op de computer van waaruit u wilt back-up van uw gegevens.
* Bereid de faseringslocatie, dit is mogelijk een netwerkshare of een extra schijf op de computer. De faseringslocatie tijdelijke opslag en tijdelijk tijdens deze werkstroom wordt gebruikt. Zorg ervoor dat de faseringslocatie onvoldoende schijfruimte beschikbaar voor het opslaan van uw eerste kopie. Bijvoorbeeld, als u back-up van een bestandsserver van 500 GB wilt, zorg ervoor dat het faseringsgebied ten minste 500 GB. (Een lager bedrag wordt gebruikt als gevolg van comprimering.)
* Zorg ervoor dat u een ondersteunde station. Alleen 2,5 inch SSD of 2.5 of 3.5-inch SATA III-II interne harde schijven worden ondersteund voor gebruik met de Import/Export-service. U kunt harde schijven tot 10 TB. Controleer de [documentatie van de service Azure Import/Export](../storage/common/storage-import-export-service.md#hard-disk-drives) voor de meest recente set met schijven die ondersteuning biedt voor de service.
* BitLocker inschakelen op de computer waarop de writer SATA-station is aangesloten.
* [Download het hulpprogramma Azure Import/Export](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) op de computer waarop de writer SATA-station is aangesloten. Deze stap is niet vereist als u hebt gedownload en deze update augustus 2016 van Azure Backup (of hoger) is geïnstalleerd.

## <a name="workflow"></a>Werkstroom
De informatie in deze sectie helpt u de werkstroom voor offline back-up uitvoeren, zodat uw gegevens kunnen worden geleverd aan een Azure-datacenter en geüpload naar Azure Storage. Als u vragen over de service importeren of een aspect van het proces hebt, raadpleegt u de [importeren Serviceoverzicht](../storage/common/storage-import-export-service.md) documentatie waarnaar wordt verwezen eerder.

### <a name="initiate-offline-backup"></a>Offline back-up starten
1. Als u een back-up plannen, ziet u het volgende scherm (in Windows Server, Windows-client of System Center Data Protection Manager).

    ![Scherm importeren](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    Hier wordt het bijbehorende scherm in System Center Data Protection Manager: <br/>
    ![DPM-importscherm](./media/backup-azure-backup-import-export/dpmoffline.png)

    De beschrijving van de invoerwaarden is als volgt:

    * **Tijdelijke locatie**: de tijdelijke opslaglocatie waarnaar de eerste back-up is geschreven. Dit kan zijn op een netwerkshare of een lokale computer. Als de computer van de kopie en de broncomputer verschillen, raden wij aan dat u het pad van het volledige netwerk van de faseringslocatie opgeeft.
    * **Azure Import-taaknaam**: de unieke naam welke Azure Import-service en Azure Backup de overdracht van gegevens die worden verzonden op schijven naar Azure bijhouden.
    * **Azure Publish Settings**: een XML-bestand dat informatie over het profiel van uw abonnement bevat. Het bevat ook beveiligde referenties die gekoppeld aan uw abonnement zijn. U kunt [download het bestand](https://manage.windowsazure.com/publishsettings). Geef het lokale pad naar het bestand publicatie-instellingen.
    * **Azure-abonnements-ID**: de Azure-abonnements-ID voor het abonnement waarbij u van plan bent om te starten van de Azure Import-taak. Als u meerdere Azure-abonnementen hebt, gebruikt u de ID van het abonnement dat u wilt koppelen aan de import-taak.
    * **Azure Storage-Account**: het klassieke type opslagaccount in de opgegeven Azure-abonnement dat wordt gekoppeld aan de Azure Import-taak.
    * **Azure Storage-Container**: de naam van de bestemmings-blob-opslag in de Azure storage-account waarin gegevens van deze taak worden geïmporteerd.

    > [!NOTE]
    > Als u uw server hebt geregistreerd voor een Azure Recovery Services-kluis uit de [Azure-portal](https://portal.azure.com) voor uw back-ups en zich niet op een abonnement Cloud Solution Provider (CSP) u kunt nog steeds een klassieke type opslagaccount maken van de Azure Portal en deze gebruiken voor de werkstroom voor offline back-up.
    >
    >

     Al deze gegevens worden opgeslagen omdat u opnieuw invoeren wilt in de volgende stappen. Alleen de *faseringslocatie* is vereist als u het hulpprogramma voor systeemvoorbereiding van Azure-schijf gebruikt voor het voorbereiden van de schijven.    
2. Voltooien van de werkstroom en selecteer vervolgens **Back-Up uit** in de Azure Backup-beheerconsole de kopie offline back-up te starten. De eerste back-up is geschreven naar het faseringsgebied als onderdeel van deze stap.

    ![Nu reservekopie maken](./media/backup-azure-backup-import-export/backupnow.png)

    Voor het voltooien van de bijbehorende werkstroom in System Center Data Protection Manager met de rechtermuisknop op de **beveiligingsgroep**, en kies vervolgens de **herstelpunt maken** optie. Kiest u de **Onlinebeveiliging** optie.

    ![DPM back-up nu](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Nadat de bewerking is voltooid, wordt de faseringslocatie is gereed om te worden gebruikt voor de schijfvoorbereiding van de.

    ![Voortgang van de back-up](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-a-sata-drive-and-create-an-azure-import-job-by-using-the-azure-disk-preparation-tool"></a>Bereid een SATA harde schijf en Azure Import-taak maken met behulp van het hulpprogramma voor systeemvoorbereiding van Azure-schijf
Het hulpprogramma voor systeemvoorbereiding van Azure-schijf is beschikbaar in de map voor installatie van de Recovery Services-agent (augustus 2016 bijwerken en hoger) in het volgende pad.

   *\Microsoft* *azure* *herstel* *Services* * Agent\Utils\*

1. Ga naar de map en kopieer de **AzureOfflineBackupDiskPrep** map op een kopie-computer waarop de stations worden voorbereid zijn gekoppeld. Zorg ervoor dat de volgende met betrekking tot de computer kopiëren:

    * De computer kopiëren de faseringslocatie voor de werkstroom offline seeding toegang met behulp van hetzelfde netwerkpad dat is opgegeven in de **offline back-up starten** werkstroom.
    * BitLocker is ingeschakeld op de computer.
    * De computer, hebben toegang tot de Azure-portal.

    Indien nodig, kan de computer kopiëren niet hetzelfde als de broncomputer.
2. Open een opdrachtprompt met verhoogde bevoegdheid op de computer kopiëren met de voorbereiding van de Azure-schijf hulpprogramma directory als de huidige map en voer de volgende opdracht:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path to PublishSettingsFile*>]`

    | Parameter | Beschrijving |
    | --- | --- |
    | s:&lt;*locatiepad fasering*&gt; |Verplichte invoer die wordt gebruikt voor het pad naar de faseringslocatie die u hebt ingevoerd in de **offline back-up starten** werkstroom. |
    | p:&lt;*pad naar PublishSettingsFile*&gt; |Optionele invoer die wordt gebruikt voor het pad naar de **Azure Publish Settings** -bestand dat u hebt ingevoerd in de **offline back-up starten** werkstroom. |

    > [!NOTE]
    > De &lt;pad naar PublishSettingFile&gt; waarde is verplicht wanneer de computer van de kopie en de broncomputer verschillen.
    >
    >

    Als u de opdracht uitvoert, vraagt het hulpprogramma voor de selectie van de Azure Import-taak die overeenkomt met de stations die moeten worden voorbereid. Als er slechts een enkele import-taak gekoppeld aan de opgegeven faseringslocatie is, ziet u een scherm, zoals de link die erna.

    ![Azure schijfvoorbereiding hulpprogramma invoer](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Voer de stationsletter zonder de afsluitende dubbele punt voor de gekoppelde schijf die u wilt voorbereiden voor overdracht naar Azure. Geeft u de bevestiging voor de opmaak van het station als u wordt gevraagd.

    Het hulpprogramma begint vervolgens met het voorbereiden van de schijf met de back-upgegevens. U moet mogelijk extra schijven als u wordt gevraagd door het hulpprogramma voor het geval de opgegeven schijf niet voldoende ruimte voor de back-upgegevens heeft koppelen. <br/>

    Aan het einde van een geslaagde uitvoering van het hulpprogramma, worden een of meer schijven die u hebt opgegeven voorbereid voor back-ups naar Azure. Bovendien een import-taak met de naam u hebt opgegeven tijdens de **offline back-up starten** werkstroom is gemaakt op de klassieke Azure portal. Ten slotte weergegeven de tool het verzendadres naar het Azure datacenter waarin de schijven moeten worden verzonden en de koppeling naar de import-taak vinden op de klassieke Azure portal.

    ![De voorbereiding van de Azure-schijf is voltooid](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Verzenden van de schijven naar het adres dat u de hulpprogramma's beschikbaar gesteld en houd het volgnummer voor toekomstig gebruik.<br/>

5. Wanneer u de koppeling die het hulpprogramma wordt weergegeven gaat, ziet u de Azure storage-account die u hebt opgegeven in de **offline back-up starten** werkstroom. Hier ziet u de zojuist gemaakte import-taak op de **voor importeren/EXPORTEREN** tabblad van het opslagaccount.

    ![Gemaakte import-taak](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>

6. Klik op **back-UPFUNCTIE INFO** onder aan de pagina om uw contactgegevens bijwerken zoals weergegeven in het volgende scherm. Microsoft gebruikt deze gegevens te verzenden van uw schijven naar u terug nadat de import-taak is voltooid.

    ![Contactgegevens](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>

7. Geef de back-ups van gegevens op het volgende scherm. Geef de **levering Carrier** en **volgnummer** details die overeenkomen met de schijven die u hebt verzonden naar het Azure-datacenter.

    ![Back-ups van gegevens](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### <a name="complete-the-workflow"></a>Voltooien van de werkstroom
Nadat de import-taak is voltooid, is eerste back-upgegevens beschikbaar in uw opslagaccount. De Recovery Services-agent en vervolgens kopieert de inhoud van de gegevens van dit account naar de Backup-kluis of een Recovery Services-kluis, afhankelijk van wat van toepassing is. In de volgende geplande back-tijd voert de Azure Backup agent de incrementele back-up via de eerste back-up.

> [!NOTE]
> De volgende secties van toepassing op gebruikers van eerdere versies van Azure Backup die geen toegang tot het hulpprogramma voor systeemvoorbereiding van Azure-schijf.
>
>

### <a name="prepare-a-sata-drive"></a>Een SATA-station voorbereiden
1. Download de [Microsoft Azure-hulpprogramma voor importeren/exporteren](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) op de computer kopiëren. Zorg ervoor dat de faseringslocatie toegankelijk is vanaf de computer waarop u van plan bent om uit te voeren van de volgende reeks opdrachten. Indien nodig, kan de computer kopiëren niet hetzelfde als de broncomputer.

2. Pak het bestand WAImportExport.zip. Voer het hulpprogramma WAImportExport die de SATA harde schijf wordt geformatteerd en versleutelt de back-upgegevens schrijft naar de SATA harde schijf. Voordat u de volgende opdracht uitvoert, zorg ervoor dat BitLocker is ingeschakeld op de computer. <br/>

    `*.\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*staging location*> /dstdir: <*DestinationBlobVirtualDirectory*>/*`

    > [!NOTE]
    > Als u de update augustus 2016 van Azure Backup (of hoger) hebt geïnstalleerd, zorg ervoor dat de faseringslocatie die u hebt ingevoerd dezelfde is als die op de **Back-Up uit** scherm en AIB en Base Blob bestanden bevat.
    >
    >

| Parameter | Beschrijving |
| --- | --- |
| /j: <*JournalFile*> |Het pad naar het journaalbestand. Elk station moet precies één journaalbestand hebben. De wijzigingslogboek-bestand moet niet op de doel-schijf. De extensie van het wijzigingslogboek is .jrn en wordt gemaakt als onderdeel van deze opdracht uit te voeren. |
| /ID: <*sessie-id*> |De sessie-ID identificeert een kopieersessie. Om ervoor te zorgen nauwkeurige herstel van een sessie onderbroken kopiëren wordt gebruikt. Bestanden die zijn gekopieerd in een kopieersessie worden opgeslagen in een map met de naam van de sessie-ID op de doel-schijf. |
| /SK: <*StorageAccountKey*> |De accountsleutel voor het opslagaccount waarnaar de gegevens zijn geïmporteerd. De sleutel moet hetzelfde tijdens het maken van back-beleid en/of beveiligingsgroep die is ingevoerd. |
| / BlobType |Het type van de blob. Deze werkstroom slaagt alleen als **PageBlob** is opgegeven. Dit is niet de standaardoptie en moet het worden vermeld in deze opdracht. |
| / t: <*TargetDriveLetter*> |De stationsletter zonder de afsluitende dubbele punt van de harde schijf doel voor de huidige kopieersessie. |
| / Format |De optie voor het formatteren van de schijf. Geef deze parameter als het station worden geformatteerd moet. anders weglaten. Voordat u het hulpprogramma voor het station is geformatteerd, wordt de gevraagd om een bevestiging van de console. Als u wilt de bevestiging onderdrukken, geeft u de parameter /silentmode. |
| / versleutelen |De optie voor het versleutelen van het station. Deze parameter opgeven wanneer de schijf nog niet is versleuteld met BitLocker en moet worden gecodeerd door het hulpprogramma. Als het station al zijn versleuteld met BitLocker, deze parameter geeft u de parameter /bk en de bestaande BitLocker-sleutel opgeven. Als u de parameter/Format opgeeft, moet u ook opgeven de / versleutelen van parameter. |
| /srcdir: <*SourceDirectory*> |De bronmap die bestanden worden gekopieerd naar het doelstation bevat. Zorg ervoor dat de naam van de opgegeven map voor een volledige in plaats van relatieve pad. |
| /dstdir: <*DestinationBlobVirtualDirectory*> |Het pad naar de virtuele map van de bestemming in uw Azure storage-account. Zorg ervoor dat namen van de geldige container gebruiken wanneer u de doel-virtuele mappen of blobs opgeeft. Houd er rekening mee dat de namen van containers in kleine letters moeten zijn.  De containernaam van deze moet degene die u hebt opgegeven tijdens het maken van back-beleid en/of beveiligingsgroep. |

> [!NOTE]
> Een journal-bestand wordt gemaakt in de map WAImportExport waarmee de volledige gegevens van de werkstroom worden vastgelegd. U moet dit bestand als u een import-taak in de Azure-portal maken.
>
>

  ![PowerShell-resultaat](./media/backup-azure-backup-import-export/psoutput.png)

### <a name="create-an-import-job-in-the-azure-portal"></a>Een importtaak maken in de Azure portal
1. Ga naar uw opslagaccount in de [klassieke Azure-portal](https://manage.windowsazure.com/), klikt u op **voor importeren/exporteren**, en vervolgens **Import-taak maken** in het taakvenster.

    ![Tabblad in de Azure-portal voor importeren/exporteren](./media/backup-azure-backup-import-export/azureportal.png)

2. In stap 1 van de wizard aangeven dat u de schijf hebt voorbereid en u het station journaal-bestand beschikbaar hebt.

3. In stap 2 van de wizard kunt u contactgegevens voor de persoon die verantwoordelijk voor deze import-taak is bevatten.

4. Upload de station journaal-bestanden die u in de vorige sectie hebt verkregen in stap 3.

5. Voer een beschrijvende naam voor de import-taak die u hebt opgegeven tijdens het maken van back-beleid/beveiligingsgroep in stap 4. De naam die u invoert mogen alleen kleine letters, cijfers, afbreekstreepjes en onderstrepingstekens bevatten, moet beginnen met een letter en mag geen spaties bevatten. De naam die u kiest, wordt gebruikt voor het bijhouden van uw taken terwijl ze worden uitgevoerd, en nadat ze zijn voltooid.

6. Selecteer vervolgens de regio van uw datacenter in de lijst. De regio datacenter geeft de datacenter en het adres waarnaar het pakket moet worden verzonden.

    ![Selecteer de regio van het datacenter](./media/backup-azure-backup-import-export/dc.png)

7. In stap 5, selecteer uw return carrier in de lijst en voer de nummer van uw provider. Microsoft gebruikt deze account voor het verzenden van uw schijven naar u terug nadat de import-taak is voltooid.

8. Verzend de schijf en voer het nummer bijhouden om de status van de verzending te volgen. Nadat de schijf is binnengekomen in het datacenter, dit wordt gekopieerd naar het opslagaccount en de status is bijgewerkt.

    ![Voltooide status](./media/backup-azure-backup-import-export/complete.png)

### <a name="complete-the-workflow"></a>Voltooien van de werkstroom
Nadat de eerste back-upgegevens is beschikbaar in uw opslagaccount, de Microsoft Azure Recovery Services agent kopieert de inhoud van de gegevens van dit account naar de Backup-kluis of Recovery Services-kluis, afhankelijk van wat van toepassing is. In het volgende geplande back-tijdstip voert de Azure Backup agent de incrementele back-up via de eerste back-up.

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg voor vragen over de Azure Import/Export-werkstroom, [de Microsoft Azure Import/Export-service gebruiken om gegevens te brengen naar Blob storage](../storage/common/storage-import-export-service.md).
* Raadpleeg het gedeelte offline back-up van de Azure Backup [Veelgestelde vragen over](backup-azure-backup-faq.md) voor vragen over de werkstroom.
