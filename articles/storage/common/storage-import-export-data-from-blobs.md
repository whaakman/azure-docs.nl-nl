---
title: Azure Import/Export gebruiken voor het exporteren van gegevens uit Azure Blobs | Microsoft Docs
description: Leer hoe u naar de export-taken maken in Azure portal voor gegevensoverdracht van Azure-Blobs.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 12/11/2018
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 0480a8d4079a39c8e365dde893a9a1af2d7341aa
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55453107"
---
# <a name="use-the-azure-importexport-service-to-export-data-from-azure-blob-storage"></a>De Azure Import/Export-service gebruiken voor het exporteren van gegevens uit Azure Blob storage
In dit artikel bevat stapsgewijze instructies over het gebruik van de Azure Import/Export-service veilig grote hoeveelheden gegevens exporteren uit Azure Blob-opslag. De service vereist dat u voor het verzenden van lege stations in de Azure-datacenter. De service exporteert gegevens van uw opslagaccount naar de stations en wordt vervolgens geleverd de schijven terug.

## <a name="prerequisites"></a>Vereisten

Voordat u een exporttaak bekijken om over te dragen gegevens uit Azure Blob-opslag maakt, zorgvuldig controleren en voer de volgende lijst met vereisten voor deze service. U moet:

- Een actief Azure-abonnement dat kan worden gebruikt voor de Import/Export-service hebben.
- Ten minste één Azure Storage-account hebben. Overzicht van de [ondersteunde opslagaccounts en typen gegevensopslag voor Import/Export-service](storage-import-export-requirements.md). Zie voor meer informatie over het maken van een nieuw opslagaccount [over het maken van een Storage-Account](storage-quickstart-create-account.md).
- Schijven van voldoende [ondersteunde typen](storage-import-export-requirements.md#supported-disks).
- Een FedEx/DHL-account hebben.  
    - Het account moet geldig zijn, saldo moet hebben en moet return verzending mogelijkheden hebben.
    - Een nummer voor de taak uitvoer gegenereerd.
    - Elke taak moet een afzonderlijke traceringsnummer hebben. Meerdere taken met de dezelfde traceringsnummer worden niet ondersteund. 
    - Als u een provider-account hebt, gaat u naar:
        - [Maak een account FedEX](https://www.fedex.com/en-us/create-account.html), of 
        - [Maak een account DHL](http://www.dhl-usa.com/en/express/shipping/open_account.html).

## <a name="step-1-create-an-export-job"></a>Stap 1: Een exporttaak maken

Voer de volgende stappen uit voor het maken van een exporttaak bekijken in Azure portal.

1. Meld u aan bij https://portal.azure.com/.
2. Ga naar **alle services > opslag > Import/export-taken**. 

    ![Ga naar Import/export-taken](./media/storage-import-export-data-from-blobs/export-from-blob1.png)

3. Klik op **Import-/ exporttaak maken**.

    ![Klik op de taak Import/export](./media/storage-import-export-data-from-blobs/export-from-blob2.png)

4. In **basisbeginselen**:
    
    - Selecteer **exporteren vanuit Azure**. 
    - Voer een beschrijvende naam voor de taak voor het exporteren. Gebruik de naam die u kiest om de voortgang van uw taken te houden. 
        - De naam mag alleen kleine letters, cijfers, afbreekstreepjes en onderstrepingstekens bevatten.
        - De naam moet beginnen met een letter en mag geen spaties bevatten. 
    - Selecteer een abonnement.
    - Typ of Selecteer een resourcegroep.

        ![Basisbeginselen](./media/storage-import-export-data-from-blobs/export-from-blob3.png) 
    
3. In **taakgegevens**:

    - Selecteer het opslagaccount waarin de gegevens worden geëxporteerd zich bevindt. Gebruik een opslagaccount dicht bij waar u zich bevinden.
    - De locatie dropoff wordt automatisch ingevuld op basis van de regio van het opslagaccount dat is geselecteerd. 
    - Geef de blob-gegevens die u wilt exporteren uit uw opslagaccount naar de lege schijf of schijven. 
    - Kies aan **exporteren alle** blob-gegevens in de storage-account.
    
         ![Alles exporteren](./media/storage-import-export-data-from-blobs/export-from-blob4.png) 

    - U kunt opgeven welke containers en blobs te exporteren.
        - **Om op te geven van een blob naar de export**: Gebruik de **gelijk aan** selector. Geef het relatieve pad naar de blob, beginnend met de containernaam van de. Gebruik *$root* om op te geven van het root-container.
        - **Om op te geven van alle blobs die beginnen met een voorvoegsel**: Gebruik de **begint met** selector. Geef het voorvoegsel, beginnen met een slash (/). Het voorvoegsel is mogelijk het voorvoegsel van de containernaam van de, de containernaam van de volledige of de containernaam van de volledige gevolgd door het voorvoegsel van de blob-naam. Zoals in deze schermafbeelding, moet u de blob-paden in de geldige indeling om fouten te voorkomen tijdens de verwerking, opgeven. Zie voor meer informatie, [voorbeelden van geldige blob paden](#examples-of-valid-blob-paths). 
   
           ![Exporteren van de geselecteerde containers en blobs](./media/storage-import-export-data-from-blobs/export-from-blob5.png) 

    - U kunt exporteren uit het bestand met de blob.

        ![Exporteren vanuit een bestand met de blob](./media/storage-import-export-data-from-blobs/export-from-blob6.png)  
   
   > [!NOTE]
   > Als de blob op die moet worden geëxporteerd gebruikt tijdens het kopiëren van gegevens wordt, worden de Azure Import/Export-service een momentopname van de blob en kopieert de momentopname.
 

4. In **verzendinfo retourneren**:

    - Selecteer de provider die in de vervolgkeuzelijst.
    - Voer een geldige provider account getal dat u hebt gemaakt met deze provider. Microsoft gebruikt deze account voor de schijven terug naar u verzenden zodra uw import-taak voltooid is. 
    - Geef een volledig en geldig contact op met de naam, telefoon, e-mailadres, adres, stad, postcode, staat/provincie en land/regio.

        > [!TIP] 
        > In plaats van op te geven in een e-mailadres voor één gebruiker, moet u een groep e-mailadres opgeven. Dit zorgt ervoor dat u meldingen ontvangt, zelfs als een beheerder verlaat.
   
5. In **samenvatting**:

    - Lees de informatie van de taak.
    - Noteer de naam van een taak en de opgegeven Azure-datacenter verzendadres voor het verzenden van schijven naar Azure. 

        > [!NOTE] 
        > De schijven altijd verzenden naar het datacenter die u hebt genoteerd in de Azure-portal. Als de schijven worden verzonden naar de verkeerde datacenter, wordt de taak niet verwerken.

    - Klik op **OK** naar de export-taak maken.

## <a name="step-2-ship-the-drives"></a>Stap 2: De schijven verzenden

Als u niet weet dat het aantal stations dat u nodig hebt, gaat u naar de [Controleer het aantal stations](#check-the-number-of-drives). Als u bekend bent met het aantal stations, gaat u verder met de schijven verzenden.

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-3-update-the-job-with-tracking-information"></a>Stap 3: De taak bijwerken met het bijhouden van informatie

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]


## <a name="step-4-receive-the-disks"></a>Stap 4: De schijven hebben ontvangen
Wanneer het dashboard dat de taak is voltooid meldt, de schijven naar u worden verzonden en het volgnummer van de verzending is beschikbaar op de portal.

1. Nadat u de stations met de geëxporteerde gegevens ontvangt, moet u de BitLocker-sleutels voor het ontgrendelen van de stations ophalen. Ga naar de exporttaak in Azure portal. Klik op **Import/Export** tabblad. 
2. Selecteer en klik op de taak voor het exporteren uit de lijst. Ga naar **BitLocker-sleutels** en kopieert u de sleutels.
   
   ![BitLocker-sleutels voor een exporttaak bekijken](./media/storage-import-export-service/export-job-bitlocker-keys.png)

3. De BitLocker-sleutels gebruiken voor het ontgrendelen van de schijven.

De export is voltooid. Op dit moment kunt u de taak verwijderen of deze automatisch na 90 dagen wordt verwijderd.


## <a name="check-the-number-of-drives"></a>Het aantal stations controleren

Dit *optionele* stap helpt u het aantal stations die vereist zijn voor de taak voor het exporteren bepaalt. Deze stap uitvoeren op een Windows-systeem met een [ondersteunde besturingssysteemversie](storage-import-export-requirements.md#supported-operating-systems).

1. [Download de WAImportExport versie 1](https://aka.ms/waiev1) op het Windows-systeem. 
2. Pak deze uit naar de standaardmap `waimportexportv1`. Bijvoorbeeld `C:\WaImportExportV1`.
3. Open een PowerShell- of vanaf de opdrachtregel-venster met beheerdersbevoegdheden. Als u de map naar de uitgepakte map, moet u de volgende opdracht uitvoeren:
    
    `cd C:\WaImportExportV1`

4. Om te controleren of het aantal schijven voor de geselecteerde blobs is vereist, moet u de volgende opdracht uitvoeren:

    `WAImportExport.exe PreviewExport /sn:<Storage account name> /sk:<Storage account key> /ExportBlobListFile:<Path to XML blob list file> /DriveSize:<Size of drives used>`

    De parameters worden in de volgende tabel beschreven:
    
    |Opdrachtregelparameter|Description|  
    |--------------------------|-----------------|  
    |**/logdir:**|Optioneel. De logboekmap. Uitgebreid logboek van de bestanden worden naar deze map geschreven. Als niet is opgegeven, wordt de huidige map gebruikt als de logboekmap.|  
    |**/sn:**|Vereist. De naam van het opslagaccount voor de taak voor het exporteren.|  
    |**/sk:**|Alleen vereist als een container SAS niet is opgegeven. De accountsleutel voor het opslagaccount voor de taak voor het exporteren.|  
    |**/csas:**|Alleen vereist als een sleutel van het opslagaccount niet is opgegeven. De container SAS voor het weergeven van de blobs in de taak voor het exporteren worden geëxporteerd.|  
    |**/ ExportBlobListFile:**|Vereist. Pad naar het XML-bestand opslaan met lijst met blob-paden of blob-voorvoegsels voor pad voor de blobs worden geëxporteerd. De bestandsindeling die wordt gebruikt de `BlobListBlobPath` -element in de [plaatsen taak](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) werking van de Import/Export-service REST-API.|  
    |**/ DriveSize:**|Vereist. De grootte van schijven te gebruiken voor een exporttaak *bijvoorbeeld*, 500 GB, 1,5 TB.|  

    Zie een [voorbeeld van de opdracht PreviewExport](#example-of-previewexport-command).
 
5. Controleer of u kunt lezen/schrijven naar de stations die worden verzonden voor de taak voor het exporteren.

### <a name="example-of-previewexport-command"></a>Voorbeeld van PreviewExport opdracht

Het volgende voorbeeld ziet u de `PreviewExport` opdracht:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Het exportbestand voor de blob-lijst kan bevatten blob-namen en blob-voorvoegsels, zoals hier wordt weergegeven:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

De Azure Import/Export-hulpprogramma geeft een lijst van alle blobs worden geëxporteerd en berekent het pack ze in de stations van de opgegeven grootte, waarbij rekening wordt gehouden met eventuele vereiste overhead en maakt een schatting van het aantal stations die nodig zijn voor het opslaan van blobs en informatie over het gebruik van de schijf.  
  
Hier volgt een voorbeeld van de uitvoer, met informatieve logboeken weggelaten:  
  
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

De volgende tabel ziet u voorbeelden van geldige blob paden:
   
   | Selector | Blobpad | Description |
   | --- | --- | --- |
   | Begint met |/ |Hiermee exporteert u alle blobs in de storage-account |
   | Begint met |/$root / |Hiermee exporteert u alle blobs in de hoofdmap-container |
   | Begint met |/Book |Hiermee exporteert u alle blobs in een container die met het voorvoegsel begint **boek** |
   | Begint met |/Music/ |Hiermee exporteert u alle blobs in de container **muziek** |
   | Begint met |/ muziek/love |Hiermee exporteert u alle blobs in de container **muziek** die beginnen met het voorvoegsel **uw favoriete** |
   | Equal To |$root/logo.bmp |Uitvoer-blob **logo.bmp** in de hoofdmap-container |
   | Equal To |videos/story.mp4 |Uitvoer-blob **story.mp4** in container **video's** |

## <a name="next-steps"></a>Volgende stappen

* [De taak en het station status bekijken](storage-import-export-view-drive-status.md)
* [Bekijk de vereisten voor Import/Export](storage-import-export-requirements.md)


