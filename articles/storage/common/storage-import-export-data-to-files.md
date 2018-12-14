---
title: Met behulp van Azure Import/Export gegevens overdraagt naar Azure Files | Microsoft Docs
description: Informatie over het maken van taken van gegevensimport in Azure portal mag gegevens overdragen naar Azure Files.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 12/13/2018
ms.author: alkohli
ms.component: common
ms.openlocfilehash: 30d0818b57057785784c1fbda1c67ca0be10d769
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384765"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>Azure Import/Export-service gebruiken om gegevens te importeren naar Azure Files

In dit artikel bevat stapsgewijze instructies over het gebruik van de Azure Import/Export-service veilig grote hoeveelheden gegevens te importeren in Azure Files. Om gegevens te importeren, moet de service u voor het verzenden van ondersteunde harde schijven met uw gegevens naar een Azure-datacenter.  

De Import/Export-service ondersteunt alleen importeren van Azure Files naar Azure Storage. Het exporteren van Azure-bestanden wordt niet ondersteund.

## <a name="prerequisites"></a>Vereisten

Voordat u een importtaak om over te dragen van gegevens naar Azure Files maakt, zorgvuldig controleren en voer de volgende lijst met vereisten. U moet:

- Een actief Azure-abonnement voor gebruik met Import/Export-service hebben.
- Ten minste één Azure Storage-account hebben. Overzicht van de [ondersteunde opslagaccounts en typen gegevensopslag voor Import/Export-service](storage-import-export-requirements.md). Zie voor meer informatie over het maken van een nieuw opslagaccount [over het maken van een Storage-Account](storage-quickstart-create-account.md).
- Schijven van voldoende [ondersteunde typen](storage-import-export-requirements.md#supported-disks). 
- Een Windows-systeem met een [ondersteunde besturingssysteemversie](storage-import-export-requirements.md#supported-operating-systems).
- [De WAImportExport versie 2 downloaden](https://aka.ms/waiev2) op het Windows-systeem. Pak deze uit naar de standaardmap `waimportexport`. Bijvoorbeeld `C:\WaImportExport`.
- Een FedEx/DHL-account hebben. 
    - Het account moet geldig zijn, saldo moet hebben en moet return verzending mogelijkheden hebben.
    - Een nummer voor de taak uitvoer gegenereerd.
    - Elke taak moet een afzonderlijke traceringsnummer hebben. Meerdere taken met de dezelfde traceringsnummer worden niet ondersteund.
    - Als u een provider-account hebt, gaat u naar:
        - [Maak een account FedEX](https://www.fedex.com/en-us/create-account.html), of 
        - [Maak een account DHL](http://www.dhl-usa.com/en/express/shipping/open_account.html).
 


## <a name="step-1-prepare-the-drives"></a>Stap 1: Voorbereiden van de schijven

Deze stap wordt een logboekbestand gegenereerd. Het logboekbestand bevat basisinformatie zoals serienummer van station, versleutelingssleutel en details opslagaccount.

Voer de volgende stappen voor het voorbereiden van de schijven.

1. Onze schijfstations verbinden met het Windows-systeem via SATA-connectors.
2. Maak een enkele NTFS-volume op elke schijf. Een stationsletter toewijzen aan het volume. Stel de volgende parameter niet gebruiken
3. Wijzig de *dataset.csv* bestand in de hoofdmap waarin het hulpprogramma zich bevindt. Afhankelijk van of u importeren van een bestand of map of beide wilt, vermeldingen toe te voegen in de *dataset.csv* bestand die vergelijkbaar is met de volgende voorbeelden.  

    - **Een bestand importeren**: In het volgende voorbeeld is de gegevens te kopiëren bevindt zich in de C:-station. Het bestand *MyFile1.txt* wordt gekopieerd naar de hoofdmap van de *MyAzureFileshare1*. Als de *MyAzureFileshare1* niet bestaat, wordt deze gemaakt in de Azure Storage-account. Mapstructuur wordt bijgehouden.

        ```
            BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
    
        ```
    - **Voor het importeren van een map**: Alle bestanden en mappen onder *MyFolder2* worden recursief gekopieerd naar de bestandsshare. Mapstructuur wordt bijgehouden.

        ```
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            
        ```
    Meerdere vermeldingen kunnen worden gemaakt in hetzelfde bestand overeenkomt met de mappen of bestanden die worden geïmporteerd. 

        ```
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
                        
        ```
    Meer informatie over [voorbereiden van de gegevensset CSV-bestand](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    

4. Wijzig de *driveset.csv* bestand in de hoofdmap waarin het hulpprogramma zich bevindt. Toevoegen van items in de *driveset.csv* bestand die vergelijkbaar is met de volgende voorbeelden. Het bestand driveset heeft de lijst van schijven en de bijbehorende stationsletters zodat het hulpprogramma kan de lijst met schijven worden voorbereid correct kiezen.

    In dit voorbeeld wordt ervan uitgegaan dat twee schijven zijn gekoppeld en eenvoudige NTFS-volumes G:\ en H:\ worden gemaakt. H:\is niet versleuteld wanneer G: al is versleuteld. Het hulpprogramma maakt en versleutelt de schijf die als host H:\ alleen fungeert (en niet G:\).

    - **Voor een schijf die is niet versleuteld**: Geef *versleutelen* inschakelen van BitLocker-versleuteling op de schijf.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        H,Format,SilentMode,Encrypt,
        ```
    
    - **Voor een schijf die al is versleuteld**: Geef *AlreadyEncrypted* en de BitLocker-sleutel hebt opgegeven.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
        ```

    Meerdere vermeldingen kunnen worden gemaakt in hetzelfde bestand overeenkomt met meerdere stations. Meer informatie over [voorbereiden van het CSV-bestand driveset](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file). 

5.  Gebruik de `PrepImport` optie om te kopiëren en voorbereiden van gegevens naar de schijf. Voor de eerste kopie-sessie om te kopiëren van mappen en/of bestanden met een nieuwe kopieersessie, kunt u de volgende opdracht uitvoeren:

        ```
        .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
        ```

    Hieronder ziet u een voorbeeld van invoer.
  
        ```
        .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
        ```
 
6. Een logboekbestand met de naam die u hebt opgegeven met `/j:` parameter is gemaakt voor elke uitvoering van de opdrachtregel. Elke schijf die u voorbereidt heeft een logboekbestand dat moet worden geüpload bij het maken van de import-taak. Stations zonder logboek bestanden niet worden verwerkt.

    > [!IMPORTANT]
    > - De gegevens op de harde schijven of het logboekbestand mag niet worden gewijzigd na het voltooien van de schijfvoorbereiding van de.

Voor meer voorbeelden, gaat u naar [-voorbeelden voor logboekbestanden](#samples-for-journal-files).

## <a name="step-2-create-an-import-job"></a>Stap 2: Een importtaak maken 

De volgende stappen uitvoeren om een import-taak maken in Azure portal.
1. Meld u aan bij https://portal.azure.com/.
2. Ga naar **alle services > opslag > Import/export-taken**. 

    ![Ga naar Import/export](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Klik op **Import-/ exporttaak maken**.

    ![Klik op de taak Import/export](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. In **basisbeginselen**:

    - Selecteer **importeren in Azure**.
    - Voer een beschrijvende naam voor de import-taak. Deze naam gebruiken voor het bijhouden van uw taken terwijl ze worden uitgevoerd en wanneer ze zijn voltooid.
        -  Deze naam mag alleen kleine letters, cijfers, afbreekstreepjes en onderstrepingstekens bevatten.
        -  De naam moet beginnen met een letter en mag geen spaties bevatten. 
    - Selecteer een abonnement.
    - Selecteer een resourcegroep. 

        ![Importeren van een taak maken - stap 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. In **taakgegevens**:
    
    - De logboekbestanden die u hebt gemaakt tijdens de voorgaande uploaden [stap 1: Voorbereiden van de schijven](#step-1-prepare-the-drives). 
    - Selecteer het opslagaccount waarin de gegevens worden geïmporteerd. 
    - De locatie dropoff wordt automatisch ingevuld op basis van de regio van het opslagaccount dat is geselecteerd.
   
       ![Importeren van een taak maken - stap 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. In **verzendinfo retourneren**:

    - Selecteer de provider die in de vervolgkeuzelijst.
    - Voer een geldige provider account getal dat u hebt gemaakt met deze provider. Microsoft gebruikt deze account voor de schijven terug naar u verzenden zodra uw import-taak voltooid is. 
    - Geef een volledig en geldig contact op met de naam, telefoon, e-mailadres, adres, stad, postcode, staat/provincie en land/regio.

        > [!TIP] 
        > In plaats van op te geven in een e-mailadres voor één gebruiker, moet u een groep e-mailadres opgeven. Dit zorgt ervoor dat u meldingen ontvangt, zelfs als een beheerder verlaat.

       ![Importeren van een taak maken - stap 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)

   
5. In de **samenvatting**:

    - Geef het Azure-datacenter verzendadres voor het verzenden van de schijven terug naar Azure. Zorg ervoor dat de taaknaam van de en het volledige adres worden vermeld op het verzendlabel.
    - Klik op **OK** import-taak maken voltooid.

        ![Importeren van een taak maken - stap 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>Stap 3: Verzending van de schijven in de Azure-datacenter 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>Stap 4: De taak bijwerken met het bijhouden van informatie

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-5-verify-data-upload-to-azure"></a>Stap 5: De gegevensupload naar Azure controleren

De taak volgen om te voltooien. Nadat de taak voltooid is, moet u controleren of uw gegevens daadwerkelijk is geüpload naar Azure. Verwijder de on-premises gegevens alleen nadat u hebt gecontroleerd dat het uploaden is voltooid.

## <a name="samples-for-journal-files"></a>Voorbeelden voor logboekbestanden

Naar **meer stations toevoegen**, maak een nieuw driveset-bestand en voer de opdracht uit als hieronder. 

Voor volgende kopie-sessies in de andere schijven dan is opgegeven in *InitialDriveset CSV* bestand, geeft u een nieuwe driveset *CSV* -bestand en geef deze als waarde aan de parameter `AdditionalDriveSet`. Gebruik de **hetzelfde logboekbestand** een naam en geef een **nieuwe sessie-ID**. De indeling van AdditionalDriveset CSV-bestand is hetzelfde als InitialDriveSet-indeling.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
    ```

Hieronder ziet u een voorbeeld van invoer.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
    ```


Aanvullende gegevens toevoegen aan de dezelfde driveset, door de opdracht PrepImport voor latere kopie-sessies te gebruiken om te kopiëren van de aanvullende bestanden en de map.

Voor volgende kopie-sessies in de dezelfde vaste-schijfstations die is opgegeven in *InitialDriveset.csv* bestand, geeft de **hetzelfde logboekbestand** een naam en geef een **nieuwe sessie-ID**; Er is geen hoeft op te geven van de opslagaccountsleutel.

    ```
    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
    ```

Hieronder ziet u een voorbeeld van invoer.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
    ```

## <a name="next-steps"></a>Volgende stappen

* [De taak en het station status bekijken](storage-import-export-view-drive-status.md)
* [Bekijk de vereisten voor Import/Export](storage-import-export-requirements.md)


