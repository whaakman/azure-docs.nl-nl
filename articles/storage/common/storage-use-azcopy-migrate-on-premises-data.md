---
title: On-premises gegevens migreren naar Azure Storage met AzCopy | Microsoft Docs
description: "AzCopy gebruiken om te migreren van gegevens of gegevens kopiëren naar of van blob, table en de inhoud van bestand. Eenvoudig gegevens migreren vanaf uw lokale opslag naar Azure-opslag."
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: azcopy
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: v-ruogun
ms.openlocfilehash: f2c0b80248ef706394b3f84df4c3da26fb79026a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-with-azcopy"></a>On-premises gegevens migreren naar cloudopslag met AzCopy

AzCopy is een opdrachtregelprogramma dat is ontworpen voor het kopiëren van gegevens uit Microsoft Azure Blob-, bestands- en tabel opslag, met behulp van eenvoudige opdrachten die zijn ontworpen voor optimale prestaties. U kunt gegevens tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts kopiëren.  

Er zijn twee versies van AzCopy die u kunt downloaden:

* [AzCopy op Linux](storage-use-azcopy.md) is gebouwd met .NET Core Framework dat gericht is op Linux-platforms biedt POSIX-stijl opdrachtregelopties. 
* [AzCopy op Windows](../storage-use-azcopy.md) is gebouwd met .NET Framework en Windows-stijl biedt opdrachtregelopties. 
 
In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een opslagaccount maken 
> * AzCopy gebruiken voor het uploaden van al uw gegevens
> * Wijzigen van de gegevens voor testdoeleinden
> * Geplande taak of cron taak maken voor het identificeren van nieuwe bestanden uploaden

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie: 

