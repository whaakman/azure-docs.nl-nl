---
title: 'Zelfstudie: On-premises gegevens migreren naar Azure Storage met behulp van AzCopy | Microsoft Docs'
description: In deze zelfstudie gebruikt u AzCopy om gegevens te migreren of gegevens te kopiëren naar of vanuit een blob, tabel en bestandsinhoud. Eenvoudig gegevens migreren vanuit uw lokale opslag naar Azure Storage.
services: storage
author: normesta
ms.service: storage
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: normesta
ms.reviewer: seguler
ms.subservice: common
ms.openlocfilehash: 193c00354b6222152e26476d0b06cfb1555c207e
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754874"
---
#  <a name="tutorial-migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Zelfstudie: On-premises gegevens migreren naar cloudopslag met behulp van AzCopy

AzCopy is een opdrachtregelprogramma voor het kopiëren van gegevens naar of van Azure Blob Storage, Azure Files en Azure Table Storage met behulp van eenvoudige opdrachten. De opdrachten zijn ontworpen voor optimale prestaties. Met AzCopy kunt u gegevens kopiëren tussen een bestandssysteem en een opslagaccount of tussen opslagaccounts. AzCopy kan worden gebruikt om gegevens te kopiëren van lokale (on-premises) gegevens naar een opslagaccount.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een opslagaccount maken. 
> * AzCopy gebruiken voor het uploaden van al uw gegevens.
> * De gegevens wijzigen voor testdoeleinden.
> * Een geplande taak of Cron-taak maken om te zoeken naar nieuwe bestanden om te uploaden.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie, moet u de nieuwste versie van AzCopy downloaden. Zie [aan de slag met AzCopy](storage-use-azcopy-v10.md).

Als u Windows gebruikt, hebt u [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) nodig omdat deze opdracht in deze zelfstudie wordt gebruikt om een taak te plannen. Linux-gebruikers maken in plaats hiervan gebruik van de opdracht crontab.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="create-a-container"></a>Een container maken

De eerste stap is het maken van een container, omdat blobs altijd moeten worden geüpload in een container. Containers worden gebruikt als methode voor het ordenen van groepen blobs, zoals u bestanden zou ordenen op de computer of in mappen.

Volg deze stappen voor het maken van een container:

