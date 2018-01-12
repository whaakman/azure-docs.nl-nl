---
title: On-premises gegevens migreert naar Azure Storage met behulp van AzCopy | Microsoft Docs
description: "AzCopy gebruiken om te migreren van gegevens of gegevens kopiëren naar of van blob, table en de inhoud van bestand. Eenvoudig gegevens migreren vanaf uw lokale opslag naar Azure Storage."
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: azcopy
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: v-ruogun
ms.openlocfilehash: 3dbfb935ac0b134e127a5dccb7bc76716c688e8a
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Migreren van on-premises gegevens cloud opslag met behulp van AzCopy

AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar of van Azure Blob storage, Azure-bestanden en Azure Table storage met behulp van eenvoudige opdrachten. De opdrachten zijn ontworpen voor optimale prestaties. U kunt gegevens tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts kopiëren.  

U kunt twee versies van AzCopy downloaden:

* [AzCopy op Linux](storage-use-azcopy.md) is gebouwd met .NET Core Framework. Het is bedoeld voor Linux-platformen door het aanbieden van opdrachtregelopties POSIX-stijl. 
* [AzCopy op Windows](../storage-use-azcopy.md) is gebouwd met .NET Framework. Opdrachtregelopties voor Windows-stijl biedt. 
 
In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een opslagaccount maken. 
> * AzCopy gebruiken voor het uploaden van al uw gegevens.
> * De gegevens voor testdoeleinden wijzigen.
> * Geplande taak of cron taak maken voor het identificeren van nieuwe bestanden te uploaden.

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie kunt u de nieuwste versie van AzCopy downloaden op [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) of [Windows](http://aka.ms/downloadazcopy). 

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Als u wilt kunnen blobs downloaden van een secundaire regio naar uw lokale opslag en vice versa ingesteld **replicatie** naar **Read-access-geografisch redundante opslag**. Deze optie maakt een [geografisch redundante opslag](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) account. 
>
>

## <a name="create-a-container"></a>Een container maken

Blobs worden altijd naar een container geüpload. Containers kunt u groepen blobs ordenen zoals ordenen van uw bestanden op uw computer in mappen. 

Volg deze stappen om een container te maken:

1. Selecteer de **opslagaccounts** op de hoofdpagina en selecteer het opslagaccount dat u hebt gemaakt.
2. Selecteer **Blobs** onder **Services**, en selecteer vervolgens **Container**. 

   ![Een container maken](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Containernamen moeten beginnen met een letter of cijfer. Ze kunnen alleen letters, cijfers en het koppelteken (-) bevatten. Zie [Naming and referencing containers, blobs, and metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer regels voor de naamgeving van blobs en containers.

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

Vervang `<key>` en `key` met de accountsleutel van uw. In de Azure-portal kunt u uw accountcode ophalen door te selecteren **toegangssleutels** onder **instellingen** in uw opslagaccount. Selecteer een sleutel en plak deze in de AzCopy-opdracht. Als de opgegeven bestemming-container niet bestaat, wordt AzCopy maakt en uploadt het bestand in de App. Het bronpad bijwerken naar de map van uw gegevens en vervang **myaccount** in de doel-URL met de naam van uw opslagaccount.

Als u wilt de inhoud van de opgegeven map uploaden naar Blob storage recursief, geef de `--recursive` (Linux) of `/S` optie (Windows). Wanneer u AzCopy uitgevoerd met een van deze opties, worden alle submappen en de bestanden ook geüpload.

## <a name="upload-modified-files-to-blob-storage"></a>Gewijzigde bestanden uploaden naar Blob storage
U kunt AzCopy om te gebruiken [bestanden uploaden](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) op basis van hun tijd voor het laatst is gewijzigd. Probeer dit, wijzigen of nieuwe bestanden maken in de bronmap voor testdoeleinden. Voeg alleen bijgewerkt of nieuw om bestanden te uploaden, de `--exclude-older` (Linux) of `/XO` (Windows)-parameter voor de AzCopy-opdracht.

Als u wilt kopiëren van de bron-resources die niet bestaan in de doel-, geeft beide `--exclude-older` en `--exclude-newer` (Linux) of `/XO` en `/XN` parameters in de AzCopy-opdracht (Windows). AzCopy bestandsuploads alleen de bijgewerkte gegevens op basis van de tijdstempel.
 
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
U kunt een geplande taak of cron-taak die wordt uitgevoerd een opdrachtscript AzCopy maken. Het script identificeert en nieuwe on-premises gegevens geüpload naar de cloudopslag op een bepaald tijdsinterval. 

De AzCopy-opdracht kopiëren naar een teksteditor. Werk de parameterwaarden van de AzCopy-opdracht op de juiste waarden. Sla het bestand als `script.sh` (Linux) of `script.bat` (Windows) voor AzCopy. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy wordt uitgevoerd met de uitgebreide `--verbose` (Linux) of `/V` optie (Windows). De uitvoer wordt omgeleid naar een logboekbestand. 

In deze zelfstudie [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) gebruikt voor het maken van een geplande taak in Windows. De [Crontab](http://crontab.org/) opdracht wordt gebruikt voor het maken van een taak cron op Linux. 
 **Schtasks** kan een beheerder maken, verwijderen, query, wijzigen, uitvoeren en beëindigen van geplande taken op een lokale of externe computer. **Cron** kunnen gebruikers voor Linux en Unix op een opgegeven datum en tijd opdrachten of scripts uitvoeren met [cron expressies](https://en.wikipedia.org/wiki/Cron#CRON_expression).


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
Voor het maken van een taak cron op Linux, voer de volgende opdracht in een terminal: 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

De cron-expressie opgeven `*/5 * * * * ` in de opdracht geeft aan dat de shell-script `script.sh` moet worden uitgevoerd om de vijf minuten. U kunt het script wilt uitvoeren op een bepaald tijdstip dagelijks, maandelijks of jaarlijks plannen. Zie voor meer informatie over het instellen van de datum en tijd voor de taakuitvoering, [cron expressies](https://en.wikipedia.org/wiki/Cron#CRON_expression). 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
Voor het maken van een geplande taak op Windows, voer de volgende opdracht achter de opdrachtprompt of in PowerShell:

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Gebruikt de opdracht:
- De `/SC` parameter een minuut planning opgeven.
- De `/MO` parameter om op te geven van een interval van vijf minuten.
- De `/TN` parameter om de naam van de taak.
- De `/TR` -parameter voor het pad opgeven naar de `script.bat` bestand. 

Zie voor meer informatie over het maken van een geplande taak in Windows, [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---
 
Om te valideren of de geplande taak/cron-taak correct wordt uitgevoerd, maakt u nieuwe bestanden op uw `myfolder` directory. Wacht vijf minuten om te bevestigen dat de nieuwe bestanden zijn geüpload naar uw opslagaccount. Ga naar de logboekmap om Logboeken van de uitvoer van de geplande taak of cron taak weer te geven. 

Zie voor meer informatie over de manieren om te verplaatsen van on-premises gegevens naar Azure Storage en omgekeerd, [gegevens verplaatsen en naar Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over Azure Storage en AzCopy:

* [Inleiding tot Azure Storage](../storage-introduction.md)
* [Gegevensoverdracht met AzCopy in Windows](storage-use-azcopy.md) 
* [Gegevensoverdracht met AzCopy op Linux](storage-use-azcopy-linux.md) 
* [Een opslagaccount maken](../storage-create-storage-account.md)
* [BLOB Storage Explorer beheren](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

