---
title: Met behulp van Azure Import/Export gegevens te exporteren uit Azure Blobs | Microsoft Docs
description: Informatie over het exporteren maken in Azure portal voor gegevensoverdracht van Azure Blobs.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: eb41708c7446b3139758678c9247ffbb11da8b40
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660803"
---
# <a name="use-the-azure-importexport-service-to-export-data-from-azure-blob-storage"></a>De Azure Import/Export-service gebruiken om gegevens te exporteren uit Azure Blob-opslag
In dit artikel bevat stapsgewijze instructies over het gebruik van de service Azure Import/Export veilig exporteren van grote hoeveelheden gegevens uit Azure Blob-opslag. De service moet u lege schijven naar het Azure datacenter verzenden. De service worden gegevens uit uw storage-account geëxporteerd naar de stations en vervolgens de back-stations wordt geleverd.

## <a name="prerequisites"></a>Vereisten

Zorgvuldig controleren voordat u een exporttaak overbrengen van gegevens uit Azure Blob-opslag maakt, en voer de volgende lijst met vereisten voor deze service. U moet doen:

- Een actief Azure-abonnement die kan worden gebruikt voor de Import/Export-service hebben.
- Ten minste één Azure Storage-account hebben. Overzicht van de [storage-accounts en opslagtypen ondersteund voor Import/Export-service](storage-import-export-requirements.md). Zie voor meer informatie over het maken van een nieuw opslagaccount [het maken van een Opslagaccount](storage-create-storage-account.md#create-a-storage-account).
- Zorg voor voldoende schijven van [ondersteunde typen](storage-import-export-requirements.md#supported-disks).

## <a name="step-1-create-an-export-job"></a>Stap 1: Een exporttaak maken

Voer de volgende stappen uit voor het maken van een exporttaak in de Azure portal.

1. Meld u aan bij https://portal.azure.com/.
2. Ga naar **alle services > opslag > importeren/exporteren taken**. 

    ![Ga naar de taken voor importeren/exporteren](./media/storage-import-export-data-from-blobs/export-from-blob1.png)

3. Klik op **maken voor importeren/exporteren taak**.

    ![Klik op taak van Import/export](./media/storage-import-export-data-from-blobs/export-from-blob2.png)

4. In **basisbeginselen**:
    
    - Selecteer **exporteren uit Azure**. 
    - Voer een beschrijvende naam voor de taak voor het exporteren. Gebruik de naam die u kiest om de voortgang van uw taken te volgen. 
        - De naam mag alleen kleine letters, cijfers, afbreekstreepjes en onderstrepingstekens bevatten.
        - De naam moet beginnen met een letter en mag geen spaties bevatten. 
    - Selecteer een abonnement.
    - Typ of Selecteer een resourcegroep.

        ![Basisbeginselen](./media/storage-import-export-data-from-blobs/export-from-blob3.png) 
    
3. In **taakgegevens**:

    - Selecteer het opslagaccount waarin de gegevens worden geëxporteerd zich bevindt. 
    - De afgiftepunt wordt automatisch gevuld op basis van de regio van het geselecteerde opslagaccount. 
    - Geef de blobgegevens die u wilt exporteren van uw opslagaccount naar uw lege schijf of schijven. 
    - Kies voor **Exporteer alle** blob-gegevens in het opslagaccount.
    
         ![Exporteer alle](./media/storage-import-export-data-from-blobs/export-from-blob4.png) 

    - U kunt opgeven welke containers en blobs te exporteren.
        - **Om op te geven van een blob exporteren**: Gebruik de **gelijk aan** selector. Geef het relatieve pad naar de blob, beginnend met de containernaam van de. Gebruik *$root* om op te geven van de basiscontainer.
        - **Om op te geven van alle blobs die beginnen met een voorvoegsel**: Gebruik de **begint met** selector. Geef het voorvoegsel, beginnen met een slash '/'. Het voorvoegsel mogelijk het voorvoegsel van de containernaam van de, de containernaam van de volledige of de containernaam van de volledige gevolgd door het voorvoegsel van de blob-naam. Zoals weergegeven in deze schermafbeelding, moet u de paden blob geldige indeling om fouten te voorkomen tijdens de verwerking opgeven. Zie voor meer informatie [voorbeelden van geldige blob-paden](#examples-of-valid-blob-paths). 
   
           ![Geselecteerde containers en blobs exporteren](./media/storage-import-export-data-from-blobs/export-from-blob5.png) 

    - U kunt exporteren uit het bestand met de blob.

        ![Exporteren van het bestand met blob](./media/storage-import-export-data-from-blobs/export-from-blob6.png)  
   
   > [!NOTE]
   > Als de blob wordt geëxporteerd gebruikt tijdens het kopiëren van gegevens wordt, wordt Azure Import/Export-service maakt een momentopname van de blob en kopieert de momentopname.
 

4. In **back-ups van gegevens retourneren**:

    - Selecteer de provider uit de vervolgkeuzelijst.
    - Voer een geldige carrier account getal dat u hebt gemaakt met deze provider. Microsoft gebruikt deze account voor de schijven naar u verzenden zodra de import-taak is voltooid. 
    - Geef een volledig en geldig contactpersoon, telefoon, e, adres, stad, zip, staat/provincie en land/regio.
   
5. In **samenvatting**:

    - Bekijk de details van de taak.
    - Noteer het adres taak naam en het opgegeven Azure-datacenter back-upfunctie voor logboekverzending schijven naar Azure. 
    - Klik op **OK** export taak maken voltooid.

## <a name="step-2-ship-the-drives"></a>Stap 2: De schijven verzenden

Als u het aantal stations u moet niet weet, gaat u naar de [het aantal stations controleren](#check-the-number-of-drives). Als u het aantal stations, gaat u verder met de schijven verzenden.

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-3-update-the-job-with-tracking-information"></a>Stap 3: De taak met traceringsgegevens bijwerken

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]


## <a name="step-4-receive-the-disks"></a>Stap 4: De schijven ontvangen
Wanneer het dashboard dat de taak is voltooid meldt, wordt de schijven naar u worden verzonden en wordt het volgnummer voor de verzending is beschikbaar op de portal.

1. Nadat u de stations met geëxporteerde gegevens ontvangt, moet u de BitLocker-sleutels voor het ontgrendelen van de stations. Ga naar de exporttaak in de Azure portal. Klik op **voor importeren/exporteren** tabblad. 
2. Selecteren en klik op het exporteren van taken in de lijst. Ga naar **BitLocker-sleutels** en kopieer de sleutels.
   
   ![BitLocker-sleutels voor de exporttaak weergeven](./media/storage-import-export-service/export-job-bitlocker-keys.png)

3. De BitLocker-sleutels gebruiken voor het ontgrendelen van de schijven.

Het exporteren is voltooid. Op dit moment kunt u de taak verwijderen of het automatisch na 90 dagen wordt verwijderd.


## <a name="check-the-number-of-drives"></a>Het aantal stations controleren

Dit *optionele* stap helpt u het aantal stations die vereist zijn voor de taak voor exporteren bepaalt. Deze stap uitvoeren op een Windows-systeem waarop een [ondersteund besturingssysteemversie](storage-import-export-requirements.md#supported-operating-systems).

1. [Download de WAImportExport versie 1](https://www.microsoft.com/en-us/download/details.aspx?id=42659) op het Windows-systeem. 
2. Uitpakken naar de standaardmap `waimportexportv1`. Bijvoorbeeld `C:\WaImportExportV1`.
3. Open een venster PowerShell of de opdrachtregel met beheerdersbevoegdheden. Als u de map naar de map uitgepakt, voer de volgende opdracht:
    
    `cd C:\WaImportExportV1`

4. Om te zien van het aantal schijven dat vereist is voor de geselecteerde blobs, voer de volgende opdracht:

    `WAImportExport.exe PreviewExport /sn:<Storage account name> /sk:<Storage account key> /ExportBlobListFile:<Path to XML blob list file> /DriveSize:<Size of drives used>`

    De parameters worden in de volgende tabel beschreven:
    
    |Opdrachtregelparameter|Beschrijving|  
    |--------------------------|-----------------|  
    |**schakeloptie/LOGDIR op:**|Optioneel. De logboekmap. Uitgebreide logboekbestanden worden geschreven naar deze map. Als niet wordt opgegeven, wordt de huidige map gebruikt als de logboekmap.|  
    |**/sn:**|Vereist. De naam van het opslagaccount voor de taak voor het exporteren.|  
    |**/SK:**|Alleen vereist als een container SAS is niet opgegeven. De accountsleutel voor het opslagaccount voor de taak voor exporteren.|  
    |**/csas:**|Alleen vereist als de sleutel van een opslagaccount is niet opgegeven. De container SAS voor het weergeven van de blobs in de taak voor het exporteren worden geëxporteerd.|  
    |**/ ExportBlobListFile:**|Vereist. Pad naar het XML-bestand met lijst met blob-paden bestand of blob-pad voorvoegsels voor de blobs worden geëxporteerd. De bestandsindeling die wordt gebruikt in de `BlobListBlobPath` -element in de [taak plaatsen](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) bewerking van de Import/Export-service REST-API.|  
    |**/ DriveSize:**|Vereist. De grootte van stations voor een exporttaak *bijvoorbeeld*, 500 GB, 1,5 TB.|  

    Zie een [voorbeeld van de opdracht PreviewExport](#example-of-previewexport-command).
 
5. Controleer of u kunt lezen/schrijven naar de stations die worden verzonden voor de taak voor exporteren.

### <a name="example-of-previewexport-command"></a>Voorbeeld van de opdracht PreviewExport

Het volgende voorbeeld toont de `PreviewExport` opdracht:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Het exportbestand voor de lijst van blob kan blobnamen bevatten en blob-voorvoegsels, zoals hier wordt weergegeven:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

De Azure-hulpprogramma voor importeren/exporteren geeft een lijst van alle blobs worden geëxporteerd en berekent hoe ze in de stations van de opgegeven grootte pack rekening wordt gehouden met eventuele benodigde overhead en maakt een schatting van het aantal stations die nodig zijn voor het opslaan van blobs en informatie over het gebruik van de schijf.  
  
Hier volgt een voorbeeld van uitvoer van de met informatief logboeken die worden weggelaten:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```

## <a name="examples-of-valid-blob-paths"></a>Voorbeelden van geldige blob-paden

De volgende tabel ziet u voorbeelden van geldige blob-paden:
   
   | Selector | Blobpad | Beschrijving |
   | --- | --- | --- |
   | Begint met |/ |Exporteert u alle blobs in de storage-account |
   | Begint met |/$root / |Alle blobs in de hoofdmap container exporteert |
   | Begint met |/Book |Alle blobs in elke container die met het voorvoegsel begint exporteert **adresboek** |
   | Begint met |/Music/ |Alle blobs in container exporteert **muziek** |
   | Begint met |hou muziek / |Alle blobs in container exporteert **muziek** die beginnen met het voorvoegsel **favoriete** |
   | Gelijk aan |$root/logo.bmp |Uitvoer-blob **logo.bmp** in de hoofdmap-container |
   | Gelijk aan |videos/Story.mp4 |Uitvoer-blob **story.mp4** in de container **video's** |

## <a name="next-steps"></a>Volgende stappen

* [De status van de taak en het station weergeven](storage-import-export-view-drive-status.md)
* [Overzicht van de vereisten voor importeren/exporteren](storage-import-export-requirements.md)


