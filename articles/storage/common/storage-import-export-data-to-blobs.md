---
title: Met behulp van Azure Import/Export mag gegevens overdragen naar Azure Blobs | Microsoft Docs
description: Informatie over het maken van importeren en exporteren van taken in Azure portal voor het overbrengen van gegevens van en naar Azure Blobs.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: fe9292459134972b44037a58235cdd817030a956
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660804"
---
# <a name="use-the-azure-importexport-service-to-import-data-to-azure-blob-storage"></a>De Azure Import/Export-service gebruiken voor het importeren van gegevens naar Azure Blob-opslag

In dit artikel bevat stapsgewijze instructies over het gebruik van de Azure Import/Export-service voor het veilig grote hoeveelheden gegevens te importeren naar Azure Blob-opslag. De service vereist om gegevens te importeren in Azure Blobs, voor het verzenden van versleutelde harde schijven met uw gegevens om een Azure-datacenter.  

## <a name="prerequisites"></a>Vereisten

Zorgvuldig controleren voordat u een import-taak, het overbrengen van gegevens in Azure Blob-opslag maakt, en voer de volgende lijst met vereisten voor deze service. U moet doen:

- Een actief Azure-abonnement die kan worden gebruikt voor de Import/Export-service hebben.
- Ten minste één Azure Storage-account met een opslagcontainer hebben. Overzicht van de [storage-accounts en opslagtypen ondersteund voor Import/Export-service](storage-import-export-requirements.md). Zie voor meer informatie over het maken van een nieuw opslagaccount [het maken van een Opslagaccount](storage-create-storage-account.md#create-a-storage-account). Voor informatie over de storage-container, gaat u naar [maken van een opslagcontainer](../blobs/storage-quickstart-blobs-portal.md#create-a-container).
- Zorg voor voldoende schijven van [ondersteunde typen](storage-import-export-requirements.md#supported-disks). 
- Hebt u een Windows-systeem waarop een [ondersteund besturingssysteemversie](storage-import-export-requirements.md#supported-operating-systems). 
- BitLocker inschakelen op het Windows-systeem. Zie [BitLocker inschakelen](http://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
- [Download de WAImportExport versie 1](https://www.microsoft.com/en-us/download/details.aspx?id=42659) op het Windows-systeem. Uitpakken naar de standaardmap `waimportexportv1`. Bijvoorbeeld `C:\WaImportExportV1`.


## <a name="step-1-prepare-the-drives"></a>Stap 1: Bereid de stations

Deze stap genereert een journal-bestand. Het journaalbestand slaat basisinformatie zoals serienummer van station versleutelingssleutel en de details van opslag. 

Voer de volgende stappen voor het voorbereiden van de stations.

1.  De schijfstations verbinding met het Windows-systeem via SATA-connectors.
1.  Maak één NTFS-volume op elk station. Een stationsletter toewijzen aan het volume. Stel de volgende parameter niet gebruikt.
2.  Schakel versleuteling in BitLocker op het NTFS-volume. Als een Windows Server-systeem, gebruikt u de instructies in [het inschakelen van BitLocker in Windows Server 2012 R2](http://thesolving.com/storage/how-to-enable-bitlocker-on-windows-server-2012-r2/).
3.  Gegevens kopiëren naar een versleuteld volume. Gebruik slepen en neerzetten of Robocopy of een dergelijke kopie-hulpprogramma.
4.  Open een venster PowerShell of de opdrachtregel met beheerdersbevoegdheden. Als u de map naar de map uitgepakt, voer de volgende opdracht:
    
    `cd C:\WaImportExportV1`
5.  Als u de BitLocker-sleutel van het station, voer de volgende opdracht:
    
    ` manage-bde -protectors -get <DriveLetter>: `
6.  Als u met het voorbereiden van de schijf, voer de volgende opdracht. **Afhankelijk van de omvang van de gegevens kan dit enkele uren duren tot dagen.** 

    ```
    ./WAImportExport.exe PrepImport /j:<journal file name> /id:session#<session number> /sk:<Storage account key> /t:<Drive letter> /bk:<BitLocker key> /srcdir:<Drive letter>:\ /dstdir:<Container name>/ /skipwrite 
    ```
    Een journal-bestand wordt gemaakt in dezelfde map waarin u het hulpprogramma hebt uitgevoerd. Er zijn ook twee andere bestanden gemaakt - een *.xml* bestand (map waar u het hulpprogramma uitvoeren) en een *station manifest.xml* bestand (de map is waarin gegevens zich bevindt).
    
    De parameters die worden gebruikt worden in de volgende tabel beschreven:

    |Optie  |Beschrijving  |
    |---------|---------|
    |/j:     |De naam van het bestand wijzigingslogboek, klikt u met de extensie .jrn. Een journal-bestand wordt gegenereerd per station. U wordt aangeraden dat u het serienummer van de schijf als de naam van het journaal gebruiken.         |
    |/ID:     |de sessie-ID. Gebruik een unieke sessie getal voor elk exemplaar van de opdracht.      |
    |/SK:     |De sleutel van de Azure Storage-account.         |
    |/ t:     |De stationsletter van de schijf moeten worden verzonden. Bijvoorbeeld station `D`.         |
    |/BK:     |De BitLocker-sleutel voor het station. Het wachtwoord van de numerieke van uitvoer van ` manage-bde -protectors -get D: `      |
    |/srcdir:     |De stationsletter van de schijf moeten worden verzonden, gevolgd door `:\`. Bijvoorbeeld `D:\`.         |
    |/dstdir:     |De naam van de doelcontainer in Azure Storage.         |
    |/skipwrite:     |De optie die aangeeft dat er geen nieuwe gegevens die nodig zijn om te worden gekopieerd en de bestaande gegevens op de schijf is om te worden voorbereid.          |
7. Herhaal de vorige stap voor elke schijf die moet worden verzonden. Een journal-bestand met de opgegeven naam is gemaakt voor elke run van de opdrachtregel.
    
    > [!IMPORTANT]
    > - Samen met het journaalbestand een `<Journal file name>_DriveInfo_<Drive serial ID>.xml` bestand wordt ook gemaakt in dezelfde map waarin het hulpprogramma zich bevindt. Het .xml-bestand wordt gebruikt in plaats van journal-bestand bij het maken van een taak als het logboek-bestand te groot is. 

## <a name="step-2-create-an-import-job"></a>Stap 2: Een import-taak maken

Voer de volgende stappen uit voor het maken van een import-taak in de Azure portal.

1. Meld u aan bij https://portal.azure.com/.
2. Ga naar **alle services > opslag > importeren/exporteren taken**. 
    
    ![Ga naar de taken voor importeren/exporteren](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Klik op **maken voor importeren/exporteren taak**.

    ![Klik op taak maken voor importeren/exporteren](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. In **basisbeginselen**:

    - Selecteer **importeren in Azure**.
    - Voer een beschrijvende naam voor de import-taak. De naam gebruiken om de voortgang van uw taken te volgen.
        - De naam mag alleen kleine letters, cijfers, afbreekstreepjes en onderstrepingstekens bevatten.
        - De naam moet beginnen met een letter en mag geen spaties bevatten.
    - Selecteer een abonnement.
    - Typ of Selecteer een resourcegroep.  

    ![Import-taak - stap 1 maken](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. In **taakgegevens**:

    - Het station journaal bestanden uploaden die u hebt verkregen tijdens de stap station ter voorbereiding. Als `waimportexport.exe version1` is gebruikt, uploadt een bestand voor elke schijf die u hebt voorbereid. Als de grootte van het wijzigingslogboek is groter dan 2 MB, dan kunt u de `<Journal file name>_DriveInfo_<Drive serial ID>.xml` ook gemaakt met het journaalbestand. 
    - Selecteer het doelopslagaccount waarin gegevens worden opgeslagen. 
    - De afgiftepunt wordt automatisch gevuld op basis van de regio van het geselecteerde opslagaccount.
   
   ![Stap 2 voor import-taak - maken](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. In **back-ups van gegevens retourneren**:

    - Selecteer de provider uit de vervolgkeuzelijst.
    - Voer een geldige carrier account getal dat u hebt gemaakt met deze provider. Microsoft gebruikt deze account voor de schijven naar u verzenden zodra de import-taak is voltooid. Als u een nummer niet hebt, maakt u een [FedEx](http://www.fedex.com/us/oadr/) of [DHL](http://www.dhl.com/) carrier account.
    - Geef een volledig en geldig contactpersoon, telefoon, e, adres, stad, zip, staat/provincie en land/regio.

    ![Stap 3 van importtaak - maken](./media/storage-import-export-data-to-blobs/import-to-blob5.png)
   
5. In de **samenvatting**:

    - Bekijk de informatie over de taak opgegeven in het overzicht. Noteer de taaknaam van de en de Azure-datacenter verzendadres om te verzenden schijven terug naar Azure. Deze informatie wordt later gebruikt op het label van de back-upfunctie.
    - Klik op **OK** de import-taak maken.

    ![Stap 4 van importtaak - maken](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

## <a name="step-3-ship-the-drives"></a>Stap 3: De schijven verzenden 

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]


## <a name="step-4-update-the-job-with-tracking-information"></a>Stap 4: De taak met traceringsgegevens bijwerken

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]


## <a name="next-steps"></a>Volgende stappen

* [De status van de taak en het station weergeven](storage-import-export-view-drive-status.md)
* [Overzicht van de vereisten voor importeren/exporteren](storage-import-export-requirements.md)


