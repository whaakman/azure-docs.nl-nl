---
title: 'Zelfstudie: On-premises gegevens migreren naar Azure Storage met behulp van AzCopy | Microsoft Docs'
description: In deze zelfstudie gebruikt u AzCopy om gegevens te migreren of gegevens te kopiëren naar of vanuit een blob, tabel en bestandsinhoud. Eenvoudig gegevens migreren vanuit uw lokale opslag naar Azure Storage.
services: storage
author: roygara
ms.service: storage
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: 347092fd7d5865379911265b19477ac16e3bcd69
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261380"
---
#  <a name="tutorial-migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Zelfstudie: on-premises gegevens migreren naar cloudopslag met behulp van AzCopy

AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar of van Azure Blob Storage, Azure Files en Azure Table Storage met behulp van eenvoudige opdrachten. De opdrachten zijn ontworpen voor optimale prestaties. Met AzCopy kunt u gegevens kopiëren tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts. AzCopy kan worden gebruikt om gegevens te kopiëren van lokale (on-premises) gegevens naar een opslagaccount.

Download de versie van AzCopy die geschikt is voor uw besturingssysteem:

* [AzCopy voor Linux](storage-use-azcopy-linux.md) is gebouwd met .NET Core Framework. Het is bedoeld voor Linux-platformen door opdrachtregelopties in POSIX-stijl aan te bieden. 
* [AzCopy voor Windows](storage-use-azcopy.md) is gebouwd met .NET Framework. Dit biedt opdrachtregelopties in Windows-stijl. 
 
In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een opslagaccount maken. 
> * AzCopy gebruiken voor het uploaden van al uw gegevens.
> * De gegevens wijzigen voor testdoeleinden.
> * Een geplande taak of Cron-taak maken om te zoeken naar nieuwe bestanden om te uploaden.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Om deze zelfstudie te volgen, dient u de nieuwste versie van AzCopy voor [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) of [Windows](https://aka.ms/downloadazcopy) te downloaden.

Als u Windows gebruikt, hebt u [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) nodig omdat deze opdracht in deze zelfstudie wordt gebruikt om een taak te plannen. Linux-gebruikers maken in plaats hiervan gebruik van de opdracht crontab.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="create-a-container"></a>Een container maken

De eerste stap is het maken van een container, omdat blobs altijd moeten worden geüpload in een container. Containers worden gebruikt als methode voor het ordenen van groepen blobs, zoals u bestanden zou ordenen op de computer of in mappen. 

Volg deze stappen voor het maken van een container:

1. Selecteer de knop **Opslagaccounts** op de hoofdpagina en selecteer de opslagaccount die u hebt gemaakt.
2. Selecteer **Blobs** onder **Services** en selecteer vervolgens **Container**. 

   ![Een container maken](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Containernamen moeten beginnen met een letter of cijfer. Ze mogen alleen letters, cijfers en het koppelteken (-) bevatten. Zie [Naming and referencing containers, blobs, and metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer regels voor de naamgeving van blobs en containers.

## <a name="upload-contents-of-a-folder-to-blob-storage"></a>Upload de inhoud van een map in Blob-opslag

U kunt AzCopy gebruiken om alle bestanden in een map te uploaden naar Blob-opslag in [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) of [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Als u alle blobs in een map wilt uploaden, voert u de volgende AzCopy-opdracht uit:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S
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

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S /XO
---

## <a name="create-a-scheduled-task"></a>Een geplande taak maken

U kunt een geplande taak of Cron-taak maken die een AzCopy-opdrachtscript uitvoert. Door het script worden nieuwe on-premises gegevens met een bepaald tijdsinterval herkend en geüpload naar de cloudopslag.

Kopieer de AzCopy-opdracht naar een teksteditor. Werk de parameterwaarden van de AzCopy-opdracht bij met de juiste waarden. Sla het bestand op als `script.sh` (Linux) of `script.bat` (Windows) voor AzCopy.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
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

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over manieren om on-premises gegevens te verplaatsen naar Azure Storage en vice versa, volgt u deze koppeling:

> [!div class="nextstepaction"]
> [Gegevens verplaatsen naar en uit Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

Voor meer informatie specifiek over AzCopy, kiest u het juiste artikel voor uw besturingssysteem:

> [!div class="nextstepaction"]
> [Gegevens overdragen met AzCopy in Windows](storage-use-azcopy.md)
> [Gegevens overdragen met AzCopy in Linux](storage-use-azcopy-linux.md)