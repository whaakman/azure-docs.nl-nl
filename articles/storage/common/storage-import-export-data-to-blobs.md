---
title: Gegevens overbrengen naar Azure-Blobs met Azure Import/Export | Microsoft Docs
description: Informatie over het maken van importeren en exporteren van taken in Azure portal om over te dragen gegevens van en naar Azure-Blobs.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 10/15/2018
ms.author: alkohli
ms.component: common
ms.openlocfilehash: 3a647d9f821007dc0f2b48864b937aee84924d8f
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353168"
---
# <a name="use-the-azure-importexport-service-to-import-data-to-azure-blob-storage"></a>De Azure Import/Export-service gebruiken om gegevens te importeren naar Azure Blob Storage

In dit artikel bevat stapsgewijze instructies over het gebruik van de Azure Import/Export-service veilig grote hoeveelheden gegevens te importeren in Azure Blob-opslag. Om gegevens te importeren in Azure Blobs, moet de service u voor het verzenden van versleutelde harde schijven met uw gegevens naar een Azure-datacenter.  

## <a name="prerequisites"></a>Vereisten

Voordat u een import-taak voor het overdragen van gegevens naar Azure Blob-opslag maakt, zorgvuldig controleren en voer de volgende lijst met vereisten voor deze service. U moet:

- Een actief Azure-abonnement dat kan worden gebruikt voor de Import/Export-service hebben.
- Ten minste één Azure Storage-account met een opslagcontainer hebben. Overzicht van de [ondersteunde opslagaccounts en typen gegevensopslag voor Import/Export-service](storage-import-export-requirements.md). 
    - Zie voor meer informatie over het maken van een nieuw opslagaccount [over het maken van een Storage-Account](storage-quickstart-create-account.md). 
    - Voor informatie over de storage-container, gaat u naar [maken van een opslagcontainer](../blobs/storage-quickstart-blobs-portal.md#create-a-container).
- Schijven van voldoende [ondersteunde typen](storage-import-export-requirements.md#supported-disks). 
- Een Windows-systeem met een [ondersteunde besturingssysteemversie](storage-import-export-requirements.md#supported-operating-systems). 
- Het inschakelen van BitLocker op het Windows-systeem. Zie [BitLocker inschakelen](http://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
- [Download de WAImportExport versie 1](https://www.microsoft.com/en-us/download/details.aspx?id=42659) op het Windows-systeem. Pak deze uit naar de standaardmap `waimportexportv1`. Bijvoorbeeld `C:\WaImportExportV1`.
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

1.  De schijfstations verbinden met het Windows-systeem via SATA-connectors.
1.  Maak een enkele NTFS-volume op elke schijf. Een stationsletter toewijzen aan het volume. Stel de volgende parameter niet gebruiken
2.  Inschakelen van BitLocker-versleuteling op het NTFS-volume. Als een Windows Server-systeem, gebruikt u de instructies in [het inschakelen van BitLocker op Windows Server 2012 R2](http://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
3.  Gegevens kopiëren naar een versleuteld volume. Gebruik slepen en neerzetten of Robocopy of een dergelijke hulpprogramma voor kopiëren.
4.  Open een PowerShell- of vanaf de opdrachtregel-venster met beheerdersbevoegdheden. Als u de map naar de uitgepakte map, moet u de volgende opdracht uitvoeren:
    
    `cd C:\WaImportExportV1`
5.  Als u de BitLocker-sleutel van het station, moet u de volgende opdracht uitvoeren:
    
    ` manage-bde -protectors -get <DriveLetter>: `
6.  Voer de volgende opdracht om voor te bereiden op de schijf. **Afhankelijk van de gegevensgrootte, kan dit enkele uren duren tot dagen.** 

    ```
    ./WAImportExport.exe PrepImport /j:<journal file name> /id:session#<session number> /sk:<Storage account key> /t:<Drive letter> /bk:<BitLocker key> /srcdir:<Drive letter>:\ /dstdir:<Container name>/ /skipwrite 
    ```
    Er wordt een logboekbestand gemaakt in dezelfde map waar u het hulpprogramma hebt uitgevoerd. Worden ook twee andere bestanden gemaakt: een *.xml* bestand (map waar u het hulpprogramma uitvoeren) en een *station manifest.xml* bestand (map waar de gegevens zich bevinden).
    
    De parameters die worden gebruikt worden in de volgende tabel beschreven:

    |Optie  |Beschrijving  |
    |---------|---------|
    |/j:     |De naam van het logboekbestand met de extensie jrn. Er wordt een logboekbestand gegenereerd per station. U wordt aangeraden dat u het serienummer van de schijf als de naam van het logboek gebruiken.         |
    |/ID:     |De sessie-ID. Gebruik een unieke sessie voor elk exemplaar van de opdracht.      |
    |/SK:     |De sleutel van de Azure Storage-account.         |
    |/ t:     |De stationsletter van de schijf om te worden verzonden. Bijvoorbeeld station `D`.         |
    |/BK:     |De BitLocker-sleutel voor het station. Het wachtwoord van de numerieke uit de uitvoer van ` manage-bde -protectors -get D: `      |
    |/srcdir:     |De stationsletter van de schijf om te worden verzonden, gevolgd door `:\`. Bijvoorbeeld `D:\`.         |
    |/dstdir:     |De naam van de doelcontainer in Azure Storage.         |
    |/skipwrite:     |De optie die aangeeft dat er geen nieuwe gegevens die nodig zijn om te worden gekopieerd en de bestaande gegevens op de schijf moet worden voorbereid.          |
7. Herhaal de vorige stap voor elke schijf die moet worden verzonden. Een logboekbestand met de opgegeven naam is gemaakt voor elke uitvoering van de opdrachtregel.
    
    > [!IMPORTANT]
    > - Samen met het logboekbestand een `<Journal file name>_DriveInfo_<Drive serial ID>.xml` bestand wordt ook gemaakt in dezelfde map waarin het hulpprogramma zich bevindt. Het .xml-bestand wordt gebruikt in plaats van een logboekbestand bij het maken van een taak als het logboekbestand te groot is. 

## <a name="step-2-create-an-import-job"></a>Stap 2: Een importtaak maken

De volgende stappen uitvoeren om een import-taak maken in Azure portal.

1. Meld u aan bij https://portal.azure.com/.
2. Ga naar **alle services > opslag > Import/export-taken**. 
    
    ![Ga naar Import/export-taken](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Klik op **Import-/ exporttaak maken**.

    ![Klik op de Import-/ exporttaak maken](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. In **basisbeginselen**:

    - Selecteer **importeren in Azure**.
    - Voer een beschrijvende naam voor de import-taak. De naam gebruiken om de voortgang van uw taken te houden.
        - De naam mag alleen kleine letters, cijfers en afbreekstreepjes bevatten.
        - De naam moet beginnen met een letter en mag geen spaties bevatten.
    - Selecteer een abonnement.
    - Typ of Selecteer een resourcegroep.  

    ![Importeren van een taak maken - stap 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. In **taakgegevens**:

    - Upload de logboekbestanden van de schijf die u hebt verkregen tijdens de voorbereiding van het station. Als `waimportexport.exe version1` is gebruikt, upload een bestand voor elke schijf die u hebt voorbereid. Als de grootte van het logboek is groter dan 2 MB, dan kunt u de `<Journal file name>_DriveInfo_<Drive serial ID>.xml` ook die zijn gemaakt met het logboekbestand. 
    - Selecteer het doelopslagaccount waar de gegevens zich bevinden. 
    - De locatie dropoff wordt automatisch ingevuld op basis van de regio van het opslagaccount dat is geselecteerd.
   
   ![Importeren van een taak maken - stap 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. In **verzendinfo retourneren**:

    - Selecteer de provider die in de vervolgkeuzelijst.
    - Voer een geldige provider account getal dat u hebt gemaakt met deze provider. Microsoft gebruikt deze account voor de schijven terug naar u verzenden zodra uw import-taak voltooid is. Als u een accountnummer hebt, maakt u een [FedEx](http://www.fedex.com/us/oadr/) of [DHL](http://www.dhl.com/) doorberekend aan.
    - Geef een volledig en geldig contact op met de naam, telefoon, e-mailadres, adres, stad, postcode, staat/provincie en land/regio. 
        
        > [!TIP] 
        > In plaats van op te geven in een e-mailadres voor één gebruiker, moet u een groep e-mailadres opgeven. Dit zorgt ervoor dat u meldingen ontvangt, zelfs als een beheerder verlaat.

    ![Importeren van een taak maken - stap 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)
   
5. In de **samenvatting**:

    - Bekijk de taakinformatie in het overzicht. Noteer de taaknaam van de en de Azure-datacenter verzendadres voor het verzenden van de schijven terug naar Azure. Deze informatie wordt later gebruikt op het verzendlabel.
    - Klik op **OK** te maken van de import-taak.

    ![Importeren van een taak maken - stap 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives"></a>Stap 3: De schijven verzenden 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]


## <a name="step-4-update-the-job-with-tracking-information"></a>Stap 4: De taak bijwerken met het bijhouden van informatie

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-5-verify-data-upload-to-azure"></a>Stap 5: Controleer of het uploaden van gegevens naar Azure

De taak volgen om te voltooien. Nadat de taak voltooid is, moet u controleren of uw gegevens daadwerkelijk is geüpload naar Azure. Verwijder de on-premises gegevens alleen nadat u hebt gecontroleerd dat het uploaden is voltooid.

## <a name="next-steps"></a>Volgende stappen

* [De taak en het station status bekijken](storage-import-export-view-drive-status.md)
* [Bekijk de vereisten voor Import/Export](storage-import-export-requirements.md)


