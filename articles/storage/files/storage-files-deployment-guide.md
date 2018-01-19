---
title: Azure-bestanden implementeren | Microsoft Docs
description: Informatie over het Azure-bestanden van begin tot eind te implementeren.
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: c33639723657d3c2875ed9607a887775d558be16
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-deploy-azure-files"></a>Azure Files implementeren
[Azure Files](storage-files-introduction.md) biedt volledig beheerd bestandsshares in de cloud die toegankelijk zijn via het SMB-standaardprotocol. In dit artikel wordt beschreven hoe u vrijwel Azure-bestanden implementeren binnen uw organisatie.

Het wordt aangeraden lezen [Planning voor de implementatie van een Azure-bestanden](storage-files-planning.md) voordat u de stappen in dit artikel.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u de volgende stappen al hebt voltooid:

- Een Azure Storage-Account gemaakt met de gewenste tolerantie en versleuteling opties, in de regio die u wenst. Zie [een Opslagaccount maken](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor stapsgewijze instructies voor het maken van een Opslagaccount.
- Een Azure-bestandsshare gemaakt met de gewenste TARGET in uw Opslagaccount. Zie [een bestandsshare maken](storage-how-to-create-file-share.md) voor stapsgewijze instructies voor het maken van een bestandsshare.

## <a name="transfer-data-into-azure-files"></a>Gegevens overdragen naar Azure-bestanden
Mogelijk wilt u de bestaande bestandsshares, zoals die lokaal opgeslagen, migreren naar uw nieuwe Azure-bestandsshare. Deze sectie wordt beschreven hoe u gegevens in een Azure-bestand delen via verschillende populaire methoden van gedetailleerde verplaatsen de [handleiding](storage-files-planning.md#data-transfer-method)

### <a name="azure-file-sync-preview"></a>Azure File-synchronisatie (Preview)
Azure File-synchronisatie (Preview) kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder geeft de flexibiliteit, prestaties en compatibiliteit van een on-premises bestand-server. Dit gebeurt door de Windows-Servers om te zetten in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Azure File-synchronisatie kan worden gebruikt om gegevens te migreren naar een Azure-bestandsshare, zelfs als het mechanisme voor synchronisatie is niet nodig voor gebruik op lange termijn. Meer informatie over het gebruik van Azure bestand Sync mag gegevens overdragen naar Azure-bestandsshare vindt u in [Planning voor de implementatie van een Azure-bestand Sync](storage-sync-files-planning.md) en [het implementeren van Azure bestand Sync](storage-sync-files-deployment-guide.md).

### <a name="azure-importexport"></a>Azure Import/Export
De Azure Import/Export-service kunt u veilig brengen grote hoeveelheden gegevens in een bestandsshare in Azure door back-upfunctie harde schijven naar een Azure-datacenter. Zie [gebruik van de Microsoft Azure Import/Export-service gegevens overdraagt naar Azure storage](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor een meer gedetailleerd overzicht van de service.

> [!Note]  
> De Azure Import/Export-service biedt geen ondersteuning voor het exporteren van bestanden van een Azure-bestandsshare op dit moment.

De volgende stappen wordt naar uw Azure-bestandsshare gegevens importeren uit een on-premises locatie.

1. Het vereiste aantal harde schijven naar e-mail schaffen in Azure. Harde schijven van elke schijfgrootte is mogelijk, maar moet een 2,5-inch of 3.5" SSD of harde schijf van de standaard SATA II of SATA III ondersteunen. 

2. Verbinding maken en koppelen van elke schijf op de server/PC tijdens het doorzoeken van de gegevensoverdracht. Voor optimale prestaties wordt u aangeraden de exporttaak lokale lokaal uitgevoerd op de server die de gegevens bevat. In sommige gevallen, zoals wanneer de bestandsserver die de gegevens fungeert een NAS-apparaat, is dit niet mogelijk. In dat geval is het perfect aanvaardbaar is voor elke schijf koppelen aan een PC.

3. Zorg ervoor dat elke schijf online is geïnitialiseerd, en een stationsletter is toegewezen. Als u wilt doen een station online brengen, initialiseren en een stationsletter toewijzen, open de module MMC-module Schijfbeheer (diskmgmt.msc).

    - Zet een schijf online (indien deze nog niet is online), met de rechtermuisknop op de schijf in het onderste deelvenster van de MMC-module Schijfbeheer en 'Online' selecteren.
    - Voor het initialiseren van een schijf met de rechtermuisknop op de schijf in het onderste deelvenster (nadat de schijf online is), en selecteer 'Initialize'. Zorg dat u selecteert 'GPT' wanneer u wordt gevraagd.

        ![Een schermopname van het menu schijf initialiseren in de MMC-module Schijfbeheer](media/storage-files-deployment-guide/transferdata-importexport-1.PNG)

    - Als u wilt een stationsletter toewijzen aan de schijf, met de rechtermuisknop op de 'vrije'-ruimte van de schijf online is en geïnitialiseerd en klik op 'Nieuw eenvoudig Volume'. Hierdoor kunt u stationsletter toewijzen. Houd er rekening mee dat u niet Formatteer het volume wilt als dit wordt later doen.

        ![Een schermopname van de wizard Nieuw eenvoudig Volume in de MMC-module Schijfbeheer](media/storage-files-deployment-guide/transferdata-importexport-2.png)

4. De gegevensset CSV-bestand maken. De gegevensset CSV-bestand is geen toewijzing tussen het pad naar de gegevens on-premises en de gegevens worden gekopieerd naar de gewenste Azure File-share. Het volgende gegevensset CSV-bestand wordt bijvoorbeeld een bestandsshare op de lokale ('F:\shares\scratch') toegewezen aan een Azure-bestandsshare ('MyAzureFileShare'):
    
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\shares\scratch\","MyAzureFileShare/",file,rename,"None",None
    ```

    Meerdere shares met een Opslagaccount kunnen worden opgegeven. Zie [voorbereiden van het CSV-bestand van gegevensset](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#prepare-the-dataset-csv-file) voor meer informatie.

5. Het driveset CSV-bestand maken. Het CSV-bestand driveset worden de schijven beschikbaar zijn voor de lokale export-agent. Bijvoorbeeld de volgende driveset CSV-bestand lijsten `X:`, `Y:`, en `Z:` stations moet worden gebruikt in de on-premises taak exporteren:

    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    X,Format,SilentMode,Encrypt,
    Y,Format,SilentMode,Encrypt,
    Z,Format,SilentMode,Encrypt,
    ```
    
    Zie [voorbereiden van het CSV-bestand driveset](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#prepare-initialdriveset-or-additionaldriveset-csv-file) voor meer informatie.

6. Gebruik de [WAImportExport hulpprogramma](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip) uw gegevens kopiëren naar een of meer harde schijven.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
    ```

    > [!Warning]  
    > De gegevens op de harde schijven of het journaalbestand mag niet worden gewijzigd na het voltooien van de schijfvoorbereiding van de.

7. [Maken van een import-taak](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-an-export-job).
    
### <a name="robocopy"></a>Robocopy
Robocopy is een bekende kopie-hulpprogramma dat wordt geleverd bij Windows en Windows Server. Robocopy kan worden gebruikt om gegevens over naar Azure-bestanden te koppelen van de bestandsshare lokaal en vervolgens de gekoppelde locatie als de bestemming in de Robocopy-opdracht. Gebruik van Robocopy is heel eenvoudig:

1. [Koppel uw Azure-bestandsshare](storage-how-to-use-files-windows.md). Voor optimale prestaties wordt u aangeraden de Azure-bestandsshare lokaal koppelen op de server die de gegevens bevat. In sommige gevallen, zoals wanneer de bestandsserver die de gegevens fungeert een NAS-apparaat, is dit niet mogelijk. In dat geval is het perfect aanvaardbaar is voor de Azure-bestandsshare op een computer koppelen. In dit voorbeeld `net use` op de opdrachtregel wordt gebruikt om de bestandsshare te koppelen:

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. Gebruik `robocopy` op de opdrachtregel om gegevens te verplaatsen naar de Azure-bestandsshare:

    ```
    robocopy <path-to-local-share> <path-to-azure-file-share> /E /Z /MT:32
    ```
    
    Robocopy heeft een groot aantal opties voor het gedrag van de kopie naar wens wijzigen. Zie voor meer informatie de [Robocopy](https://technet.microsoft.com/library/cc733145.aspx) handmatig pagina.

### <a name="azcopy"></a>AzCopy
AzCopy is een opdrachtregelprogramma dat is ontworpen voor het kopiëren van gegevens naar en van de Azure-bestanden, evenals Azure Blob-opslag met behulp van eenvoudige opdrachten met optimale prestaties. Met behulp van AzCopy is eenvoudig:

1. Download de [meest recente versie van AzCopy op Windows](http://aka.ms/downloadazcopy) of [Linux](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#download-and-install-azcopy).
2. Gebruik `azcopy` op de opdrachtregel om gegevens te verplaatsen naar de Azure-bestandsshare. De syntaxis van de op Windows is als volgt: 

    ```
    azcopy /Source:<path-to-local-share> /Dest:https://<storage-account>.file.core.windows.net/<file-share>/ /DestKey:<storage-account-key> /S
    ```

    Op Linux is de opdrachtsyntaxis enigszins verschillen:

    ```
    azcopy --source <path-to-local-share> --destination https://<storage-account>.file.core.windows.net/<file-share>/ --dest-key <storage-account-key> --recursive
    ```

    AzCopy heeft een groot aantal opties voor het gedrag van de kopie naar wens wijzigen. Zie voor meer informatie [AzCopy op Windows](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) en [AzCopy op Linux](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="automatically-mount-on-needed-pcsservers"></a>Automatisch koppelen op pc's /-Servers nodig
Ter vervanging van een lokale bestandsshare, is het handig om te koppelen van de shares vooraf op de computers die zullen worden gebruikt op. Dit kan automatisch worden gedaan op een lijst met computers.

> [!Note]  
> Koppelen van een Azure-bestandsshare met behulp van de sleutel van het opslagaccount als het wachtwoord is vereist, daarom raadzaam alleen montage in een vertrouwde omgeving. 

### <a name="windows"></a>Windows
PowerShell kan worden gebruikt met deze opdracht uitvoeren op meerdere computers. In het volgende voorbeeld `$computers` handmatig is ingevuld, maar u kunt de lijst met computers koppelen van het automatisch genereren. U kunt bijvoorbeeld deze variabele met resultaten uit Active Directory vullen.

```PowerShell
$computer = "MyComputer1", "MyComputer2", "MyComputer3", "MyComputer4"
$computer | ForEach-Object { Invoke-Command -ComputerName $_ -ScriptBlock { net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name> /PERSISTENT:YES } }
```

### <a name="linux"></a>Linux
Een eenvoudige bash-script gecombineerd met SSH kan hetzelfde resultaat in het volgende voorbeeld worden verkregen. De `$computer` variabele op dezelfde manier nog worden gevuld door de gebruiker:

```
computer = ("MyComputer1" "MyComputer2" "MyComputer3" "MyComputer4")
for item in "${computer[@]}"
do
    ssh $item "sudo bash -c 'echo \"//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino\" >> /etc/fstab'", "sudo mount -a"
done
```

## <a name="next-steps"></a>Volgende stappen
- [Een Azure-bestand Sync-implementatie plannen](storage-sync-files-planning.md)
- [Azure Files in Windows oplossen](storage-troubleshoot-windows-file-connection-problems.md)
- [Azure Files op Linux oplossen](storage-troubleshoot-linux-file-connection-problems.md)