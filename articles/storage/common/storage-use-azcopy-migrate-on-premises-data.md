---
title: On-premises gegevens migreren naar Azure Storage met behulp van AzCopy | Microsoft Docs
description: AzCopy gebruiken om gegevens te migreren of gegevens te kopiëren naar of vanuit een blob, een tabel en bestandsinhoud. Eenvoudig gegevens migreren vanuit uw lokale opslag naar Azure Storage.
services: storage
author: roygara
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: azcopy
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: rogarana
ms.openlocfilehash: 3f9735a1e5a6973ab1c1c3f575cf3aa345a3a5a4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35267438"
---
#  <a name="migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>On-premises gegevens migreren naar cloudopslag met behulp van AzCopy

AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar of van Azure Blob Storage, Azure Files en Azure Table Storage met behulp van eenvoudige opdrachten. De opdrachten zijn ontworpen voor optimale prestaties. U kunt gegevens tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts kopiëren.  

U kunt twee versies van AzCopy downloaden:

* [AzCopy voor Linux](storage-use-azcopy-linux.md) is gebouwd met .NET Core Framework. Het is bedoeld voor Linux-platformen door opdrachtregelopties in POSIX-stijl aan te bieden. 
* [AzCopy voor Windows](storage-use-azcopy.md) is gebouwd met .NET Framework. Dit biedt opdrachtregelopties in Windows-stijl. 
 
In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een opslagaccount maken. 
> * AzCopy gebruiken voor het uploaden van al uw gegevens.
> * De gegevens wijzigen voor testdoeleinden.
> * Een geplande taak of Cron-taak maken om te zoeken naar nieuwe bestanden om te uploaden.

## <a name="prerequisites"></a>Vereisten

