---
title: Over het implementeren van Azure Files | Microsoft Docs
description: Informatie over het implementeren van Azure Files van begin tot eind.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 05/22/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: b9a98cea8ee19846afc30b322c3fdacb21f2bf44
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242446"
---
# <a name="how-to-deploy-azure-files"></a>Azure Files implementeren
[Azure Files](storage-files-introduction.md) biedt volledig beheerde bestandsshares in de cloud die toegankelijk zijn via het industriestandaard SMB-protocol. Dit artikel wordt beschreven hoe u vrijwel Azure Files implementeren binnen uw organisatie.

Het wordt aangeraden lezen [Planning voor de implementatie van Azure Files](storage-files-planning.md) voordat u de stappen in dit artikel te volgen.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u de volgende stappen al hebt voltooid:

- Een Azure Storage-Account gemaakt met de gewenste tolerantie en versleuteling opties, in de regio die u nodig hebt. Zie [een Opslagaccount maken](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor stapsgewijze instructies voor het maken van een Storage-Account.
- Een Azure-bestandsshare gemaakt met de gewenste quota in uw Opslagaccount. Zie [een bestandsshare maken](storage-how-to-create-file-share.md) voor stapsgewijze instructies voor het maken van een bestandsshare.

## <a name="transfer-data-into-azure-files"></a>Gegevens overdragen naar Azure Files
U kunt desgewenst voor het migreren van bestaande bestandsshares, zoals de lokaal opgeslagen, naar uw nieuwe Azure-bestandsshare. In deze sectie wordt beschreven hoe u gegevens verplaatsen naar een Azure-bestandsshare via verschillende populaire methoden beschreven van de [handleiding voor capaciteitsplanning](storage-files-planning.md#data-transfer-method)

### <a name="azure-file-sync"></a>Azure File Sync
Met Azure File Sync kunt u bestandsshares van uw organisatie in Azure Files centraliseren zonder in te leveren op de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Dit gebeurt door het transformeren van uw Windows-Servers in een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is in Windows Server gebruiken voor lokale toegang tot uw gegevens (inclusief SMB, NFS en FTPS) en u kunt zoveel caches hebben als waar ook ter wereld u nodig hebt.

Azure File Sync kan worden gebruikt om gegevens te migreren naar een Azure-bestandsshare, zelfs als het mechanisme voor synchronisatie is niet is voor gebruik op de lange termijn gewenst. Meer informatie over het gebruik van Azure File Sync gegevens over te dragen naar Azure-bestandsshare kan worden gevonden [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md) en [over het implementeren van Azure File Sync](storage-sync-files-deployment-guide.md).

### <a name="azure-importexport"></a>Azure Import/Export
De Azure Import/Export-service kunt u veilig grote hoeveelheden gegevens overdragen naar een Azure-bestandsshare met de verzending van harde schijven naar een Azure-datacenter. Zie [gebruik van de Microsoft Azure Import/Export-service gegevens overdraagt naar Azure storage](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor een meer gedetailleerd overzicht van de service.

> [!Note]  
> De Azure Import/Export-service biedt geen ondersteuning voor het exporteren van bestanden van een Azure-bestandsshare op dit moment.

De volgende stappen wordt naar uw Azure-bestandsshare gegevens importeren uit een on-premises locatie.

1. Het vereiste aantal harde schijven naar e-mail in Azure aanschaffen. Harde schijven van elke schijfgrootte is mogelijk, maar moet een 2,5-inch of 3,5-inch SSD of HDD ondersteunen de SATA II en III SATA-standaard. 

2. Verbinding maken en koppelen van elke schijf op de server /-PC doet de overdracht van gegevens. Voor optimale prestaties wordt u aangeraden de exporttaak voor de on-premises lokaal wordt uitgevoerd op de server die de gegevens bevat. In sommige gevallen, zoals wanneer de server die de gegevens verstuurt een NAS-apparaat, is dit niet mogelijk. In dat geval is het perfecte aanvaardbaar is voor elke schijf op een PC koppelen.

3. Zorg ervoor dat elke schijf online is, is geïnitialiseerd, en een stationsletter is toegewezen. Als u wilt doen een station online brengen, initialiseren en een stationsletter toewijzen, open de module MMC-module Schijfbeheer (diskmgmt.msc).

    - Om te zetten van een schijf online (als deze nog niet online), met de rechtermuisknop op de schijf in het onderste deelvenster van de MMC-module Schijfbeheer en selecteer 'Online'.
    - Voor het initialiseren van een schijf met de rechtermuisknop op de schijf in het onderste deelvenster (nadat de schijf online is), en selecteer 'Initialize'. Zorg dat u selecteert u 'GPT"wanneer u wordt gevraagd.

        ![Een schermopname van het menu schijf initialiseren in de MMC-module Schijfbeheer](media/storage-files-deployment-guide/transferdata-importexport-1.PNG)

    - Als u wilt een stationsletter toewijzen aan de schijf, met de rechtermuisknop op de 'niet-toegewezen' ruimte van de schijf online en geïnitialiseerd en klikt u op "Nieuw eenvoudig Volume". Dit kunt u een stationsletter toewijzen. Houd er rekening mee dat u niet het volume formatteren wilt als dit wordt later opnieuw uitgevoerd.

        ![Een schermafbeelding van de wizard Nieuw eenvoudig Volume in de MMC-module Schijfbeheer](media/storage-files-deployment-guide/transferdata-importexport-2.png)

4. Maak de gegevensset CSV-bestand. Het CSV-bestand van gegevensset is een toewijzing tussen het pad naar de gegevens on-premises en de gewenste Azure-bestandsshare de gegevens moet worden gekopieerd naar. De volgende gegevensset CSV-bestand wordt bijvoorbeeld een on-premises-bestandsshare ("F:\shares\scratch") toegewezen aan een Azure-bestandsshare ("MyAzureFileShare"):
    
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\shares\scratch\","MyAzureFileShare/",file,rename,"None",None
    ```

    Meerdere shares met een Opslagaccount kunnen worden opgegeven. Zie [voorbereiden van het CSV-bestand van gegevensset](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#prepare-the-dataset-csv-file) voor meer informatie.

5. Maak het driveset CSV-bestand. Het driveset CSV-bestand bevat de schijven beschikbaar voor de on-premises export-agent. Bijvoorbeeld, de volgende driveset CSV-bestand een lijst met `X:`, `Y:`, en `Z:` stations moet worden gebruikt in de on-premises taak exporteren:

    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    X,Format,SilentMode,Encrypt,
    Y,Format,SilentMode,Encrypt,
    Z,Format,SilentMode,Encrypt,
    ```
    
    Zie [voorbereiden van het CSV-bestand driveset](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#prepare-initialdriveset-or-additionaldriveset-csv-file) voor meer informatie.

6. Gebruik de [WAImportExport hulpprogramma](https://www.microsoft.com/en-us/download/details.aspx?id=55280) uw gegevens kopiëren naar een of meer harde schijven.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
    ```

    > [!Warning]  
    > De gegevens op de harde schijven of het logboekbestand mag niet worden gewijzigd na het voltooien van de schijfvoorbereiding van de.

7. [Een importtaak maken](../common/storage-import-export-data-to-files.md#step-2-create-an-import-job).
    
### <a name="robocopy"></a>Robocopy
Robocopy is een bekende kopie-hulpprogramma dat wordt geleverd met Windows en Windows Server. Robocopy kan worden gebruikt om gegevens over naar Azure Files te koppelen van de bestandsshare lokaal en klik vervolgens met behulp van de gekoppelde locatie als het doel in de Robocopy-opdracht. Met behulp van Robocopy is heel eenvoudig:

1. [Uw Azure-bestandsshare koppelen](storage-how-to-use-files-windows.md). Voor optimale prestaties wordt u aangeraden het koppelen van de Azure-bestandsshare lokaal op de server die de gegevens bevat. In sommige gevallen, zoals wanneer de server die de gegevens verstuurt een NAS-apparaat, is dit niet mogelijk. In dat geval is het perfecte aanvaardbaar is voor de Azure-bestandsshare koppelen op een PC. In dit voorbeeld `net use` wordt gebruikt op de opdrachtregel voor het koppelen van de bestandsshare:

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. Gebruik `robocopy` op de opdrachtregel om gegevens te verplaatsen naar de Azure-bestandsshare:

    ```
    robocopy <path-to-local-share> <path-to-azure-file-share> /E /Z /MT:32
    ```
    
    Robocopy is een groot aantal opties voor het gedrag van de kopie naar wens wijzigen. Bekijk voor meer informatie de [Robocopy](https://technet.microsoft.com/library/cc733145.aspx) handmatige pagina.

### <a name="azcopy"></a>AzCopy
AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar en van Azure Files, evenals Azure Blob-opslag met behulp van eenvoudige opdrachten met optimale prestaties. AzCopy is eenvoudig te gebruiken:

1. Download de [meest recente versie van AzCopy in Windows](https://aka.ms/downloadazcopy) of [Linux](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#download-and-install-azcopy).
2. Gebruik `azcopy` op de opdrachtregel om gegevens te verplaatsen naar de Azure-bestandsshare. De syntaxis van de op Windows is als volgt: 

    ```
    azcopy /Source:<path-to-local-share> /Dest:https://<storage-account>.file.core.windows.net/<file-share>/ /DestKey:<storage-account-key> /S
    ```

    Op Linux is de syntaxis van de opdracht er enigszins anders uit:

    ```
    azcopy --source <path-to-local-share> --destination https://<storage-account>.file.core.windows.net/<file-share>/ --dest-key <storage-account-key> --recursive
    ```

    AzCopy is een groot aantal opties voor het gedrag van de kopie naar wens wijzigen. Raadpleeg voor meer informatie, [AzCopy in Windows](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) en [AzCopy in Linux](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="automatically-mount-on-needed-pcsservers"></a>Automatisch koppelen op pc's / Servers nodig
Ter vervanging van een on-premises-bestandsshare, is het handig om vooraf koppelen van de shares op de machines die zullen worden gebruikt op. Dit kan automatisch worden uitgevoerd op een lijst met computers.

> [!Note]  
> Koppelen van een Azure-bestandsshare is vereist als het wachtwoord met behulp van de opslagaccountsleutel, raden wij daarom alleen koppelen in een vertrouwde omgeving. 

### <a name="windows"></a>Windows
PowerShell kan worden gebruikt met deze opdracht uitvoeren op meerdere pc's. In het volgende voorbeeld `$computers` handmatig is ingevuld, maar u kunt de lijst met computers te koppelen het automatisch genereren. Bijvoorbeeld, kunt u deze variabele met de resultaten van Active Directory invullen.

```PowerShell
$computer = "MyComputer1", "MyComputer2", "MyComputer3", "MyComputer4"
$computer | ForEach-Object { Invoke-Command -ComputerName $_ -ScriptBlock { net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name> /PERSISTENT:YES } }
```

### <a name="linux"></a>Linux
Een eenvoudige bash-script in combinatie met SSH kunt in het volgende voorbeeld hetzelfde resultaat opleveren. De `$computer` variabele blijft op dezelfde manier als u wilt worden ingevuld door de gebruiker:

```
computer = ("MyComputer1" "MyComputer2" "MyComputer3" "MyComputer4")
for item in "${computer[@]}"
do
    ssh $item "sudo bash -c 'echo \"//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino\" >> /etc/fstab'", "sudo mount -a"
done
```

## <a name="next-steps"></a>Volgende stappen
- [Een Azure File Sync-implementatie plannen](storage-sync-files-planning.md)
- [Problemen met Azure Files op Windows oplossen](storage-troubleshoot-windows-file-connection-problems.md)
- [Problemen oplossen met Azure Files op Linux](storage-troubleshoot-linux-file-connection-problems.md)