1. Selecteer de knop **Opslagaccounts** op de hoofdpagina en selecteer de opslagaccount die u hebt gemaakt.
2. Selecteer **Blobs** onder **Services** en selecteer vervolgens **Container**.

   ![Een container maken](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Containernamen moeten beginnen met een letter of cijfer. Ze mogen alleen letters, cijfers en het koppelteken (-) bevatten. Zie [Naming and referencing containers, blobs, and metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer regels voor de naamgeving van blobs en containers.

## <a name="download-azcopy"></a>AzCopy downloaden

Het uitvoerbare bestand van AzCopy V10 downloaden.

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

Plaats het bestand AzCopy overal op uw computer. De locatie van het bestand toevoegen aan uw padomgevingsvariabele system, zodat u naar dit uitvoerbare bestand in een map op uw computer verwijzen kunt.

## <a name="authenticate-with-azure-ad"></a>Verifiëren met Azure AD

Eerst, wijzen de [Gegevensbijdrager voor Blob](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor) rol aan uw identiteit. Zie [toegang verlenen tot Azure blob- en wachtrijservices gegevens met RBAC in Azure portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal).

Vervolgens opent u een opdrachtprompt, typ de volgende opdracht en druk op ENTER.

```azcopy
azcopy login
```

Met deze opdracht retourneert een verificatiecode op te geven en de URL van een website. Open de website, de op te geven en kies vervolgens de **volgende** knop.

![Een container maken](media/storage-use-azcopy-v10/azcopy-login.png)

Er wordt een venster Aanmelden weergegeven. In dit venster, meld u aan bij uw Azure-account met behulp van de referenties van uw Azure-account. Nadat u hebt aangemeld, kunt u het browservenster sluiten en beginnen met behulp van AzCopy.

## <a name="upload-contents-of-a-folder-to-blob-storage"></a>Upload de inhoud van een map in Blob-opslag

U kunt AzCopy gebruiken om alle bestanden in een map te uploaden naar Blob-opslag in [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) of [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux). Als u alle blobs in een map wilt uploaden, voert u de volgende AzCopy-opdracht uit:

```AzCopy
azcopy copy "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive=true
```

* Vervang de `<local-folder-path>` tijdelijke aanduiding door het pad naar een map met bestanden (bijvoorbeeld: `C:\myFolder` of `/mnt/myFolder`).

* Vervang de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

* Vervang de `<container-name>` tijdelijke aanduiding door de naam van de container die u hebt gemaakt.

Als u wilt de inhoud van de opgegeven map uploaden naar Blob storage recursief, geef de `--recursive` optie. Wanneer u AzCopy met deze optie uitvoert, worden alle submappen en de bestanden ook geüpload.

## <a name="upload-modified-files-to-blob-storage"></a>Gewijzigde bestanden uploaden naar Blob-opslag

U kunt AzCopy gebruiken voor het uploaden van bestanden op basis van hun tijd van laatste wijziging. 

Als u dit wilt uitproberen, wijzigt u bestanden of maakt u nieuwe bestanden in uw bronmap voor testdoeleinden. Vervolgens gebruikt u de AzCopy `sync` opdracht.

```AzCopy
azcopy sync "<local-folder-path>" "https://<storage-account-name>.blob.core.windows.net/<container-name>" --recursive=true
```

* Vervang de `<local-folder-path>` tijdelijke aanduiding door het pad naar een map met bestanden (bijvoorbeeld: `C:\myFolder` of `/mnt/myFolder`.

* Vervang de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

* Vervang de `<container-name>` tijdelijke aanduiding door de naam van de container die u hebt gemaakt.

Voor meer informatie over de `sync` opdracht, Zie [bestanden te synchroniseren](storage-use-azcopy-blobs.md#synchronize-files).

## <a name="create-a-scheduled-task"></a>Een geplande taak maken

U kunt een geplande taak of Cron-taak maken die een AzCopy-opdrachtscript uitvoert. Door het script worden nieuwe on-premises gegevens met een bepaald tijdsinterval herkend en geüpload naar de cloudopslag.

Kopieer de AzCopy-opdracht naar een teksteditor. Werk de parameterwaarden van de AzCopy-opdracht bij met de juiste waarden. Sla het bestand op als `script.sh` (Linux) of `script.bat` (Windows) voor AzCopy. 

Deze voorbeelden wordt ervan uitgegaan dat de map met de naam `myFolder`, de naam van uw opslagaccount is `mystorageaccount` en de naam van uw container `mycontainer`.

> [!NOTE]
> Het Linux-voorbeeld voegt een SAS-token. U moet opgeven in de opdracht. De huidige versie van AzCopy V10 biedt geen ondersteuning voor Azure AD-autorisatie in cron-taken.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy sync "/mnt/myfiles" "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-05-30T06:57:40Z&st=2019-05-29T22:57:40Z&spr=https&sig=BXHippZxxx54hQn%2F4tBY%2BE2JHGCTRv52445rtoyqgFBUo%3D" --recursive=true

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    azcopy sync "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive=true

---

In deze zelfstudie wordt [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) gebruikt om een geplande taak in Windows te maken. De opdracht [Crontab](http://crontab.org/) wordt gebruikt voor het maken van een Cron-taak in Linux.

 Met **Schtasks** kan een beheerder geplande taken op een lokale of externe computer maken, verwijderen, opvragen, wijzigen, uitvoeren en beëindigen. Met **Cron** kunnen Linux- en Unix-gebruikers op een opgegeven datum en tijd opdrachten of scripts uitvoeren met behulp van [Cron-expressies](https://en.wikipedia.org/wiki/Cron#CRON_expression).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Voor het maken van een Cron-taak in Linux, voert u de volgende opdracht op een terminal uit:

```bash
crontab -e
*/5 * * * * sh /path/to/script.sh
```

Wanneer de Cron-expressie `*/5 * * * *` wordt opgegeven in de opdracht, geeft dit aan dat het shellscript `script.sh` om de vijf minuten moet worden uitgevoerd. U kunt het script plannen voor uitvoering op een bepaald dagelijks, maandelijks of jaarlijks tijdstip. Zie [Cron-expressies](https://en.wikipedia.org/wiki/Cron#CRON_expression) voor meer informatie over het instellen van de datum en tijd voor de taakuitvoering.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Als u een geplande taak in Windows wilt maken, voert u de volgende opdracht uit via de opdrachtprompt of in PowerShell:

In dit voorbeeld wordt ervan uitgegaan dat uw script bevindt zich in het basisstation van de computer, maar uw script overal zijn kan dat u wilt.

```cmd
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\script.bat
```

De opdracht gebruikt:
- De parameter `/SC` om een planning van een minuut op te geven.
- De parameter `/MO` om een interval van vijf minuten op te geven.
- De parameter `/TN` om de naam van de taak op te geven.
- De parameter `/TR` om het pad naar het bestand `script.bat` op te geven.

Zie [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes) voor meer informatie over het maken van een geplande taak in Windows.

---

Als u wilt valideren of de geplande taak/Cron-taak correct wordt uitgevoerd, maakt u nieuwe bestanden in de map `myFolder`. Wacht vijf minuten om te bevestigen dat de nieuwe bestanden zijn geüpload naar uw opslagaccount. Ga naar de logboekmap om de uitvoerlogboeken van de geplande taak of Cron-taak weer te geven.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over manieren om on-premises gegevens te verplaatsen naar Azure Storage en vice versa, volgt u deze koppeling:

* [Gegevens verplaatsen naar en uit Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

Zie voor meer informatie over AzCopy, een van deze artikelen:

* [Aan de slag met AzCopy](storage-use-azcopy-v10.md)

* [Gegevens overdragen met AzCopy en blob-opslag](storage-use-azcopy-blobs.md)

* [Gegevens overdragen met AzCopy en bestandsopslag](storage-use-azcopy-files.md)

* [Gegevens overdragen met AzCopy en Amazon S3 buckets](storage-use-azcopy-s3.md)
 
* [Configureer, optimaliseren en oplossen van AzCopy](storage-use-azcopy-configure.md)