Om deze zelfstudie te volgen, dient u de nieuwste versie van AzCopy voor [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) of [Windows](http://aka.ms/downloadazcopy) te downloaden. 

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Als u blobs uit een secundaire regio naar uw lokale opslag wilt kunnen downloaden en andersom, stelt u **Replicatie** in op **Geografisch redundante opslag met leestoegang**. Als u deze optie kiest, wordt een [geografisch redundante opslagaccount](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) gemaakt. 
>
>

## <a name="create-a-container"></a>Een container maken

Blobs worden altijd naar een container geüpload. U kunt containers gebruiken om groepen blobs te ordenen op dezelfde manier als u bestanden in mappen op de computer ordent. 

Volg deze stappen voor het maken van een container:

1. Selecteer de knop **Opslagaccounts** op de hoofdpagina en selecteer de opslagaccount die u hebt gemaakt.
2. Selecteer **Blobs** onder **Services** en selecteer vervolgens **Container**. 

   ![Een container maken](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Containernamen moeten beginnen met een letter of cijfer. Ze mogen alleen letters, cijfers en het koppelteken (-) bevatten. Zie [Naming and referencing containers, blobs, and metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer regels voor de naamgeving van blobs en containers.

## <a name="upload-all-files-in-a-folder-to-blob-storage"></a>Alle bestanden in een map naar Blob-opslag uploaden

U kunt AzCopy gebruiken om alle bestanden in een map te uploaden naar Blob-opslag in [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) of [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Als u alle blobs in een map wilt uploaden, voert u de volgende AzCopy-opdracht uit:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S
---

Vervang `<key>` en `key` door uw accountsleutel. In Azure Portal kunt u uw accountsleutel ophalen door **Toegangssleutels** te selecteren onder **Instellingen** in uw opslagaccount. Selecteer een sleutel en plak deze in de AzCopy-opdracht. Als de opgegeven doelcontainer niet bestaat, wordt deze door AzCopy gemaakt en wordt het bestand erin geüpload. Werk het bronpad bij in uw gegevensmap en vervang **myaccount** in de doel-URL door de naam van uw opslagaccount.

Als u de inhoud van de opgegeven map recursief wilt uploaden naar Blob-opslag, geeft u de optie `--recursive` (Linux) of `/S` optie (Windows) op. Wanneer u AzCopy uitvoert met een van deze opties, worden alle submappen en de bijbehorende bestanden ook geüpload.

## <a name="upload-modified-files-to-blob-storage"></a>Gewijzigde bestanden uploaden naar Blob-opslag
U kunt AzCopy gebruiken om [bestanden te uploaden](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) op basis van de datum/tijd waarop deze het laatst zijn gewijzigd. Als u dit wilt uitproberen, wijzigt u bestanden of maakt u nieuwe bestanden in uw bronmap voor testdoeleinden. Als u alleen bijgewerkte of nieuwe bestanden wilt uploaden, voegt u de parameter `--exclude-older` (Linux) of `/XO` (Windows) toe aan de AzCopy-opdracht.

Als u alleen resources wilt kopiëren die niet in het doel bestaan, geeft u zowel `--exclude-older` als `--exclude-newer` (Linux) of `/XO` als `/XN` (Windows) als parameters op in de AzCopy-opdracht. Door AzCopy worden alleen de bijgewerkte gegevens geüpload, op basis van het tijdstempel.
 
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

## <a name="create-a-scheduled-task-or-cron-job"></a>Een geplande taak of Cron-taak maken 
U kunt een geplande taak of Cron-taak maken die een AzCopy-opdrachtscript uitvoert. Door het script worden nieuwe on-premises gegevens met een bepaald tijdsinterval herkend en geüpload naar de cloudopslag. 

Kopieer de AzCopy-opdracht naar een teksteditor. Werk de parameterwaarden van de AzCopy-opdracht bij met de juiste waarden. Sla het bestand op als `script.sh` (Linux) of `script.bat` (Windows) voor AzCopy. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy wordt uitgevoerd met de uitgebreide optie `--verbose` (Linux) of `/V` (Windows). De uitvoer wordt omgeleid naar een logboekbestand. 

In deze zelfstudie wordt [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) gebruikt om een geplande taak in Windows te maken. De opdracht [Crontab](http://crontab.org/) wordt gebruikt voor het maken van een Cron-taak in Linux. 
 Met **Schtasks** kan een beheerder geplande taken op een lokale of externe computer maken, verwijderen, opvragen, wijzigen, uitvoeren en beëindigen. Met **Cron** kunnen Linux- en Unix-gebruikers op een opgegeven datum en tijd opdrachten of scripts uitvoeren met behulp van [Cron-expressies](https://en.wikipedia.org/wiki/Cron#CRON_expression).


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
Voor het maken van een Cron-taak in Linux, voert u de volgende opdracht op een terminal uit: 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Wanneer de Cron-expressie `*/5 * * * * ` wordt opgegeven in de opdracht, geeft dit aan dat het shellscript `script.sh` om de vijf minuten moet worden uitgevoerd. U kunt het script plannen voor uitvoering op een bepaald dagelijks, maandelijks of jaarlijks tijdstip. Zie [Cron-expressies](https://en.wikipedia.org/wiki/Cron#CRON_expression) voor meer informatie over het instellen van de datum en tijd voor de taakuitvoering. 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
Als u een geplande taak in Windows wilt maken, voert u de volgende opdracht uit via de opdrachtprompt of in PowerShell:

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

De opdracht gebruikt:
- De parameter `/SC` om een planning van een minuut op te geven.
- De parameter `/MO` om een interval van vijf minuten op te geven.
- De parameter `/TN` om de naam van de taak op te geven.
- De parameter `/TR` om het pad naar het bestand `script.bat` op te geven. 

Zie [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes) voor meer informatie over het maken van een geplande taak in Windows.

---
 
Als u wilt valideren of de geplande taak/Cron-taak correct wordt uitgevoerd, maakt u nieuwe bestanden in de map `myfolder`. Wacht vijf minuten om te bevestigen dat de nieuwe bestanden zijn geüpload naar uw opslagaccount. Ga naar de logboekmap om de uitvoerlogboeken van de geplande taak of Cron-taak weer te geven. 

Zie [Gegevens verplaatsen vanuit en naar Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor meer informatie over de manieren om on-premises gegevens te verplaatsen naar Azure Storage en omgekeerd.  

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over Azure Storage en AzCopy:

* [Kennismaking met Azure Storage](../storage-introduction.md)
* [Gegevens overdragen met AzCopy voor Windows](storage-use-azcopy.md) 
* [Gegevens overdragen met AzCopy voor Linux](storage-use-azcopy-linux.md) 
* [Een opslagaccount maken](../storage-create-storage-account.md)
* [Blobs beheren met Opslagverkenner](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