* Download de nieuwste versie van AzCopy op [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) of [Windows](http://aka.ms/downloadazcopy). 

## <a name="log-in-to-the-azure-portalhttpsportalazurecom"></a>Meld u aan bij [Azure Portal](https://portal.azure.com/).

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Als u wilt kunnen blobs downloaden van een secundaire regio naar uw lokale opslag en vice versa ingesteld **replicatie** naar **Read-access-geografisch redundante opslag**. Deze optie maakt een [geografisch redundante opslag](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) account. 
>
>

## <a name="create-a-container"></a>Een container maken

Blobs worden altijd naar een container geüpload. Containers kunt u groepen blobs ordenen zoals ordenen van uw bestanden op uw computer in mappen. 

Volg deze stappen om een container te maken:

1. Selecteer de **opslagaccounts** op de hoofdpagina en selecteer het opslagaccount dat u hebt gemaakt.
2. Selecteer **Blobs** onder **Services**, selecteer daarna **Container**. 

![Een container maken](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Containernamen moeten beginnen met een letter of cijfer en mogen alleen letters, cijfers en het koppelteken (-) bevatten. Zie [Naming and referencing containers, blobs, and metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer regels voor de naamgeving van blobs en containers.

## <a name="upload-all-files-in-a-folder-to-blob-storage"></a>Alle bestanden in een map uploaden naar Blob storage

U kunt AzCopy gebruiken voor het uploaden van alle bestanden in een map in Blob-opslag op [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) of [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Als u wilt uploaden alle blobs in een map, voer de volgende AzCopy-opdracht:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S
---

Vervang  **\<sleutel\>**  met de accountsleutel van uw. In de Azure-portal kunt u uw accountcode ophalen door te selecteren **toegangssleutels** onder de instellingen in uw opslagaccount. Selecteer een sleutel en plak deze in de AzCopy-opdracht. Als de opgegeven bestemming-container niet bestaat, wordt AzCopy maakt en uploadt het bestand in de App. Het bronpad bijwerken naar de map van uw gegevens en vervang **myaccount** in de doel-URL voor de naam van uw opslagaccount.

Geef de `--recursive` en `/S` opties, op Linux- en Windows respectievelijk om de inhoud van de opgegeven map uploaden naar Blob storage recursief. Wanneer u AzCopy uitgevoerd met een van deze opties, worden alle submappen en de bestanden ook geüpload.

## <a name="upload-modified-files-to-blob-storage"></a>Gewijzigde bestanden uploaden naar Blob storage
U kunt AzCopy om te gebruiken [bestanden uploaden](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) op basis van hun tijd voor het laatst is gewijzigd. Probeer dit, wijzigen of nieuwe bestanden in de bronmap voor het testen van doel maken. Uploaden van bestanden alleen bijgewerkt of nieuw toevoegen `--exclude-older`of `/XO` respectievelijk opdracht de parameter voor de AzCopy Linux- en Windows.

Als u wilt kopiëren van de bron-resources die niet bestaan in de doel-, geeft beide `--exclude-older` en `--exclude-newer` of `/XO` en `/XN` parameters in de AzCopy Linux en Windows opdracht respectievelijk. AzCopy bestandsuploads alleen de bijgewerkte gegevens op basis van hun timestamp.
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S /XO
---

## <a name="create-a-scheduled-task-or-cron-job"></a>Een geplande taak of cron-taak maken 
U kunt maken als een geplande taak/cron-taak die een script AzCopy-opdracht die uitvoert identificeert en uploadt deze nieuwe on-premises gegevens naar de cloud-opslag op een bepaald tijdsinterval. 

De AzCopy-opdracht kopiëren naar een teksteditor. Werk de parameterwaarden van AzCopy-opdracht op de juiste waarden. Sla het bestand als `script.sh` of `script.bat` voor AzCopy op Linux- en Windows respectievelijk. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy wordt uitgevoerd met uitgebreide `--verbose` (Linux) en `/V` optie (Windows) en de uitvoer wordt omgeleid naar een logboekbestand. 

In deze zelfstudie [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) gebruikt voor het maken van een geplande taak op Windows, en [Crontab](http://crontab.org/) opdracht wordt gebruikt voor het maken van een taak cron op Linux. 
 **Schtasks** kan een beheerder maken, verwijderen, query, wijzigen, uitvoeren en beëindigen van geplande taken op lokale of externe computer. **Cron** kunnen gebruikers van Linux en Unix-opdrachten of scripts uitvoeren op de opgegeven datum en tijd met [cron-expressie](https://en.wikipedia.org/wiki/Cron#CRON_expression)


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
**Maken van een taak cron op Linux**, voer de volgende opdracht in een terminal. 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

De cron-expressie opgeven `*/5 * * * * ` in de opdracht geeft de shell-script `script.sh` moet worden uitgevoerd om de vijf minuten. Het script kan worden uitgevoerd op een bepaald tijdstip dagelijks, maandelijks of jaarlijks worden gepland. Zie [cron expressies](https://en.wikipedia.org/wiki/Cron#CRON_expression) voor meer informatie over het instellen van de datum en tijd voor de taakuitvoering. 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
**Een geplande taak te maken op Windows**, voer de volgende opdracht op de opdrachtprompt of PowerShell.

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Gebruikt de opdracht de `/SC` parameter om een minuut planning te specificeren en de `/MO` parameter om op te geven van een interval van vijf minuten. De `/TN` parameter wordt gebruikt om op te geven van de naam van de taak en `/TR` het pad naar de parameter `script.bat` bestand. Ga naar [schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes) voor meer informatie over het maken van de geplande taak in Windows.

---
 
Valideren van de geplande taak/cron taak wordt uitgevoerd correct, nieuwe bestanden maken in uw directory `myfolder`. Wacht vijf minuten om te bevestigen dat de nieuwe bestanden zijn geüpload naar uw opslagaccount. Ga naar de logboekmap om Logboeken van de uitvoer van de geplande taak/cron-taak weer te geven. 

Ga naar [verplaatsen op lokale gegevens](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) meer informatie over het verplaatsen van on-premises gegevens naar Azure-opslag en vice versa.  

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over Azure Storage en AzCopy:

### <a name="azure-storage-documentation"></a>Documentatie bij Azure Storage:
* [Inleiding tot Azure Storage](../storage-introduction.md)
* [Gegevensoverdracht met AzCopy in Windows](storage-use-azcopy.md) 
* [Gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md) 
* [Een opslagaccount maken](../storage-create-storage-account.md)
* [BLOB Storage Explorer beheren](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

