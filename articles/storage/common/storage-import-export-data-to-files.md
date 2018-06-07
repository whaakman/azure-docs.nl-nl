---
title: Met behulp van Azure Import/Export mag gegevens overdragen naar Azure-bestanden | Microsoft Docs
description: Informatie over het importeren van taken in de Azure portal mag gegevens overdragen naar Azure-bestanden maken.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 4349b471f960e7844511c473bffcd2177a34e055
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660809"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>Azure Import/Export-service gebruiken voor het importeren van gegevens naar Azure-bestanden

In dit artikel bevat stapsgewijze instructies over het gebruik van de service Azure Import/Export veilig grote hoeveelheden gegevens importeren in Azure-bestanden. Om gegevens te importeren, moet de service u ondersteunde harde schijven met uw gegevens om een Azure-datacenter worden geleverd.  

De Import/Export-service ondersteunt alleen importeren van Azure-bestanden in de Azure-opslag. Azure Files exporteren wordt niet ondersteund.

## <a name="prerequisites"></a>Vereisten

Zorgvuldig controleren voordat u een import-taak, het overbrengen van gegevens naar Azure-bestanden maken, en voer de volgende lijst met vereisten. U moet doen:

- Een actief Azure-abonnement voor gebruik met Import/Export-service hebben.
- Ten minste één Azure Storage-account hebben. Overzicht van de [storage-accounts en opslagtypen ondersteund voor Import/Export-service](storage-import-export-requirements.md). Zie voor meer informatie over het maken van een nieuw opslagaccount [het maken van een Opslagaccount](storage-create-storage-account.md#create-a-storage-account).
- Zorg voor voldoende schijven van [ondersteunde typen](storage-import-export-requirements.md#supported-disks). 
- Hebt u een Windows-systeem waarop een [ondersteund besturingssysteemversie](storage-import-export-requirements.md#supported-operating-systems).
- [Download de WAImportExport versie 2](https://www.microsoft.com/download/details.aspx?id=55280) op het Windows-systeem. Uitpakken naar de standaardmap `waimportexport`. Bijvoorbeeld `C:\WaImportExport`.


## <a name="step-1-prepare-the-drives"></a>Stap 1: Bereid de stations

Deze stap genereert een journal-bestand. Het journaalbestand slaat basisinformatie zoals serienummer van station versleutelingssleutel en de details van opslag.

Voer de volgende stappen voor het voorbereiden van de stations.

1. Onze schijfstations verbinding met het Windows-systeem via SATA-connectors.
2. Maak één NTFS-volume op elk station. Een stationsletter toewijzen aan het volume. Stel de volgende parameter niet gebruikt.
3. Wijzig de *dataset.csv* bestand in de hoofdmap waarin het hulpprogramma zich bevindt. Afhankelijk van of u importeren van een bestand of map, of beide wilt, toevoegen vermeldingen in de *dataset.csv* bestand vergelijkbaar met de volgende voorbeelden.  

    - **Een bestand importeren**: In het volgende voorbeeld wordt de gegevens te kopiëren in het station C: zich bevindt. Uw bestand *MyFile1.txt* wordt gekopieerd naar de hoofdmap van de *MyAzureFileshare1*. Als de *MyAzureFileshare1* niet bestaat, deze wordt gemaakt in de Azure Storage-account. Mapstructuur wordt bijgehouden.

        ```
            BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
    
        ```
    - **Voor het importeren van een map**: alle bestanden en mappen onder *MyFolder2* worden recursief gekopieerd naar de bestandsshare. Mapstructuur wordt bijgehouden.

        ```
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            
        ```
    Meerdere vermeldingen kunnen worden gemaakt in hetzelfde bestand overeenkomt met de mappen of bestanden die worden geïmporteerd. 

        ```
            "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
            "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None 
            "F:\MyFolder3\MyFile3.txt","MyAzureFileshare2/",file,rename,"None",None 
            
        ```
    Meer informatie over [voorbereiden van het CSV-bestand van gegevensset](storage-import-export-tool-preparing-hard-drives-import.md#prepare-the-dataset-csv-file).
    

4. Wijzig de *driveset.csv* bestand in de hoofdmap waarin het hulpprogramma zich bevindt. Toevoegen van items in de *driveset.csv* bestand vergelijkbaar met de volgende voorbeelden. Het bestand driveset heeft de lijst van schijven en de bijbehorende stationsletters zodat het hulpprogramma kan correct kiezen voor de lijst met schijven worden voorbereid.

    In dit voorbeeld wordt ervan uitgegaan dat twee schijven zijn gekoppeld en standaard NTFS-volumes G:\ en H:\ worden gemaakt. H:\is niet versleuteld wanneer G: al is versleuteld. Het hulpprogramma indelingen en de schijf waarop H:\ alleen versleuteld (en niet G:\).

    - **Voor een schijf die is niet versleuteld**: Geef *versleutelen* BitLocker-versleuteling op de schijf in te schakelen.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        H,Format,SilentMode,Encrypt,
        ```
    
    - **Voor een schijf die al is versleuteld**: Geef *AlreadyEncrypted* en de BitLocker-sleutel op te geven.

        ```
        DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
        G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
        ```

    Meerdere vermeldingen kunnen worden gemaakt in hetzelfde bestand overeenkomt met meerdere stations. Meer informatie over [voorbereiden van het CSV-bestand driveset](storage-import-export-tool-preparing-hard-drives-import.md#prepare-initialdriveset-or-additionaldriveset-csv-file). 

5.  Gebruik de `PrepImport` optie voor het kopiëren en voorbereiden van gegevens naar de schijf. Voer de volgende opdracht voor de eerste sessie kopiëren kopiëren mappen en/of bestanden met een nieuwe kopieersessie:

        ```
        .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
        ```

    Een voorbeeld van invoer worden hieronder weergegeven.
  
        ```
        .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
        ```
 
6. Een journal-bestand met de naam u opgaf met `/j:` parameter is gemaakt voor elke run van de opdrachtregel. Elk station dat u bereid heeft een journal-bestand dat moet worden geüpload bij het maken van de import-taak. Stations zonder journaal bestanden niet zijn verwerkt.

    > [!IMPORTANT]
    > - De gegevens op de schijfstations of het journaalbestand mag niet worden gewijzigd na het voltooien van de schijfvoorbereiding van de.

Voor aanvullende voorbeelden, gaat u naar [voorbeelden voor het wijzigingslogboek bestanden](#samples-for-journal-files).

## <a name="step-2-create-an-import-job"></a>Stap 2: Een import-taak maken 

Voer de volgende stappen uit voor het maken van een import-taak in de Azure portal.
1. Meld u aan bij https://portal.azure.com/.
2. Ga naar **alle services > opslag > importeren/exporteren taken**. 

    ![Ga naar voor importeren/exporteren](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Klik op **maken voor importeren/exporteren taak**.

    ![Klik op taak van Import/export](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. In **basisbeginselen**:

    - Selecteer **importeren in Azure**.
    - Voer een beschrijvende naam voor de import-taak. Gebruik deze naam voor het bijhouden van uw taken terwijl ze zijn uitgevoerd en wanneer ze zijn voltooid.
        -  Deze naam mag alleen kleine letters, cijfers, afbreekstreepjes en onderstrepingstekens bevatten.
        -  De naam moet beginnen met een letter en mag geen spaties bevatten. 
    - Selecteer een abonnement.
    - Selecteer een resourcegroep. 

        ![Import-taak - stap 1 maken](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. In **taakgegevens**:
    
    - Het wijzigingslogboek bestanden uploaden die u hebt gemaakt tijdens de voorgaande [stap 1: voorbereiden van de stations](#step-1-prepare-the-drives). 
    - Selecteer het opslagaccount waarin de gegevens worden geïmporteerd. 
    - De afgiftepunt wordt automatisch gevuld op basis van de regio van het geselecteerde opslagaccount.
   
       ![Stap 2 voor import-taak - maken](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. In **back-ups van gegevens retourneren**:

    - Selecteer de provider in de vervolgkeuzelijst.
    - Voer een geldige carrier account getal dat u hebt gemaakt met deze provider. Microsoft gebruikt deze account voor de schijven naar u verzenden zodra de import-taak is voltooid. 
    - Geef een volledig en geldig contactpersoon, telefoon, e, adres, stad, zip, staat/provincie en land/regio.

       ![Stap 3 van importtaak - maken](./media/storage-import-export-data-to-blobs/import-to-blob5.png)

   
5. In de **samenvatting**:

    - Geef de Azure-datacenter verzendadres voor het verzenden van schijven terug naar Azure. Zorg ervoor dat de taaknaam en het volledige adres worden vermeld op het label van de back-upfunctie.
    - Klik op **OK** om uit te voeren van import-taak maken.

        ![Stap 4 van importtaak - maken](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>Stap 3: De schijven naar het Azure datacenter verzenden 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>Stap 4: De taak met traceringsgegevens bijwerken

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="samples-for-journal-files"></a>Voorbeelden voor het wijzigingslogboek bestanden

Naar **meer stations toevoegen**, maakt u een nieuw driveset-bestand en voer de opdracht zoals hieronder. 

Voor latere kopie-sessies in de verschillende schijfstations dan is opgegeven in *InitialDriveset CSV* bestand, geeft u een nieuwe driveset *CSV* bestands- en opgeven als een waarde in de parameter `AdditionalDriveSet`. Gebruik de **hetzelfde journaalbestand** een naam en geef een **nieuwe sessie-ID**. De indeling van AdditionalDriveset CSV-bestand is hetzelfde als InitialDriveSet-indeling.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
    ```

Een voorbeeld van invoer worden hieronder weergegeven.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
    ```


Om aanvullende gegevens toevoegen aan de dezelfde driveset, door de opdracht PrepImport voor latere kopie sessies te gebruiken voor het kopiëren van de aanvullende bestanden en de map.

Voor latere kopie-sessies door naar de dezelfde harde schijven die is opgegeven in *InitialDriveset.csv* bestand, geeft de **hetzelfde journaalbestand** een naam en geef een **nieuwe sessie-ID**; Er is niet nodig om de sleutel van het opslagaccount.

    ```
    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
    ```

Een voorbeeld van invoer worden hieronder weergegeven.

    ```
    WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
    ```

## <a name="next-steps"></a>Volgende stappen

* [De status van de taak en het station weergeven](storage-import-export-view-drive-status.md)
* [Overzicht van de vereisten voor importeren/exporteren](storage-import-export-requirements.md)


